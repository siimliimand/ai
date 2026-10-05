# 2.1 Head promptini: struktuur, roll, näide, väljundi vorming

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [1.3 Promptide põhitõed](../01-alused/03-promptide-pohitoed.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- panna korralikult kirjutatud prompt (ingl k *prompt* — mudelile antav juhis) aluseks korduvkasutatavale promptimallile (korduvkasutatav juhiste raam — kinnised osad + kohatäited);
- eristada malli kinniseid osi muutuvatest ja kirjutada kohatäited selgelt [NIMI_SUURTÄHTEDEGA];
- viia prompt läbi testimise tsükli: testjuhtumid, variantide kõrvutustabel ja aktsepteerimiskriteeriumid;
- juhtida rolli sõnastusega vastuse tooni ja sügavust ning nõuda ühtset väljundit iga kord.

## Lihtsalt öeldes

> Dokumendis 1.3 kirjutatud hea prompt on õnnestunud eine. Promptimall on retsept: sama toit tuleb lauale ka homme ja ka siis, kui köögis on keegi teine. Muutumatu osa — mõõdud ja ahjukraad — on testitud juhiste püsisõnastus; muutuv osa — täna porgand, homme kõrvits — on kohatäited, kuhu kleebid iga kasutuskorra andmed.

## Nõrgast promptist mallini

1.3 näitas, et töökindel prompt koosneb viiest osast. See töötab hästi ühe korra jaoks. Aga äriliselt väärtuslikud ülesanded korduvad: arveküsimusi tuleb iga päev, kokkuvõtteid iga nädal. Kui kirjutad prompti iga kord nullist, kulub aeg ja iga versioon on natuke teistsugune — tulemus kõigub koos juhistega.

Promptimall lahendab selle: korra hästi tehtud töö pannakse raami, milles kinnised osad jäävad alati samaks ning muutuvad kohad on märgitud kohatäitega [NIMI_SUURTÄHTEDEGA] — nurksulgudes, suurtähtedes ja allkriipsuga nimetatud koht, kuhu iga kasutuskord kleebib uued andmed. Mallist saab tööriist, mida kasutab kogu meeskond: sama sisend annab sama kvaliteediga tulemuse.

Üks realistlik kulunüanss: püsisõnastus kulutab iga kord mõned tokenid (sõnatükk — umbes üks sõna või selle osa), aga seni, kuni kõik mahub AI-mudeli kontekstiaknasse (ingl k *context window* — tekstihulk, mida AI-mudel korraga näeb), on see odav hind ühtlase tulemuse eest.

## Malli osad: kinnised ja muutuvad (kohatäited)

**Kinnised osad** on püsisõnastus, mis peab alati olema täpselt sellisena, nagu testitud: roll, ülesande kirjeldus, reeglid, väljundi vormingu nõuded ja näited. Nende muutmine pole „väike parandus töö juures“, vaid malli uus versioon, mis tuleb uuesti testida.

**Muutuvad osad** on kohatäited. Kolm reeglit hoiavad need selged:

1. **Nimi ütleb, mis sinna läheb.** [KLIENDI_KIRI] tähendab kogu kirja teksti, mitte ainult kliendi nime — [KLIENDI_NIMI] oleks eraldi kohatäide.
2. **Iga muutuv koht on märgitud.** Põhimuster on alati sama: kinnine juhis peal, selge eraldaja ja andmed all.
3. **Tühja kohatäite puhul kehtib reegel.** Juhis „kui andme pole, kirjuta ‚puudub'“ keelab mudelil lünki ära arvata.

Malli luistet võib kirja panna nii:

```text
[ROLL — kinnine osa]
[ÜLESANNE — kinnine osa]
[REEGLID — kinnine osa]
[VÄLJUNDI VORMING — kinnine osa]
[NÄITED — kinnine osa]

KLIENDI_KIRI:
[KLIENDI_KIRI]   ← muutuv osa
```

> **Lihtsalt öeldes:** kinnised osad on retsepti mõõdud ja kohatäited on koht, kuhu lisad tänased toiduained. Retsepti muudad ainult siis, kui oled uue versiooni läbi proovinud — nii ka malliga.

## Testimise tsükkel: kuidas teada, et prompt on hea

Uut prompti ei kiideta heaks ühe õnnestunud katse põhjal. Testimise tsükkel on korduv protseduur, mis teeb kvaliteediotsuse tõendite, mitte tunnete põhjal:

1. **Vali 3–5 testjuhtumit erinevate sisenditega:** tüüpiline juhtum; juhtum, kus andmeid on vähe või ebaselgelt; erandlik juhtum; ja üks, kus kirjas on midagi, mida vastata ei tohiks. Parimad on päris vanad juhtumid (kliendiandmed anonümiseeritult).
2. **Käivita mall iga juhtumiga ja salvesta väljundid.**
3. **Võrdle väljundeid aktsepteerimiskriteeriumitega** — kokkulepitud nõuetega, mille täitmisel tulemus loetakse heaks. Näiteks arvetoimingu kokkuvõtte puhul:

| Aktsepteerimiskriteerium | Nõue |
|---|---|
| Struktuur | alati samad pealkirjad samas järjekorras |
| Faktid | kõik andmed tulevad sisendist; puuduv info on märgitud, mitte ära arvatud |
| Reeglite järgimine | keelatud tegevused (nt nõuande andmine) on väljas |
| Toon | sobib adressaadile — kolleegile märkme, mitte kliendile kirjale |

4. **Tee üks muudatus korraga ja anna versioonile nimi** (A, B, C). Jooksuta uus versioon läbi kõigi samade testjuhtumite ja pane tulemused kõrvutustabelisse:

| Testjuhtum | Versioon A | Versioon B |
|---|---|---|
| Tüüpiline | õige | õige |
| Andmed puudulikud | leiutas tähtaja | kirjutas „puudub“ |
| Erandlik | andis nõu, mida polnud palutud | ainult kokkuvõte |

**Millal piisab ja millal tagasi?** Kui iga testjuhtum läbib iga kriteeriumi ja paar päeva reaalses kasutuses möödub ilma paranduseta, on mall heaks kiidetud. Kui mõni juhtum rikub ühte kriteeriumit, tuleb tagasi: üks muudatus, uus versioon, uus ring. Kõrvalpõhimõte: ära tee korraga mitut muudatust — muidu ei tea enam, kumb muudatus aitas. Väljundeid ja versioonide ajalugu hoitakse kindlas kohas — kuidas, õpetab [2.6](06-promptide-haldus.md).

## Roll ja väljundi vorming sügavamalt

1.3 tutvustas rolli ühe reana: kelle nurgast vastatakse. Praktikas juhib rolli sõnastus palju rohkem kui tooni — sõnavara täpsust, detailitaset ja seda, mida mudel üldse ette võtab. Sama ülesanne, kolm rolli:

| Rollisõnastus | Mida väljundiga juhtub |
|---|---|
| „Sa oled abivalmis assistent.“ | Üldsõnaline, seletab laialt, lisab omalt poolt soovitusi |
| „Sa oled raamatupidamisbüroo vanemspetsialist, kes kirjutab kolleegile asjalikke sisemisi märkmeid.“ | Täpne terminoloogia, lühidus, ei seleta põhitõdesid |
| „Sa oled töötleja, kes kirjutab üles ainult kirjas olevad faktid ja ei anna nõuandeid.“ | Kõige rangem: ei lisa midagi, mida sisendis pole |

Sisukujundaja töö on valida roll ranguse skaalalt: mida kallim on võimalik viga, seda rangem roll.

> **Lihtsalt öeldes:** roll on mudelile antud ametijuhend. Sama „töötaja“ käitub hoopis teisiti, olenevalt sellest, kas talle öeldakse „ole lahke ja aita kõigega“ või „sul pole luba midagi lisada“. Ilma piiritud juhendis on kõik lubatud — ja mudel lisab ka seda, mida sa ei küsinud.

**Väljundi vorming iga kord.** Ühtlase väljundi saladus on formaadinõude täpsus:

- kirjelda kuju konkreetselt: väljade nimed, järjekord, pikkus;
- ütle välja, mida teha puuduva infoga;
- keela lisad, mida sa ei tellinud: tutvustused, kommenteerimine, fraasid nagu „siin on sinu vastus“;
- kui väljund läheb edasi programmile, nõua masinloetavat vormi — selle tehnika õpetab [2.2](02-struktureeritud-valjund.md).

```text
Väljund on ainult kolm rida pealkirjadega KÜSIMUS, TÄHTAEG, VAJALIK TEGEVUS.
Kui andme pole, kirjuta „puudub“ — ära paku.
Ära lisa tutvustust, järeldust ega lauseid nagu „siin on sinu vastus“.
```

## Näide samm-sammult: Nummerbüroo arvetoimingu kokkuvõtte mall

Nummerbüroo (1.6) uuesti: kliendid kirjutavad arve küsimustega ja raamatupidaja vajab igast kirjast sekunditega nähtavat ühtlast kokkuvõtet — küsimus, tähtaeg, vajalik tegevus. Sisukujundajana koostab malli raamatupidaja; kontrollija vaatab tulemused siiski üle, sest inimene jääb kinnitusahelasse (ingl k *human-in-the-loop* — töövoog, kus inimene kinnitab tulemuse enne kasutust).

**1. samm — algvariant (versioon A):**

```text
Sa oled raamatupidamisbüroo assistent. Loe kliendi kiri ja tee kokkuvõte:
mis on küsimus, mis tähtaeg ja mida tuleb teha.
KLIENDI_KIRI: [KLIENDI_KIRI]
```

**2. samm — kolm testjuhtumit:**

1. *Tüüpiline:* „Tere! Kas arve nr 1421 on veel maksmata ja millisel tähtajal?“
2. *Ebaselge:* kiri räägib kahest teemast, tähtaega ei mainita, summa on mainitud kõrvalausest.
3. *Äärmuslik:* klient kirjutab, et arve „tundub vale“ ja palub nõu, mida ette võtta — andmeid on vähe.

Tulemus: juhtum 1 läbis. Juhtum 2 näitas nõrkust — mudel kirjutas tähtajaks „30 päeva“, kuigi seda kirjas polnud: hallutsinatsioon (mudeli kindlalt öeldud, aga vale vastus). Juhtum 3: vastus algas sõnadega „Muidugi, aitan hea meelega!“ ja andis nõu, kuigi ülesanne on ainult kokkuvõte; pealkirjad kõigusid („Aeg“ vs „Tähtaeg“).

Nõrkused: (1) puuduva info äraarvamine; (2) tellimata lisad ja kõikuv vorming; (3) roll ei keela nõustamist.

**3. samm — versioon B:** igale nõrkusele üks muudatus — lisareegel „puuduv info jääb ‚puudub'-ks“, roll rangemaks (ainult kokkuvõte, mitte nõustamine) ja vorming kivisse koos keelatud lisadega. Iga nõrkus sai kõrvutustabelis oma rea, seega on iga muudatuse mõju siiski eraldi jälgitav — korraga mitme muudatuse puhul on nõue, et igaühel peab olema oma jälgitav kriteerium.

**4. samm — kõrvutustabel:**

| Testjuhtum | Kriteerium | Versioon A | Versioon B |
|---|---|---|---|
| Tüüpiline | struktuur | ✓ | ✓ |
| Ebaselge | faktid | ✗ leiutas tähtaja | ✓ „puudub“ |
| Äärmuslik | reeglite järgimine | ✗ andis nõu | ✓ ainult kokkuvõte |
| Kõik | vorming | ✗ pealkirjad kõigusid | ✓ alati samad |

Versioon B läbis kõik juhtumid.

**5. samm — heaks kiidetud lõppmall:**

```text
Sa oled raamatupidamisbüroo töötleja. Sinu ainus ülesanne on kokkuvõte
kliendi kirjast arve küsimuse osas. Sa EI anna nõuandeid ja EI vasta
kliendile — sinu väljundit loeb kolleeg.

Reeglid:
- Kasuta ainult kirjas olevaid fakte.
- Kui infot pole, kirjuta selle välja kohale „puudub“ — ära paku.
- Ära lisa tutvustust, kommenteeri ega lõpeta viisakusfraasiga.

Vorming (alati täpselt nii):
KÜSIMUS: [üks lause]
TÄHTAEG: [kuupäev või „puudub“]
VAJALIK TEGEVUS: [üks lause]

KLIENDI_KIRI:
[KLIENDI_KIRI]
```

Kontroll jääb: iga kokkuvõtte vaatab raamatupidaja enne tegevust üle — mall vähendab tööd, mitte vastutust (vt 1.6). Mall pannakse kirja koos versiooninumbriga ja testjuhtumitega; kuhu ja kuidas, õpetab [2.6](06-promptide-haldus.md).

## Kokkuvõte

- **Promptimall on korduvkasutatav juhiste raam:** kinnised osad (roll, ülesanne, reeglid, vorming, näited) jäävad alati samaks, muutuvad andmed kleebitakse kohatäitesse [NIMI_SUURTÄHTEDEGA].
- **Kinniseid osi ei muudeta ilma uue testimiseta** — iga muudatus on uus versioon, mis käib läbi sama tsükli.
- **Testimise tsükkel on 3–5 testjuhtumit, aktsepteerimiskriteeriumid ja variantide kõrvutustabel** — „hea“ pole tunne, vaid kriteeriumite täitmine kõigil juhtumitel.
- **Roll juhib tooni, sügavust ja piire** — andmetöötluses on rangem roll enamasti parem kui „abivalmis assistent“.
- **Vormingu täpsus tagab ühtluse:** väljade nimed ja järjekord kivisse, puuduv info märgitakse, tellimata lisad keelatakse.

## Mis edasi?

- eelmine → [1.6 Rollid ja vastutus projektis](../01-alused/06-rollid-ja-vastutus.md)
- järgmine → [2.2 Struktureeritud väljund: loendid, tabelid ja JSON](02-struktureeritud-valjund.md)
- Kus prompte hoida ja versioonida → [2.6 Promptide haldus kui vara](06-promptide-haldus.md)
- Mitmeosalised protsessid, kus mall on üks samm → [2.3 Workflow algtasandil: sammud ja tingimused](03-workflow-algtasandil.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
