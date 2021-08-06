# StackStorm

Passive sensor examples

```sourcegraph
repogroup:stackstorm from st2reactor.sensor.base import Sensor
```
Polling sensor examples

```sourcegraph
repogroup:stackstorm from st2reactor.sensor.base import PollingSensor
```

Trigger examples in rules

```sourcegraph
repogroup:stackstorm repo:Exchange trigger: file:.yaml$
```

Actions that use the Orquesta runner

```sourcegraph
repogroup:stackstorm repo:Exchange runner_type:\s*"orquesta"
```

All instances where a trigger is injected with an explicit payload

```sourcegraph
repogroup:stackstorm repo:Exchange sensor_service.dispatch(:[1], payload=:[2])
```
