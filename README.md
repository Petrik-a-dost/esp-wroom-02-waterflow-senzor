<img src="img/esp-wroom-02.jpg" align="right" width="320" alt="esp-wroom-02"/>

# Waterflow Senzor Esp-Wroom-02 a impulzuní senzor Sensus HRI
![ESPHome Badge](https://img.shields.io/badge/ESPHome-000?logo=esphome&logoColor=fff&style=for-the-badge)

> I jako začátečním v ESPHome a vůbec v HomeAssistant jsem si dovolil udělat návod na meření spotřeby vody, aby když to bude někdo řešit, kdo bude nazačátku v HA, aby se nemusel trápit tako jako já :D Takže mě klidně opravte, když bych tu měl chybu a třeba to i někomu pomůže.

### Motivace této desky

- Jelikož se stane, že občas vypadne elektrika, tak jsem si říkal, že už bych použil něco s battery shieldem a vyšlo z toho ESP-Wroom-02 s baterkou 18650

## Hardware
 
- Deska Espressif WeMos ESP-wroom-02 s baterií 18650 [WeMos ESP-WROOM-02 vývojová deska](https://www.laskakit.cz/wemos-esp-wroom-02-vyvojova-deska/)
- impulzní senzor na vodoměr Sensus HRI varianta A4 [Sensus snímač HRI](https://www.kapka-vodomery.cz/download/vodomery/prislusenstvi/sensus-snimac-hri.pdf)

## Použití

- vycházel jsem z již existujícího projektu, kde je použitá deska NodeMCU: [Build a cheap water usage sensor using ESPhome and a proximity sensor](https://www.pieterbrinkman.com/2022/02/02/build-a-cheap-water-usage-sensor-using-esphome-home-assistant-and-a-proximity-sensor/?fbclid=IwAR31Jy8ggQwYve9YchUbq6ylLgxr2Dd_sI1BzMqI2mSxeaGAOkKCJtEPZPA) - Zde jsem se inspiroval a požil kod a trochu ho poupravil pro potřeby ESP-Wroom-02.

# Sensus HRI
- v dokumentaci se dozvíte, že pro každé provedení (A1/A2/A3/ a v ném případe A4) mužě být zapojení trochu jiné a vodiče mají jiný význam, proto si to zkontrolujte.

<img src="img/hri_A4.png" align="left" width="320" alt="hri_A4"/>
<img src="img/hri_technicke_udaje.png" align="right" width="320" alt="hri_technicke_udaje"/>

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

___

# WeMos ESP-wroom-02
- pinout desky může být náročné zjistit, protože se to může splést za vystupy na desce POZOR výstup D2 není to stejné jako GPIO2 potom v kódu
- je tedy potřeba zjistit na jaké výstupty (D1, D2, D3,..) na desce vedou na GPIO, v mém případě podle [Dokumentace](https://www.studiopieters.nl/esp8266-esp-wroom-02-with-18650-battery/?fbclid=IwAR3pYxmAYj-FDfEqrm_vUxPI422bKUUkJtDhnZe3pk4s2nu2D7qoiI4uFco)
- já jsem použil pin D2, což je GPIO4

# Zapojení

<img src="img/zapojeni.png" align="center" width="350" alt="zapojeni"/>
<br/>

- na pin D2 jsem připojil bílý vodič, což by měl být + (pulzy)
- na GND jsem připojil hnědý vodič (zem)
- Zjistil jsem, že HRI má v sobě baterii takže se HRI napájet nemusí
- žlutý vodič jsem nepřipojoval, protože v mé variantě A4 je žlutý vodič pro indikaci chyby

# Kód v ESPHome

- jak jsem psal v úvodu tak jsem upravil kód z projektu [Build a cheap water usage sensor using ESPhome and a proximity sensor](https://www.pieterbrinkman.com/2022/02/02/build-a-cheap-water-usage-sensor-using-esphome-home-assistant-and-a-proximity-sensor/?fbclid=IwAR31Jy8ggQwYve9YchUbq6ylLgxr2Dd_sI1BzMqI2mSxeaGAOkKCJtEPZPA), ale protože nepoužívám NodeMCU a mám jiný impulzní snímač tak piny mám definované podle [ESPHome Pulse counter](https://esphome.io/components/sensor/pulse_counter.html#wiring)
- zde jsem použil reed switch mezi GPIO a GND tzn:

```
  pin:
    number: GPIO4
    inverted: true
    mode:
      input: true
      pullup: true
```

# Výsledný kód

```
esphome:
  name: waterflow-senzor

esp8266:
  board: esp_wroom_02

# Enable logging
logger:

# Enable Home Assistant API
api:
  encryption:
    key: "XXXXXXXXXXXXXXXXXXXX"

ota:
  password: "XXXXXXXXXXXXXXXXXXX"

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Waterflow-Senzor"
    password: "XXXXXXXXXXXX"

captive_portal:

sensor:
- platform: pulse_counter
  pin:
    number: GPIO4
    inverted: true
    mode:
      input: true
      pullup: true
  update_interval : 6s
  name: "water pulse"
  id: water_pulse

- platform: pulse_meter
  pin:
    number: GPIO4
    inverted: true
    mode:
      input: true
      pullup: true
  name: "Water Pulse Meter"
  unit_of_measurement: "liter/min"
  icon: "mdi:water"
  total:
    name: "Water Total"
    unit_of_measurement: "liter"

- platform: pulse_meter
  pin:
    number: GPIO4
    inverted: true
    mode:
      input: true
      pullup: true
  name: "Water Pulse Meter"
  unit_of_measurement: "liter/min"
  icon: "mdi:water"
  total:
    name: "Water Meter Total"
    unit_of_measurement: "m³"
    id: water_meter_total
    accuracy_decimals: 3
    device_class: water
    state_class: total_increasing
    filters:
      - multiply: 0.001

- platform: template
  name: "Water Usage Liter"
  id: water_flow_rate
  accuracy_decimals: 1
  unit_of_measurement: "l/min"
  icon: "mdi:water"
  lambda: return (id(water_pulse).state * 10);
  update_interval: 6s

```

# Závěrem

- už stačí jen vytisknout krabičku
- když by někoho napadlo, jak to ještě vylepšit budu rád za jakýkoliv tip

