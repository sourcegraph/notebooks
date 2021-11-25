# Interactive introduction to the codebase

This is a technical deep dive into how OpenVSCode Server turns VS Code into a web IDE, with interactive code search queries and snippets. This is [best viewed on Sourcegraph](https://sourcegraph.com/github.com/sourcegraph/openvscode-server/-/blob/doc/sourcedive.snb.md).

The code snippets in this file correspond to search queries and can be displayed by clicking the blue "Run search" button to the right of each query. For example, here is a snippet that shows off an instance of dependency injection within VS Code:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@5c8a1f/-/blob/src/vs/code/browser/workbench/workbench.ts?L517-531

## Architectural overview

OpenVSCode Server is a fork of VS Code that extends the editor to be runnable in the browser, speaking to a web server that provides a remote dev environment.

Upstream VS Code consists of [layers](https://github.com/microsoft/vscode/wiki/Source-Code-Organization):

- `base`: Provides general utilities and user interface building blocks.
- `platform`: Defines service injection support and the base services for VS Code.
- `editor`: The "Monaco" editor is available as a separate downloadable component.
- `workbench`: Hosts the "Monaco" editor and provides the framework for "viewlets" like the Explorer, Status Bar, or Menu Bar, leveraging Electron to implement the VS Code desktop application.
- `code`: The entry point to the desktop app that stitches everything together, this includes the Electron main file and the CLI for example.

OpenVSCode Server adds an additional [`server` layer](https://github.com/gitpod-io/openvscode-server/tree/main/src/vs/server). The client side remains largely unchanged, save for the injection of RPC-based handlers for things like filesystem and terminal interactions, in place of local handlers. The `server` layer has 3 main components:

- Web-based workbench
- Remote server
- Remote CLI

The web-based workbench lives on the client side and is the place where the RPC-based dependencies are injected. VS Code's codebase is modular and makes heavy use of dependency injection, which makes it easier to substitute different implementations. The entrypoint into the web-based workbench is in [workbench.ts](https://sourcegraph.com/github.com/gitpod-io/openvscode-server/-/blob/src/vs/code/browser/workbench/workbench.ts). In that file, the `create` function creates the workbench using dependency injection:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@5c8a1f/-/blob/src/vs/server/browser/workbench/workbench.ts?L469-498

The workbench talks to the remote server via RPC. There are 2 RPC channels:

- The management connection handles filesystem and terminal requests
- The extension connection creates the remote extension host process and handles extension-related requests

Let's take a look at how those connections are set up on the server side. The main entrypoint into the server lives in [`server.main.ts`](https://sourcegraph.com/github.com/gitpod-io/openvscode-server@5c8a1f/-/blob/src/vs/server/node/server.main.ts?L11):

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@5c8a1f/-/blob/src/vs/server/node/server.main.ts?L328

Of particular note in the `main` function is `channelServer`, which registers different service channels for handling different types of requests received from the client, such as logging, debugging, and filesystem requests.

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@5c8a1f/-/blob/src/vs/server/node/server.main.ts?L352

These channels then relay the requests to the appropriate service implementations. Lower in the `main` function, a `ServiceCollection` instance is used as a dependency injection container that holds all the concrete implementations of the various service types:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/server.main.ts?L612

Then the whole bundle is wrapped by an HTTP server, which is the outermost container that handles requests from the client:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/server.main.ts?L664

Some of the handler endpoints correspond to static resources:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/server.main.ts?L677

Then there are the endpoints that upgrade a HTTP request to a WebSocket connection:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/server.main.ts?L730

The aforementioned management connection and extension connection use these WebSocket connections. The management connection connects `channelServer` with your editor window, including requests for handling terminal and fileystem requests.

On the extension side, there's a special protocol over WebSocket that initiates a handshake to set up the remote extension host process:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/server.main.ts?L771

The extension connection will fork the extension host process and connect the user's editor window. Among other things, keystrokes are sent down this connection, as they may be relevant to extensions in use.

## Startup

Now, let's walk through what happens at startup.

On server startup, first we create the channel server and register channels to handle RPC calls and events from the web workbench:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/server.main.ts?L354

Then we create the service collection (effectively the dependency injection container for service implementations, as described earlier) with all services required for the RPC channels:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/server.main.ts?L613

Then we instantiate these services and start the HTTP server:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/server.main.ts?L662

When a user tries to access the server, the web workbench is served by HTTP listener:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/server.main.ts?L689

The web workbench first loads 3rd party dependencies like xterm:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/browser/workbench/workbench-dev.html?L46

...and then itself:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/browser/workbench/workbench-dev.html?L61

The web workbench uses dependency injection to configure how to establish WebSockets, load static resources, load webviews, and so on:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/browser/workbench/workbench.ts?L469-498

When the web workbench is created, it opens WebSocket connections to the server:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/server.main.ts?L731

There is one connection to the RPC channel server to notify about a new client:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/server.main.ts?L841

...and another for the extension host process which is running remote extensions:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/server.main.ts?L913

When a user creates a new terminal the management connection is used to call the remote terminal channel:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/remote-terminal.ts?L81

...which delegates to pseudo terminal service:

https://sourcegraph.com/github.com/gitpod-io/openvscode-server@8b3e975/-/blob/src/vs/server/node/remote-terminal.ts?L252

The terminal service then enacts the corresponding action and relays the response back through the request chain covered above.

## Diving in

This hopefully gives you a good overview of how OpenVSCode Server turns VS Code into a web-based IDE. The code is completely open-source and released by Gitpod. You can try out [Gitpod](https://www.gitpod.io/) as a service or dive into more of the [source code on Sourcegraph](https://sourcegraph.com/github.com/gitpod-io/openvscode-server).
