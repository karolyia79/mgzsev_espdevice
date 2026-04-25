Ez a könyvtár a projekt futtatható állományait, a bináris disztribúciókat és a frissítési csomagokat tartalmazza.

- Már telepített eszközök esetén: Ha az egységen már fut egy korábbi verzió, nem szükséges a teljes gyári visszaállítás. Az eszköz webes felületén vagy hálózaton keresztül elegendő csak az evmgzs_ota.bin fájlt feltölteni. Ez gyorsabb és biztonságosabb frissítést tesz lehetővé.

- ESPHome konfiguráció: Az OTA csomag már tartalmazza a módosított YAML konfigurációt is. A frissítés során a szoftver automatikusan átvezeti a változásokat az előző verzióhoz képest, így a felhasználónak nincs szüksége manuális kódmódosításra vagy fordításra.

- Mikor kell a Factory? Az evmgzs_factory.bin fájlt továbbra is csak az első telepítéskor (szűz hardver esetén), vagy teljes rendszer-újratelepítéskor kell használni, kábeles kapcsolaton keresztül.
