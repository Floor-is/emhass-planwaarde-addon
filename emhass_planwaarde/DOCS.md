# EMHASS planwaarde

Deze add-on draait [Floor-is/emhass](https://github.com/Floor-is/emhass), tak `planwaarde`: EMHASS met drie
extra runtime-parameters. Zonder die parameters gedraagt hij zich als de upstream-versie in het versienummer
(`v0.18.4-planwaarde.1` = EMHASS v0.18.4).

| parameter | eenheid | wat |
|---|---|---|
| `battery_terminal_value` | EUR/kWh | geen vaste eindstand; energie aan het eind van de horizon is zoveel waard (0 = vrij tot de vloer) |
| `deferrable_load_energy_max` | Wh per load | eis wordt `eis <= E <= max`; de eis is de vloer |
| `deferrable_load_value` | EUR/kWh per load | elke kWh in de load is zoveel waard |

## Naast de officiële add-on

- ⚠️ Zet `continual_publish: false` in de `config.json` van deze add-on als hij naast de officiële draait.
  Beide publiceren anders onder dezelfde sensornamen (`sensor.p_batt_forecast`, …) en overschrijven elkaar.
  Roep om dezelfde reden `publish-data` niet aan op deze add-on zolang de officiële de sturing levert.
- Een onhaalbare eis maakt het plan niet infeasible: kolom `deferrable<k>_tekort_wh` toont het tekort.

- Poort **5001** op de host (de officiële add-on houdt 5000). Via ingress werkt de web-UI ook.
- Config en data staan in `addon_configs/<slug>/` (`/config` in de container). `/share` is niet gemapt:
  de officiële add-on bewaart zijn `config.json`, `params.pkl` en resultaten in `/share/emhass`, en deze
  add-on kan daar niet bij.
- Zet vóór de eerste start een `config.json` in `addon_configs/<slug>/`. Zonder dat bestand start EMHASS
  met zijn fabriekswaarden.

## Updates

De versie wijst naar een vaste fork-tag. Een nieuwe upstream-release komt niet vanzelf binnen: de fork
herbaseert dagelijks (workflow `PLANWAARDE upstream-wacht`) en wordt rood als de patch niet meer past of niet
meer werkt. Een nieuwe add-on-versie vraagt een nieuwe fork-tag en een versiewijziging hier.
