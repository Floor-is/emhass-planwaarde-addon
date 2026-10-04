# EMHASS planwaarde

This add-on runs [Floor-is/emhass](https://github.com/Floor-is/emhass), branch `planwaarde`: EMHASS with three
extra runtime parameters. Without those parameters it behaves like the upstream version in the version number
(`v0.18.4-planwaarde.5` = EMHASS v0.18.4). For EMHASS itself, see
[davidusb-geek/emhass](https://github.com/davidusb-geek/emhass) and the official
[add-on](https://github.com/davidusb-geek/emhass-add-on).

| parameter | unit | what it does |
|---|---|---|
| `battery_terminal_value` | EUR/kWh | no fixed end state; energy left at the end of the horizon is worth this much (0 = free down to the minimum) |
| `deferrable_load_energy_max` | Wh per load | the requirement becomes `requirement <= E <= max`; the requirement is the floor |
| `deferrable_load_value` | EUR/kWh per load | every kWh into the load is worth this much |

An unreachable requirement does not make the plan infeasible: column `deferrable<k>_shortfall_wh` shows the
shortfall. Details on the [fork's front page](https://github.com/Floor-is/emhass).

## Alongside the official add-on

- Set `method_ts_round: "first"` in this add-on's `config.json`. With `"nearest"` every runtime input (prices, PV,
  load) sits one step late under the plan labels when an optimisation runs in the second half of a time step.

- ⚠️ Set `continual_publish: false` in this add-on's `config.json` when it runs next to the official one.
  Otherwise both publish under the same sensor names (`sensor.p_batt_forecast`, …) and overwrite each other.
  For the same reason, do not call `publish-data` on this add-on while the official one drives your system.
- Port **5001** on the host (the official add-on keeps 5000). The web UI also works through ingress.
- Config and data live in `addon_configs/<slug>/` (`/config` inside the container). `/share` is not mapped:
  the official add-on keeps its `config.json`, `params.pkl` and results in `/share/emhass`, and this add-on
  cannot reach them.
- Put a `config.json` in `addon_configs/<slug>/` before the first start. Without it EMHASS starts with its
  factory defaults.

## Updates

The version points to a fixed fork tag. A new upstream release does not arrive by itself: the fork rebases
daily (workflow `PLANWAARDE upstream watch`) and turns red if the patch no longer applies or no longer works.
A new add-on version needs a new fork tag and a version change here.
