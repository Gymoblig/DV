# B-DV: Databázy a vizualizácia — ŤAHÁK

> FEI STU, doc. Ing. Ladislav Körösi, PhD.
> Skratky: PK = primárny kľúč, CK/FK = cudzí kľúč, NF = normálna forma

---

# ČASŤ A — DATABÁZY

## 1. Základné pojmy

- **Databáza (DB)** — zbierka previazaných údajov bez zbytočnej redundancie. Cieľ: minimalizovať redundanciu → vyhnúť sa **anomáliám** (vkladania, aktualizácie, mazania).
- **SRBD / DBMS** (Systém riadenia bázy dát) — softvér nad OS, ktorý umožňuje prístup, uchovávanie, aktualizáciu, zálohu a zabezpečenie dát.
- **Klient-server**: aplikácie → DBMS → úložisko (RAM/SSD/HDD).

### Typy databáz (podľa dátového modelu)
| Typ | Forma uloženia | Príklady |
|---|---|---|
| Relačné (SQL) | tabuľky, prepojené kľúčmi | MySQL, Oracle, MariaDB, PostgreSQL, MS SQL |
| Dokumentové | JSON/BSON dokumenty | MongoDB |
| Grafové | uzly + hrany | Neo4j |
| Key-Value | dvojica kľúč-hodnota | Redis |
| Stĺpcové | dáta organizované po stĺpcoch | Cassandra |

### SQL — Structured Query Language
- Štandardizovaný jazyk pre RDBMS. Implementácie sa môžu líšiť.
- **CRUD**: Create, Read, Update, Delete.
- Tri kategórie:
  - **DDL** — definícia štruktúry (CREATE, ALTER, DROP)
  - **DML** — manipulácia s dátami (SELECT, INSERT, UPDATE, DELETE)
  - **DCL/TCL** — riadenie prístupu, transakcie (GRANT, COMMIT)

### Tabuľka — základné pojmy
- **Tabuľka** — množina záznamov o objektoch rovnakého typu = entita.
- **Stĺpec (atribút)** — vlastnosť záznamu určitého dátového typu.
- **Riadok** — jeden záznam (inštancia).
- **Hodnota** — údaj v konkrétnom riadku/stĺpci. `NULL` = neznáma hodnota (nie nula ani prázdny reťazec).
- **Odvodené údaje** — neukladáme, počítame pri dopyte (napr. vek z dátumu narodenia).
- **Doména** — množina prípustných hodnôt atribútu.

### Funkčná závislosť
- Zápis: **A → B** = pre každú hodnotu A existuje práve jedna hodnota B.
- Príklady: `id_pouzivatela → meno`, `mesto → PSC`, `funkcia → plat`.

---

## 2. Databázové kľúče (SUMARIZÁCIA — často na skúške!)

| Kľúč | Definícia |
|---|---|
| **Primary key (PK)** | jednoznačne identifikuje riadok, nesmie obsahovať NULL, vynucuje jedinečnosť |
| **Candidate key** | minimálna množina atribútov, ktorá jednoznačne identifikuje riadky |
| **Superkey** | množina atribútov, ktorá jednoznačne identifikuje riadky (nemusí byť minimálna) |
| **Alternate key** | kandidátny kľúč, ktorý NEbol zvolený za PK (jedinečnosť zabezpečuje `UNIQUE`) |
| **Foreign key (FK/CK)** | stĺpec(e) odkazujúce na PK/jedinečný kľúč inej tabuľky; vytvára relácie, zabezpečuje **referenčnú integritu**; môže byť NULL |
| **Surrogate key** | umelo vytvorený identifikátor bez významu (napr. AUTO_INCREMENT ID) |
| **Natural key** | kľúč z reálnych atribútov (napr. rodné číslo, ISIC) |
| **Simple key** | tvorený 1 stĺpcom |
| **Composite key** | tvorený kombináciou stĺpcov |
| **Compound key** | synonymum pre composite |
| **Intelligent key** | jednotlivé časti nesú význam (napr. `kl-st-02`) |

### Anomálie pri zlom návrhu
- **Anomália vkladania** — nemôžem evidovať X bez Y (napr. internát bez študenta).
- **Anomália aktualizácie** — zmena údaja vyžaduje úpravu vo viacerých riadkoch → nekonzistencia.
- **Anomália mazania** — zmazaním záznamu stratím aj iné informácie (napr. zmaže sa posledný študent → stratí sa info o internáte).

---

## 3. Vybrané dátové typy v MySQL

### Celé čísla
| Typ | Veľkosť | Rozsah (signed) |
|---|---|---|
| TINYINT | 1 B | -128 až 127 (unsigned 0-255) |
| SMALLINT | 2 B | -32 768 až 32 767 |
| INT | 4 B | -2 147 483 648 až 2 147 483 647 |
| BIGINT | 8 B | -2^63 až 2^63-1 |

### Desatinné
- **DECIMAL(c,d)** — PRESNÁ hodnota, c číslic spolu, z toho d desatinných. Napr. DECIMAL(5,2) = -999.99 až 999.99. Max c=65. Vhodné pre **peniaze**.
- **FLOAT(m,d)** — približná hodnota, 4 B.
- **DOUBLE(m,d)** — približná hodnota, 8 B.

### Textové
- **CHAR(Length)** — pevná dĺžka 0-255 B, doplnené medzerami. Rýchle vyhľadávanie, plytvanie miestom.
- **VARCHAR(Length)** — premenlivá dĺžka 0-65535 B. Šetrí miesto, pomalšie vyhľadávanie.
- TEXT — pre dlhé reťazce.

### Dátum a čas
- **YEAR** — 1901 až 2155 (alebo 0000).
- **DATE** — `'RRRR-MM-DD'`, rozsah `1000-01-01` až `9999-12-31`.
- **TIME** — `'HH:MM:SS'`, rozsah ±838:59:59.
- **DATETIME** — `'RRRR-MM-DD HH:MM:SS'`.
- **TIMESTAMP** — DATETIME naviazaný na časové pásmo.

---

## 4. Modelovanie (ERA — Entity-Relationship-Attribute)

### Úrovne modelovania
1. **Konceptuálny model** — vysoká abstrakcia, len entity a vzťahy.
2. **Logický model** — entity + atribúty + vzťahy.
3. **Fyzický model** — konkrétne tabuľky, dátové typy v DB.

### Základné pojmy
- **Entita** — objekt reálneho sveta (Študent, Predmet) → tabuľka.
- **Atribút** — vlastnosť entity (meno, vek) → stĺpec.
- **Vzťah (relácia)** — prepojenie entít (študent **navštevuje** predmet).

### Kardinalita (mohutnosť) — maximum
- **1:1** — manželia, osoba↔občiansky preukaz. V DB: jedna tabuľka alebo FK.
- **1:N** — čitateľ má viac kníh, ale kniha má 1 čitateľa. FK na strane "N".
- **M:N** — študent↔predmety, film↔herci. Nutná **prepojovacia (väzobná) tabuľka** s dvoma FK.

### Modalita / Parcialita — minimum
- 0 — vzťah je nepovinný; 1 — povinný.
- Zápis intervalom: **(min, max)** napr. (0,5) = 0 až 5 výskytov.
- POZOR: interval sa zapisuje na opačnú stranu, než ktorej entity sa týka.

### Crow's Foot symboly
- `|` — presne jeden (povinný)
- `o` — nula (nepovinný)
- `<` — viac (mnoho)
- `|<` — 1..N
- `o<` — 0..N

### Prevod ERA → relačný model
- entita → tabuľka
- atribút → stĺpec
- PK identifikuje riadok
- vzťah 1:N → FK na strane N
- vzťah M:N → **nová prepojovacia tabuľka**

---

## 5. Normalizácia

**Cieľ:** efektívna organizácia údajov, eliminácia redundancie, zachovanie integrity. Bezstratová dekompozícia = pri spätnom spojení sa nesmú stratiť údaje. V praxi sa normalizuje **do 3NF**.

### 1NF — Prvá normálna forma
Tabuľka spĺňa 1NF ak:
1. Má **primárny kľúč**.
2. Každý atribút obsahuje **atomické hodnoty** (ďalej nedeliteľné).
3. Nemá **opakujúce sa atribúty** (typu telefon1, telefon2, telefon3).

**Porušenia**: zoznam v jednej bunke (`100, 105, 102`); stĺpce kurz1, kurz2, kurz3; neatomické "adresa" obsahujúca ulicu+mesto.

### 2NF — Druhá normálna forma
Tabuľka spĺňa 2NF ak:
1. Je v **1NF**.
2. Každý nekľúčový atribút je **úplne funkčne závislý od celého PK** (nie od jeho podmnožiny).

Problém vzniká len pri **zloženom PK**. Ak časť atribútov závisí len od časti PK, treba ich vyčleniť do samostatnej tabuľky.

**Príklad porušenia**: tabuľka Hodnotenie(student PK, kod_predmetu PK, nazov_predmetu, znamka) — nazov_predmetu závisí len od kod_predmetu. Riešenie: vyčleniť tabuľku Predmety(kod_predmetu, nazov_predmetu).

### 3NF — Tretia normálna forma
Tabuľka spĺňa 3NF ak:
1. Je v **2NF**.
2. Žiadny nekľúčový atribút nie je **tranzitívne závislý** od PK (t. j. cez iný nekľúčový atribút).

**Tranzitívna závislosť**: `PK → A → B`, kde A aj B sú nekľúčové.

**Príklad porušenia**: Zamestnanec(rc PK, meno, mesto, PSC, funkcia, plat) — `mesto → PSC` a `funkcia → plat`. Riešenie: oddeliť tabuľky Mesto a Funkcia.

### Denormalizácia
- **Opačný proces** k normalizácii — zámerné pridanie redundancie.
- **Cieľ**: zrýchliť SELECT, znížiť počet JOIN-ov.
- **Cena**: duplicita, zložitejšie INSERT/UPDATE, riziko nekonzistencie.
- Používa sa **až keď je výkon dôležitejší** než minimalizácia redundancie.

---

## 6. SQL — najdôležitejšie príkazy

### DDL — definícia štruktúry
```sql
CREATE DATABASE nazov;
USE nazov;

CREATE TABLE Zamestnanci (
    id INT AUTO_INCREMENT PRIMARY KEY,
    meno VARCHAR(50) NOT NULL,
    plat DECIMAL(8,2) DEFAULT 0,
    id_oddelenie INT,
    FOREIGN KEY (id_oddelenie) REFERENCES Oddelenia(id)
        ON DELETE SET NULL ON UPDATE CASCADE
);

ALTER TABLE Zamestnanci
    ADD COLUMN email VARCHAR(100) UNIQUE,
    MODIFY COLUMN meno VARCHAR(100),
    DROP COLUMN plat,
    ADD CONSTRAINT fk_odd FOREIGN KEY (id_oddelenie) REFERENCES Oddelenia(id);

DROP TABLE Zamestnanci;
DROP DATABASE nazov;
```

**Obmedzenia (constraints):**
- `NOT NULL`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY`, `CHECK (vek >= 18)`, `DEFAULT hodnota`, `AUTO_INCREMENT`.

**Akcie pri FK (ON DELETE / ON UPDATE):**
- `CASCADE` — zmena/zmazanie sa prepíše do závislých záznamov.
- `SET NULL` — nastaví NULL.
- `RESTRICT` / `NO ACTION` — zakáže operáciu.

### DML — manipulácia s dátami

```sql
-- INSERT
INSERT INTO Zamestnanci (meno, plat) VALUES ('Jano', 1200.00);
INSERT INTO Zamestnanci VALUES (DEFAULT, 'Marek', 1500, 1);

-- UPDATE
UPDATE Zamestnanci SET plat = plat * 1.10 WHERE id_oddelenie = 1;

-- DELETE
DELETE FROM Zamestnanci WHERE id = 5;
TRUNCATE TABLE Zamestnanci; -- vymaže všetky riadky rýchlejšie, resetuje AUTO_INCREMENT
```

### SELECT — výber dát
```sql
SELECT [DISTINCT] stlpce
FROM tabulka
[WHERE podmienka]
[GROUP BY stlpce]
[HAVING podmienka_na_grupy]
[ORDER BY stlpce ASC|DESC]
[LIMIT pocet OFFSET start];
```

**Operátory v WHERE:**
- `=, <>, <, >, <=, >=`
- `AND, OR, NOT`
- `BETWEEN x AND y` (vrátane)
- `IN (a, b, c)`
- `LIKE 'A%'` (% = ľubovoľný počet znakov, _ = jeden znak)
- `IS NULL`, `IS NOT NULL`

### Agregačné funkcie + GROUP BY / HAVING
```sql
SELECT id_oddelenie, COUNT(*) AS pocet, AVG(plat) AS priemer
FROM Zamestnanci
WHERE plat > 500
GROUP BY id_oddelenie
HAVING AVG(plat) > 1000;
```
- **COUNT, SUM, AVG, MIN, MAX**.
- **WHERE** filtruje riadky pred zoskupením; **HAVING** filtruje skupiny po zoskupení.
- Agregačné funkcie nemôžu byť vo WHERE, len v HAVING.

### JOIN — spájanie tabuliek
| Typ | Význam |
|---|---|
| `INNER JOIN` | iba zhody v oboch tabuľkách |
| `LEFT JOIN` (LEFT OUTER) | všetko z ľavej + zhody z pravej (chýbajúce = NULL) |
| `RIGHT JOIN` | všetko z pravej + zhody z ľavej |
| `FULL JOIN` | všetko z oboch (MySQL nemá → simuluje sa cez UNION) |
| `CROSS JOIN` | kartézsky súčin (každý s každým) |
| `SELF JOIN` | tabuľka so sebou samou (cez alias) |

```sql
SELECT z.meno, o.nazov
FROM Zamestnanci z
INNER JOIN Oddelenia o ON z.id_oddelenie = o.id;
```

### Poradie vykonania SELECT (logické)
**FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT**

### VIEW (pohľad)
- Virtuálna tabuľka definovaná dopytom. Zjednodušuje opakované dotazy, skrýva komplexnosť, zvyšuje bezpečnosť.
```sql
CREATE VIEW v_top_platy AS
SELECT meno, plat FROM Zamestnanci WHERE plat > 2000;

DROP VIEW v_top_platy;
```

### INDEX
- Štruktúra (typicky B-strom) pre zrýchlenie vyhľadávania (SELECT, WHERE, JOIN).
- **Spomaľuje** INSERT/UPDATE/DELETE (treba aktualizovať aj index).
- PK má automaticky unique index.
```sql
CREATE INDEX idx_meno ON Zamestnanci(meno);
CREATE UNIQUE INDEX idx_email ON Zamestnanci(email);
DROP INDEX idx_meno ON Zamestnanci;
```

### TRANSAKCIE — ACID
- **A**tomicity — buď celé alebo nič.
- **C**onsistency — DB ostane v konzistentnom stave.
- **I**solation — paralelné transakcie sa neovplyvňujú.
- **D**urability — po COMMIT-e zmeny trvajú aj po zlyhaní systému.

```sql
START TRANSACTION;
UPDATE Ucty SET zostatok = zostatok - 100 WHERE id = 1;
UPDATE Ucty SET zostatok = zostatok + 100 WHERE id = 2;
COMMIT;  -- alebo ROLLBACK;
```

**Izolačné úrovne (od najnižšej):** READ UNCOMMITTED → READ COMMITTED → REPEATABLE READ → SERIALIZABLE.
**Problémy súbežnosti:** dirty read, non-repeatable read, phantom read.

### LOCKS (zámky)
- **Shared (S)** — pre čítanie, viacero transakcií súčasne.
- **Exclusive (X)** — pre zápis, len jedna transakcia.
- Riziko **deadlocku** (vzájomné čakanie) → DBMS deteguje a jednu transakciu zruší.

### TRIGGER (spúšťač)
Procedúra automaticky spustená pri INSERT/UPDATE/DELETE.
```sql
CREATE TRIGGER pred_insert
BEFORE INSERT ON Zamestnanci
FOR EACH ROW
SET NEW.datum_vytvorenia = NOW();
```
- Časy: `BEFORE` / `AFTER`. Udalosti: `INSERT/UPDATE/DELETE`. Pseudo-tabuľky: `NEW`, `OLD`.

### STORED PROCEDURE vs FUNCTION
- **Procedure** — neslúži priamo vo výraze, môže nemať návratovú hodnotu, volá sa cez `CALL`.
- **Function** — vracia hodnotu, dá sa použiť vo `SELECT`.
```sql
CREATE PROCEDURE zvys_plat(IN id_z INT, IN o DECIMAL(5,2))
BEGIN
    UPDATE Zamestnanci SET plat = plat + o WHERE id = id_z;
END;
CALL zvys_plat(1, 100.00);

CREATE FUNCTION rocny_plat(p DECIMAL(8,2)) RETURNS DECIMAL(10,2)
DETERMINISTIC
RETURN p * 12;
```

### USERS — správa používateľov
```sql
CREATE USER 'jano'@'localhost' IDENTIFIED BY 'heslo';
GRANT SELECT, INSERT ON db.* TO 'jano'@'localhost';
REVOKE INSERT ON db.* FROM 'jano'@'localhost';
DROP USER 'jano'@'localhost';
FLUSH PRIVILEGES;
```

### SHOW — informácie o DB
```sql
SHOW DATABASES;
SHOW TABLES;
SHOW COLUMNS FROM Zamestnanci;   -- alebo DESCRIBE Zamestnanci;
SHOW CREATE TABLE Zamestnanci;
SHOW INDEX FROM Zamestnanci;
```

---

## 7. MongoDB (NoSQL — dokumentová DB)

- Dáta uložené ako **dokumenty** vo formáte **BSON** (binary JSON).
- Dokumenty zoskupené v **kolekciách** (analógia tabuliek).
- **Bez pevnej schémy** — každý dokument môže mať iné polia.
- Každý dokument má jedinečné `_id` (ObjectId).
- Škálovanie horizontálne (sharding), vhodné pre veľké objemy a nezosúladené dáta.

```javascript
db.pouzivatelia.insertOne({ meno: "Jano", vek: 25, hobby: ["futbal", "kniha"] });
db.pouzivatelia.find({ vek: { $gt: 18 } });
db.pouzivatelia.updateOne({ meno: "Jano" }, { $set: { vek: 26 } });
db.pouzivatelia.deleteOne({ meno: "Jano" });
```

**Operátory:** `$eq, $gt, $lt, $in, $and, $or, $set, $inc, $push, $pull`.

**SQL vs MongoDB:**
| SQL | MongoDB |
|---|---|
| Tabuľka | Kolekcia |
| Riadok | Dokument |
| Stĺpec | Pole |
| JOIN | $lookup (alebo vnorené dokumenty) |
| PK | _id |

---

# ČASŤ B — VIZUALIZÁCIA

## 8. Vizualizácia v priemyselných riadiacich systémoch

### Základné komponenty riadiaceho systému
- **Technologický proces** — fyzická časť (nádrže, čerpadlá, ventily, motory).
- **Senzory** — meranie veličín (teplota, tlak, hladina, prietok).
- **Akčné členy** — menia stav procesu (ventily, motory, čerpadlá).
- **PLC** (Programmable Logic Controller) — programovateľný automat vykonávajúci riadiaci algoritmus.
- **Operátor** — človek sledujúci a zasahujúci.

### Čo robí PLC
- Číta vstupy zo senzorov.
- Vykonáva riadiaci algoritmus.
- Riadi výstupy (akčné členy).
- Diagnostika a alarmy.

### Úlohy vizualizácie
- **Monitoring** — zobrazenie stavu, meraných veličín.
- **Ovládanie** — nastavovanie parametrov, žiadaných hodnôt, štart/stop.
- **Diagnostika a údržba** — alarmy, stavy zariadení.
- **Archivácia** — historické dáta procesu.

> **Vizualizácia premieňa interné dáta riadiaceho systému na informácie zrozumiteľné pre človeka.**

### Roly v projekte
- **Programátor PLC** — riadiaci algoritmus.
- **Vývojár HMI/SCADA** — vizualizácia, ovládanie.
- **Technológ** — definuje proces a požiadavky.
- **IT/DB špecialista** — archivácia, integrácia.
- **Projektant** — elektrické a P&ID schémy, dokumentácia.

### Premenné (tagy) — prenos PLC ↔ HMI
| Typ údaja | Smer |
|---|---|
| Procesné veličiny (teplota, tlak...) | **čítame z PLC** |
| Stavy zariadení (binárne) | **čítame z PLC** |
| Parametre/recepty (žiadaná hodnota...) | **zapisujeme do PLC** |
| Riadiace povely (ZAP/VYP, AUTO/MAN) | **zapisujeme do PLC** |

---

## 9. Stavebné prvky HMI aplikácie

- **Premenné (tags)** — externé (z PLC) / interné (len HMI). Vytváranie: manuálne / import / linkovanie.
- **Obrazovky** — Base (technologická), Master/Template (spoločné prvky: hlavička, navigácia), Popup (pomocné okná).
- **Grafické objekty** — text, číselné polia, tlačidlá, indikátory, symboly (motory, ventily, nádrže), animácie.
- **Alarmy** — binárne (diskrétny stav) / analógové (limity HI, LO, HIHI, LOLO).
- **Trendy** — online (real-time) / historické (archivované).
- **Recepty** — uložené sady parametrov pre konkrétny výrobok.
- **Používateľské prístupy** — rôzne úrovne oprávnení.
- **Jazykové verzie**.

### Alarmy — info pri alarme
- Dátum a čas vzniku, alarmová správa, názov a hodnota tagu, **priorita (severity)**, kto potvrdil.
- **Stavy alarmu**: aktívny / zaniknutý / potvrdený (ACK).

### Recepty — operácie
- **Send** — odoslať parametre receptu do PLC.
- **Snapshot** — načítať aktuálne hodnoty z PLC do receptu.
- **Save** — uložiť recept do úložiska HMI.
- **Delete** — odstrániť recept.

---

## 10. Návrh HMI obrazoviek

### Definícia vizualizácie (Mudrončík, Zolotová)
> Použitie teoretických, technických, programových a komunikačných prostriedkov v priemyselnom podniku na zviditeľňovanie objektov technologického procesu a jeho riadiaceho systému s cieľom podpory rozhodovania a riadenia v reálnom čase.

- HMI = **50–75 % nákladov** riadiaceho systému.

### Ergonómia a vnímanie
- **Ergonómia** — vedná disciplína prispôsobenia prostredia človeku.
- Človek prijíma veľa, spracuje málo informácií. Priveľa info → znížená pozornosť.

### Použiteľnosť (Usability) — **ISO 9241**
Norma ISO 9241 (ergonómia interakcie človek–počítač) definuje 3 vlastnosti:
1. **Účinnosť (effectiveness)** — splní používateľ úlohu?
2. **Efektívnosť (efficiency)** — splní ju rýchlo s minimom úsilia/chýb?
3. **Spokojnosť (satisfaction)** — je práca prirodzená a bez stresu?

### Proces práce operátora (3 kroky)
1. **Detekcia** — zistiť, že nastala zmena (farba, blikanie, alarm, zvuk).
2. **Pochopenie** — interpretovať info (čitateľný text, jasné symboly, štandardné farby).
3. **Reakcia** — vykonať zásah (ovládacie prvky, dostatočne veľké tlačidlá).

### Rozdelenie obrazovky (typické 3 oblasti)
| Časť | Obsah |
|---|---|
| **Prehľadová (information area)** | Hlavička, názov, čas, alarmy, hlásenia |
| **Pracovná (work area)** | Hlavný obsah — schéma procesu, trendy, recepty |
| **Ovládacia (control area)** | Navigácia medzi obrazovkami, ovládacie tlačidlá |

Prehľadová a ovládacia časť bývajú v **šablóne (template)**.

### Hierarchia obrazoviek
1. Prehľadová obrazovka celého systému.
2. Technologické obrazovky častí procesu.
3. Detailné obrazovky zariadení.

### Farby — IEC 60073 (HMI významy)
| Farba | Stav | Reakcia operátora |
|---|---|---|
| **Červená** | Alarm / nebezpečenstvo | Okamžitý zásah |
| **Žltá** | Varovanie, odchýlka | Venovať pozornosť |
| **Žltozelená** | Výstraha | Zvýšiť pozornosť |
| **Zelená** | Normálny stav | Bez zásahu |
| **Modrá** | Výzva na zásah | Vykonať akciu |
| **Biela / sivá** | Neutrálne, info | — |

### Princípy návrhu
- **Konzistentnosť** — rovnaké farby/symboly/rozloženie naprieč celým projektom.
- **Kontrast** — text musí byť čitateľný oproti pozadiu.
- Farby slúžia na **prenos významu**, nie ako dekorácia.

### Normy a smernice
- **ISO 9241** — ergonómia interakcie človek-počítač.
- **VDI/VDE 3699** — návrh operátorských obrazoviek v procesoch.
- **ISA-101** — moderné odporúčania HMI návrhu (High Performance HMI).
- **HMI style guides** — interné štandardy firiem.

### Proces návrhu HMI
1. Definovať princípy (farby, symboly, štruktúra).
2. Analýza technológie (veličiny, zariadenia, úlohy operátora).
3. Návrh obrazoviek (hierarchia, grafika).
4. Testovanie a iteratívne úpravy.

---

## 11. Hardvérové aspekty HMI

### Typy HMI podľa hardvéru
- **Diskrétne ovládacie prvky** — tlačidlá (štart, stop, EMERGENCY), prepínače, joysticky.
- **Signalizačné prvky** — kontrolky, majáky, signalizačné stĺpiky, zvuková signalizácia.
- **Grafické operátorské rozhrania** — HMI panely (displej + tlačidlá), dotykové displeje, priemyselné PC.
- **SCADA operátorské pracoviská**.

### Farby tlačidiel a signalizácie — **IEC 60204-1**
- **Červená** — nebezpečenstvo, alarm, núdzové zastavenie.
- **Žltá** — neobvyklý stav, upozornenie.
- **Zelená** — normálny stav, potvrdenie.
- **Biela** — všeobecná info, napájanie.
- **Modrá** — požiadavka na zásah.

### 6 faktorov výberu HMI zariadenia
1. **Prevádzkové prostredie** — teplota, vlhkosť, prach, voda, vibrácie, krytie **IP**, **EX** (výbušné), EMI rušenie.
2. **Typ aplikácie** — počet obrazoviek, množstvo dát, trendy, archivácia, výkon, pamäť.
3. **Možnosti pripojenia** — fyzické rozhrania (Ethernet, USB, SUB-D), protokoly (**Modbus, Profinet, EtherCAT, Modbus TCP/IP**).
4. **Spôsob zadávania údajov** — dotykový displej / fyzické tlačidlá (napr. operátor v rukaviciach).
5. **Programovací softvér** — proprietárny / otvorený, knižnice, kompatibilita.
6. **Ďalšie** — rozšíriteľnosť, podpora výrobcu, dokumentácia, cena.

> Najprv overiť technické požiadavky, potom porovnávať cenu.

---

## 12. SCADA systémy

### Definícia
**SCADA** = **S**upervisory **C**ontrol **A**nd **D**ata **A**cquisition.
- Systém pre **dohľad a riadenie geograficky rozložených procesov** (telecontrol system).
- Typické aplikácie: **energetické siete, vodárne, plynovody, ropovody, doprava, veľké závody**.

### Prečo nestačí PLC + HMI
- Veľké množstvo dát (desaťtisíce až milióny tagov).
- Centrálny dohľad nad rozľahlým systémom.
- Zber a archivácia historických dát.
- Vzdialené riadenie cez WAN.
- Vyžaduje sa **redundancia** kritických komponentov.

### SCADA vs DCS
| | SCADA | DCS |
|---|---|---|
| Rozloženie | geograficky distribuované | jeden závod |
| Riadenie | cez PLC/RTU na vzdialených miestach | distribuované riadiace stanice |
| Typické nasadenie | energetika, vodárne, doprava | chémia, petrochémia, energetické bloky |

### Architektúra SCADA (vrstvy)
1. **Senzory a akčné členy** (field level).
2. **PLC / RTU** — lokálne riadenie + zber dát.
3. **Komunikačná sieť**.
4. **SCADA server** — agregácia dát.
5. **Historian** — historická databáza.
6. **Operátorské pracoviská**.

### RTU (Remote Terminal Unit)
- Zariadenie pre **vzdialený zber dát a komunikáciu so SCADA**.
- Funkcie: pripojenie senzorov/akčných členov, lokálne riadenie, prenos do SCADA, prijímanie povelov.
- **Vlastnosti**: robustná konštrukcia, RTC (hodiny reálneho času) pre časové značky, **zálohované napájanie** (UPS/batéria), lokálna pamäť pri výpadku komunikácie, **watchdog**, redundantné porty.
- **Protokoly**: Modbus, **IEC 60870-5-101/104**, **DNP3**, **IEC 61850**.

### PLC vs RTU
| | PLC | RTU |
|---|---|---|
| Použitie | linka, závod | geograficky rozložené |
| Optimalizácia | rýchle real-time riadenie | telemetria, komunikácia |
| Hodiny RTC | externé | vstavané |
| Zálohovanie | externé (UPS) | vstavané (batéria) |
| Príklad | Siemens S7, Vijeo | Siemens CP1243-1 (telecontrol modul) |

### 4 generácie SCADA
1. **Centralizované** — sálové počítače, proprietárne protokoly.
2. **Distribuované** — LAN, viac staníc, stále proprietárne.
3. **Sieťové** — TCP/IP, LAN/WAN, škálovateľnosť.
4. **Moderné** — Internet, **cloud, IoT**, otvorené protokoly.

### Redundancia v SCADA
- Redundantné servery, databázy (historian), komunikačné trasy, operátorské stanice, **UPS napájanie**.

### Distribuovaná architektúra (rozdelenie funkcií)
- **I/O server** — komunikácia s PLC/RTU.
- **Alarm server** — spracovanie alarmov.
- **Historian (trend) server** — archivácia.
- **SCADA server** — distribúcia dát klientom.
- **Operátorské stanice**.

### Vývoj HMI obrazoviek (od 80. rokov)
- 80. roky: **mimic displays** — fyzické tlačidlá, indikátory na paneli.
- 90. roky: farebné grafické obrazovky (často preplnené farbami).
- 2010+: **High Performance HMI** — minimalistické, prehľadné, šedé pozadie, farby len pre alarmy/odchýlky.

---

## 13. Vijeo Designer (cvičenia)

Nástroj **Schneider Electric** používaný na cvičeniach na tvorbu HMI vizualizácie nad PLC projektom.

**Na cvičeniach sa implementuje**:
- Technologická obrazovka (objekty s premennými).
- Ovládanie (tlačidlá, popup okná pre AUTO/MAN, OTV/ZATV ventilov, ZAP/VYP čerpadiel).
- Alarmový systém + história.
- Recepty (sady parametrov).
- Trendy (časové priebehy).
- Používateľské prístupy.
- Jazykové verzie.

---

## 14. Normy pre symboliku merania a regulácie (P&ID)

### Hlavné normy
- **ISO 3511** — Process measurement control functions and instrumentation (najmä **Európa**).
- **ANSI/ISA-5.1** — Instrumentation Symbols and Identification (najmä **USA**, procesný/petro priemysel).
- **STN** — slovenské prevzatia (STN ISO, STN EN ISO, STN EN IEC).

### Základná značka prístroja
**Kružnica** (~10 mm) s **písmenkovým kódom**.

**Umiestnenie prístroja (podľa čiar v kružnici):**
- Bez čiary — prístroj **priamo v technológii** (field instrument).
- Jedna čiara — prístroj dostupný operátorovi (panel, HMI).
- Dve čiary — pomocný panel / za panelom.

**V kružnici:**
- nad čiarou — písmenkový kód funkcie,
- pod čiarou — identifikátor (číslo meracieho miesta).

### Štruktúra písmenkového kódu
1. **Prvé písmeno** — meraná/riadená **veličina**.
2. **2. písmeno (dodatkové)** — spresnenie (diferencia, pomer, ...).
3. **Ďalšie písmená** — funkcia v poradí **I-R-C-T-Q-S-Z-A**.

### Prvé písmeno (veličina) — TABUĽKA
| Pís. | Anglicky | Význam |
|---|---|---|
| **T** | Temperature | teplota |
| **P** | Pressure | tlak |
| **F** | Flow | prietok |
| **L** | Level | hladina |
| **S** | Speed | rýchlosť |
| **Q** | Quality | kvalita (pH, zloženie, vodivosť) |
| **G** | Gauging | poloha, rozmer |
| **U** | Multivariable | viacero veličín |

### 2. písmeno — dodatok (modifikátor)
| Pís. | Význam |
|---|---|
| **D** | Difference (rozdiel) |
| **F** | Ratio (pomer) |
| **J** | Scan (snímanie) |
| **Q** | Integrate/Totalize (sumácia) |

### Funkčné písmená (poradie I-R-C-T-Q-S-Z-A)
| Pís. | Anglicky | Význam |
|---|---|---|
| **I** | Indicator | indikácia (zobrazenie) |
| **R** | Recorder | zapisovač |
| **C** | Controller | regulátor |
| **T** | Transmitter | prevodník |
| **Q** | Integrator/Totalizer | integrácia, sumácia |
| **S** | Switch | spínač |
| **Z** | Position | poloha |
| **A** | Alarm | signalizácia |
| **B** | Binary | binárny stav |

### Príklady čítania
| Kód | Význam |
|---|---|
| **TIC** | Temperature Indicator Controller — indikácia + regulácia teploty |
| **TDC** | Temperature Difference Controller — regulácia diferencie teploty |
| **FRC** | Flow Recorder Controller — zapisovanie + regulácia prietoku |
| **FRQ** | Flow Recorder + Totalize — zapisovanie a sumácia prietoku |
| **PA**<sup>H</sup> | Pressure Alarm High — alarm hornej medze tlaku |
| **LCA** | Level Controller Alarm — regulácia hladiny s alarmom |
| **LIT** | Level Indicator Transmitter — meranie hladiny s indikáciou |
| **PDR** | Pressure Difference Recorder — zápis diferencie tlaku |
| **QRC** | Quality (pH) Recorder Controller |
| **FFC** | Flow + Ratio Controller — pomerová regulácia |
| **HS** | Hand Switch — ručné ovládanie |

### Symboly v schéme
- **Hrubá čiara** — technologické zariadenia / potrubie.
- **Tenká čiara** — pripojenie snímača / signálne vedenie.
- **Bodka** — spojenie čiar (bez bodky = len križujú sa).
- **Trojuholník** — korekčný člen (nešpecifikovaný typ).
- **Kosoštvorec** (motýlik) — regulačný ventil.
- **Šesťhran** — riadiaci počítač (PLC/DCS) — softvérová funkcia.
- **Štvorec s kruhom** — zdieľané zobrazenie a riadenie (DCS/SCADA HMI).

### Akčný člen = korekčný člen + pohon
- **Krúžok prázdny** — automatický pohon (servo).
- **Krúžok s H** — manuálny pohon.
- **Trojuholník s krúžkom** — automatický akčný člen.

### Správanie ventilu pri výpadku energie/signálu
- **Šípka nahor** — pri výpadku **otvorí** (fail open).
- **Šípka nadol** — pri výpadku **zatvorí** (fail close).
- **Dve čiarky** — **zostane v polohe** (fail freeze).
- Kombinácie pre rôzne typy porúch (strata energie / signálu).

### Typy ventilov (gate vs globe)
- **Gate valve (šupátkový)** — len úplne otvorené / zatvorené, **nie na reguláciu**.
- **Globe valve (sedlový)** — **plynulá regulácia** prietoku.

### Druhy signálov v schéme (typom čiary)
- **Elektrický** — čiarky cez čiaru.
- **Pneumatický** — šípka.
- **Hydraulický** — vlnka / "L".

### Praktické využitie znalostí P&ID
- Návrh regulačných slučiek.
- Tvorba HMI/SCADA vizualizácií.
- Uvádzanie do prevádzky (commissioning).
- Diagnostika porúch.
- P&ID = spoločný jazyk projektanta, programátora PLC/DCS, technológa, obsluhy.

---

# RÝCHLY PREHĽAD NORIEM

| Norma | Predmet |
|---|---|
| **ISO 9241** | Ergonómia interakcie človek–počítač (usability) |
| **IEC 60073** | Farebné kódovanie HMI (červená alarm, zelená normál, ...) |
| **IEC 60204-1** | Bezpečnosť strojov — farby tlačidiel a signalizácie |
| **VDI/VDE 3699** | Návrh operátorských obrazoviek |
| **ISA-101** | Moderné HMI (High Performance HMI) |
| **ISO 3511** | Symbolika merania a regulácie (EU) |
| **ANSI/ISA-5.1** | Symbolika merania a regulácie (US) |
| **DIN 2403** | Farebné označovanie potrubí podľa média |
| **IEC 60870-5-101/104, DNP3, IEC 61850** | Komunikačné protokoly SCADA/telemetria |
| **IEC 61131** | Programovanie PLC |

---

# POSLEDNÉ TIPY PRED SKÚŠKOU

1. **Naučte sa kľúče** (Primary, Foreign, Candidate, Surrogate, Natural, Composite) — pýtajú sa skoro vždy.
2. **Normalizácia** — vedieť rozhodnúť, či tabuľka spĺňa 1NF/2NF/3NF a vedieť ju upraviť.
3. **SQL JOIN** — vedieť rozdiel medzi INNER, LEFT, RIGHT, CROSS.
4. **WHERE vs HAVING** — WHERE pred GROUP BY, HAVING po GROUP BY.
5. **ACID** — vedieť, čo znamená každé písmeno.
6. **Kardinalita M:N** — vždy potrebuje prepojovaciu tabuľku.
7. **HMI vs SCADA vs DCS** — HMI = lokálne ovládanie; SCADA = geograficky rozložené; DCS = jeden závod, distribuované riadenie.
8. **ISO 3511 značky** — vedieť prečítať aspoň: TIC, FRC, PRC, LCA, FT, PT, TT, HS.
9. **Farby HMI**: červená = alarm, zelená = OK, žltá = varovanie, modrá = výzva na zásah.
10. **IEC 60204-1 vs IEC 60073** — prvá pre hardvér tlačidlá, druhá pre obrazovkové HMI.
