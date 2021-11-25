# StackStorm

Passive sensor examples

```sourcegraph
context:stackstorm from st2reactor.sensor.base import Sensor
```

Polling sensor examples

```sourcegraph
context:stackstorm from st2reactor.sensor.base import PollingSensor
```

Trigger examples in rules

```sourcegraph
context:stackstorm repo:Exchange trigger: file:.yaml$
```

Actions that use the Orquesta runner

```sourcegraph
context:stackstorm repo:Exchange runner_type:\s*"orquesta"
```

All instances where a trigger is injected with an explicit payload

```sourcegraph
context:stackstorm repo:Exchange sensor_service.dispatch(:[1], payload=:[2])
```
