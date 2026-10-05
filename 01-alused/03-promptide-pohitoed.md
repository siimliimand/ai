# 1.3 Promptide põhitõed

> **Sihtpublik:** kõik | **Eeltingimused:** [1.2 Võimalused ja piirid: mida automatiseerida tasub](02-voimalused-ja-piirid.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, mis on prompt ja miks tulemuse kvaliteet sellest otseselt sõltub;
- koostada töökindla prompti viiest osast: selge ülesanne, kontekst, roll, väljundi vorming ja näited;
- vältida viit sagedamat viga, mille tõttu vastused muutuvad ettearvatuks;
- täiustada prompti järk-järgult — testi, paranda, testi uuesti.

## Lihtsalt öeldes

> Prompt on töökorraldus, mille sa annad väga targa, aga sinu ettevõtet mitte tundvale assistendile. Ta on lahke, kiire ja hästi lugenud — aga ta ei tea sinu kliente, tooteid, hindu ega tavasid, kuni sina talle neid ära ütled. Mida täpsem korraldus, seda parem tulemus: ebamäärane käsk toodab ebamäärase vastuse, mitte taipava assistendi.

## Mis on prompt täpselt

> **Lihtsalt öeldes:** prompt on kogu juhis, mille sa mudelile enne vastust kaasa annad — küsimus või ülesanne, andmed, piirangud ja näited kokku. Mudel vastab sellele tervikule, mitte ainult sinu küsimusele.

Dokumendist 1.1 tead, et mudel ennustab vastuse selle põhjal, mida ta korraga näeb — kontekstiaknast (ingl k *context window* — tekstihulk, mida mudel korraga näeb). Prompt on täpselt see, mille sina selle aknasse paned. Seepärast pole hea prompt stiilivärk, vaid töö vahend: sama ülesanne võib nõrga ja tugeva promptiga anda täiesti erineva tulemuse.

Kaks reeglit, mida igal juhul meeles pidada:

- **Mudel ei loe mõtteid.** Ta teab ainult seda, mis promptis kirjas on. Kui sa ei ütle, et klient on viieaastane püsiklient, siis mudel seda ei tea — ja vastab nii, nagu tegu oleks igaüks.
- **Mudel ei tooda fakte iseseisvalt.** Kui vastuseks vajalik info promptis puudub, täidab mudel lüngad üldteadmistega — need aga ei pruugi sinu ettevõttega kokku minna. Lüngade äraarvamine ongi 1.1-s kirjeldatud hallutsinatsiooni (mudeli kindlalt öeldud, aga vale vastus) üks levinud põhjusi.

## Hea prompt 5 osa

> **Lihtsalt öeldes:** hea prompt on nagu korralik tööbrift: mida teha, mis olukord, kelle nurgast, milline tulemus välja näeb ja mis on hea töö näidis. Viis osa — viis vastust.

### 1. Selge ülesanne

Üks ülesanne, selge tegusõna, konkreetsed nõuded.

```text
Kokkuvõta järgnev koosoleku protokoll viiele põhipunktile.
```

### 2. Kontekst ja kohalik info

Kõik faktid, mida mudel ise teada ei saa: ettevõtte reeglid, kliendi andmed, dokumendi sisu, piirangud.

```text
Meie poes on tagastusaeg 14 päeva. Klient ostis toote 03.01.2026 ja küsib tagastust.
```

### 3. Roll

Kelle nurgast ja missuguse tooniga tuleb vastata. Roll seab sõnavara, hoiaku ja sügavuse.

```text
Sa oled viisakas ja asjalik klienditeenindaja väikses e-poes.
```

### 4. Väljundi vorming

Pikkus, kuju, keel ja sihtgrupp — et tulemus oleks prognoositav ja kohe kasutatav.

```text
Vasta e-kirja kujul, kuni 100 sõna, eesti keeles, kahe lühikese lõiguna.
```

### 5. Näited

Üks-kaks näidet soovitud tulemusest õpetab rohkem kui pikad seletused — mudel jäljendab näidatud mustrit.

```text
Näide: klient küsib „kas toode on laos?“ → „Tere! Jah, toode X on laos — saadame selle teele juba homme!“
```

Kõiki osi pole alati vaja: lihtsa päringu jaoks piisab ülesandest. Mida enam süsteem peab vastusele kindlalt toetuma — näiteks kui vastus läheb edasi teisele programmile kindlas, masinloetavas vormis (nt tabel või JSON; sellest lähemalt dokumendis [2.2](../02-praktika/02-struktureeritud-valjund.md)) — seda enam tasub kõiki viit osa välja kirjutada.

## Nõrgast promptist tugevani: enne ja pärast

Ülesanne: vastata kliendile, kes on pahane, et tema tellimus hilines.

**Nõrk prompt:**

```text
Kirjuta vastus kliendile.
```

**Sama ülesanne hästi püstitatuna:**

```text
Sa oled meie veebipoe klienditeenindaja — rahulik ja lugupidav (roll).

Kirjuta vastuskiri kliendile, kes on rahulolematu, et tellimus saabus
kaks päeva hiljemalt (ülesanne).

Kontekst: klient on meie püsiklient juba kolm aastat. Hilinemise põhjus
oli transpordifirma viga. Meie tava on hilinemise korral pakkuda 10%
soodustust järgmisele ostule (kohalik info).

Vorming: e-kiri, kuni 100 sõna, sõbralik toon (väljundi vorming).

Näide toonist: „Tere, Mart! Vabandame hilinemise pärast — saame aru,
et see on tülikas…“ (näide)
```

**Miks teine töötab paremini?**

| Lisa | Mida ta reaalselt muudab |
|---|---|
| Ülesanne | Mudel teab, mida kirjutada — mitte „midagi kliendile“, vaid konkreetset olukorda lahendav kiri |
| Roll | Toon ja sõnavara: vastus kõlab klienditeenindajalt, mitte neutraalselt robotilt |
| Kontekst | Faktid tulevad sinu andmetest, mitte mudeli fantaasiast — see kaitseb ka hallutsinatsiooni eest |
| Vorming | Pikkus ja toon on prognoositavad — kiri sobib kohe inimese kiirele läbivaatusele enne saatmist |
| Näide | Annab stiilivõrdluse, mille järgi kiri kujundatakse |

Nõrk prompt pole „vale“ — ta on hea lähtepunkt. Vahe on selles, et nõrk prompt jätab kõik otsused mudelile, tugev prompt võtab tähtsamad otsused enne ära.

## 5 sagedamat viga

> **Lihtsalt öeldes:** kui vastus tuleb kehv, on peaaegu alati üks neist viiest veast süüdi — ja igaühe parandus on sama lühike kui viga ise.

1. **Liiga lühike juhis.** „Kirjuta müügitekst.“ — millele, kellele, kui pikk, mis toonis? Mudel peab kõik otsused ise tegema ja teeb neid suvaliselt. **Parandus:** lisa vähemalt ülesanne ja vorming.
2. **Vastuolulised nõuded.** „Selgita võimalikult põhjalikult, aga ära kasuta rohkem kui kaht lauset.“ Mudel ei saa mõlemat täita ja otsustab ise, kumb olulisem — iga kord võib otsustada teisiti. **Parandus:** ütle prioriteet välja: „hoiame lühidalt, kuni kolm lauset, aga garantiiaeg peab mainitud olema“.
3. **Oluline info maetud pikasse keskele.** Pikas promptis saavad algus ja lõpp kõige rohkem tähelepanu, keskmine osa kõige vähem (vt 1.1 tähelepanu kohta). **Parandus:** juhised prompti algusesse, andmed selgelt eraldatult ja kõige kriitilisem nõue veel lõppu korrata.
4. **Ülesanne ilma eesmärgita.** „Kirjuta vastus kliendile“ vs „kirjuta vastus kliendile, et ta jääks meie püsikliendiks“. Eesmärk — kellele ja milleks — muudab kirja sisu ja rõhu. **Parandus:** lisa ühe lausega, mille jaoks tulemust vaja on.
5. **Eeldus, et mudel loeb mõtteid.** „Nagu tavaliselt“, „meie tavapärase stiiliga“ — mudel ei tea sinu tavasid ega varasemaid vestluseid, kui need pole kontekstis. **Parandus:** kirjuta tava välja või anna näide.

## Näide samm-sammult: kliendikirjale vastava prompti ehitamine

Olukord: klient kirjutab: *„Tere! Millal mu tellimus nr 8812 kohale jõuab? Tellisin juba nädal tagasi.“*

**1. samm — alusta nõrga promptiga.**

```text
Vasta kliendi kirjale.
```

Tulemus: suvaline — liiga ammu või leiutatud tarneajaga.

**2. samm — lisa selge ülesanne.**

```text
Vasta kliendi kirjale: ütle, millal tellimus nr 8812 kohale jõuab.
```

**3. samm — lisa kontekst ja kohalik info.**

```text
Vasta kliendi kirjale: ütle, millal tellimus nr 8812 kohale jõuab.
Andmed süsteemist: tellimus esitati 28.09.2026, saadeti teele 02.10.2026,
kullerifirma prognoos: 2–3 tööpäeva.
```

Nüüd põhineb vastus sinu andmetel, mitte mudeli pakkumisel.

**4. samm — lisa roll.**

```text
Sa oled meie e-poe klienditeenindaja — sõbralik ja täpne.
Vasta kliendi kirjale: ütle, millal tellimus nr 8812 kohale jõuab.
Andmed süsteemist: tellimus esitati 28.09.2026, saadeti teele 02.10.2026,
kullerifirma prognoos: 2–3 tööpäeva.
```

**5. samm — lisa väljundi vorming.**

```text
Sa oled meie e-poe klienditeenindaja — sõbralik ja täpne.
Vasta kliendi kirjale: ütle, millal tellimus nr 8812 kohale jõuab.
Andmed süsteemist: tellimus esitati 28.09.2026, saadeti teele 02.10.2026,
kullerifirma prognoos: 2–3 tööpäeva.
Vasta e-kirja kujul, kuni 80 sõna, eesti keeles.
```

**6. samm — testi ja täiusta.** Käivita prompt ja loe vastust. Näiteks: vastus on korrektne, aga ei vabanda hilinemise pärast — lisa üks rida: „Kui tellimus on hilinemas, vabanda ühe lausega.“ Ja jooksuta uuesti. Nii areneb prompt reaalses kasutuses: üks väike muudatus korraga, iga muudatuse mõju testitud. Kliendile minevat kirja vaatab enne saatmist üle inimene — nii hoiame inimese kinnitusahelas (ingl k *human-in-the-loop*); selle põhimõttest räägib põhjalikumalt dokument 3.5.

## Kokkuvõte

- **Prompt on kogu sinu saadetav juhis** ja tulemuse kvaliteet sõltub sellest otseselt — mudel ei loe mõtteid ega tooda sinu ettevõtte fakte iseseisvalt.
- **Töökindel prompt koosneb viiest osast:** selge ülesanne, kontekst ja kohalik info, roll, väljundi vorming, näited. Mida tähtsam on tulemus, seda rohkem osi kasuta.
- **Viis sagedamat viga:** liiga lühike juhis, vastuolulised nõuded, oluline info maetud pikasse keskele, ülesanne ilma eesmärgita ja eeldus, et mudel loeb mõtteid.
- **Prompt täiustub järk-järgult:** esimene katse pole kunagi lõplik — testi, tee üks muudatus, testi uuesti.

## Mis edasi?

- eelmine → [1.2 Võimalused ja piirid: mida automatiseerida tasub](02-voimalused-ja-piirid.md)
- järgmine → [1.4 Kus AI-automatiseerimine juba töötab: kasutujuhtumid](04-kasutujuhtumid.md)
- **Süvenemine mallide ja testimise tsükliga** → [2.1 Head promptini: struktuur, roll, näide, väljundi vorming](../02-praktika/01-hea-prompt.md)
- **Struktureeritud väljund** — JSON (masinloetav andmevorming) ja muud prognoositavad väljundivormid → [2.2 Struktureeritud väljund: tabelid, mallid ja JSON](../02-praktika/02-struktureeritud-valjund.md)
- **Promptide versioonihaldus** — kuidas prompte salvestada ja versioonida → [2.6 Promptide haldus kui vara](../02-praktika/06-promptide-haldus.md)
- **API** (programmiline liides — kuidas mudelit koodist kutsuda) → [3.1 API integratsioonid: mudel programmist välja kutsuda](../03-susteemi-ulesehitus/01-api-integratsioonid.md)
- **AI-agendid** — süsteemid, kes kasutavad prompte plaanimiseks ja iseseisvaks tegutsemiseks → [4.1 Agendi süsteemid: mis need on ja millal vaja](../04-agendid-ja-mootmine/01-agendi-susteemid.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
