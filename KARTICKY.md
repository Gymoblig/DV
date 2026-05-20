# B-DV — Kartičky na opakovanie (Q&A)

> Skús si najprv odpovedať z hlavy, až potom pozeraj odpoveď.

---

## A. DATABÁZY — ÚVOD A KĽÚČE

**1. Čo je databáza?**
> Zbierka navzájom previazaných informácií bez zbytočnej redundancie. Cieľ: vyhnúť sa anomáliám pri vkladaní, aktualizácii a mazaní.

**2. Čo je SRBD (DBMS)?**
> Systém riadenia bázy dát — softvér bežiaci nad OS, ktorý umožňuje prístup k dátam z aplikácií, ich uchovávanie, aktualizáciu, zálohy a zabezpečenie.

**3. Vymenuj 3 anomálie pri zlom návrhu databázy.**
> Anomália vkladania, aktualizácie, mazania.

**4. Vymenuj 5 typov databáz podľa dátového modelu.**
> Relačné (SQL), Dokumentové (MongoDB), Grafové, Key-Value, Stĺpcové (column).

**5. Čo znamená SQL a aké má 3 kategórie príkazov?**
> Structured Query Language. DDL (CREATE/ALTER/DROP), DML (SELECT/INSERT/UPDATE/DELETE), DCL/TCL (GRANT/COMMIT).

**6. Čo znamená CRUD?**
> Create, Read, Update, Delete.

**7. Čo je NULL?**
> Neznáma (nedefinovaná) hodnota — NIE nula, NIE prázdny reťazec.

**8. Čo je funkčná závislosť A → B?**
> Pre každú hodnotu A existuje práve jedna hodnota B (A jednoznačne určuje B).

**9. Čo je Primary Key (PK)?**
> Jednoznačne identifikuje riadok. Neopakujúce sa hodnoty, NEsmie obsahovať NULL.

**10. Čo je Foreign Key (FK/CK)?**
> Stĺpec(ce) odkazujúce na PK/UNIQUE inej tabuľky. Vytvára relácie a zabezpečuje referenčnú integritu. Môže byť NULL.

**11. Rozdiel medzi Superkey a Candidate key?**
> Superkey = MNOŽINA atribútov, ktorá jednoznačne identifikuje riadky (nemusí byť minimálna). Candidate key = MINIMÁLNA množina, ktorá to dokáže.

**12. Čo je Alternate key?**
> Kandidátny kľúč, ktorý NEbol zvolený za PK. Jedinečnosť sa zabezpečuje cez `UNIQUE` constraint.

**13. Surrogate key vs Natural key?**
> Surrogate = umelo vytvorený identifikátor bez významu (napr. AUTO_INCREMENT ID). Natural = z reálnych atribútov entity (napr. rodné číslo).

**14. Composite key?**
> Primárny kľúč tvorený kombináciou viacerých stĺpcov.

**15. Intelligent key (smart key)?**
> Kľúč, ktorého jednotlivé časti nesú vlastný význam (napr. `kl-st-02` = klasické-stolové-02).

**16. Aký dátový typ použiť pre peniaze a prečo nie FLOAT?**
> `DECIMAL(c,d)` — ukladá presné číselné hodnoty. FLOAT/DOUBLE sú APROXIMOVANÉ a zaokrúhľujú.

**17. Rozdiel CHAR vs VARCHAR?**
> CHAR — pevná dĺžka (rýchlejšie vyhľadávanie, doplnené medzerami). VARCHAR — premenlivá dĺžka (šetrí miesto, pomalšie).

**18. Rozsahy: TINYINT, SMALLINT, INT?**
> TINYINT 1 B (-128..127), SMALLINT 2 B (-32 768..32 767), INT 4 B (-2,1 mld..2,1 mld).

**19. Aký dátový typ má DATE v MySQL?**
> Formát `'RRRR-MM-DD'`, rozsah `1000-01-01` až `9999-12-31`.

---

## B. ERA A KARDINALITA

**20. Čo je entita a atribút?**
> Entita = objekt reálneho sveta (Študent), v DB → tabuľka. Atribút = vlastnosť entity (meno), v DB → stĺpec.

**21. Aké sú úrovne modelovania?**
> Konceptuálny → Logický → Fyzický.

**22. Aké sú 3 typy kardinality vzťahov?**
> 1:1, 1:N, M:N.

**23. Príklad 1:1?**
> Osoba ↔ Občiansky preukaz; manžel ↔ manželka.

**24. Príklad 1:N?**
> Čitateľ má požičaných viac kníh, ale kniha patrí 1 čitateľovi. Jeden učiteľ → viac predmetov.

**25. Príklad M:N?**
> Študent ↔ predmety; film ↔ herci; album ↔ žánre.

**26. Ako sa rieši vzťah M:N v relačnej databáze?**
> Vytvorí sa **prepojovacia (väzobná) tabuľka** s dvoma FK na obe entity.

**27. Kde je FK pri vzťahu 1:N?**
> Na strane "N" (na strane "viac").

**28. Čo je modalita/parcialita vzťahu?**
> Určuje minimálny počet väzieb (0 = nepovinný, 1 = povinný vzťah).

**29. Crow's foot — čo znamená `|<`?**
> Jeden alebo viac (1..N). Povinný vzťah s ľubovoľne mnohými.

**30. Crow's foot — čo znamená `o<`?**
> Nula alebo viac (0..N). Nepovinný vzťah.

**31. Pravidlá prevodu ERA na relačný model?**
> Entita → tabuľka, atribút → stĺpec, vzťah 1:N → FK na strane N, vzťah M:N → nová prepojovacia tabuľka.

---

## C. NORMALIZÁCIA

**32. Čo je cieľ normalizácie?**
> Efektívne organizovať údaje, eliminovať redundanciu a zabezpečiť integritu dát.

**33. Čo je bezstratová dekompozícia?**
> Pri spätnom spojení rozdelených tabuliek sa nesmú stratiť žiadne údaje.

**34. Tri podmienky pre 1NF?**
> (1) Tabuľka má PK. (2) Každý atribút má atomické hodnoty. (3) Žiadne opakujúce sa atribúty (typu kurz1, kurz2, kurz3).

**35. Vyhovuje 1NF tabuľka `student | telefony` kde telefony obsahuje "123, 324, 3452"?**
> NIE — atribút telefony nie je atomický.

**36. Čo požaduje 2NF?**
> Musí byť v 1NF + každý nekľúčový atribút úplne funkčne závisí od CELÉHO PK (nie iba od jeho časti). Problém vzniká pri zložených PK.

**37. Príklad porušenia 2NF?**
> Hodnotenie(student PK, kod_predmetu PK, nazov_predmetu, znamka) — `nazov_predmetu` závisí len od `kod_predmetu`, nie od celého PK. Riešenie: vyčleniť Predmety.

**38. Čo požaduje 3NF?**
> Musí byť v 2NF + žiadny nekľúčový atribút nie je tranzitívne závislý od PK (cez iný nekľúčový atribút).

**39. Čo je tranzitívna závislosť?**
> PK → A → B, kde A aj B sú nekľúčové. B závisí od PK len cez A.

**40. Príklad porušenia 3NF?**
> Zamestnanec(rc, meno, mesto, PSC) — `rc → mesto → PSC`. PSC závisí od mesta, nie priamo od rc. Riešenie: oddeliť tabuľku Mesto.

**41. Po čo je denormalizácia?**
> Zámerne pridať redundanciu pre zrýchlenie čítania (SELECT) a zníženie počtu JOIN-ov. Nevýhody: duplicita, riziko nekonzistencie, zložitejšie INSERT/UPDATE.

**42. Kedy denormalizovať?**
> Pri veľkých databázach a častom čítaní dát, keď je výkon dôležitejší než minimalizácia redundancie.

---

## D. SQL — DML

**43. Štruktúra SELECT s GROUP BY a HAVING?**
> `SELECT stĺpce FROM tabuľka WHERE filter GROUP BY stĺpce HAVING filter_grup ORDER BY ... LIMIT n;`

**44. Rozdiel WHERE vs HAVING?**
> WHERE filtruje riadky PRED zoskupením. HAVING filtruje skupiny PO GROUP BY (môže obsahovať agregačné funkcie).

**45. Logické poradie vykonania SELECT?**
> FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT.

**46. Vymenuj 5 agregačných funkcií.**
> COUNT, SUM, AVG, MIN, MAX.

**47. LIKE 'A%' vs LIKE 'A_'?**
> `%` = ľubovoľný počet znakov; `_` = presne jeden znak.

**48. Ako sa testuje NULL?**
> `IS NULL` / `IS NOT NULL`. NIE `= NULL`.

**49. Rozdiel INNER JOIN vs LEFT JOIN?**
> INNER vráti len zhody v oboch tabuľkách. LEFT vráti všetko z ľavej + zhody z pravej (chýbajúce hodnoty z pravej = NULL).

**50. Čo robí CROSS JOIN?**
> Kartézsky súčin — každý riadok ľavej s každým riadkom pravej.

**51. Príklad UPDATE s WHERE?**
> `UPDATE Zamestnanci SET plat = plat * 1.10 WHERE id_oddelenie = 1;`

**52. Aký je rozdiel medzi DELETE a TRUNCATE?**
> DELETE — zmaže riadky podľa WHERE, dá sa rollbackovať, zachová AUTO_INCREMENT. TRUNCATE — vymaže celú tabuľku rýchlo, resetuje AUTO_INCREMENT, nedá sa rollbackovať.

---

## E. SQL — DDL, VIEW, INDEX

**53. Aké constraints poznáš?**
> NOT NULL, UNIQUE, PRIMARY KEY, FOREIGN KEY, CHECK, DEFAULT, AUTO_INCREMENT.

**54. Čo robí `ON DELETE CASCADE`?**
> Pri zmazaní rodičovského záznamu sa automaticky zmažú aj všetky závislé záznamy v podriadenej tabuľke.

**55. `ON DELETE SET NULL`?**
> Pri zmazaní rodiča sa FK v závislých záznamoch nastaví na NULL.

**56. Čo je VIEW?**
> Virtuálna tabuľka definovaná SELECT-om. Zjednodušuje opakované dotazy, skrýva zložitosť, zvyšuje bezpečnosť.

**57. Čo je INDEX a aký má dopad?**
> Štruktúra (B-strom) pre zrýchlenie vyhľadávania. Zrýchľuje SELECT/WHERE/JOIN, ale spomaľuje INSERT/UPDATE/DELETE (treba aktualizovať aj index).

**58. Má PK automaticky index?**
> Áno, PK má automaticky unique index.

---

## F. SQL — TRANSAKCIE, TRIGGER, PROCEDURE

**59. Čo znamená ACID?**
> **A**tomicity (celá alebo nič), **C**onsistency (zachová konzistentný stav), **I**solation (paralelné transakcie sa neovplyvňujú), **D**urability (COMMIT trvá aj po zlyhaní).

**60. Aké sú izolačné úrovne?**
> READ UNCOMMITTED → READ COMMITTED → REPEATABLE READ → SERIALIZABLE (od najnižšej po najvyššiu).

**61. Vymenuj problémy súbežnosti.**
> Dirty read, non-repeatable read, phantom read.

**62. Príkazy transakcie?**
> `START TRANSACTION;` ... `COMMIT;` alebo `ROLLBACK;`.

**63. Shared vs Exclusive lock?**
> Shared (S) — pre čítanie, viacero transakcií súčasne. Exclusive (X) — pre zápis, len jedna transakcia.

**64. Čo je deadlock?**
> Dve+ transakcie sa navzájom čakajú na zámky. DBMS to deteguje a jednu transakciu zruší.

**65. Čo je TRIGGER?**
> Procedúra automaticky spustená pri INSERT/UPDATE/DELETE. BEFORE/AFTER + INSERT/UPDATE/DELETE + FOR EACH ROW. Pseudo-tabuľky NEW a OLD.

**66. Rozdiel medzi PROCEDURE a FUNCTION v SQL?**
> Procedure — volá sa cez CALL, nemusí vracať hodnotu, nedá sa použiť v SELECT. Function — vracia hodnotu, môže sa použiť priamo v SELECT.

**67. Príkazy na správu používateľov?**
> `CREATE USER`, `GRANT`, `REVOKE`, `DROP USER`, `FLUSH PRIVILEGES`.

---

## G. MONGODB

**68. Čo je MongoDB?**
> Dokumentová NoSQL databáza. Dokumenty v BSON formáte, zoskupené v kolekciách. Bez pevnej schémy.

**69. Analógia SQL → MongoDB?**
> Tabuľka → Kolekcia. Riadok → Dokument. Stĺpec → Pole. PK → _id (ObjectId). JOIN → $lookup alebo vnorené dokumenty.

**70. Príklad insert v MongoDB?**
> `db.kolekcia.insertOne({ meno: "Jano", vek: 25 });`

---

## H. HMI A VIZUALIZÁCIA — ZÁKLADY

**71. Čo robí PLC?**
> Spracúva vstupy zo senzorov, vykonáva riadiaci algoritmus, riadi výstupy (akčné členy), poskytuje diagnostiku.

**72. Aký je rozdiel medzi PLC a HMI?**
> PLC vykonáva riadenie procesu (automaticky). HMI poskytuje človeku rozhranie na monitorovanie a ovládanie.

**73. Vymenuj 5 úloh vizualizácie.**
> Monitoring procesu, ovládanie technológie, signalizácia porúch/alarmy, diagnostika systému, sledovanie trendov (history).

**74. Aké údaje sa čítajú z PLC?**
> Procesné veličiny (teplota, tlak, hladina, prietok) a stavy zariadení (binárne).

**75. Aké údaje sa zapisujú do PLC?**
> Parametre/recepty (žiadané hodnoty) a riadiace povely (ZAP/VYP, AUTO/MAN).

**76. Aké sú typické roly v HMI projekte?**
> Programátor PLC, vývojár HMI/SCADA, technológ, IT/DB špecialista, projektant.

---

## I. PRVKY HMI APLIKÁCIE

**77. Vymenuj základné stavebné prvky HMI projektu.**
> Premenné (tags), obrazovky, grafické objekty, alarmy, trendy, recepty.

**78. Typy premenných v HMI?**
> Externé (prepojené na PLC) a interné (len v HMI).

**79. Typy obrazoviek?**
> Base (technologická), Master/Template (spoločné prvky — hlavička, navigácia), Popup (pomocné okná).

**80. Aké sú typy alarmov?**
> Binárne (diskrétny stav) a analógové (limity HI, LO, HIHI, LOLO).

**81. Aké informácie zobrazuje alarm?**
> Dátum/čas vzniku, alarmová správa, názov a hodnota premennej, priorita (severity), operátor ktorý alarm potvrdil.

**82. Stavy alarmu?**
> Aktívny, zaniknutý, potvrdený (ACK).

**83. Typy trendov?**
> Online (real-time) a historické (archivované).

**84. Operácie s receptami?**
> Send (odoslať do PLC), Snapshot (načítať z PLC), Save (uložiť do HMI), Delete (odstrániť).

---

## J. NÁVRH HMI — ERGONÓMIA A FARBY

**85. Norma pre ergonómiu interakcie človek-počítač?**
> **ISO 9241**.

**86. Tri vlastnosti použiteľnosti (usability) podľa ISO 9241?**
> Účinnosť (effectiveness), Efektívnosť (efficiency), Spokojnosť (satisfaction).

**87. 3 kroky práce operátora?**
> Detekcia → Pochopenie → Reakcia.

**88. Aké sú 3 oblasti obrazovky podľa odporúčaní?**
> Prehľadová (information), Pracovná (work), Ovládacia (control).

**89. Norma pre farby HMI?**
> **IEC 60073** (HMI obrazovky) a **IEC 60204-1** (hardvér — tlačidlá, signalizácia).

**90. Význam farieb v HMI (IEC 60073)?**
> Červená = alarm; Žltá = varovanie; Žltozelená = výstraha; Zelená = normál; Modrá = výzva na zásah; Biela/sivá = neutrálne.

**91. Norma pre návrh operátorských obrazoviek?**
> **VDI/VDE 3699** (staršia smernica) a **ISA-101** (moderné High Performance HMI).

**92. Princíp konzistentnosti?**
> Rovnaké farby/symboly/rozloženie naprieč celým HMI projektom — operátor sa nemusí znovu učiť.

---

## K. HARDVÉR HMI

**93. Vymenuj 4 typy HMI hardvéru.**
> Diskrétne ovládacie prvky (tlačidlá), signalizačné prvky (kontrolky, majáky), grafické HMI panely/displeje, SCADA operátorské pracoviská.

**94. Norma pre farby tlačidiel?**
> **IEC 60204-1**.

**95. Vymenuj 6 faktorov výberu HMI zariadenia.**
> 1) Prevádzkové prostredie, 2) typ aplikácie, 3) možnosti pripojenia, 4) spôsob zadávania údajov, 5) programovací softvér, 6) ďalšie hľadiská (cena, podpora).

**96. Čo je krytie IP?**
> Stupeň ochrany pred prachom/vodou. Prvé číslo = prach, druhé = voda.

**97. EX prostredie?**
> Výbušné prostredie — vyžaduje špeciálne certifikované zariadenia.

**98. Aké komunikačné protokoly poznáš pre PLC/HMI?**
> Modbus, Modbus TCP/IP, Profinet, EtherCAT.

---

## L. SCADA

**99. Čo znamená skratka SCADA?**
> **S**upervisory **C**ontrol **A**nd **D**ata **A**cquisition.

**100. Definícia SCADA?**
> Systém, ktorý dozerá a riadi GEOGRAFICKY ROZLOŽENÉ procesy (telecontrol system).

**101. Príklady aplikácií SCADA?**
> Energetické siete, vodárne a kanalizácie, ropovody/plynovody, doprava, veľké závody.

**102. Rozdiel SCADA vs DCS?**
> SCADA = geograficky distribuované systémy (energetika, vodárne). DCS = riadenie procesov v rámci jedného závodu (chémia, petrochémia).

**103. Čo je RTU?**
> **R**emote **T**erminal **U**nit — zariadenie pre vzdialený zber dát a komunikáciu so SCADA. Funkcie ako PLC ale prispôsobené na telemetriu.

**104. Vlastnosti RTU?**
> Robustná konštrukcia, hodiny reálneho času (časové značky), zálohované napájanie (UPS/batéria), lokálna pamäť pri výpadku, watchdog, redundantné porty.

**105. Aké protokoly používa RTU/SCADA?**
> Modbus, IEC 60870-5-101/104, DNP3, IEC 61850.

**106. Rozdiel PLC vs RTU?**
> PLC — rýchle real-time riadenie v závode. RTU — vzdialené, geograficky rozložené, vstavané RTC, zálohované napájanie, optimalizované na telemetriu.

**107. 4 generácie SCADA?**
> 1) Centralizovaná (sálové počítače), 2) Distribuovaná (LAN), 3) Sieťová (TCP/IP, WAN), 4) Moderná (cloud, IoT, otvorené protokoly).

**108. Čo je redundancia v SCADA?**
> Záložné komponenty (servery, databáza, sieť, operátorské stanice, UPS) — pri poruche prevezmú funkciu.

**109. Vymenuj komponenty distribuovanej SCADA architektúry.**
> I/O server, alarm server, historian (trend) server, SCADA server, operátorské stanice.

**110. Čo je High Performance HMI?**
> Moderný koncept (od konca 2000-ých) — prehľadné, minimalistické HMI s šedým pozadím; farby len pre alarmy a odchýlky.

---

## M. P&ID — ISO 3511 / ANSI/ISA-5.1

**111. Čo znamená P&ID?**
> **P**iping and **I**nstrumentation **D**iagram — technologická schéma s potrubím, zariadeniami a meracími prístrojmi.

**112. Dve hlavné normy pre symboliku merania a regulácie?**
> **ISO 3511** (Európa) a **ANSI/ISA-5.1** (USA).

**113. Základná značka prístroja?**
> Kružnica (~10 mm) s písmenkovým kódom.

**114. Bez čiary / s 1 čiarou / s 2 čiarami v značke?**
> Bez čiary — prístroj v technológii (field). 1 čiara — dostupný operátorovi (panel/HMI). 2 čiary — pomocný panel / za panelom.

**115. Štruktúra písmenkového kódu?**
> Prvé písmeno = veličina. 2. písmeno (voliteľné) = dodatok/modifikátor. Ďalšie písmená = funkcia (poradie I-R-C-T-Q-S-Z-A).

**116. Prvé písmená pre veličiny: T, P, F, L?**
> T = Temperature (teplota), P = Pressure (tlak), F = Flow (prietok), L = Level (hladina).

**117. Prvé písmená pre veličiny: S, Q, G, U?**
> S = Speed, Q = Quality (pH, vodivosť), G = Gauging (poloha), U = Multivariable.

**118. Význam písmen I, R, C, T?**
> I = Indicator (indikácia), R = Recorder (zapisovač), C = Controller (regulátor), T = Transmitter (prevodník).

**119. Význam písmen A, S, Z, Q?**
> A = Alarm, S = Switch (spínač), Z = Position (poloha), Q = Integrator/Totalizer (sumácia).

**120. Význam dodatkov D, F, J, Q (2. písmeno)?**
> D = Difference, F = Ratio (pomer), J = Scan (snímanie), Q = Integrate/Totalize.

**121. Prečítaj značku TIC.**
> Temperature Indicator Controller — indikácia a regulácia teploty.

**122. Prečítaj značku FRC.**
> Flow Recorder Controller — zapisovanie a regulácia prietoku.

**123. Prečítaj značku PDR.**
> Pressure Difference Recorder — zapisovanie diferencie tlaku.

**124. Prečítaj značku LCA.**
> Level Controller Alarm — regulácia hladiny so signalizáciou.

**125. Prečítaj značku FFC.**
> Flow + Ratio Controller — pomerová regulácia prietoku.

**126. Prečítaj značku HS.**
> Hand Switch — ručné ovládanie.

**127. Symbol regulačného ventilu (motýlik) — typy podľa pohonu?**
> Krúžok prázdny = automatický (servopohon). Krúžok s H = manuálny pohon. Trojuholník = nešpecifikovaný korekčný člen.

**128. Šípka nahor/nadol/dve čiarky pri ventile — význam?**
> Šípka NAHOR = pri výpadku otvorí (fail open). Šípka NADOL = zatvorí (fail close). Dve čiarky = zostane v polohe (fail freeze).

**129. Gate valve vs Globe valve?**
> Gate (šupátkový) = len úplne OTV alebo ZATV (nie na reguláciu). Globe (sedlový) = plynulá regulácia prietoku.

**130. Šesťhran v P&ID značke?**
> Riadiaci počítač (PLC/DCS) — softvérová funkcia.

**131. Štvorec s kruhom v P&ID?**
> Zdieľané zobrazenie a riadenie (DCS, SCADA HMI).

**132. Hrubá vs tenká čiara v schéme?**
> Hrubá = technologické zariadenie / potrubie. Tenká = pripojenie snímača alebo signálne vedenie.

**133. Bodka na križovaní čiar v schéme?**
> Bodka = čiary sa spájajú. Bez bodky = len križujú sa.

**134. Druhy signálnych vedení?**
> Elektrický (čiarky cez čiaru), pneumatický (šípka), hydraulický (vlnka/L).

---

## N. KOMBINOVANÉ OTÁZKY (TESTY ZAPÁJANIA POJMOV)

**135. Mám tabuľku Studenti s opakujúcim sa atribútom kurz1, kurz2, kurz3. V akej je NF a ako ju opravím?**
> Nie je v 1NF (opakujúce sa atribúty). Riešenie: vyčleniť tabuľku KurzyStudenta(id PK, id_student CK, kurz CK) s riadkami namiesto stĺpcov.

**136. Mám tabuľku Sklad(nazov, vyrobca, telefon_vyrobcu, cena, mnozstvo) s PK (nazov, vyrobca). V akej je NF a prečo?**
> 1NF áno (atomické). 2NF nie — `telefon_vyrobcu` závisí len od `vyrobca`, nie od celého PK. Riešenie: vyčleniť tabuľku Vyrobca.

**137. Mám tabuľku Zam(id PK, meno, mesto, PSC). Vyhovuje 3NF?**
> Nie — `id → mesto → PSC` je tranzitívna závislosť. Riešenie: oddeliť tabuľku Mesto(mesto_id PK, mesto, PSC).

**138. Mám napísať dotaz: zoznam oddelení s priemerným platom nad 1000.**
> `SELECT id_oddelenie, AVG(plat) FROM Zamestnanci GROUP BY id_oddelenie HAVING AVG(plat) > 1000;`

**139. Chcem všetkých zamestnancov spolu s názvom oddelenia, aj keď nemajú priradené oddelenie.**
> `SELECT z.meno, o.nazov FROM Zamestnanci z LEFT JOIN Oddelenia o ON z.id_oddelenie = o.id;`

**140. Aký vzťah je medzi študentom a predmetmi (jeden študent má veľa predmetov, jeden predmet má veľa študentov)?**
> M:N. V DB potrebujem prepojovaciu tabuľku napr. StudentPredmet(id_student FK, id_predmet FK, znamka).

**141. Mám zaviesť kontrolu, že plat zamestnanca nesmie klesnúť pod minimum. Akým mechanizmom?**
> TRIGGER `BEFORE UPDATE` ktorý overí novú hodnotu a vyvolá chybu, alebo CHECK constraint.

**142. Operátor potrebuje rýchlo zistiť, že došlo k poruche čerpadla. Aký prvok HMI použiť?**
> Alarmová správa + zmena farby objektu na červenú + prípadne zvukový signál.

**143. Vo veľkej vodárenskej spoločnosti chcú riadiť čerpacie stanice rozmiestnené po celom kraji z centra. Aký typ systému zvoliť?**
> SCADA s RTU na jednotlivých čerpacích staniciach (geograficky distribuovaný systém).

**144. V chemickej továrni chcú jeden integrovaný riadiaci systém pre celý závod. SCADA alebo DCS?**
> DCS — určený pre riadenie technologických procesov v rámci jedného závodu.

**145. V schéme vidím značku FT v technológii a značku FIC v paneli. Čo to znamená?**
> FT = Flow Transmitter (prevodník prietoku v poli). FIC = Flow Indicator Controller (zobrazenie + regulácia prietoku, na paneli/HMI).

**146. Operátor pracuje s rukavicami a v hlučnom prostredí. Aké HMI zvoliť?**
> Fyzické tlačidlá (alebo dotykový displej prispôsobený na rukavice), prípadne aj akustická signalizácia (silnejšia) + vizuálna signalizácia (majáky).

**147. Aký dôležitý faktor okrem ceny zohľadniť pri výbere HMI?**
> Prevádzkové prostredie, technické požiadavky aplikácie, komunikačné možnosti, kompatibilita s PLC. Cena až nakoniec.

**148. V tabuľke je stĺpec datum_narodenia a chcem zobraziť vek. Mám ho ukladať?**
> Nie — vek je odvodený údaj. Počíta sa pri dopyte (napr. `TIMESTAMPDIFF(YEAR, datum_narodenia, CURDATE())`).

**149. Tlačidlá STOP a EMERGENCY — akú farbu?**
> **Červenú** (norma IEC 60204-1).

**150. ACID — vysvetli D (Durability).**
> Po úspešnom COMMIT-e zmeny v databáze trvajú aj pri zlyhaní systému (výpadok prúdu, pád servera).

---

## TIPY NA UČENIE

1. **Najprv prejdi celý ťahák**, potom skús kartičky bez nahliadania.
2. Zameraj sa na **kľúče (PK, FK, candidate, surrogate, natural)** a **normalizáciu (1NF, 2NF, 3NF)** — tie sa pýtajú takmer vždy.
3. Z VIZ časti najdôležitejšie: **SCADA vs DCS vs HMI**, **farby IEC 60073/60204-1**, **čítanie ISO 3511 značiek** (TIC, FRC, LCA, FT, PT).
4. Skús si **napísať jednoduchý SELECT s JOIN, GROUP BY a HAVING** — to je klasická SQL otázka.
5. Pamätaj: **WHERE pred GROUP BY**, **HAVING po GROUP BY**.
6. Pri normalizácii vždy uvažuj o **funkčných závislostiach** — A → B znamená, že A určuje B.
