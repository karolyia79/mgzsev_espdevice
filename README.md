# 🚗 MG ZS EV (2019-2021) CAN Bus Reader (ESPHome)

Ez a projekt egy nyílt forráskódú megoldás az **MG ZS EV (2019-2021)** elektromos autó belső adatainak kiolvasására. Az eszköz az autó OBD2 portjára csatlakozik, és saját Wi-Fi hálózaton (Access Point) vagy Bluetooth-on keresztül teszi elérhetővé az adatokat.

## ✨ Főbb funkciók
- 🔋 **Akku adatok:** Valós SOC (%), SOH (egészségi állapot), feszültség, akku- és hűtőfolyadék hőmérséklet.
- ⚡ **Töltés:** BMS státusz, HV akku hőmérséklet, hátralévő idő.
- 🛣️ **Menetadatok:** Odometer (km), keréknyomás (TPMS), külső hőmérséklet.
- 🔒 **Állapot:** Autó zárva/nyitva, 12V-os akku állapota (V/A).<br>
        <br>Ezeken kívül több PID-kből kiolvasható adat is létezik, de azok átadása vagy nem stabil, vagy nem releváns!

## 🛒 Ajánlott bevásárlólista
A projekt az alábbi (vagy ezekkel kompatibilis) alkatrészekre lett optimalizálva:

1. **Mikrokontroller:** [ESP32 DevKit V1](https://www.aliexpress.com/item/1005007806091039.html)<br>
    A testboard kényelmes a csavarozás miatt, de jelentősen megnöveli a méretet, azzal már nem fér a dobozos odb2 csatlakozó dobozába!     
2. **CAN busz illesztő:** [SN65HVD230 CAN Board](https://www.aliexpress.com/item/1005009014401256.html) (3.3V kompatibilis!)<br>
    Ajánlott az érintkező tüskéket leforrasztani és fixre forrasztani a kábelezést!
3. **Tápellátás:** [DC-DC Step-Down konverter](https://www.aliexpress.com/item/1005010535958276.html) (12V -> 5V)
4. **Csatlakozó:** [OBD2 Male csatlakozófej](https://www.aliexpress.com/item/1005003700654250.html)<br>
    Csatlakozóből azt válaszd, amelyiket szeretnéd. A dobozos verzióba belefér az összes alkatrész. Összeépítésnél figyel az érintkezők szigetelésére (clapton szalaggal szigetelj!). A lábkiosztás számozva van az összes terméken.

## 🔌 Bekötési segédlet (Wiring)

### 1. Tápellátás
Az autó OBD2 portja adja a tápot, amit le kell transzformálni az ESP32 számára:
- **OBD2 Pin 16 (12V)** ➡️ DC-DC IN (+)
- **OBD2 Pin 4/5 (GND)** ➡️ DC-DC IN (-)
- **DC-DC OUT (5V)** ➡️ ESP32 `VIN` láb
- **DC-DC OUT (GND)** ➡️ ESP32 `GND` láb

### 2. Adatkommunikáció
- **OBD2 Pin 6 (CAN High)** ➡️ CAN Modul `CAN_H`
- **OBD2 Pin 14 (CAN Low)** ➡️ CAN Modul `CAN_L`
- **CAN Modul VCC** ➡️ ESP32 `3V3` láb
- **CAN Modul GND** ➡️ ESP32 `GND` láb
- **CAN Modul CTX** ➡️ ESP32 `GPIO5`
- **CAN Modul CRX** ➡️ ESP32 `GPIO4`

## 🚀 Szoftver telepítése és használata

### A - Ha van home assitant szervered:
1. Hozz létre egy új eszközt, alapbeállításokkal az ESPHome Builder-el (Ha nincs, akkor kell HACS kiegészítőt telepítened!)
2. A szükséges részeket az `ha_mg_zs_ev.yaml` fájlból másold be. <br>
    [Firmware letöltése](https://github.com/karolyia79/mgzsev_espdevice/raw/main/yaml/m)
4. Kösd össze az ESP32-t a géppel
5. Flash-eld az ESP32-re az [ESPHome Web Tools](https://web.esphome.io/) vagy a Home Assistant ESPHome kiegészítője segítségével.
6. Az ESPHome Builder kezelőfelületén online lesz és használd az új entitásokat.

### B - Ha standalone akarod használni (nincs home assitant)
1. Kösd össze az ESP32-t számítógéppel
2. **[Kattints ide az MG ZS EV Reader telepítéséhez](https://karolyia79.github.io/mgzsev_espdevice/)**
3. Csatlakozhatsz az esközhöz WiFi-n (megjenik egy AP, amit be kell konfigurálni) vagy Bluetooth-on is.
4. WiFi-n a konfigurálás után kapott IP-re csatlakozva minden adatot látni fogsz.
5. Bluetooth-ra csatlakozva kell valamilyen Bluetooth-terminál applikáció, amivel látható lak lesznek az adatok. Pl: Android: Serial Bluetooth Terminal (Fejlesztő: Kai Morich). Apple: Bluetooth Terminal (Lukas Pistrol)

### Ha a kapcsolat létrejött és nincs azonnal adat ne ess kétségbe! Kell neki néhány másodperc, mire az autó észreveszi, hogy miket kér tőle CAN buszon az eszköz!

## ⚠️ Biztonsági figyelmeztetés
Az OBD2 port a kormányoszlop alatt, bal oldalon található. Ügyelj rá, hogy a kábelezés ne akadályozza a pedálok használatát vagy a kormányzást! A DC-DC konvertert az első használat előtt mérd ki multiméterrel, hogy pontosan 5V-ot adjon le! A kábelek és csatlakozópontok szigetelésére, illetve a bekötések pólusaira nagyon figyelj, mert akár kárt is okozhat az autó CAN rendszerében! A kábelezés legyen jó minőségű és megfelelő keresztmetszetű vezetékekből! A forrasztások legyenek erősek! Az eszköz használata EGYÉNI FELELŐSSÉG, bármilyen kárért a felhasználó felelős! <br>
### Ha nem vagy biztos magadban, akkor nem kötelező az ODB2 csatlakozón megtáplálni az eszközt. Az ESP32 USB portján is adhatsz neki áramot, így a legrosszabb esetben csak felcseréled a CAN hi és CAN low lábakat, ami nem okozhat semmilyen kárt semmiben, csak egyszerűen nem működik az eszköz.

---
*Készült az MG ZS EV tulajdonosok számára. Használd felelősséggel!*
