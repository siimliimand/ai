# 5.8 Mallide teek: kontroll-loendid ja näidised

> **Sihtpublik:** kõik | **Eeltingimused:** kogu käsiraamat (ükski dokument ei ole kohustuslik eeltingimus — see on teek, mida kasutada vajadusel)

## Mis see teek on ja kuidas kasutada

See on käsiraamatu viimane dokument — ja sisuliselt tööriistakast. Viies tasand õpetas, kuidas suured projektid korraldatud saavad; see teek koondab selle kopeeritavaks kasutamiseks: neli kontroll-loendit (ingl k *checklist* — kindel punktide nimekiri, mis käiakse läbi enne edasiminekut) ja neli malli, mis viitavad tagasi käsiraamatu õpetatule. Uut teooriat siin ei ole — ainult kokkuvõte kasutuskujul.

Kasutamine on kolm sammu:

1. **Võta mall.** Vali olukorra järgi: uus süsteem käiku → loend 1; prompti muudatus → loend 2; kuu läbi → loend 3; mudeli vahetus → loend 4; kirjapanek → mallid allpool.
2. **Täida.** Kopeeri enda dokumenti ja käi punktide läbi. Märge „☐“ tähendab „pole tehtud“ — tehtud punkti juurde pane kuupäev ja nimi.
3. **Pane kirja.** Täidetud loend ei ole ühekordne paber: ta jääb süsteemi kirjapaneku juurde tõendiks, mida, millal ja kes kontrollis.

> **Lihtsalt öeldes:** see teek on nagu kontroll-nimekiri enne lendu: piloot ei loe seda selleks, et ta ei oskaks, vaid selleks, et mitte ükski samm ei jääks meelest ära — ja et järellehes oleks kirjas, kes ja millal üle kontrollis.

## Kontroll-loend 1: uus süsteem enne käikuandmist

Sünniprotokoll (uue süsteemi enne käikuandmist läbitav kontroll, [5.2](02-meeskonnatoo-ja-standardid.md)): enne, kui esimene tulemus kliendini jõuab, peab iga punkt olema märgitud.

- ☐ Ülesanne on läbinud otsustusmaski — teada, miks just see ülesanne AI-le sobib ja kus inimene asendamatuks jääb → [1.2 Võimalused ja piirid](../01-alused/02-voimalused-ja-piirid.md)
- ☐ Ohutuse test kirjalikult läbi: mis on halvim asi, mida süsteem teha saab, ja milline kaitsekiht (ingl k *safeguard* — disainis olev piir, mitte lootus tubli mudelile) selle ära hoiab → [3.5 Ohutus](../03-susteemi-ulesehitus/05-ohutus.md)
- ☐ Inimene kinnitusahelas (ingl k *human-in-the-loop* — töövoog, kus inimene kinnitab tulemuse enne kasutust): kinnitus- ja peatumiskohad kirjas → [3.5 Ohutus](../03-susteemi-ulesehitus/05-ohutus.md)
- ☐ Kuldstandard (ingl k *golden set* — inimese kinnitatud õiged vastused) valmis, asjatundja kinnitanud, süsteemi tulemused tabelisse kirja → [4.5 Hindamine](../04-agendid-ja-mootmine/05-hindamine.md)
- ☐ Kulu hinnang tehtud ja hoiatuskünnis (ingl k *alert threshold* — kulu piir, mille ületamisest tuleb teade) seatud → [3.6 Kulude haldamine](../03-susteemi-ulesehitus/06-kulude-haldamine.md)
- ☐ Turvalisus üle: API-võtmed kaitstud, andmete ligipääs piiratud, prompt injection tõkestatud → [3.7 Turvalisus](../03-susteemi-ulesehitus/07-turvalisus.md)
- ☐ Arhitektuurileht: süsteemi osad ja sõltuvused ühel lehel → [5.1 Arhitektuur suures mahus](01-arhitektuur-suures-mahus.md)
- ☐ Omanik (ingl k *owner* — süsteemi/prompti vastutaja, üks nimi) määratud ja kirja pandud → [5.2 Meeskonnatöö ja standardid](02-meeskonnatoo-ja-standardid.md)
- ☐ Monitooringuks valmis: mõõdikud ja konkreetsed hoiatused püsti, teave läheb ühele inimesele → [4.6 Monitooring](../04-agendid-ja-mootmine/06-monitooring.md)
- ☐ Promptipanga kirje olemas (mall allpool) → [2.6 Promptide haldus](../02-praktika/06-promptide-haldus.md)

Reegel on lihtne: kui mõni punkt jääb tühjaks, süsteem käiku ei saa — esimene kliendile minev tulemus ei tohi olla ühtlasi esimene test.

## Kontroll-loend 2: prompti muudatus tootmises

Prompt tootmises pole fail, mida korrigeeritakse käigu pealt — muudatus on tükk tööd, mis läbib alati sama vood ([2.6](../02-praktika/06-promptide-haldus.md), [2.1](../02-praktika/01-hea-prompt.md), [5.2](02-meeskonnatoo-ja-standardid.md)).

- ☐ Muudatuse vajadus kirjas: mida muudetakse ja miks (logisse, mitte mällu)
- ☐ Testjuhtumid valmis: testikomplektist piisav valik, sealhulgas piiritagused juhtumid → [4.5 Hindamine](../04-agendid-ja-mootmine/05-hindamine.md)
- ☐ Kuldstandardiga võrdlus: numbrid enne ja pärast tabelisse — „tundub parem“ ei ole tulemus → [4.5 Hindamine](../04-agendid-ja-mootmine/05-hindamine.md)
- ☐ Teine ülevaatus: veel üks inimene on muudatuse üle vaadanud → [5.2 Meeskonnatöö ja standardid](02-meeskonnatoo-ja-standardid.md)
- ☐ Kinnitus: ainult prompti omanik või nimetatud asendaja annab tootmisse → [5.2 Meeskonnatöö ja standardid](02-meeskonnatoo-ja-standardid.md)
- ☐ Pank: uus versiooninumber, muudatuse põhjus kirjas, vana versioon arhiivi → [2.6 Promptide haldus](../02-praktika/06-promptide-haldus.md)

## Kontroll-loend 3: kuine ülevaatus

Kord kuus — 30 minutit tabeli täitmiseks; kogu tsükkel koos aruteluga 1–2 tundi, kus monitooringu numbrid muutuvad otsusteks (rutiinist [4.6](../04-agendid-ja-mootmine/06-monitooring.md), tsüklist [5.6](06-pidev-taiustamine.md)). Täida tabel:

| Näitaja | Eelmine kuu | See kuu | Märkus |
|---|---|---|---|
| maht (päringud päevas) | | | |
| veamäär | | | |
| inimese sekkumise määr | | | |
| latentsus | | | |
| kulu | | | |
| erijuhud (varuteele/inimesele) | | | |

Mõõdikute selgitused ja hoiatusmärgid leiab [4.6 Monitooring](../04-agendid-ja-mootmine/06-monitooring.md). Seejärel kolm küsimust:

1. **Millised näitajad liikusid ja miks?** Järsk muutus ilma põhjuseta on uurimise väärt — ka siis, kui see on heas suunas.
2. **Mis läks inimesele ja miks?** Korduv eksimus on juhise viga, mitte halb õnn → [4.6 Monitooring](../04-agendid-ja-mootmine/06-monitooring.md).
3. **Mida teen sellega kuu jooksul?** Iga tabatud viga läheb testikomplekti ([4.5](../04-agendid-ja-mootmine/05-hindamine.md)), iga uus kirjatüüp näidena prompti juurde ([2.6](../02-praktika/06-promptide-haldus.md)) — nii käib mõõtmine otsuste, mitte pelga numbrite kogumise üle ([5.6](06-pidev-taiustamine.md)).

## Kontroll-loend 4: mudeli vahetus

Kuus sammu lühidalt ([5.5 Mudelite vahetamine](05-mudelite-vahetamine.md) — kehtib nii plaanitud vahetuse kui tahtmatu muutuse puhul):

- ☐ Sõltuvused maha märkitud: kes kasutab vana mudelit ja keda vahetus puudutab → [5.1 Arhitektuur suures mahus](01-arhitektuur-suures-mahus.md)
- ☐ Kuldstandard läbi uue mudeliga, tulemused tabelisse → [4.5 Hindamine](../04-agendid-ja-mootmine/05-hindamine.md)
- ☐ Tulemused võrreldud: täpsus, latentsus, kulu — enne ja pärast → [4.5 Hindamine](../04-agendid-ja-mootmine/05-hindamine.md)
- ☐ Prompte täpsustatud: kui uus mudel mõnes liigis nõrgem, lisatud näited → [2.1 Head promptini](../02-praktika/01-hea-prompt.md)
- ☐ Üle test-keskkonnas, siis tootmisse → [5.1 Arhitektuur suures mahus](01-arhitektuur-suures-mahus.md)
- ☐ Esimesel nädalal järelvaadatud: numbrid päris kasutuses, mitte ainult testis → [4.6 Monitooring](../04-agendid-ja-mootmine/06-monitooring.md)

> **Lihtsalt öeldes:** vahetus on nagu kolimine — loendad asjad ära, proovid mööbli ära, võrdled arveid ja käid nädala peale vaatamas, kas kõik saab uues kohas hakkama.

## Mall: promptipanga kirje

Iga prompti juures pankas kuus põhiandmet + viide testimisele ([2.6](../02-praktika/06-promptide-haldus.md) mall on kuueosaline, teek lisab testimise viite); täida:

```text
PROMPTIPANGA KIRJE
------------------
Nimi:              klientkiri-klassifitseerija
Eesmärk:           Sorteerib kliendikirjad liikide kaupa: tagastus, info, muu (vt 2.5 ja 4.5)
Kasutaja:          e-poo klienditoe workflow (samm 1), igapäev
Viimati testitud:  2026-10-01 — kuldstandard 40/40
Olek:              kasutusel  (teised väärtused: testimisel | arhiivis)
Versioon:          v3 (2026-09-28 — lisatud näited kampaaniatellimuse kohta)
Viide testimisele: 4.5 testikomplekt, tabel „klassifitseerija v3 tulemused“
```

Kuupäev „viimati testitud“ on tähtsaim rida: ta ütleb teadmise vanuse.

## Mall: otsuste tabel ja riskiregister

Kui standardid kasvavad ettevõtte laiuseks, on tegu governance (süsteemi juhtimise reeglid) küsimusega ([5.7](07-vastutus-ja-eetika.md)). Kaks malli alguseks:

```text
OTSUSTE TABEL
-------------
| Otsus                        | Kes teeb            | Kes kinnitab      | Kus kirjas           |
|------------------------------|---------------------|-------------------|----------------------|
| Uue süsteemi käikuandmine    | tellija + ehitaja   | juht              | sünniprotokolli leht |
| Prompti muudatus tootmises   | ehitaja             | prompti omanik    | promptipanga logi    |
| Mudeli vahetus               | haldaja             | omanik            | otsuste logi         |
| Hoiatuskünnise muutmine      | haldaja             | süsteemi omanik   | arhitektuurileht     |
```

```text
RISKIREGISTER
-------------
| Risk                                    | Tõenäosus | Mõju     | Kes jälgib |
|-----------------------------------------|-----------|----------|------------|
| Pakkuja muudab mudelit ilma teateta     | keskmine  | kõrge    | haldaja    |
| Kulud kasvavad mahu kasvust kiiremini   | keskmine  | keskmine | omanik     |
| Prompt injection läbi kasutaja sisendi  | madal     | kõrge    | omanik     |
```

Reegel mõlemale: iga rea juures üks nimi, mitte komisjon — muidu vastutus jääb vaikimisi kellelegi ([5.7](07-vastutus-ja-eetika.md)).

## Mall: otsuse protokoll

Iga olulisel otsusel kirjalik protokoll ([5.7](07-vastutus-ja-eetika.md)): mis, miks, kes, millal.

```text
OTSUSE PROTOKOLL
----------------
Mis otsustati:  [otsus]
Miks:           [põhjendus, mis andmetel]
Kes otsustas:   [nimi, roll]
Millal:         [kuupäev]
```

## Lõpusõna: käsiraamat on läbi — mis edasi?

Sellega on „AI-automatiseerimise käsiraamat“ — 34 dokumenti viies tasandis — läbi. Kõige parem järgmine samm pole juurdelugemine, vaid ühe teenuse valimine ja läbi ehitamine: võta üks tüütu, korduv töö, pane see läbi otsustusmaski ([1.2](../01-alused/02-voimalused-ja-piirid.md)) ja ehitada esimene töövoog algusest lõpuni ([2.5](../02-praktika/05-esimene-workflow.md)). Edasi kasvatab süsteemi alles siis, kui esimene töötab ja on mõõdetud — tasandid peavad järjestuses, sest iga järgmine eeldab eelmist. Kui käigu peal midagi äpardub, on see teek seal selleks: loend püsti, mall täis, tulemus kirjas. Kogu käsiraamatu juurde pääseb [README index](../README.md) kaudu, ja esimene samm alati [1.1 Mis on AI-mudel ja kuidas ta „mõtleb“](../01-alused/01-mis-on-ai-mudel.md)-ist.

---

- eelmine → [5.7 Vastutus, eetika ja governance](07-vastutus-ja-eetika.md)
- alusta otsast → [1.1 Mis on AI-mudel ja kuidas ta „mõtleb“](../01-alused/01-mis-on-ai-mudel.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
