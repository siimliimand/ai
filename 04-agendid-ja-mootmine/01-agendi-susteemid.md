# 4.1 Agendi süsteemid: mis need on ja millal vaja

> **Sihtpublik:** kõik | **Eeltingimused:** [3.7 Turvalisus: võtmed, andmed, pahatahtlikud juhised](../03-susteemi-ulesehitus/07-turvalisus.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, mis teeb agendist agendi — eesmärk, tööriistad ja otsustamisvabadus — ja kuidas agendi tsükkel (ingl k *agent loop* — plaani → tegutse → vaata → otsusta) käib;
- otsustada workflow (töövoog) ja agendi vahel ning põhjendada valikut;
- nimetada agendi neli peamist riski ja neid vähendav kaitsekiht (ingl k *safeguard* — piirang süsteemi disainis);
- ehitada hübriidi (workflow agendi sammuga) ja selgitada, miks ta on tihti parim valik.

## Lihtsalt öeldes

> Workflow (töövoog) on pakettreis: programm on ette kirjutatud ja lõpp teada. AI-agent on kohalik giid: annad talle sihi („klient peab saama selge vastuse“), telefonid ja loa ise otsustada, keda ja millal helistab. Giid leiab tee ka sinna, kuhu pakettreis ei ulatu — aga keegi ei tea ette, kui palju ta helistab ja kui palju see maksab. Seepärast pannakse giid tööle ainult siis, kui pakettreis enam ei kata.

Dokumendist [1.5](../01-alused/05-susteemi-anatoomia.md) teame kolme kuju ja põhireglit: agent on viimane, mitte esimene valik — siin see reegel lahti kirjutatakse. [3.3](../03-susteemi-ulesehitus/03-tooriistad-ja-tegevused.md) näitas, kuidas mudel tööriistu küsib; nüüd paneme osad kokku agendi süsteemiks.

## Mis teeb agendist agendi: kolm osa

AI-agent (süsteem, kellel on eesmärk, tööriistad ja otsustamisvabadus) erineb ühest mudelikutsest kolme omadusega:

**1. Eesmärk — mis lõppseisund on hea.** Workflow-s on siht peidus sammude jadas — tee ise näitab, kuhu minnakse. Agendil teed pole, seepärast peab eesmärk olema välja öeldud ja kontrollitav: mitte „aita klienti“, vaid „küsimusele on vastatud ja vastuses on ainult andmeid, mis tulid süsteemist“. Viimane hoiab eemal ka hallutsinatsiooni (mudeli kindlalt öeldud, aga vale vastus).

**2. Tööriistad — mida ta saab kasutada.** Kehtib 3.3 põhimõte: tööriistakutse (ingl k *function calling*) tähendab, et mudel küsib, süsteem teeb — agent ei saa ise midagi ära teha, ta saab ainult nõuda, et sinu programm teeks. Tüübid: andmeotsing, täpne arvutus, väljaspool süsteemi tegevus — viimane alati kinnitusahelaga.

**3. Otsustamisvabadus — ta ise valib järjekorra ja sammud.** Tegelik erinevus ei ole tööriistades ega mudelis, vaid teesis: workflow-s on tee ette kirjutatud (2.3), agendi järgmise sammu otsustab mudel ise — selle põhjal, mida ta seni teada sai.

Kolme osa koostöö on agendi tsükkel:

```text
   EESMÄRK: „vastus põhineb ainult süsteemist tulnud andmetel“
        │
        ▼
  ┌─► PLAANI — mis on järgmine kasulik samm?
  │        │
  │        ▼
  │   TEGUTSE — tööriistakutse
  │   (mudel küsib, süsteem teeb — 3.3)
  │        │
  │        ▼
  │   VAATA — mida tulemus ütles:
  │   andmed käes / kutse nurjus / vastus poolik?
  │        │
  │        ▼
  │   OTSUSTA — kas eesmärk on täidetud?
  │        │
  │        ├── jah ──► TULEMUS väljundisse + kirje ajalukku
  │        │
  └── ei ◄─┘  (tagasi algusesse, uus plaan)
```

Tsükli pikkus pole ette teada: workflow-l on sammude arv kirjas, agendi ringide arvu otsustab ta ise. Seepärast paned piiri sina — „kümme ringi, siis peatu ja vii inimesele“ — sest piiramata tsükkel on avatud lõpuga kulu ja risk.

> **Lihtsalt öeldes:** workflow on masin nupuga — vajutad ja ta käib kindla tee läbi; agent on kogenud assistent, kellele ütled eesmärgi ja annad tööriistad, aga tee leiab ta ise. Assistendist teeb agendi teadmine „mis lõpp on hea“, tööriistad (mudel küsib, süsteem teeb) ja luba ise otsustada järjekord.

## Agent vs workflow: võrdlus

Agent on õigustatud, kui kolm asja kehtivad korraga:

- **Tee ei ole ette teada** — milliseid samme ülesanne võtab, teatakse alles töö käigus.
- **Sammud sõltuvad sisust** — järgmine samm otsustatakse selle põhjal, mida eelmine leidis.
- **Ülesanded on mitmekesised** — iga uue küsimuseliigi jaoks eraldi voo tähendab lõputut ehitamist.

Ja kolm märki, et agent ei ole õigustatud:

- **Tee on teada** → workflow (2.3): ette kirjutatud tee on kiirem, odavam ja kontrollitavam.
- **Vead on kallid** → kontrollpunktid ja inimene kinnitusahelas (ingl k *human-in-the-loop* — inimene kinnitab tulemuse enne kasutust) töötavad kindlalt ainult kindlas kohas (3.5) — agendi teel sellist kohta enne ei tea.
- **Lihtsus piisab** → proovi alati kõigepealt lihtsamat kuju — see on 1.5 põhireegel.

| Kriteerium | Workflow | Agent |
|---|---|---|
| **Ettearvatavus** | kõrge — sama sisend käib alati sama teed | madalam — sarnased juhtumid võivad käia erinevaid teid |
| **Kulud** | madalad ja prognoositavad — mudelikutsed kindlates kohtades | kasvavad iga ringiga; ringide arvu otsustab agent ([3.6](../03-susteemi-ulesehitus/06-kulude-haldamine.md)) |
| **Vea risk** | piiratud — viga tabab kindel kontrollpunkt kindlas kohas | kasvab teepikkusega — varane viga suunab kõik järgmised sammud |
| **Hooldus** | muudatus kindlas sammus, mõju on teada | piiride ja kirjelduste hooldus + pidev jälgimine |

Kõigis neljas reas eelistab tabel workflow-d — ja see ongi mõte: agent on õigustatud ainult seal, kus teed pole võimalik ette kirjutada.

## Agendi riskid

**1. Kulud kasvavad.** Iga ring on vähemalt üks mudelikutse — kümnesammuline tee tähendab kümnet või enamat kutset, kus fikseeritud vool oleks piirdunud kahega. Kuna ringide arvu otsustab agent, on kulu enne täitmist ainult hinnang ([3.6](../03-susteemi-ulesehitus/06-kulude-haldamine.md)).

**2. Viga kandub edasi.** Ringid toetuvad üksteisele: kui agent varakult võtab vale tellimuse numbri või valib vale tööriista, põhinevad järgmised sammud juba valel teabel — ja ta ise seda kahtlustada ei pruugi. Fikseeritud voolus tabab viga kindel kontrollpunkt; agendi teel tuleb see punkt ette näha paigaldada.

**3. Ettearvatus kadub.** Sama küsimus võib täna ja homme käia erinevat teed — üks testjuhtum ei tõesta enam midagi. Kvaliteeti tuleb hinnata valimitega ja pidevalt, mitte ühe korra enne käikuandmist ([4.5](05-hindamine.md)).

**4. Ohutus peab olema tegelik.** Agent on ühe sammu kaugusel väljasmaailmast, seepärast kehtivad [3.5 kaitsekihid](../03-susteemi-ulesehitus/05-ohutus.md) eriti rangelt: tööriistu, mida agendil pole, ei saa kasutada — ükskõik mida eesmärk nimetab; raha ja saatmine käivad alati inimese allkirja läbi; ja juhises kirjas keeld üksi ei kaitse, sest pahatahtlikud juhised oskavad keelde ümber lüüa ([3.7](../03-susteemi-ulesehitus/07-turvalisus.md)).

> **Lihtsalt öeldes:** agent teeb rohkem kui vool — ja rohkem võimalusi eksida; iga eksimus läheb kallimaks, mida hiljem ta kinni püütakse. Riskid pole agenti vastu — nad määravad ainult tingimuse: tee peab tõesti ette kirjutamatu olema.

## Hübriid: workflow agendi sammuga

Enamikul ülesannetest on tee suures osas ikkagi teada — ja seal hoiab workflow ta ettearvatavana. **Hübriid (workflow agendi sammuga)** paneb mõlemad kokku: fikseeritud põhitee ja ÜKS agendi samm seal, kus tee pole ette kirjutatav. See on tihti parim lahendus: iga osa teeb seda, mida ta kõige paremini oskab.

Kus agendi samm paigutada? Kohta, kus workflow laguneks — kui üks punkt vajaks iga uue juhtumiliigi jaoks uut haru, kasvab harude hulk kiiremini, kui jõuad neid hooldada. Fikseeritud osa teeb kindlad sammud, agent lahendab ülejäänu — ja agendi väljund ei jõua enam otse kliendini: see on kavand, mis läbib sama kinnituse nagu iga teine tee. Nii annab esimene ettearvatavuse, teine paindlikkuse ja kinnitusahel püüab viga enne kliendini jõudmist.

## Näide samm-sammult: e-poo valib tee

**Lähteseis.** Kodutoa poe tagastusvoog ([2.5](../02-praktika/05-esimene-workflow.md)) töötab: umbes 40 kirja päevas, klassifitseerija määrab liik, fikseeritud tee koostab kavandi, Piret kinnitab. Nüüd mitmekesistuvad kirjad: suuruse vahetus, tellimuse muutmine, kaebused ja terve hulk küsimusi, mis ei mahu ühtegi teadaolevasse liiki. Kolm teed:

**Tee a — viis uut workflow-t.** Iga uue liigi jaoks eraldi voo: sammud, tingimused, testid. Töötab, aga iga uus küsimuseliik on uus ehitus ja klient, kes kirjutab korraga kahest asjast, satub ikkagi „teadmata“ teele.

**Tee b — üks agent tööriistadega.** Agent saab eesmärgi („kliendi küsimusele on vastatud ja vastus põhineb päris andmetel“) ja kolm tööriista: tellimuse otsing, toote info, vahetusvoo käivitamine — ning otsustab ise, mida millal küsida. Paindlik, aga kõik neli riski kehtivad; rahalised otsused aga käivad alati inimese allkirja läbi (3.5).

**Tee c — hübriid.** Fikseeritud alus jääb puutumata: tagastusvoog töötab edasi, kaebused lähevad nagu enne inimesele. Muudatus on üks: „arusaamatu“ haru ei vii kirja enam kohe Pireti loendisse, vaid agendi sammu — agent, kelle tööriistad (tellimuse otsing, toote info — ainult lugemine) koostavad ettepaneku („see on suuruse vahetus; siin on andmed ja sobiv variant“), mis läheb Pireti kinnitusele.

**Soovitus.** Alusta hübridiga. Jälgi mõni nädal ([4.5](05-hindamine.md), [4.6](06-monitooring.md)), millised kirjad agendi sammu satuvad ja mitu ettepanekut Piret muudab. Kui 90 protsenti „arusaamatutest“ osutub kolmeks korduvaks küsimuseks, ehita nendele fikseeritud vood — agendi koormus väheneb. Kui ettepanekud on kasutatavad ja kulud kontrolli all, võib agendi osa kasvada. Otsustavad andmed, mitte mulje — agent pole eesmärk omaette.

## Kokkuvõte

- **AI-agent on süsteem, kellel on eesmärk, tööriistad ja otsustamisvabadus**, ja töötab agendi tsüklis: plaani → tegutse → vaata → otsusta. Ringide arvu otsustab agent — piiri paned sina.
- **Valik kuju:** tee ette teada → workflow; tee sõltub sisust, pole ette kirjutatav, vead pole kallid ja lihtsam kuju ei piisanud → agent.
- **Neli riski:** kulud kasvavad (3.6), viga kandub edasi, ettearvatavus kadub (4.5) ja ohutus vajab tegelikke kaitsekihte, mitte ainult keeldu juhises (3.5).
- **Hübriid — fikseeritud põhitee + agendi samm ühes punktis, alati kinnitusega — on tihti parim lahendus** ja mõistlik lähtekoht.
- **Lihtsam lahendus, mis töötab, on parem kui muljetavaldav agent, mis vahel eksib.** Agendid on võimas tööriist, mitte staatuse sümbol.

## Mis edasi?

- eelmine → [3.7 Turvalisus](../03-susteemi-ulesehitus/07-turvalisus.md) (3. tase on läbi)
- järgmine → [4.2 RAG: oma andmete kasutamine vastuste allikana](02-rag.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
