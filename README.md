# Predbat Card

[![HACS](https://img.shields.io/badge/HACS-Default-41BDF5.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=mimikm&repository=Predbat-Card&category=plugin)
[![GitHub release](https://img.shields.io/github/v/release/mimikm/Predbat-Card)](https://github.com/mimikm/Predbat-Card/releases)
[![Downloads](https://img.shields.io/github/downloads/mimikm/Predbat-Card/total?label=downloads&color=7950f2)](https://github.com/mimikm/Predbat-Card/releases)
[![Downloads of latest release](https://img.shields.io/github/downloads/mimikm/Predbat-Card/latest/total?label=latest%20release)](https://github.com/mimikm/Predbat-Card/releases/latest)
[![Validate](https://github.com/mimikm/Predbat-Card/actions/workflows/validate.yaml/badge.svg)](https://github.com/mimikm/Predbat-Card/actions/workflows/validate.yaml)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20me%20a%20coffee-FFDD00?logo=buymeacoffee&logoColor=000)](https://buymeacoffee.com/mimikm)

A Home Assistant Lovelace card that shows what [Predbat](https://github.com/springfall2008/batpred) is doing and what it plans to do next, at a glance.

<table>
  <tr>
    <th>Dark theme</th>
    <th>Light theme</th>
  </tr>
  <tr>
    <td><img src="https://github.com/mimikm/Predbat-Card/raw/main/images/preview-dark.png" alt="Predbat Card in a dark theme" width="100%"></td>
    <td><img src="https://github.com/mimikm/Predbat-Card/raw/main/images/preview-light.png" alt="Predbat Card in a light theme" width="100%"></td>
  </tr>
</table>

## Features

- **Status header** – current Predbat status, mode, last update and version. Tap to open more-info.
- **Overview tiles** – battery SoC ring, what the battery is doing now with current import/export rates, cost today with a forecast for the rest of the day, predicted saving today (with yesterday's actual saving), next charge and next export windows (real charge and export slots only; hold and freeze can be included with `next_include_hold`).
- **Battery forecast chart** – the best-plan SoC over the next hours, the baseline for comparison, the import rate as a step line, reserve level and shaded charge/export windows. Hover or tap for details.
- **Grid import & export chart** – predicted export (up) and import (down) for every plan slot, with Predbat's solar and house load forecast drawn over the top and totals for the period.
- **Solar forecast** – a Solar tab for today, tomorrow and the next two days: today's actual generation against the forecast, the forecast line, the 10–90% range as a shaded band, Predbat's calibrated forecast against the raw one, plus a Solar today tile. See [Solar, carbon and inverters](#solar-carbon-and-inverters).
- **Carbon intensity** – when Predbat's carbon option is on: a Grid carbon tile and a Carbon tab with the grid's carbon intensity per slot, the cleanest hour and the plan's total.
- **Per-inverter breakdown** – with two or more inverters, each one's battery level, charging or discharging power and solar power.
- **Energy rates chart** – import and export rates as step lines with charge/export windows shaded.
- **Car charging** – each car's charge level now and at the end of the plan, Predbat's planned charging slots with energy and cost, a Car chart tab and the slots on the plan timeline. See [Car and iBoost](#car-and-iboost).
- **iBoost (hot water)** – energy used today against the daily limit, whether it's heating now, and planned heating windows.
- **Zero-load alarm** – spots the "zero load" problems Predbat warns about in its log (your daily load total stops rising while the house is using power), with a header chip, a list of recent periods and a 7-day data-quality view. See [Zero-load alarm](#zero-load-alarm).
- **Charts your way** – turn each chart on or off, and show them as tabs (one at a time) or stacked.
- **Drag-and-drop layout** – rearrange sections, overview tiles and chart tabs in the visual editor. Blocks snap into place, and you can drop them on Hidden to hide them. There's a separate phone layout too. See [Layout](#layout).
- **Text size** – scale all the card's text from 80% to 160%.
- **Plan timeline** – a colour-coded ribbon of every plan slot (charge, hold, freeze, export, demand).
- **Upcoming actions** – the next forced charge/export windows merged into readable blocks with average rate and the SoC they end at.
- **Full plan table** – one tap to expand the slot-by-slot plan.
- **Six visual styles** – Modern, Glass, Neon, Minimal, Home Assistant native and Terminal, picked from a dropdown in the editor.
- **Visual editor** with every Predbat entity prefilled and a live entity check that shows which ones were found.
- No dependencies – no ApexCharts or other cards needed.
- Follows your Home Assistant theme (light and dark) and time format.
- Translations: English, Polski, Español.

## Requirements

- Home Assistant 2024.8 or newer.
- Predbat running and publishing its entities (default prefix `predbat`).
- The plan timeline, upcoming actions and table read the JSON plan in the `raw` attribute of `predbat.plan_html`. Use a recent Predbat release that publishes it.

Excluding Predbat entities from the recorder doesn't affect the card, which reads their current state. The only exception is **actual solar** on the Solar chart: it reads today's history of `predbat.pv_power`. If that isn't recorded, the card falls back to your inverters' PV power sensors, or the sensor you set in `pv_actual`.

## Installation

### HACS (recommended)

[![Open your Home Assistant instance and open Predbat Card in HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=mimikm&repository=Predbat-Card&category=plugin)

1. Open **HACS** in Home Assistant and search for **Predbat Card**, or use the button above.
2. Open it and press **Download**.
3. Reload the browser when asked (on the Companion app, close and reopen it).

HACS adds the dashboard resource for you, so there is nothing else to set up. Updates appear in HACS like any other.

<details>
<summary>Not showing up in HACS search?</summary>

Until Predbat Card is in the HACS default list for your install, add it as a custom repository:

1. Open HACS → three dots (top right) → **Custom repositories**.
2. Add `https://github.com/mimikm/Predbat-Card` with type **Dashboard**.
3. Search for **Predbat Card**, download it, and reload the browser when asked.

</details>

### Manual

1. Download `predbat-card.js` from the [latest release](https://github.com/mimikm/Predbat-Card/releases/latest), or from the [`dist` folder](https://github.com/mimikm/Predbat-Card/tree/main/dist).
2. Copy it to `/config/www/predbat-card.js`.
3. Settings → Dashboards → three dots → **Resources** → add `/local/predbat-card.js` as a **JavaScript module**.

## Adding the card

Edit a dashboard → **Add card** → search for **Predbat Card**. With a standard Predbat install no configuration is needed:

```yaml
type: custom:predbat-card
```

Everything can be set in the visual editor, grouped into sections you can open and close:

| Section | What's in it |
| --- | --- |
| **Look** | Style, light/dark, title, language, text size |
| **Sections** | Header, overview tiles, plan timeline, upcoming actions, plan table, how many actions to list |
| **Charts** | Which charts to show, tabs or stacked, hours ahead |
| **Car and hot water** | Show car / iBoost, car names, battery size, your car's own sensors |
| **Solar, carbon and inverters** | Show the solar forecast, carbon and inverter breakdown, inverter names, your inverter sensors |
| **Load data alarm** | Turn it on or off, days to check, how long counts as flat, load sensors |
| **Layout** | Drag sections, overview tiles and chart tabs into order or onto Hidden, with separate Desktop and Phone layouts. See [Layout](#layout). |
| **Advanced: Predbat entities** | Prefix, battery size, and every Predbat entity the card reads, grouped by topic |

The **Entity check** at the bottom shows how many entities were found. It opens by itself when something required is missing.

## Options

| Option | Default | Description |
| --- | --- | --- |
| `style` | `modern` | Visual style: `modern`, `glass`, `neon`, `minimal`, `native` or `terminal`. See [Styles](#styles). |
| `theme_mode` | `auto` | `auto` follows your Home Assistant theme, `light` or `dark` forces that look on this card whatever the theme. |
| `title` | `Predbat` | Card title. |
| `prefix` | `predbat` | Predbat entity prefix. Change it if you run more than one Predbat instance or changed `prefix` in `apps.yaml`. All default entities follow it. |
| `language` | `auto` | `auto` (follows Home Assistant), `en`, `pl` or `es`. |
| `show_header` | `true` | Status header. |
| `show_overview` | `true` | Overview tiles. |
| `show_chart` | `true` | Battery forecast chart. |
| `show_grid_chart` | `true` | Grid import & export chart. |
| `show_rates_chart` | `true` | Energy rates chart. |
| `show_car` | `auto` | Car tiles, car slots in the timeline and upcoming actions. `auto` shows them when Predbat has a car set up; `on` or `off` to force. |
| `show_iboost` | `auto` | Hot water (iBoost) tile, timeline lane and heating windows. `auto` shows them when Predbat's iBoost is enabled. |
| `show_car_chart` | `true` | Car chart tab (only when cars are shown). |
| `car_names` | – | Names for your cars, comma separated, e.g. `Tesla, Leaf`. |
| `car_battery_size` | – | Car battery size in kWh, comma separated per car. Only needed to turn a kWh charge level (or Predbat's manual SoC) into %. |
| `show_load_forecast` | `true` | House load forecast line on the grid chart. |
| `show_grid_pv` | `true` | Solar forecast line on the grid chart. |
| `show_pv_actual` | `true` | Today's actual solar generation on the Solar chart, next to the forecast. |
| `next_include_hold` | `false` | Count hold and freeze slots as the **Next charge** / **Next export**. By default those tiles only show real charge and export slots, where the battery is filled or emptied. |
| `show_solar` | `auto` | Solar tab and Solar today tile. `auto` shows them when a solar forecast is found; `on` or `off` to force. |
| `show_carbon` | `auto` | Carbon tab and Grid carbon tile. `auto` shows them when Predbat's `carbon_enable` is on. |
| `show_inverters` | `auto` | Per-inverter breakdown. `auto` shows it when two or more inverters are found. `on` shows it for one inverter too. |
| `inverter_names` | – | Names for your inverters, comma separated, e.g. `Garage, House`. |
| `inverter_power_invert` | `false` | Set to `true` if your battery power sensors are positive when charging (the same as `battery_power_invert` in `apps.yaml`). |
| `show_data_quality` | `true` | Zero-load alarm: header chip, notice and the Data chart tab. |
| `data_quality_days` | `7` | Days of history to check, 1–14. Set it to the largest number in your Predbat `days_previous` so the count matches Predbat's log. |
| `data_quality_minutes` | `30` | How long the load total must stay flat before it's flagged (Predbat's default is 30). |
| `chart_layout` | `tabs` | `tabs` shows one chart at a time with a switcher; `stacked` shows all enabled charts one under another. |
| `font_scale` | `1` | Text size, from `0.8` to `1.6`. Charts grow a little with it. |
| `show_timeline` | `true` | Plan timeline ribbon. |
| `show_actions` | `true` | Upcoming actions list. |
| `show_table` | `true` | “Show full plan” button and table. |
| `show_base` | `true` | Baseline SoC line on the chart. |
| `show_rates` | `true` | Import rate line on the chart. |
| `hours` | `24` | How many hours ahead the chart, timeline and actions cover (4–48). |
| `max_actions` | `4` | Maximum number of upcoming actions. |
| `battery_capacity` | – | Battery size in kWh. Only needed if the card cannot read it from Predbat. |
| `currency` / `currency_minor` | from Predbat | Override the currency symbols, e.g. `€` and `c`. |
| `section_order` | default order | Order of the sections: `header`, `alerts`, `overview`, `inverters`, `charts`, `timeline`, `actions`, `table`. Anything you leave out is added at the end. |
| `tile_order` | default order | Order of the overview tiles: `battery`, `now`, `cost`, `saving`, `charge`, `export`, `solar`, `carbon`, `car`, `iboost`. |
| `chart_order` | default order | Order of the chart tabs: `battery`, `grid`, `rates`, `solar`, `carbon`, `car`, `data`. |
| `hidden_sections` / `hidden_tiles` / `hidden_charts` | – | Lists of sections, tiles or chart tabs to hide. Warnings (`alerts`) can't be hidden. |
| `phone_layout` | – | A separate layout for phones, with the same six keys as above. Leave it out and phones use the desktop layout. |
| `phone_breakpoint` | `600` | The phone layout is used on screens narrower than this, in px. |
| `entities` | see below | Override any individual entity. |

### Entities

Defaults for the `predbat` prefix. Only add the ones you want to change.

| Key | Default entity | Used for |
| --- | --- | --- |
| `status` | `predbat.status` | Header status, last update, version, error banner |
| `plan` | `predbat.plan_html` | Timeline, upcoming actions, table, current rates |
| `mode` | `select.predbat_mode` | Mode shown in header |
| `soc_now` | `predbat.soc_kw_h0` | Current battery kWh |
| `soc_best` | `predbat.soc_kw_best` | Best-plan SoC forecast |
| `soc_base` | `predbat.soc_kw` | Baseline SoC forecast |
| `inverter_config` | `sensor.predbat_inverter_config` | Battery capacity |
| `hours_left` | `predbat.best_battery_hours_left` | Hours of battery left |
| `rates` | `predbat.rates` | Import rate line on chart |
| `rates_export` | `predbat.rates_export` | Fallback export rate |
| `cost_today` | `predbat.cost_today` | Cost today |
| `best_metric` | `predbat.best_metric` | Predicted cost with the Predbat plan |
| `base_metric` | `predbat.metric` | Predicted cost with the baseline plan (for saving today) |
| `savings` | `predbat.savings_yesterday_predbat` | Savings yesterday |
| `charge_start` / `charge_end` / `charge_limit` | `predbat.best_charge_*` | Next charge tile |
| `export_start` / `export_end` / `export_limit` | `predbat.best_export_*` | Next export tile |
| `export_energy` | `predbat.best_export_energy` | Grid chart – predicted export |
| `import_energy` | `predbat.best_import_energy` | Grid chart – predicted import |
| `load_today` | auto | Zero-load alarm – the `load_today` sensor(s) from your Predbat `apps.yaml`. Found automatically for the inverters Predbat has templates for (see below). Can be a list. |
| `car_slot` | `binary_sensor.predbat_car_charging_slot` | Car plan (planned slots, kWh, cost). Second car adds `_1`, third `_2`. |
| `car_soc` | `predbat.car_soc_best` | Car charge level forecast (same `_1`, `_2` for more cars) |
| `car_plan_time` | `select.predbat_car_charging_plan_time` | Ready-by time shown on the Car chart |
| `iboost_today` | `input_number.predbat_iboost_today` | iBoost energy today |
| `iboost_max` | `input_number.predbat_iboost_max_energy` | iBoost daily limit |
| `iboost_active` | `binary_sensor.predbat_iboost_active` | iBoost heating now |
| `iboost_enable` | `switch.predbat_iboost_enable` | Whether iBoost is on in Predbat |
| `car_start` | `predbat.car_charging_start` | Next car charge start (used when there's no slot list) |
| `car_power` | `predbat.car_charging_power` | Car charging power now (single car) |
| `car_cost_today` / `car_cost_yesterday` | `predbat.cost_today_car` / `predbat.cost_yesterday_car` | Car charging cost |
| `car_manual_soc` / `car_manual_soc_kwh` | `switch.predbat_car_charging_manual_soc` / `input_number.predbat_car_charging_manual_soc_kwh` | Predbat's manual car SoC |
| `car_rate` | `input_number.predbat_car_charging_rate` | Car charging rate |
| `car_plan_smart` / `car_max_price` / `car_from_battery` | `switch.predbat_car_charging_plan_smart` / `input_number.predbat_car_charging_plan_max_price` / `switch.predbat_car_charging_from_battery` | Car plan settings shown on the Car tab |
| `octopus_intelligent` | `switch.predbat_octopus_intelligent_charging` | Shows when the car follows Octopus Intelligent |
| `car_soc_sensor` | – | **Your car's own** charge level sensor (% or kWh). One per car, as a list. |
| `car_limit_sensor` | – | Your car's charge limit / target (% or kWh) |
| `car_plugged_sensor` | – | Plugged in (on/off, or states like `connected`, `plugged`) |
| `car_charging_sensor` | – | Charging now (on/off, or a power sensor in W/kW) |
| `pv_today` / `pv_tomorrow` / `pv_d2` / `pv_d3` | `sensor.predbat_pv_today` / `_pv_tomorrow` / `_pv_d2` / `_pv_d3` | Solar forecast per day. If Predbat's aren't there, the Solcast integration's `sensor.solcast_pv_forecast_forecast_today` (and tomorrow, day 3, day 4) are used. |
| `pv_now` | `sensor.predbat_pv_forecast_h0` | Solar forecast now |
| `pv_actual` | auto | Actual solar for the Solar chart: one or more PV **power** sensors (W/kW) or PV **today** energy sensors (kWh), added together. By default the card uses `predbat.pv_power`, then your inverters' PV power. |
| `carbon_now` | `predbat.carbon_now` | Grid carbon intensity now |
| `carbon_best` / `carbon_today` | `predbat.carbon_best` / `predbat.carbon_today` | Planned carbon and carbon so far today |
| `carbon_intensity` | `sensor.predbat_carbon_intensity` | Carbon forecast, used when the plan has no carbon column |
| `pv_power` / `battery_power` | `predbat.pv_power` / `predbat.battery_power` | Totals for all inverters, shown above the inverter breakdown |
| `inverter_soc` | auto | **Your inverters'** battery level in %, one per inverter, as a list. Found automatically for GivTCP, GivEnergy, SolarEdge, Fox ESS, Solis, Sunsynk, SolaX, Kostal and Growatt. |
| `inverter_soc_kwh` | auto | Battery level in kWh (or Wh), one per inverter |
| `inverter_capacity` | auto (GivTCP, GivEnergy, SolarEdge) | Battery size in kWh per inverter: a sensor, an `input_number`, or just a number in YAML. One value covers all inverters. With it, the card works out % from kWh or kWh from %, so both always show. |
| `inverter_battery_power` | auto | Battery power per inverter (W or kW, positive = discharging) |
| `inverter_pv_power` | auto | Solar power per inverter (W or kW) |
| `load_power` | auto | Zero-load alarm – the `load_power` sensor(s) from your `apps.yaml`. Found automatically like `load_today`. Can be a list. |

### Example

```yaml
type: custom:predbat-card
title: Battery plan
hours: 36
max_actions: 6
show_base: false
chart_layout: stacked
show_rates_chart: false
font_scale: 1.15
entities:
  savings: predbat.savings_yesterday_pvbat
```

### Full YAML with every option

If you'd rather not use the visual editor, paste this into a **Manual** card. These are the defaults, so the card looks exactly the same as `type: custom:predbat-card` on its own. Change what you need and delete the rest.

```yaml
type: custom:predbat-card

# Look
style: modern            # modern | glass | neon | minimal | native | terminal
theme_mode: auto         # auto | light | dark
font_scale: 1            # text size, 0.8 – 1.6
title: Predbat
language: auto           # auto | en | pl | es

# Sections
show_header: true
show_overview: true
show_timeline: true
show_actions: true
show_table: true

# Charts
show_chart: true         # battery forecast
show_grid_chart: true    # grid import & export
show_rates_chart: true   # energy rates
chart_layout: tabs       # tabs | stacked
show_base: true          # baseline line on the battery chart
show_rates: true         # import rate line on the battery chart
show_load_forecast: true # house load line on the grid chart
show_grid_pv: true       # solar line on the grid chart
show_solar: auto         # auto | on | off – Solar tab and tile
show_pv_actual: true     # today's actual solar on the Solar chart
next_include_hold: false # hold/freeze slots count as Next charge/export
show_carbon: auto        # auto | on | off – Carbon tab and tile

# Inverters
show_inverters: auto     # auto | on | off – shown for 2+ inverters
# inverter_names: Garage, House
inverter_power_invert: false

# Zero-load alarm
# Car and iBoost
show_car: auto           # auto | on | off
show_iboost: auto        # auto | on | off
show_car_chart: true
# car_names: Tesla, Leaf
# car_battery_size: 75

show_data_quality: true
data_quality_days: 7     # days of history to check, 1 – 14
data_quality_minutes: 30 # flag the load total when flat this long

# Layout (easier in the editor: Layout → drag and drop)
# section_order: [header, alerts, overview, inverters, charts, timeline, actions, table]
# tile_order: [battery, now, cost, saving, charge, export, solar, carbon, car, iboost]
# chart_order: [battery, grid, rates, solar, carbon, car, data]
# hidden_sections: []
# hidden_tiles: []
# hidden_charts: []
# phone_layout:            # optional, used on screens narrower than phone_breakpoint
#   section_order: [overview, header, charts]
#   hidden_sections: [table]
# phone_breakpoint: 600

# Time and limits
hours: 24                # hours ahead to show, 4 – 48
max_actions: 4           # upcoming actions to list, 1 – 12

# Optional – only if the card can't work these out
# battery_capacity: 19.0   # usable battery size in kWh
# currency: "£"
# currency_minor: "p"

# Predbat entity prefix (change if you renamed it in apps.yaml)
prefix: predbat

# Entities – defaults for the "predbat" prefix.
# You only need the ones you want to change; delete the rest.
entities:
  status: predbat.status
  plan: predbat.plan_html
  mode: select.predbat_mode
  soc_now: predbat.soc_kw_h0
  soc_best: predbat.soc_kw_best
  soc_base: predbat.soc_kw
  inverter_config: sensor.predbat_inverter_config
  hours_left: predbat.best_battery_hours_left
  rates: predbat.rates
  rates_export: predbat.rates_export
  cost_today: predbat.cost_today
  best_metric: predbat.best_metric
  base_metric: predbat.metric
  savings: predbat.savings_yesterday_predbat
  charge_start: predbat.best_charge_start
  charge_end: predbat.best_charge_end
  charge_limit: predbat.best_charge_limit
  export_start: predbat.best_export_start
  export_end: predbat.best_export_end
  export_limit: predbat.best_export_limit
  export_energy: predbat.best_export_energy
  import_energy: predbat.best_import_energy
  car_slot: binary_sensor.predbat_car_charging_slot
  car_soc: predbat.car_soc_best
  car_plan_time: select.predbat_car_charging_plan_time
  iboost_today: input_number.predbat_iboost_today
  iboost_max: input_number.predbat_iboost_max_energy
  iboost_active: binary_sensor.predbat_iboost_active
  iboost_enable: switch.predbat_iboost_enable
  car_start: predbat.car_charging_start
  car_power: predbat.car_charging_power
  car_cost_today: predbat.cost_today_car
  car_cost_yesterday: predbat.cost_yesterday_car
  car_manual_soc: switch.predbat_car_charging_manual_soc
  car_manual_soc_kwh: input_number.predbat_car_charging_manual_soc_kwh
  car_rate: input_number.predbat_car_charging_rate
  car_plan_smart: switch.predbat_car_charging_plan_smart
  car_max_price: input_number.predbat_car_charging_plan_max_price
  car_from_battery: switch.predbat_car_charging_from_battery
  octopus_intelligent: switch.predbat_octopus_intelligent_charging
  pv_today: sensor.predbat_pv_today
  pv_tomorrow: sensor.predbat_pv_tomorrow
  pv_d2: sensor.predbat_pv_d2
  pv_d3: sensor.predbat_pv_d3
  pv_now: sensor.predbat_pv_forecast_h0
  # pv_actual: [sensor.givtcp_XXXX_pv_power]   # actual solar; auto if left out
  carbon_now: predbat.carbon_now
  carbon_best: predbat.carbon_best
  carbon_today: predbat.carbon_today
  carbon_intensity: sensor.predbat_carbon_intensity
  pv_power: predbat.pv_power
  battery_power: predbat.battery_power
  # your inverters' own sensors (optional, one per inverter; leave out to auto-detect):
  # inverter_soc: [sensor.givtcp_XXXX_soc, sensor.givtcp2_YYYY_soc]
  # inverter_soc_kwh: [sensor.givtcp_XXXX_soc_kwh, sensor.givtcp2_YYYY_soc_kwh]
  # inverter_capacity: [9.5, 9.5]
  # inverter_battery_power: [sensor.givtcp_XXXX_battery_power, sensor.givtcp2_YYYY_battery_power]
  # inverter_pv_power: [sensor.givtcp_XXXX_pv_power, sensor.givtcp2_YYYY_pv_power]
  # your car's own sensors (optional, one per car):
  # car_soc_sensor: [sensor.my_car_battery]
  # car_limit_sensor: [number.my_car_charge_limit]
  # car_plugged_sensor: [binary_sensor.my_charger_plugged_in]
  # car_charging_sensor: [sensor.my_charger_power]
  # load_today / load_power: leave out to auto-detect GivTCP sensors,
  # or list the same sensors as load_today and load_power in apps.yaml
```

## Styles

Pick a style in the visual editor, or set `style:` in YAML.

| Style | Look |
| --- | --- |
| `modern` | Default. Soft tiles, purple accent, follows your theme. |
| `glass` | Frosted translucent tiles over a soft colour gradient, rounded pill timeline. |
| `neon` | Always dark. Glowing cyan, magenta and lime lines on a grid background, monospace numbers. |
| `minimal` | No tile backgrounds, hairline dividers, light typography, thin timeline. |
| `native` | Matches built-in Home Assistant cards and uses your theme's primary, success and warning colours. |
| `terminal` | Always dark. Green phosphor retro LCD with scanlines, monospace text and dotted bars. |

```yaml
type: custom:predbat-card
style: glass
```

<details open>
<summary><b>All six styles – dark theme</b></summary>

![All six styles in a dark theme](https://github.com/mimikm/Predbat-Card/raw/main/images/styles-dark.png)

</details>

<details>
<summary><b>All six styles – light theme</b></summary>

![All six styles in a light theme](https://github.com/mimikm/Predbat-Card/raw/main/images/styles-light.png)

</details>

Top row: `modern`, `glass`, `neon`. Bottom row: `minimal`, `native`, `terminal`.

Every style follows your theme by default, except `neon` and `terminal`, which are dark unless you choose otherwise. Set `theme_mode: light` or `theme_mode: dark` (or **Light / dark** in the editor) to force either look on any style. `neon` and `terminal` each have their own light version: a pale grid, and a classic grey-green LCD.

## Layout

![Layout editor](https://github.com/mimikm/Predbat-Card/raw/main/images/layout-editor.png)

Open **Layout** at the bottom of the visual editor to arrange the card by dragging:

- **Sections** (header, warnings, overview tiles, inverters, charts, plan timeline, upcoming actions, full plan table): drag up or down.
- **Overview tiles**: drag inside the tile grid.
- **Chart tabs**: drag inside the Charts block. The first one is the tab that opens first.

While you drag, the block follows your finger or mouse, the others slide out of the way, and it snaps into the gap when you let go. Drop anything on **Hidden** to hide it, and drag it back to show it again. It comes back where it was. Warnings can be moved but not hidden, so you never miss an error. A block marked **off** is turned off in the settings above it (for example *Overview tiles* under Sections). Layout only changes where things go.

**Phone layout.** Switch to **📱 Phone** and press **Create phone layout**. It starts as a copy of your desktop layout. Arrange it for small screens, for example the tiles first and the table hidden. It's used on screens narrower than 600 px; you can change that number. Your desktop layout isn't affected. Use **Copy desktop layout** to start again, or **Remove phone layout** to go back to one layout for everything.

**Reset layout** puts the desktop layout back to the default.

## Zero-load alarm

![Zero-load alarm](https://github.com/mimikm/Predbat-Card/raw/main/images/data-quality.png)

Predbat builds its load forecast from the history of your `load_today` sensor. Each time it runs it looks for stretches of 30 minutes or more where that total didn't go up, and logs:

> Warn: Found N periods of zero load with power data, filling using power integration

This usually happens when the inverter integration briefly drops out and comes back with the same reading, or after a Home Assistant restart. It's most common at night, when the total only goes up slowly. That warning only appears in Predbat's log, so the card runs the same check itself on Home Assistant history of the same sensors.

- **Header chip** – shows how many periods were found. Amber means Predbat filled them from load power, or the card can't tell (no load power sensor); red means some had no load power data, so Predbat used averages. Tap it to open the details.
- **Notice** – the count for the whole period and for the last 24 hours, plus the most recent periods with time, length and the energy Predbat filled in.
- **Data tab** – one row per day, one square per half hour: grey is fine, amber was filled, red wasn't, and muted amber means *power unknown* (no load power sensor to check against). Repeating patterns (the same time every night, say) usually point to something restarting.

**Which sensors are checked.** The card needs the *same* sensors Predbat uses: `load_today` and `load_power` from your `apps.yaml`. The card can't read `apps.yaml`, so:

1. **If you set them** (editor → *Load data alarm* → *Load sensors*, or YAML), those are used. Both accept a list, just like `apps.yaml`, and a list is added up the same way Predbat does.
2. **If you don't**, the card looks for the sensor names used in Predbat's own `apps.yaml` templates: GivTCP, GivEnergy (cloud integration), Fox ESS, Solis, SolaX, Sofar, Sunsynk, LuxPower, Sigenergy, Kostal, Fronius, SolarEdge, Tesla Powerwall, Growatt (Solar Assistant), Hanchu, Huawei and Victron. The notice and the Data tab say which sensors were used and which inverter type they matched.

```yaml
entities:
  load_today:
    - sensor.givtcp_XXXX_load_energy_today_kwh
  load_power:
    - sensor.givtcp_XXXX_load_power
```

**When something's missing**

| Situation | What the card shows |
| --- | --- |
| No sensors set and none found | Data tab: "No load sensors found", with what to set. No chip. |
| A sensor you set doesn't exist | "Not found: …" in the notice (or on its own if there's nothing else to report) and in the Data tab |
| A sensor exists but has no history | "No history: …" |
| `load_today` works but there's no `load_power` | Periods are marked *power unknown* rather than "not filled", with a note to set `load_power` |
| `load_today` is in Wh or MWh | Converted to kWh automatically |

To match the number in Predbat's log, set `data_quality_days` to the largest value in your Predbat `days_previous` (7 by default), since that's how far back Predbat looks. The count can still be off by one or two at the edges because the two checks run at different times. If Predbat gets its load data from the GivEnergy cloud (`ge_cloud_data`), it doesn't use these sensors, so the card's result won't match.

History is read once every 30 minutes. Turn it all off with `show_data_quality: false`.

## Car and iBoost

![Car and iBoost](https://github.com/mimikm/Predbat-Card/raw/main/images/car-iboost.png)

If Predbat plans your car charging or a solar diverter (iBoost), the card shows it automatically. Nothing appears when they're not set up in Predbat. Like everything else on the card, this is view only: it never changes Predbat's car or iBoost settings.

**Car** (one tile per car, up to four). The card uses whatever is available, so it works whether Predbat plans your charging, follows Octopus Intelligent, or you only want to see the car:
- **Charge level now**, from the first of these it finds: your car's own sensor (`car_soc_sensor`), Predbat's manual SoC when that's switched on, then Predbat's forecast (`predbat.car_soc_best`).
- **Target**: your car's charge limit (`car_limit_sensor`) if you've added it, otherwise where Predbat's plan ends.
- **Status**: charging now (from your charger sensor, Predbat's car charging power, or a Predbat slot being active), plugged in or not, the next slot with energy and cost, or the next start time.
- **Warnings**: if Predbat is planning from a different charge level than the car reports (10% or more apart), or Predbat thinks the battery is empty, the tile and Car tab say so. That usually means `car_charging_soc` in your Predbat `apps.yaml` isn't set or isn't working, so Predbat may plan more charging than you need.
- Every planned slot in **Upcoming actions** with its energy, average rate and cost, from the `planned` list on `binary_sensor.predbat_car_charging_slot`. If that list is empty, the card falls back to the car column in Predbat's plan.
- A lane under the plan timeline for each car, and a Car column in the full plan table.
- **Car tab**: Predbat's charge level forecast, the car's own reading, your target, the charging slots, ready-by time, and Predbat's car settings (smart plan or Octopus Intelligent, max price, whether it can use the home battery, cost today and yesterday).

Your car's own sensors are optional and set per car in the editor under **Your car's own sensors**, or in YAML as lists:

```yaml
entities:
  car_soc_sensor: [sensor.tesla_battery, sensor.leaf_battery]
  car_limit_sensor: [number.tesla_charge_limit]
  car_plugged_sensor: [binary_sensor.tesla_charger]
  car_charging_sensor: [sensor.zappi_power]
car_battery_size: 75, 40   # only needed for kWh sensors or Predbat's manual SoC
```

If Predbat has no car set up but you add your own sensors, the car tile still shows them.

**iBoost (hot water):**
- Energy used today against the daily limit (`input_number.predbat_iboost_today` and `input_number.predbat_iboost_max_energy`), with a small progress bar.
- "Heating now" (or "from solar") while `binary_sensor.predbat_iboost_active` is on, otherwise the next planned heating window.
- Heating windows in **Upcoming actions**, a lane under the plan timeline, and an iBoost column in the full plan table. These come from the iBoost amounts in Predbat's plan.

Name your cars with `car_names: Tesla, Leaf`. Predbat numbers its car entities from the second car onwards (`_1`, `_2`), and the card follows the same pattern.

## Solar, carbon and inverters

![Inverters, solar forecast, grid chart with house load, carbon intensity](https://github.com/mimikm/Predbat-Card/raw/main/images/solar-carbon.png)

**Solar forecast.** Predbat publishes its solar forecast (from Solcast, Forecast.Solar or its own solar API) as `sensor.predbat_pv_today`, `_pv_tomorrow`, `_pv_d2` and `_pv_d3`, with a half-hour forecast and the 10% and 90% versions. The Solar tab shows:
- the forecast in kW across the whole day, with the **10–90% range** as a shaded band (solar should land inside it about 8 days out of 10),
- the day's total and range, and on today how much is still to come,
- when Predbat's PV calibration is on, the calibrated forecast Predbat plans with, and the raw forecast as a dashed line,
- buttons to switch between today, tomorrow and the next two days.

**Actual solar.** On today's chart, green bars show what your panels actually made in each half hour, next to the forecast line, and the legend compares the total so far with what the forecast expected by now (for example *Actual 15.6 kWh vs 20.7 forecast so far (75%)*). The card reads today's history of `predbat.pv_power`. If that isn't recorded, it adds up your inverters' PV power sensors instead. You can set your own sensors in the editor (*Solar sensors*) or with `pv_actual`, either power sensors or a "PV today" energy total. History is read every 5 minutes. Turn it off with `show_pv_actual: false`.

The **Solar today** tile shows today's total, what's left and tomorrow's total. If Predbat's sensors aren't there, the card uses the Solcast integration's own forecast sensors. If neither is there, the tab falls back to the solar in Predbat's plan (no range, and only as far ahead as the plan goes).

**House load on the grid chart.** The grid chart now also draws Predbat's forecast for the house load (below the line, with the import bars) and solar (above the line, with the export bars), from the plan. Where the import bar is shorter than the load line, the battery or solar is covering the rest. The tooltip also shows the higher load Predbat allows for (its 10% case). Turn the lines off with `show_load_forecast: false` and `show_grid_pv: false`.

**Carbon intensity.** If you've turned on `carbon_enable` in Predbat's `apps.yaml`, the card adds a **Grid carbon** tile (g CO₂ per kWh now, its level and today's total so far) and a **Carbon** tab: the carbon intensity for each slot, coloured from very low (green) to very high (red) using Predbat's own bands, the cleanest hour ahead, the carbon so far today and the plan's total. Charge and export slots are marked underneath. Nothing appears when the carbon option is off.

**Inverters.** Predbat only publishes totals for all your inverters, so for a per-inverter view the card reads each inverter's own sensors, the same ones you list in `apps.yaml`. For each inverter it shows battery level in % and kWh, out of the battery size when it's known, charging or discharging power and solar power, with Predbat's totals at the top. Tap one to open its sensor.

1. **If you set them** (editor → *Solar, carbon and inverters* → *Inverter sensors*, or YAML), those are used, one entry per inverter in the same order as `apps.yaml`.
2. **If you don't**, the card looks for the names from Predbat's templates: GivTCP (`givtcp_`, `givtcp2_` …), GivEnergy cloud, SolarEdge (`b1`, `b2` …), Fox ESS, Solis, Sunsynk, SolaX, Kostal and Growatt. For the brands that don't put a serial number in the name, a second inverter is found by Home Assistant's `_2` ending.

When it looks for sensors by name, the card leaves out:
- **sensors that are unavailable**, such as leftovers from an integration you removed or added again,
- **a GivEnergy Gateway** (serial starting `gw`) when there are AIOs too, because the gateway only reports totals of the same batteries. With one AIO and a gateway, you get one inverter,
- **anything over Predbat's own count** (`num_inverters` in `sensor.predbat_inverter_config`). If it finds more, it keeps the ones with the most live data. The exception is several AIOs run through a gateway, which Predbat counts as one; then each AIO is shown.

The editor's **Entity check** lists the inverters it found and anything it left out, with the reason.

The breakdown appears with two or more inverters (`show_inverters: on` shows it for one). If Predbat says it runs more than one inverter but the card can't find their sensors, a short note tells you where to add them. Battery power is read the Predbat way (positive = discharging); if yours is the other way round, set `inverter_power_invert: true`.

You only need one of `inverter_soc` (%) and `inverter_soc_kwh` (kWh). Add `inverter_capacity` and the card fills in the other, so each inverter shows both.

```yaml
inverter_names: Garage, House
entities:
  inverter_soc: [sensor.givtcp_XXXX_soc, sensor.givtcp2_YYYY_soc]
  inverter_soc_kwh: [sensor.givtcp_XXXX_soc_kwh, sensor.givtcp2_YYYY_soc_kwh]
  inverter_capacity: [9.5, 9.5]        # or sensors / input_numbers
  inverter_battery_power: [sensor.givtcp_XXXX_battery_power, sensor.givtcp2_YYYY_battery_power]
  inverter_pv_power: [sensor.givtcp_XXXX_pv_power, sensor.givtcp2_YYYY_pv_power]
```

## Saving today

The **Saving today** tile is a prediction. Predbat publishes two cost forecasts from midnight onwards: one for its own plan (`predbat.best_metric`) and one for the baseline, where the battery is left to run with no changes (`predbat.metric`). The card reads both at midnight tonight; the difference is what Predbat expects to save you today. The tile also shows yesterday's actual saving from `predbat.savings_yesterday_predbat`.

Yesterday's figure uses a different baseline (one cheap charge per night), so the two numbers aren't directly comparable. If the forecast isn't available, the tile shows yesterday's saving instead, as before.

## Plan states

| Colour | State | Predbat plan state |
| --- | --- | --- |
| Green | Charge | `Chrg` |
| Blue | Hold | `HoldChrg` |
| Mint | Freeze charge | `FrzChrg` |
| Orange | Export | `Exp` |
| Yellow | Freeze export | `FrzExp` |
| Light orange | Hold export | `HoldExp` |
| Grey | Demand | `Demand` |
| Green/orange stripes | Charge + export (mixed) | A slot Predbat marks as split between charge and export without saying when it changes |

**Split slots.** Sometimes Predbat splits a slot, for example *demand until 17:15, then export*. When it gives the change time, the timeline, chart bands, upcoming actions and tooltips split the slot at that time. When a slot mixes charge and export without a change time, it's drawn with stripes rather than split at a made-up point.

## Troubleshooting

- **“No plan data yet”** – check that `predbat.plan_html` exists and has a `raw` attribute in Developer tools → States. Update Predbat if it doesn’t.
- **Battery % shows “–”** – set `battery_capacity` to your usable capacity in kWh.
- **Wrong entity names** – open the visual editor; the entity check lists every entity it could not find. Set `prefix` or override the entity.
- **Card not found after installing** – clear the browser cache or reload the app.
- **“Configuration error” or “Custom element doesn't exist” in the Companion app** – the app is still using an old copy of the card. In the app go to **Settings → Companion app** and use **Reset frontend cache** (on iOS it's under **Debugging**), then close and reopen the app.
- **The card shows a red “Predbat Card v… :” message** – something unexpected happened while drawing the card. Open **Details for a bug report** under the message and paste it into a [GitHub issue](https://github.com/mimikm/Predbat-Card/issues).
- **Predbat logs zero load but the card shows nothing** – open the Data tab and check which sensors it says it checked. They need to be the `load_today` and `load_power` sensors from your `apps.yaml` (see [Zero-load alarm](#zero-load-alarm)).
- **Every zero-load period says “power unknown”** – the card has no `load_power` sensor. Set it to the same sensor as in your `apps.yaml`.
- **Data tab says “No load sensors found” or “No history for …”** – set `load_today` and `load_power` to the sensors from your `apps.yaml`, and make sure they're recorded.
- **No Solar tab** – check that `sensor.predbat_pv_today` exists. Predbat only creates it when it fetches the solar forecast itself; if you use the Solcast integration, the card reads its sensors instead. Set `pv_today` / `pv_tomorrow` if yours are named differently.
- **The card shows two inverters but you have one** – open the editor's Entity check to see which sensors it picked. Update to 0.4.5 or later, which skips unavailable leftovers, GivEnergy Gateway totals and anything over Predbat's own inverter count. You can always list your sensors yourself, or set `show_inverters: off`.
- **Inverter breakdown missing or wrong** – set `inverter_soc`, `inverter_battery_power` and `inverter_pv_power` to the sensors from your `apps.yaml`. If charging shows as discharging, set `inverter_power_invert: true`.
- **“N Predbat entities not found”** – tap the notice to see which ones. Check the names in the editor's entity check, or set `prefix` if you changed it in Predbat.

## Roadmap

Predbat Card is **view-only**: it shows what Predbat is doing and planning, and never changes Predbat's settings or plan. That won't change.

This is the plan for Predbat Card up to v1.0. It will change as people try the card and ask for things, so treat versions and order as a guide, not a promise.

Want something added or moved up? [Open an issue](https://github.com/mimikm/Predbat-Card/issues) or 👍 an existing one.

### ✅ Done

| Version | Highlights |
| --- | --- |
| 0.1.0 | First release: status header, overview tiles, battery forecast chart, plan timeline, upcoming actions, plan table, visual editor with prefilled entities, EN/PL/ES |
| 0.2.0 | Six styles: Modern, Glass, Neon, Minimal, Native, Terminal |
| 0.2.1 | Force light or dark mode, light versions of Neon and Terminal, predicted saving today, forecast cost today |
| 0.2.2 | Grid import & export chart, energy rates chart, charts as tabs or stacked, text size slider |
| 0.3.0 | Mobile and reliability: errors shown inside the card instead of breaking it, tolerant YAML, missing-entity notice, phone and tablet layout, tap-to-inspect charts on touch screens, only changed parts redraw, fully view-only header, fallbacks for older iOS and Android |
| 0.3.1 | Zero-load alarm: header chip, notice with recent periods and a 7-day data-quality view |
| 0.3.2 | Zero-load alarm uses the same check and the same sensors as Predbat, auto-detects GivTCP sensors, counts the whole period |
| 0.4.0 | Car charging (charge level, planned slots, cost, Car chart) and iBoost hot water (today vs limit, heating windows), shown automatically when set up in Predbat |
| 0.4.1 | Car support uses every Predbat car entity plus your car's own sensors, with fallbacks; warns when Predbat's car charge level is wrong |
| 0.4.2 | Load data alarm finds sensors for many inverters, reports missing sensors, handles no load power and Wh units; visual editor reorganised into sections |
| 0.4.3 | Solar forecast tab (4 days, 10–90% range, calibrated vs raw) and tile, house load and solar on the grid chart, carbon intensity tab and tile, per-inverter breakdown with auto-detected sensors |
| 0.4.4 | Drag-and-drop layout in the visual editor: reorder and hide sections, overview tiles and chart tabs, with a separate phone layout |
| 0.4.5 | Inverter breakdown no longer counts one inverter twice: skips unavailable leftover sensors and GivEnergy Gateway totals, never shows more than Predbat's own inverter count; the editor shows what was found and what was left out. New battery level (kWh) and battery size slots per inverter, so each shows % and kWh |
| 0.4.6 | Plan table escapes the currency symbol like the rest of the card (HACS review); README install leads with HACS search |
| 0.4.7 | Actual solar today on the Solar chart against the forecast ([#3](https://github.com/mimikm/Predbat-Card/issues/3)); Next charge / Next export only show real charge and export slots, with an option to include hold and freeze ([#4](https://github.com/mimikm/Predbat-Card/issues/4)) |
| 0.4.8 | Plan slots keep their full length, and split slots are shown at Predbat's change time, with mixed charge/export slots striped. Contributed by [@graememorgan](https://github.com/graememorgan) ([#5](https://github.com/mimikm/Predbat-Card/pull/5)) |

### 🔜 0.4.x – More of Predbat's data

- [x] Car charging schedule: Predbat's planned car charging slots, times and cost
- [x] Car battery SoC: current level and predicted level at the end of the plan
- [x] iBoost / solar diverter slots
- [x] Solar forecast chart (today, tomorrow, 10% and 90% range)
- [x] House load forecast on the grid chart
- [x] Carbon intensity (optional)
- [x] Per-inverter breakdown for multi-inverter setups

### 0.5 – Savings and history

- [ ] Savings history chart (daily and running total)
- [ ] Cost today vs yesterday
- [ ] Week and month summaries

### 0.6 – Customisation

- [x] Choose which overview tiles to show, and in what order (done early, in 0.4.4)
- [x] Reorder sections (done early, in 0.4.4)
- [x] Reorder and hide chart tabs, and a separate phone layout (done early, in 0.4.4)
- [ ] Custom accent colour
- [ ] Compact mode: a small card with battery, now, next action and saving
- [ ] More translations (German, French, Dutch – volunteers welcome)

### 🎯 1.0 – Stable release

- [ ] Configuration options locked in (no breaking changes after 1.0)
- [ ] Automated tests for plan parsing and the editor
- [ ] Full documentation with screenshots for every style
- [ ] Link in the Predbat documentation
- [x] Submitted to the HACS default store

### 💡 Ideas we're considering

Not planned yet. Comment on an issue if you'd use one of these.

- Weather alongside the plan
- Notifications or a banner when Predbat's plan changes a lot
- A separate small "badge" card for the dashboard header
- Tariff comparison view

## Contributing

Issues and pull requests are welcome, including new translations. Thanks to everyone who has contributed so far, including [@graememorgan](https://github.com/graememorgan) (plan slot lengths and split slots). Translations live in the `TRANSLATIONS` object at the top of `dist/predbat-card.js`; copy the `en` block, translate it and open a PR.

## Credits

<table>
<tr>
<td>🦇</td>
<td>

**This card exists because of [Predbat](https://github.com/springfall2008/batpred), created and maintained by Trefor Southwell ([@springfall2008](https://github.com/springfall2008)).**

Predbat does all the clever work: it predicts your house load, solar and tariff rates, then plans when to charge, hold and export your battery to save money. This card only shows what Predbat has already worked out. Thank you, Trefor, and everyone who contributes to Predbat and its documentation.

- 📖 [Predbat documentation](https://springfall2008.github.io/batpred/)
- 💬 [Predbat on GitHub](https://github.com/springfall2008/batpred)
- 🍺 Support Predbat: [GitHub Sponsors](https://github.com/sponsors/springfall2008) · [PayPal](https://paypal.me/predbat)

</td>
</tr>
</table>

Predbat Card is an independent community project. It is not part of Predbat and is not endorsed by its author. Please report card problems here, not on the Predbat repository.

Card made by [@mimikm](https://github.com/mimikm). If you find it useful you can [buy me a coffee](https://buymeacoffee.com/mimikm) ☕

## License

[MIT](LICENSE)
