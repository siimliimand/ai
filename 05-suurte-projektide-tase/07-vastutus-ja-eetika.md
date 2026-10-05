# 5.7 Vastutus, eetika ja governance

> **Sihtpublik:** juhtiv mitte-tehniline + tehniline | **Eeltingimused:** [5.6 Pidev täiustamine: mõõtmisest otsusteni](06-pidev-taiustamine.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, mis on governance ja miks kasvavas süsteemis ei piisa enam üksikute inimeste kohusetundest;
- koostada otsuste tabel — mis otsus, kes teeb, kes kinnitab, kus kirjas;
- esitada iga automatiseerimisotsuse juures neli eetika küsimust;
- panna kirja lihtne riskiregister ja siduda ta kvartaalse ülevaatusega;
- käituda õigesti siis, kui midagi läheb valesti — õppida, mitte süüdlasi otsida.

## Lihtsalt öeldes

> Väikese süsteemi juures jõuavad inimesed kõige tähtsa üle jutu pealt kokku. Kui süsteem kasvab — rohkem töövoogusid, rohkem inimesi, rohkem kliente —, hakkab iga otsus sõltuma sellest, kes parasjagu kohal on. Siin tulevad reeglid: kes tohib mis otsuseid teha, kes kinnitab, kus see kirjas on. **Reeglite kirjapanek pole kontrollitsemine — ta on kaitse ja selgus kõigile:** inimene teab, mille üle ta tohib ise otsustada, juht teab, mis vajab tema allkirja, ja kui midagi läheb valesti, on näha, kus otsus tehti.

## Governance: kes tohib mis otsuseid teha

Governance (ingl k *governance* — juhtimisreeglite kogu: kes tohib mis otsuseid teha, kuidas riske hinnatakse, kes vastutab). [5.2 Meeskonnatöö ja standardid](02-meeskonnatoo-ja-standardid.md) pani kirja, kuidas tööd muudetakse; governance vaatab astme võrra ülespoole — mitte töö, vaid otsused.

Mis juhtub ilma. Ehitaja käivitab uue süsteemi, sest tundus hea mõte — poole aasta pärast küsib keegi, kes selle otsustas. Kolleeg saadab kliendiandmed uuele pakkujale, sest teenus on tasuta — ja nüüd kuulub küsimus juhile. Ilma reegliteta on iga otsus erand ja iga erand risk.

Governance'i lihtsaim vahend on otsuste tabel (otsus | kes teeb | kes kinnitab | kus kirjas). Viis rida, millega algus tehtud:

| Otsus | Kes teeb | Kes kinnitab | Kus kirjas |
|---|---|---|---|
| Uue süsteemi käivitamine | tellija + ehitaja ([1.6](../01-alused/06-rollid-ja-vastutus.md)) | juht | sünniprotokoll ([5.2](02-meeskonnatoo-ja-standardid.md)) |
| Tundliku valdkonna automatiseerimine — tervis, raha, inimese õigused | juht koos valdkonna asjatundjaga | juhtkond | ohutusreeglid ([3.5](../03-susteemi-ulesehitus/05-ohutus.md)) |
| Kliendiandmete saatmine uuele pakkujale | haldaja ettepanekuna | juht + andmekaitse ekspert | pakkuja nimekiri ([5.3](03-turvalisus-ja-andmekaitse.md)) |
| Mudeli vahetus | haldaja | süsteemi omanik | nimekiri ([5.5 Mudelite vahetamine](05-mudelite-vahetamine.md)) |
| Ohutusreeglite muutmine | sisukujundaja | süsteemi omanik + juht | promptipank ([5.2](02-meeskonnatoo-ja-standardid.md)) |

> **Lihtsalt öeldes:** otsuste tabel töötab nagu allkirjaõigus pangas — keegi ei arutle, kas raamatupidaja „tohib“ suurt summat üle kanda, see on kirjas. Kui kirjas on, et andmete saatmine uuele pakkujale vajab juhi ja andmekaitse eksperti nõusolekut, ei pea keegi arvama ega ootama koosolekut: tee on teada.

Tabel pole summutus. Valdav osa päevaotsustest — tagasivõetavad ja ühe süsteemi piiresse jäävad — jääb tabelist välja: need teeb süsteemi omanik (ingl k *owner* — süsteemi/prompti vastutaja, üks nimi; [5.2](02-meeskonnatoo-ja-standardid.md) põhimõte) ise. Tabelis on ainult otsused, mis on raskesti tagasivõetavad, puudutavad kliente või kannavad vastutust.

## Vastutus: „mudel eksis“ pole vabandus

Vastutus AI vea eest jääb alati inimesele ja ettevõttele ([1.6 Rollid ja vastutus projektis](../01-alused/06-rollid-ja-vastutus.md)) — klient, pank ja järelevalveasutus pöörduvad ettevõtte, mitte mudeli poole. „Mudel eksis“ pole vabandus, mida keegi tunnistaks: mudelil pole lepingut, allkirja ega registrit — seega pole tal ka vastutust.

Praktikas toetub vastutus kahel asjal:

1. **Igal süsteemil on omanik.** Küsimusele „kes selle eest vastutab?“ vastab nimi, mitte „meeskond“ — sest kõigi vastutus on kiiresti mitte ühegi vastutus.
2. **Igal olulisel otsusel on kirjalik otsuseprotokoll:** mis otsustati, miks, kes kinnitas, millal. Kolm rida, kaks minutit — ja aastate pärast pole keegi sunnitud mälestustest taastama, miks just nii otsustati.

Kõike seda paneb põhu alla jälge (ingl k *audit trail* — iga automaatse tegevuse kirje, [3.5](../03-susteemi-ulesehitus/05-ohutus.md)): kui küsitakse „miks süsteem just nii käitus?“, vastab kirje, mitte kellegi mälu.

> **Lihtsalt öeldes:** kirja pandud otsused on ettevõtte kindlustus. Kui keegi küsib „kes otsustas, et vestluse logid hoitakse 90 päeva?“, on vastus dokumendis, koos põhjendusega — ja see kaitseb nii ettevõtet kui ka inimest, kes otsuse tegi. Parem kolm rida kirja täna kui uurimine mälestuste peal homme.

## Neli eetika küsimust

Eetika pole siin tunnete, vaid neli konkreetset vastatavat küsimust, mida iga automatiseerimise otsuse juures üle küsida:

**1. Läbipaistvus** (inimene teab, et räägib süsteemiga). *Küsimus: kas klient teab, et vestluspartner pole inimene?* Ausus on ka lihtsaim tee: lisa vestluse algusesse „Vastan sulle süsteemi kaudu — kui vajad inimest, siit jõuad tema juurde.“ Klienti inimesega suhtlemise usus jätmine on lühiajaline mugavus ja pikaajaline usalduskaotus.

**2. Õiglus** (sarnased juhtumid — sarnased vastused). *Küsimus: kas süsteem käitub erinevalt erinevate rühmade suhtes — keele, nime, elukoha järgi?* Lihtne test: võrdle vastuseid sarnaste juhtumitega, mis erinevad ainult ühe tunnuse poolest — sama küsimus eesti ja inglise keeles, sama juhtum kahe erineva nimega.

**3. Kahjumi ennetus.** *Küsimus: mis on halvim asi, mida süsteem teha saab — ja kas disain hoiab selle ära?* See on [3.5](../03-susteemi-ulesehitus/05-ohutus.md) ohutuse test: kui vastuseks on „loodame, et mudel on tubli“, pole süsteem valmis.

**4. Inimese väärikus.** *Küsimus: kas automatiseerime midagi, mis peab jääma inimlikuks?* Kaebuste vastuvõtt, halbade uudiste teatamine — need on kohad, kus inimene ootab inimest. Kiirus pole siin väärtus, kui hind on raskel hetkel masinaga rääkimine.

## Riskiregister

Governance ei saa hallata riske, mida keegi pole nimetanud. Selleks on riskiregister (ingl k *risk register* — oluliste riskide kirja tabel: risk | tõenäosus | mõju | kes jälgib).

Neli reeglit, mis registri elusana hoiavad:

- **5–10 rida piisab** — parem viis tõelist rida kui viiskümmend üldist.
- **Iga rea juures on nimi** — „meeskond jälgib“ tähendab, et ei jälgi keegi; sama põhimõte kui omanikul ([5.2](02-meeskonnatoo-ja-standardid.md)).
- **Tõenäosus ja mõju kolmel astmel** — madal, keskmine, kõrge; hindamine peab jääma minutite, mitte päevade juurde.
- **Uuendatakse kord kvartalis** — loomulik koht on kvartaalne ülevaatus ([5.4 Kulustrateegia suures mahus](04-kulustrateegia.md)), kus juhtkond niigi numbrid üle vaatab: mis risk teostus, mis kadus, mis tuli juurde.

## Näide samm-sammult: Ravikoda paneb reeglid kirja

Väljamõeldud **„Ravikoda“** — e-apteegi vestlussüsteem, keda [3.5](../03-susteemi-ulesehitus/05-ohutus.md) ja [5.3](03-turvalisus-ja-andmekaitse.md) juba tundsid: vestlus vastab laoseisu ja hindade kohta, terviseküsimused suunab apteekrile. Aasta jooksul süsteem kasvas: kliente tunduvalt rohkem, lisandus ingliskeelne vestlus ja kaks uut töövoogu. Apteegi juht Liis Telline märkas, et otsused olid hakanud sõltuma sellest, kes parasjagu majas on — ja pani reeglid kirja.

**1. Otsuste tabel:**

| Otsus | Kes teeb | Kes kinnitab | Kus kirjas |
|---|---|---|---|
| Uue automatiseerimise käivitamine | ehitaja + sisukujundaja ettepanekuna | apteeker juhina + raamatupidaja | sünniprotokoll ([5.2](02-meeskonnatoo-ja-standardid.md)) |
| Kliendiandmete saatmine uuele pakkujale | haldaja ettepanekuna | juht + andmekaitse ekspert | pakkuja nimekiri ([5.3](03-turvalisus-ja-andmekaitse.md)) |
| Tervisenõuannete automatiseerimine | — | keelatud: inimene alati | ohutusreeglid ([3.5](../03-susteemi-ulesehitus/05-ohutus.md)) |
| Mudeli vahetus | haldaja | süsteemi omanik (apteeker) | nimekiri ([5.5](05-mudelite-vahetamine.md)) |
| Vestluse juhise muutmine | sisukujundaja | süsteemi omanik | promptipank ([5.2](02-meeskonnatoo-ja-standardid.md)) |

Tervise rida on tabeli tähtsaim sõnastus: mitte „keelatud, välja arvatud“, vaid keelatud — sest tervisenõuanne on tundlik valdkond, kus inimene kinnitusahelas (ingl k *human-in-the-loop* — töövoog, kus inimene kinnitab tulemuse enne kasutust) on alati.

**2. Neli eetika küsimust Ravikoda juures.** Läbipaistvus: vestluse sissejuhatusse lisati lause „Sinuga vestleb Ravikoda süsteem; ravimiküsimustes jõuad apteekrini“. Õiglus: keeleline võrdsuse test — sama küsimus eesti ja inglise keeles; laoseis, hind ja suunamine olid samaväärsed; test läks kalendrisse kord kvartalis. Kahjumi ennetus: halvim stsenaarium on eksitav ravimiinfo — kaitse pole lootus, vaid disain, sellised küsimused suunatakse alati inimesele. Väärikus: kaebused jäävad inimesele.

**3. Riskiregister — viis esimest rida:**

| Risk | Tõenäosus | Mõju | Kes jälgib |
|---|---|---|---|
| Süsteem annab vananenud hinna või laoseisu | keskmine | madal | haldaja |
| Terviseküsimus libiseb süsteemile ja ta vastab ise | madal | kõrge | apteeker, omanik |
| Andmed jõuavad uuele pakkujale ilma kokkuleppeta | madal | kõrge | juht + andmekaitse ekspert |
| Ingliskeelne klient saab nõrgema vastuse | keskmine | keskmine | sisukujundaja |
| Mudeli vahetus muudab vastuste tooni | keskmine | keskmine | haldaja |

**4. Juhtum: kui midagi läks valesti.** Klient küsis, kuidas hoiustada ravimit, mis tuli hoida külmkapis — süsteem vastas enesekindlalt „toatemperatuuril“. Viga märgati nädala kokkuvõttest: suunamise asemel oli süsteem ise vastanud.

Edasi toimus õppimine, mitte süüdlaste otsimine:

- **Faktid logist** ([3.4 Vead ja veakäsitlus](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md)): millal, milline küsimus, kust süsteem vastuse võttis.
- **Süüdlasi ei otsitud** — viga polnud inimeses, vaid reeglis: süsteemil oli vastamine väljaspool oma allikat ikka veel lubatud.
- **Parandus:** allikas muudeti vastuse kohustuslikuks osaks ([4.2 RAG](../04-agendid-ja-mootmine/02-rag.md)) — vastus ainult apteegi käibeteabe andmetest ja allikaga; allikata tulemus suunab inimesele.
- **Kinnitus ja õppetund:** juhendi muudatuse kinnitas omanik, juhtum läks riskiregistrisse uue reana ja järgmine kvartaalne ülevaatus vaatas üle, kas parandus hoidis paika ([5.3](03-turvalisus-ja-andmekaitse.md) „õpi“-samm).

> **Lihtsalt öeldes:** juhtumis ei otsitud süüdlasi: süsteem eksis, reegel parandati, kõik nägid, kes ja miks. Firma, kus iga viga on häbi, peidab vigu; firma, kus viga on info, parandab reegleid. Ja kirja pandud otsused on see, mis ettevõtet kaitseb — nii klientide kui järelevalve ees.

## Kokkuvõte

- **Governance on juhtimisreeglite kogu** — ja lihtsaim kuju on otsuste tabel: otsus, kes teeb, kes kinnitab, kus kirjas.
- **Vastutus jääb alati inimesele ja ettevõttele** — „mudel eksis“ pole vabandus; iga süsteemil omanik ja igal olulisel otsusel kirjalik protokoll.
- **Neli eetika küsimust:** läbipaistvus, õiglus, kahjumi ennetus, inimese väärikus — igaüks ühe testiga vastatav.
- **Riskiregister:** 5–10 rida, iga rea juures nimi, uuendus kvartaalse ülevaatuse juures.
- **Kui midagi läks valesti:** faktid kirja, reegel parandada, süüdlasi mitte otsida — reeglid kirja on kaitse ja selgus kõigile.

## Mis edasi?

- eelmine → [5.6 Pidev täiustamine: mõõtmisest otsusteni](06-pidev-taiustamine.md)
- järgmine → [5.8 Mallide teek: kontroll-loendid ja näidised](08-mallide-teek.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
