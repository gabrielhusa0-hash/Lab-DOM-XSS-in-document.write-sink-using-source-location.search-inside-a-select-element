# Lab: DOM XSS in document.write sink using source location.search inside a select element

## 📚 Studijní materiál
Tento repozitář slouží jako můj osobní studijní zápisník a přehled řešení laboratoří z platformy **PortSwigger Web Security Academy**. Dokumentuji zde postupy, zranitelnosti a způsob jejich exploitationu pro budoucí reference a rozvoj znalostí v oblasti kybernetické bezpečnosti.

---

## 🛠️ Postup řešení laboratoře

1. **Spuštění a zkoumání:** Spustil jsem laboratoř a vybral si některou z nabízených možností v rozbalovacím menu, přičemž jsem sledoval chování URL adresy.
2. **Úprava URL adresy (Injekce payloadu):** Do adresního řádku prohlížeče jsem na konec URL přidal parametr s payloadem pro útek z elementu:
   ```text
   &storeId="></select><img%20src=1%20onerror=alert(1)>


# Proč použitý payload fungoval?

Když jsem do URL vložili hodnotu &storeId="></select><img%20src=1%20onerror=alert(1)>, v kódu stránky to vyvolalo tyto tři kroky:

1
Ukončení atributu ("): Dvojitá úvodzovka předčasně uzavřela aktuální HTML atribut, ve kterém byl vstup původně zasazen.

2
Uzavření elementu (></select>): Zobáček > ukončil aktuální tag a zápis </select> násilně zavřel celý rozbalovací seznam, čímž se otevřel prostor pro vložení vlastního kódu.

3
Injekce škodlivého tagu (<img src=1 onerror=alert(1)>): Vložili jsme vlastní obrázek, který se pokusí načíst neplatný zdroj (1). Protože načtení selže, okamžitě se spustí událost onerror, která aktivuje funkci alert(1).