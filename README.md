# Home Assistant Blueprints

## ERS-10TZBVK-AA.yaml

Forked from and with thanks to:  https://github.com/pbergman/ha-blueprints

This version exposes the "action_step_size" data in to the blueprint variables.  You can then use this to dynamically change the step size when, for example increasing the brightness.  The step size is between 1 and 255.  Instead of, for example, adding 10 to the brightness, instead add `{{ step_size }}`.

An action might be:

```
service: light.turn_on
target:
  device_id: 1234567890abcdef1234
data:
  brightness: >
    {% set current_bri = state_attr('light.my_smart_bulb','brightness')|int(255) %}
    {% set new_bri = current_bri + step_size %}
    {% if new_bri > 255 %}
      {% set new_bri = 255 %}
    {% endif %}
    {{ new_bri }}
```

Then when you turn the knob slowly the brightness will increase slowly, when you turn the knob quickly it will increase quickly.

Be aware of the lag between turning the knob and the execuction of the action, it can be quite slow.  Also, if you turn the knob too quickly the knob struggles to send the correct data.

