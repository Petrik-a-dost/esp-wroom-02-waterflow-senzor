<img src="img/esp-wroom-02.jpg" align="right" width="320" alt="esp-wroom-02"/>

# Waterflow Senzor Esp-Wroom-02 a impulzuní senzor Sensus HRI
![ESPHome Badge](https://img.shields.io/badge/ESPHome-000?logo=esphome&logoColor=fff&style=for-the-badge)

> I jako začátečním v ESPHome a vůbec v HomeAssistant jsem si dovolil udělat návod na meření spotřeby vody, aby když to bude někdo řešit, kdo bude nazačátku v HA, aby se nemusel trápit tako jako já :D Takže mě klidně opravte, když bych tu měl chybu a třeba to i někomu pomůže.

### Motivace

- Jelikož se stane, že občas vypadne elektrika, tak jsem si říkal, že už bych použil neco s baterry shieldem a vyšlo z toho ESP-Wroom-02 s baterkou 18650

## Hardware
 
- Deska Sspressif WeMos ESP-wroom-02 s baterií 18650 [WeMos ESP-WROOM-02 vývojová deska](https://www.laskakit.cz/wemos-esp-wroom-02-vyvojova-deska/)
- impulzní senzor na vodoměr Sensus HRI varianta A4 [Sensus snímač HRI](https://www.kapka-vodomery.cz/download/vodomery/prislusenstvi/sensus-snimac-hri.pdf)

## Použití

- vycházel jsem z už existujícího projektu, kde je použitá deska NodeMCU: [Build a cheap water usage sensor using ESPhome and a proximity sensor](https://www.pieterbrinkman.com/2022/02/02/build-a-cheap-water-usage-sensor-using-esphome-home-assistant-and-a-proximity-sensor/?fbclid=IwAR31Jy8ggQwYve9YchUbq6ylLgxr2Dd_sI1BzMqI2mSxeaGAOkKCJtEPZPA) - Zde jsem se inspiroval a požil kod a trochu ho poupravil pro potřeby ESP-Wroom-02.

# Sensus HRI

 - v dokumentaci se dozvíte, že pro každé provedení (A1/A2/A3/ a v ném případe A4) mužě být zapojkení trochu jiné.

<img src="img/hri_A4.png" align="left" width="320" alt="esp-wroom-02"/>
<img src="img/hri_technicke_udaje.png" align="right" width="320" alt="esp-wroom-02"/>






