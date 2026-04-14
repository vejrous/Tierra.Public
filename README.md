# TierraSign — Uživatelská příručka

TierraSign je nástroj pro elektronické podepisování geodetických adresářů (např. ZPMZ).
Vytváří ověřovací soubory s kryptografickými hashi, CMS podpisem a časovým razítkem TSA.

---

## Požadavky

- Windows 7 nebo novější (32-bit aplikace)
- Platný kvalifikovaný certifikát v úložišti certifikátů Windows (`Osobní / MY`) nebo na tokenu / čipové kartě
- Soubory OpenSSL 1.x vedle `TierraSign.exe`:
  - `libssl32.dll` + `libeay32.dll`
- Přístup na TSA server (internet nebo VPN)

---

## Spuštění

Spusťte `TierraSign.exe`. Adresář se soubory lze předat jako argument příkazové řádky nebo přetažením složky na okno aplikace.

---

## Nastavení (první spuštění)

Klikněte na tlačítko **Nastavení...** a vyplňte:

### Připojení TSA

Klikněte na **Spravovat připojení...** a vytvořte nové připojení:

| Pole | Popis |
|------|-------|
| Název | Libovolný název připojení |
| TSA jméno | Přihlašovací jméno k TSA serveru |
| TSA heslo | Heslo k TSA serveru |
| URL TSA | Adresa TSA serveru (např. `https://tsa.postsignum.cz/...`) |

Po uložení se nové připojení objeví v seznamu.

> Pro testování je předvyplněno demo připojení PostSignum (`DEMO`).

> Při použití DEMO přípojení nelze ověřit časové razítko pomocí **KDirVerify5Gui.exe**.

### Certifikát

Klikněte na **Vybrat** a ze seznamu Windows zvolte svůj podpisový certifikát.
Volitelně můžete zaškrtnout **Zobrazit pouze platné certifikáty**.

Uložte nastavení tlačítkem **Uložit**.

---

## Postup podepisování

1. Zadejte nebo vyberte tlačítkem **Procházet...** adresář se soubory ZPMZ.
2. Vyplňte **Číslo ověření** (např. `1/2026`).
3. Zkontrolujte nebo upravte **Datum** (předvyplněno dnešním datem).
4. Klikněte na **Podepsat adresář**.
5. Pokud certifikát je na tokenu/čipové kartě, můžete být požádáni o zadání PIN.

Po úspěšném podpisu jsou v adresáři uloženy tři soubory:

| Soubor | Obsah |
|--------|-------|
| `overeni.txt` | Seznam souborů s SHA-512 hashi |
| `overeni.txt.p7s` | CMS detached podpis souboru `overeni.txt` |
| `overeni.txt.p7s.tsr` | RFC 3161 časové razítko souboru `overeni.p7s` |

---

## Přetažení souborů (Drag & Drop)

Na okno aplikace lze přetáhnout soubor nebo složku přímo z Průzkumníka Windows.
Aplikace automaticky načte adresář, ve kterém se přetažený soubor nachází.

---

## Konfigurace (INI soubory)

Nastavení se ukládá do složky `%LOCALAPPDATA%\Tierra\`:

| Soubor | Obsah |
|--------|-------|
| `TierraSign.ini` | Otisk certifikátu, aktivní připojení, poslední použitý adresář |
| `ConnectionsTSA.ini` | Seznam TSA připojení (jméno, heslo, URL) |

Soubory lze editovat v Poznámkovém bloku (kódování Windows-1250 / ANSI).

---

## Řešení problémů

| Problém | Řešení |
|---------|--------|
| *Nelze načíst OpenSSL* | Zkopírujte `libssl32.dll` a `libeay32.dll` (OpenSSL 1.x) vedle `TierraSign.exe` |
| *Certifikát nenalezen* | Ověřte, že certifikát je v úložišti `Osobní (MY)` - spusťte `certmgr.msc` |
| *TSA odmítla žádost* | Ověřte správnost přihlašovacích údajů a URL v Nastavení |
| *PIN dialog se neobjeví* | Zkontrolujte, zda je token/čipová karta připojena a middleware nainstalován |

---

## Výstupní soubory — ověření platnosti

Platnost podpisu a razítka lze ověřit nástrojem **KDirVerify5Gui.exe**.