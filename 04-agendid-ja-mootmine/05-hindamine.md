# 4.5 Hindamine: kuidas teada, kas süsteem on hea

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [4.4 Mitme agendi arhitektuurid](04-mitme-agendi-arhitektuurid.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, miks „tundub hea“ pole otsustamise alus ja mis on hindamine (ingl k *evaluation* — süsteemi kvaliteedi plaanitud mõõtmine arvudega);
- ehitada testikomplekt (ingl k *test set* — ettevalitud sisendite kogu, mida iga muudatuse järel läbi jooksetatakse) ja kuldstandard (ingl k *golden set* — inimese kinnitatud õiged vastused);
- lugeda kuut põhimõõdikut — alates täpsusest (ingl k *accuracy* — mitu % vastustest õige) kuni inimese sekkumise määradeni (mitu % juhtumitest läks inimesele);
- tabada regressioon (ingl k *regression* — varem töötanu läbikukkumine) enne, kui muudatus jõuab klientideni;
- öelda, kes ja millal hindab: automaat, inimese näidisvalik ja kolm kindlat hetke.

## Lihtsalt öeldes

> „Tundub hea“ on arvamus, mitte tõend. Selle asemel kogu 30–50 päris juhtumit, kirjuta igaühe juurde õige vastus ja jooksuta nad läbi iga olulise muudatuse järel. Vana sai 37 õiget, uus 38 — muudatus on hea. Uus sai 35 — regressioon, mida näed paberil, mitte kliendi kirjast.

## „Tundub hea“ pole mõõdik

Süsteem on elus, demod jooksid ilusti ja meeskond ütleb: „tundub päris hea“. Hiljem selgub, et kolm inimest pidid „heas“ silmas pidama kolme erinevat asja — üks ilusaid vastuseid, teine kiirust, kolmas oma õnnestunud juhtumeid.

Kolm põhjust, miks tunne mõõdikuks ei sobi:

1. **Igaüks vaatab teisi juhtumeid** — rahul töötaja nägi just neid kirju, mis läksid hästi.
2. **Mudel on kõikuv** — sama sisend ei anna alati täpselt sama väljundit, seega üks õnnestunud katse tähendab veel vähe.
3. **Miski ei hoiata halvenemise eest** — pakkuja uuendab mudeleid taustal ja päris sisendid muutuvad; kvaliteet võib langeda nii rahulikult, et enne märkab klient.

Lahendus on see, mille [2.1](../02-praktika/01-hea-prompt.md) juba õpetas ühe prompti juures: aktsepteerimiskriteerium (kokkulepitud nõue, mille täitmisel tulemus läbi pääseb) ja testid — siin skaleeritakse need kogu süsteemi hindamiseks: numbrid enne ja pärast iga muudatust.

Mõõtmine pole tehniline detail, vaid viis otsustada ilma vaidluseta.

## Kuldstandard ja testikomplekt

Kaks mõistet, mida siit edasi pidevalt kasutad:

- **Testikomplekt** on eksam: kindel hulk juhtumeid, mis käivad läbi alati samamoodi, iga muudatuse järel.
- **Kuldstandard** on võtmeleht: iga juhtumi juures on kirjas, milline vastus või otsus on õige — ja kinnitab seda inimene, mitte mudel.

Hindamine tähendab sisuliselt: jooksuta testikomplekt läbi ja võrdle iga vastust kuldstandardiga. Ehitada käib nii — üks põhjalik päev, mitte eraldi projekt:

1. **Kogu 30–50 päris juhtumit.** Vanad juhtumid (andmed anonümiseeritult) on jõulised — nad pärinevad reaalsusest, mitte ettekujutusest. Vali neid **erinevatest tüüpidest**: tüüpiline; piiritagune, kus asjatundja ise kahleb; andmetega vaene ja erandlik; ning üks, kus on proovitud süsteemi eksitada.
2. **Kirjuta iga juhtumi juurde õige vastus või otsus** — klassifitseerija puhul õige liik, kokkuvõtte puhul faktide nimekiri, mida vastus sisaldama peab.
3. **Lase asjatundjal kinnitada.** Kuldstandard on kulda ainult siis, kui keegi, kes asja tunneb, on „õiged“ vastused üle vaadanud.
4. **Hoia komplekti elavana.** Iga tootmises tabatud viga läheb testikomplekti uue juhtumina — komplekt kasvab koos teadmistega.

> **Lihtsalt öeldes:** testikomplekt on eksam, kuldstandard on õpetaja kinnitatud võtmeleht. Kvaliteet pole „mulle meeldib“ — see on arv: mitu küsimust neljakümnest vastati võtmelehe järgi.

## Mida mõõta: põhimõõdikud

Kuus mõõdikut (ingl k *metric* — numbrina väljendatav mõõtmisühik) annab enamikus süsteemides piisava pildi:

| Mõõdik | Mis mõõdab | Näide |
|---|---|---|
| Täpsus | mitu % vastustest on õige | 38/40 liiki õige = 95% |
| Klassifitseerimise eksimused | mis liiki ja kuhu eksitakse | 2× „kaebus“ → „arusaamatu kiri“ |
| Formaadi õigsus | kas vastus on kokkulepitud, masinloetavas vormis | JSON kehtib, väljad olemas (vt [2.2](../02-praktika/02-struktureeritud-valjund.md)) |
| Latentsus (ingl k *latency*) | kui kaua sisendist vastuseni kulub | keskmine 4 s, p95 12 s (vt [4.7](07-joudlus-ja-latentsus.md)) |
| Kulu | kui palju ühe juhtumi töötlemine maksab | 0,004 € / kiri (vt [3.6](../03-susteemi-ulesehitus/06-kulude-haldamine.md)) |
| Inimese sekkumise määr | mitu % juhtumitest läks inimesele | 12%, siht alla 15% |

Kaks selgitust:

- **Klassifitseerimise eksimused.** Segadusmaatriksi (ingl k *confusion matrix* — eksimuste tabel õigete ja pakutud liikide ristina) lihtne idee: tabel, kus read on õiged liigid, veerud pakutud liigid ja diagonaalil õiged vastused — ilma selleta parandad pimesi, sest täpsus võib jääda samaks, aga vead on kolinud olulisemasse liiki.
- **Inimese sekkumise määr.** Liiga kõrge number tähendab, et automatiseerimine ei tööta — inimene teeb ise töö, mille süsteem pidi ära tegema. Liiga madal pole aga alati võit: võib-olla on liiga palju otsustamist süsteemi sisse jäädvustatud ja riskantsed juhtumid jäävad läbivaatamata. Sobiv number sõltub vea hinnast (vt [1.2](../01-alused/02-voimalused-ja-piirid.md)).

**Kes hindab?** Kolm kihti:

1. **Automaatsed kontrollid** — formaat ja reeglid: kas JSON kehtib, kas väljad olemas, kas tellimata lisad puuduvad. Odav ja igal jooksul kaasas.
2. **Inimese näidisvalik** — näiteks 20 vastust nädalas läbi lugeda; see on kontrollija (vt [1.6](../01-alused/06-rollid-ja-vastutus.md)) tavatöö: valim üle vaadata, vead märkida ja testikomplekti viia. Kui süsteem jääb inimese kinnitusahelasse (ingl k *human-in-the-loop* — inimene kinnitab tulemuse enne kasutust), saab valimi võtta otse kontrollitööst.
3. **Mudel hindajana** — suurem mudel võib hinnata väiksema mudeli vastuseid („kas reegleid järgiti?“), aga ka hindaja mudel eksib: tema hinnang pole kunagi lõppotsus.

Hindamine on plaanitud test teadaolevatel sisenditel; seda, mis tootmises pidevalt juhtub, jälgib monitooring ([4.6](06-monitooring.md)) — mõlemat on vaja, aga see pole sama asi.

## Regressioon: muudatuste hindamine

Regressioon on olukord, kus juhtum, mis enne õigesti läbis, kukub pärast muudatust läbi. See on kõige õelatum risk, sest muudatus tehakse alati hea eesmärgiga: keegi parandab juhtumit X ega märka, et juhtum Y läks vahepeal vigaseks.

Seepärast üks kindel reegel: **iga muudatuse järel — uus prompt, uus juhis, uus mudel või uus mudeli versioon — jooksuta testikomplekt läbi enne kasutuselevõttu** ja pane tulemus tabelisse:

| Muudatus | Enne | Pärast | Otsus |
|---|---|---|---|
| juhisele lisati kaks näidet | 86% | 94% | võetakse kasutusele |
| prompt „paremaks“ ümber sõnastatud | 94% | 90% | tagasi lükatud — regressioon |
| uus mudeli versioon | 90% | 90% | ei võeta: ei paranda midagi, aga on kallim (3.6) |

Kolm hetke, millal hinnata:

1. **Enne käikuandmist — täielik läbijooks:** süsteem ei elustu enne, kui kogu testikomplekt läbib aktsepteerimiskriteeriumid ([2.5](../02-praktika/05-esimene-workflow.md) näitas, kuidas; 2.5 seitsme juhtumiga proovisõit kontrollis voo käiku; siin on komplekt suurem, sest mõõdame täpsust arvuliselt).
2. **Iga olulise muudatuse järel — regressioonitest:** enne muudatust mõõdame, pärast mõõdame uuesti ja otsustame numbrite järgi.
3. **Perioodiliselt — näidisvalik reaalsetest juhtumitest:** näiteks kord kuus vaata juhuslik valik üle — reaalsus muutub ka siis, kui sinu süsteem ei muutu.

> **Lihtsalt öeldes:** iga muudatus parandab alati midagi ja võib kogemata lõhkuda midagi muud. Testikomplekt on foto enne ja foto pärast — kui mõni varem hea asi on uuel fotol rikkis, näed seda tabelis, mitte kliendi meilis.

Täielik protseduur on omaette teema — vt [5.5](../05-suurte-projektide-tase/05-mudelite-vahetamine.md); tuum on lihtne: enne vahetust jooksuta testikomplekt läbi, pärast uuesti.

## Näide samm-sammult: uus mudel vanale klassifitseerijale

Kodutoa e-poe tagastusvoog ([2.5](../02-praktika/05-esimene-workflow.md)) on kasvanud: kolme liigi asemel eristab klassifitseerija nüüd 8 liiki — „tagastus“, „info“, „kaebus“, „juriidiline nõue“, „tellimuse muutmine“, „aadressi muutmine“, „arve küsimus“ ja „arusaamatu kiri“; kaebused ja juriidilised nõuded lähevad tähtsuse märgiga otse Piretile. Mudeli pakkuja teatab uue versiooni — käime hindamise läbi.

**1. samm — kuldstandard.** 40 päris kirja (andmed anonümiseeritult): igast 8 liigist 5, iga kirja juures õige liik kirjas ja kontrollija kinnitatud.

**2. samm — vana mudeli baastulemus.** 40 kirja läbi jooksutatult on kuldstandardiga võrreldes **37/40 õige (92,5%)**. Eksimused:

| Eksimus | Mitu | Tagajärg |
|---|---|---|
| „kaebus“ → „arusaamatu kiri“ | 2 | kaebus jõuab Pireti juurde küll, aga ilma tähtsuse märgita |
| „tagastus“ → „info“ | 1 | klient saab infokavandi tagastuskavandi asemel |

**3. samm — uus mudel ja regressioon.** Uus versioon: **38/40 (95%)** — aga eksimuste tabel näitab seda, mida kokkuvõttev arv varjab:

| Eksimus | Mitu | Märkus |
|---|---|---|
| „kaebus“ → „info“ | 1 | **REGRESSIOON** — see kiri oli vana mudelil õige |
| „tagastus“ → „info“ | 1 | sama viga jäi alles |

| Muudatus | Enne | Pärast | Otsus |
|---|---|---|---|
| uus mudeli versioon | 37/40 | 38/40 | võetakse kasutusele; liigi „kaebus“ juhiseid täpsustatakse |

Üks kiri, mis varem jõudis õigesti kaebuste loendisse, sai nüüd infokavandi — regressioon on reaalne. Otsus: uus mudel jääb (95% vs 92,5%), kaebuse juhiseid täpsustati selgemate näidetega ja testikomplekt jooksutati uuesti: 39/40. Jäänud „tagastus → info“ eksimus läks kirja kui silmas hoitav juhtum.

**4. samm — ülejäänud mõõdikud.** Formaadi kontroll: 40/40 kehtivat JSON-i kõigi väljadega — **100%**. Inimese sekkumise määr esimese nädala päris kasutuses: **12%**, siht oli alla 15% — süsteem teeb tööd ja Piret peab käsitsi läbi umbes iga kaheksanda kirja.

## Kokkuvõte

- **„Tundub hea“ pole mõõdik** — kvaliteet peab olema arvuleval ja mõõtmine korratav: sama meetod, samad juhtumid, numbrid enne ja pärast.
- **Testikomplekt on sisendite kogu, kuldstandard on inimese kinnitatud õiged vastused** — 30–50 päris juhtumit erinevatest tüüpidest on hea algus; iga tabatud viga läheb komplekti juurde.
- **Kuus põhimõõdikut:** täpsus, eksimuste liik, formaat, latentsus, kulu ja inimese sekkumise määr — viimase mõlemad äärmused on hoiatused.
- **Iga muudatuse järel jooksuta testikomplekt läbi** — regressioon tabatakse tabelis (muudatus | enne | pärast | otsus), mitte kliendi kirjades.
- **Kes ja millal hindab:** automaat igal jooksul, inimese näidisvalik perioodiliselt, hindajaks pandud mudel ainult abivahendina; hindamine toimub enne käikuandmist, iga olulise muudatuse järel ja kord kuus.

## Mis edasi?

- eelmine → [4.4 Mitme agendi arhitektuurid](04-mitme-agendi-arhitektuurid.md)
- järgmine → [4.6 Monitooring tootmises](06-monitooring.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
