# 5.5 Mudelite vahetamine ja drift

> **Sihtpublik:** tehniline + juhtiv mitte-tehniline | **Eeltingimused:** [5.4 Kulustrateegia suures mahus](04-kulustrateegia.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- eristada kaht muutuse liiki: plaanitud mudeli vahetus (uue mudeli võtmine kasutusele) ja tahtmatu muutus (pakkuja uuendab mudelit sinu all);
- selgitada, mis on drift (ingl k *model drift* — mudeli käitumise nihkumine ajas ilma teavitamata: sama sisend, veidi erinev vastus) ja miks suures süsteemis puudutab üks mudelipuudutus mitut kohta korraga;
- käia vahetuse läbi kuues sammus — sõltuvuste lehelt järelmonitooringuni;
- kaitsta süsteemi tahtmatu muutuse vastu: kinnitatud mudeli versioon (kinnitatud konkreetne kuju, nt v3), monitooringu hoiatused ja kord kuus tehtav proovijooks;
- öelda juhile, miks vahetus on numbritega toetatud protsess, mitte nupuvajutus.

## Lihtsalt öeldes

> Sinu automatiseerimine jookseb teise ettevõtte mudelil ja see mudel ei jää paigale. Kord teatab pakkuja ette: „uus versioon tuleb, vana läheb“ — siis valid sina hetke ja käid vahetuse läbi testidega. Teinekord muudab pakkuja midagi vaikselt — ja ainus, kes seda märkab, on sinu monitooring. Mõlemal juhul on reegel sama: tea, kes millist mudelit kasutab, jooksuta enne vahetust testid läbi ja jälgi numbreid pärast. Mudeli vahetus on kuuesammuline protsess.

## Kaks muutust: plaanitud vahetus ja tahtmatu muutus

**Plaanitud vahetus** on sinu otsus. Võtad kasutusele uue mudeli, sest see on odavam ([5.4](04-kulustrateegia.md)), kiirem või võimekam. Kuupäeva valid sina, testid jooksutad sina ja otsused tulevad numbrite järgi ([4.5](../04-agendid-ja-mootmine/05-hindamine.md)).

**Tahtmatu muutus** on pakkuja otsus: ta uuendab mudelit sinu all. [4.6](../04-agendid-ja-mootmine/06-monitooring.md) hoiatusmärk „pakkuja muudab mudeleid“ tähistab just seda — vastuse kiirus, keel või vorm võib nihkuda ilma et sina midagi klõpsiksid. E-poo nägi seda esimesel kuul: pakkuja muutis öösel vastuseaegu ja veamäär hüppas 2%-lt 14%-ni.

Kolmas, vaikseim liik on **drift**: üksik juhtum pole viga, aga nihked liituvad nädalate ja kuudega ning kvaliteet langeb nii rahulikult, et enne märkab klient.

Miks see suures süsteemis tõsine on:

- **Sõltuvus** (milline süsteem millist mudelit kasutab) on suures süsteemis pikk nimekiri — sama mudelit kasutavad kümned voolud, agendid ja mitme meeskonna tööd. Ühe mudeli vahetus mõjutab kõiki kasutajaid korraga — ja kui arhitektuurileht ([5.1](01-arhitektuur-suures-mahus.md)) seda nimekirja ei peeta, ei teagi, keda teavitada ja keda testima panna.
- **Promptid on testitud konkreetsele mudelile.** [Promptipank](../02-praktika/06-promptide-haldus.md) hoiab iga prompti juures kirjas „viimati testitud“ ja versiooni — aga see test tehti teatud mudeli versiooniga. Vaheta mudel ja kogu pank on testimata: näited ja reeglid, mis ühe mudeliga töötasid, ei kanna teisele üle.

| Muutuse liik | Kelle otsus | Kui kiire | Kuidas märkad |
|---|---|---|---|
| Plaanitud vahetus | sinu | sinu valitud ajal | testide arvud enne ja pärast |
| Tahtmatu uuendus | pakkuja | üleöö | monitooringu hoiatus |
| Drift | keegi, vaikselt | nädalate, kuudega | sekkumise määra vaikne tõus |

> **Lihtsalt öeldes:** plaanitud vahetus on sinu kolimine — pakid ise ja valid päeva. Tahtmatu muutus on naabri remont sinu seinas — teavitust ei tule, ainus abinõu on mõõta. Drift on maja, mis vajub sentimeetri võrra aastas: ükski päev pole kriitiline, aga kümme aastat on.

## Vahetusprotsess kuues sammus

Protsess kehtib mõlemal liigil: plaanitud vahetuse puhul valid sina aja, tahtmatu uuenduse puhul paneb pakkuja tähtaja.

| # | Samm | Tegevus | Tööriist |
|---|---|---|---|
| 1 | Sõltuvused maha märkida | kes kasutab vana mudelit ja keda vahetus puudutab | [5.1](01-arhitektuur-suures-mahus.md) arhitektuurileht |
| 2 | Kuldstandard läbi uue mudeliga | testikomplekt uue versiooniga, tulemus tabelisse | [4.5](../04-agendid-ja-mootmine/05-hindamine.md) regressioon |
| 3 | Tulemused võrrelda | täpsus, latentsus, kulu — enne ja pärast | 4.5 kuus mõõdikut |
| 4 | Prompte täpsustada | kui uus mudel mõnes liigis nõrgem, lisa näited | [2.1](../02-praktika/01-hea-prompt.md) mallid |
| 5 | Üle test-keskkonnas, siis tootmisse | käik läbi eraldatud keskkonnas enne päris kliente | [5.1](01-arhitektuur-suures-mahus.md) keskkonnad |
| 6 | Esimesel nädalal järelvaadata | numbrid päris kasutuses, mitte ainult testis | [4.6](../04-agendid-ja-mootmine/06-monitooring.md) monitooring |

Kaks sammu väärivad eraldi lauset.

**2.–3. samm on südamik.** Kuldstandard (ingl k *golden set* — inimese kinnitatud õiged vastused, [4.5](../04-agendid-ja-mootmine/05-hindamine.md) põhimõte) annab võrdluse, mis pole arvamus. Regressioon (ingl k *regression* — varem töötanu läbikukkumine) peitub sageli eksimuste tabelis, mitte kokkuvõtvas arvus: üks liik paranes, teine läks rikkis. Otsus kirjutatakse nagu 4.5-s tabelisse: muudatus, enne, pärast ja otsus.

**6. samm on turvavõrk.** Test näitab, kuidas süsteem käitus 40 tuntud kirjaga; tootmine, kuidas ta käitub tuhandete päris kirjadega. Paigal sekkumise määraga ja ilma hoiatusteta on vahetus läbi — hoiatusega tead seda nädalaga, mitte kliendi kirjast.

> **Lihtsalt öeldes:** vahetus on nagu kolimine: loendad ära, kes kus töötab, proovid mööbli ära, võrdled arveid, parandad plaani, kolid esmalt varukontorisse ja käid nädala jooksul peale vaatamas, kas kõik saab uues kohas hakkama.

## Kaitse tahtmatu muutuse vastu

Plaanitud vahetust saab ajastada, tahtmatut mitte. Kolm kaitset teevad tahtmatu muutuse hallatavaks:

1. **Kinnita mudeli versioon, kui pakkuja lubab.** Päringus pole kirjas „anna uusimat“, vaid täpne näidik, nt „mudel-v3-2026-06“. Pakkuja ei saa vaikselt midagi vahetada — versioon jääb, kuni sina selle vahetad. Kaks piiri: kõik pakkujad seda ei luba ja vana versiooni tugi lõpeb niikuinii. Kinnitamine on aja ostmine, mitte igavene lahendus.
2. **Monitooringu hoiatused ([4.6](../04-agendid-ja-mootmine/06-monitooring.md)) on sinu varuandur.** Hoiatuskünnis (ingl k *alert threshold* — piir, mille ületamisel teavitus käivitub) arvul näitajal — nt „p95 latentsus üle 10 s“ või „sekkumise määr nädalas üle 20%“ — tabab pakkuja muudatuse isegi siis, kui sina pakkuja uudiskirja ei lugenud.
3. **Perioodiline proovijooks.** Kord kuus jooksuta kuldstandard läbi — isegi kui midagi pole muudetud. Kui tulemus hüppab 39/40-lt 36/40-le ilma su enda muudatuseta, on muutus toimunud sinu all. Boonusena hoiab see promptipanga kuupäeva „viimati testitud“ ([2.6](../02-praktika/06-promptide-haldus.md)) värskena.

Neljas kaitse on pikaaegne — **väljumisstrateegia**. Ära põle ühe pakkujaga: hoia alles abstraktsioon (pakkujatest sõltumatu kiht, [5.1](01-arhitektuur-suures-mahus.md) värava põhimõte) — mudeli nimi ja võti on süsteemis ühes kohas, ühe lausega asendatavad. Dokumenteeritud kuldstandard teeb pakuja vahetuse võimalikuks: testid jooksevad läbi igas suunas.

> **Lihtsalt öeldes:** kinnitatud versioon on kindluslukk — tavaline võti enam ei ava. Hoiatused on valvekell, mis heliseb, kui keegi ikkagi sisse pääseb. Proovijooks on kord kuus tehtav inventuur — see näitab, kas riiulid on paigas ka siis, kui keegi pole midagi puudutanud.

## Näide samm-sammult: e-poo klassifitseerija v2 → v3

E-poo klassifitseerija seis pärast [4.5](../04-agendid-ja-mootmine/05-hindamine.md): kaheksa liiki, kuldstandard 40 päris kirja, tootmises v2 tulemusega **39/40**, inimese sekkumise määr **12%** (siht alla 15%), keskmine latentsus **4 s** ja p95 **12 s**, kulu **0,004 €** kirja kohta. Siis tuleb pakujast kiri: v2 lahkub 60 päevaga, v3 võtab üle — tahtmatu muutus, mis sunnib plaanitud vahetuse ette võtma.

Kuus sammu:

| # | Samm | E-poo juures |
|---|---|---|
| 1 | Sõltuvused | arhitektuurileht: klassifitseerija kasutab v2 — üks süsteem, kasutajaks Piret ja klienditeeninduse meeskond |
| 2 | Kuldstandard | 40 kirja läbi v3: **38/40** — eksimuste profiil vahetus |
| 3 | Võrdlus | latentsus **−40%**, kulu **−15%**, täpsus alguses madalam |
| 4 | Prompti täpsustus | „kaebus“ juhisele lisati kaks näidet → uus jooks **40/40** |
| 5 | Keskkonnad | nädal test-keskkonnas, siis tootmisse |
| 6 | Järelmonitooring | nädal tootmises: sekkumise määr püsis **12%** |

Eksimuste tabel (2. samm) näitab seda, mida number 38/40 varjab:

| Eksimus | v2 | v3 | Tagajärg |
|---|---|---|---|
| „tagastus“ → „info“ | 1 | 0 | 4.5 silmas hoitud juhtum paranes |
| „kaebus“ → „info“ / „arusaamatu kiri“ | 0 | 2 | **regressioon** — kaebused jäävad tähtsuse märketa |

Uus mudel on „tagastuse“ jaoks parem, „kaebuse“ jaoks nõrgem. Regressioon parandatakse 4. sammuga: kaebuse juhised said kaks selget näidet (2.1 mall), uus jooks andis 40/40 ja prompt sai panka uue versiooni kirjega, mida muudeti, miks ja kes kinnitas. Otsus numbritega:

| Muudatus | Täpsus | Latentsus | Kulu | Otsus |
|---|---|---|---|---|
| v2 → v3 + prompti täpsustus | 39/40 → 40/40 | keskmine 4 s → 2,4 s; p95 12 s → ~7 s | 0,004 → 0,0034 €/kiri | võetakse kasutusele |

**Ja siis tahtmatu muutus.** Kolm kuud hiljem käivitus hoiatus: p95 latentsus, mis oli tootmises veel rahunenud ~6 sekundini (kiirem kui testinädala ~7 s), hüppas üleöö **9 sekundini** — ilma et keegi midagi muutis. Kinnitatud versiooninumber polnud muutunud, kuldstandard andis ikka 40/40 — ainult aeg oli pikenenud. Pakkujalt küsiti — selgus, et osa v3 liiklusest oli öösel üle kolitud teisele taristule. Lahendus: süsteemi kinnitati täpse versiooninäidiku „mudel-v3-2026-06“ juurde ja pakkuja lubab edaspidi taristumuudatustest ette teatada; hoiatuskünnis jäi püsti. Nädalaga oli p95 tagasi ~6 s juures.

> **Lihtsalt öeldes:** plaanitud vahetus võttis kaks päeva tööd ja tõi 15% kulusäästu — sest testid olid juba olemas. Tahtmatu muutus maksis ühe hoiatuse ja ühe küsimuse pakkujale — sest valvekell helises. Mõlemad jäid väikesteks, sest kaitse oli ehitatud enne, mitte pärast.

## Kokkuvõte

- **Kaks muutuse liiki:** plaanitud vahetus on sinu otsus ja sinu ajastus; tahtmatu muutus on pakkuja oma — 4.6 hoiatusmärk „pakkuja muudab mudeleid“. Drift liitub vaikselt.
- **Vahetus puudutab kogu süsteemi:** sõltuvus (milline süsteem millist mudelit kasutab) seisab arhitektuurilehel ja promptid on testitud konkreetse mudeli versiooniga — mudeli vahetus muudab mõlemat korraga.
- **Kuus sammu:** sõltuvused → kuldstandard läbi → võrdlus (täpsus, latentsus, kulu) → promptide täpsustus → test-keskkond ja tootmine → nädal monitooringut.
- **Kolm kaitset tahtmatu muutuse vastu:** kinnitatud mudeli versioon, hoiatused hoiatuskünnisega ja kord kuus tehtav proovijooks.
- **Väljumisstrateegia:** abstraktsioon (pakkujatest sõltumatu kiht, 5.1 värava põhimõte) ja dokumenteeritud kuldstandard teevad vahetuse võimalikuks — ka pakuja vahetuse.

## Mis edasi?

- eelmine → [5.4 Kulustrateegia suures mahus](04-kulustrateegia.md)
- järgmine → [5.6 Pidev täiustamine: mõõtmisest otsusteni](06-pidev-taiustamine.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
