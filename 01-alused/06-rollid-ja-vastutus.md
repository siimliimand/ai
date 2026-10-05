# 1.6 Rollid ja vastutus projektis

> **Sihtpublik:** kõik | **Eeltingimused:** [1.5 AI-automatiseeritud süsteemi anatoomia](05-susteemi-anatoomia.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- nimetada viis rolli, mis igas AI-automatiseerimise projektis (töö delegeerimine AI-mudelile) peavad olema olemas;
- öelda, milliseid otsuseid iga roll teeb ja mis juhtub, kui mõni roll täitmata jääb;
- paigutada need rollid nii väikeses kui suures meeskonnas;
- selgitada, miks vastutus AI vea eest jääb inimesele ja miks otsused tasub kirja panna.

## Lihtsalt öeldes

> AI-projektis on üks inimene, kes teab, mida vaja, üks, kes teeb, ja üks, kes kontrollib — nagu köögis: kellegi peab otsustama menüü, keegi peab valmistama ja keegi peab enne serveerimist maitse proovima. Ka siis, kui kõik kolm on üks ja sama kokk, rollid ära ei kao: menüüotsus, valmistamine ja maitsekontroll peavad ikkagi aset leidma. Ahel, kus keegi enne serveerimist ei maitsesta, pole efektiivne köök, vaid risk.

## Viis rolli, viis inimest (või vähem)

Dokument 1.5 jättis ühe küsimuse lahti: iga süsteemi osa eest peab keegi vastutama. Osad ise aga ei otsusta ega seadista — seda teevad inimesed.

### 1. Tellija või omanik — teab, mis probleemi lahendatakse

**Mida teeb.** Teab ärilist probleemi — kuhu aeg ja raha kaob — ja määrab, mis väärtust luuakse: mida automatiseeritakse, mida mitte ja mis piirides. Tehnikat ta tundma ei pea; äri ta tundma peab.

**Milliseid otsuseid teeb.** Mis probleemi lahendatakse ja mis on edu; kui suur on eelarve; kus peab inimene vahele seisma; kas süsteem käiku anda.

**Kui roll puudub.** Süsteem ehitatakse põhimõttel „midagi AI-ga, äkki kasu tuleb ise“ — lahendus otsib probleemi ja keegi ei kanna lõpuks otsuste eest vastutust.

### 2. Ehitaja ehk tehnik — paneb süsteemi kokku

**Mida teeb.** Seadistab süsteemi: seob osad töötavaks tervikuks ja hoolitseb, et tervik koos hoiaks. Programmeerija ta olla ei pea — no-code tööriistadega (visuaalsed keskkonnad, kus süsteemi kokku klikitakse) saab hakkama ka kontori IT-huvilane.

**Milliseid otsuseid teeb.** Milliseid tööriistu kasutatakse; mis juhtub, kui mudel ei vasta või tulemus ei läbi kontrolli (varutee); kus on süsteemi piirid.

**Kui roll puudub.** Esimese rikke või vajaliku muudatuse juures süsteem seisab: keegi ei mõista, kuidas osad kokku on pandud, ega julge midagi muuta.

### 3. Sisukujundaja — kirjutab, mis „hea“ tähendab

**Mida teeb.** Kirjutab prompti (ingl k *prompt* — mudelile antav juhis) ja sõnastab kriteeriumid, mis eristab hea tulemuse keskpärasest. See roll ei nõua tehnikat, vaid asjatundmist: parim sisukujundaja on tihtipeale just valdkonda tundev inimene.

**Milliseid otsuseid teeb.** Mis on „hea tulemus“ selles ettevõttes; millised näited juhiste juurde panna; mis reeglid kehtivad — toon, keel, mida süsteem ei tohi kunagi teha.

**Kui roll puudub.** Kõik otsused jäävad mudelile — tulemus on keskpärane ja iga korraga natuke teistsugune. Kõik tunnevad, et „midagi ei sobi“, aga keegi ei oska öelda, mida parandada.

### 4. Kontrollija — inimene kinnitusahelas

**Mida teeb.** Vaatab tulemused üle enne, kui need jõuavad kliendi, koostööpartneri või teise süsteemini — ja peab saama öelda „ei“ ning töö tagasi saata. See on inimene kinnitusahelas (ingl k *human-in-the-loop* — töövoog, kus inimene kinnitab tulemuse enne kasutust).

**Milliseid otsuseid teeb.** Kas see konkreetne tulemus läheb edasi; mis läheb käsitsi töötlusse; millal on vigu nii palju, et süsteem tuleb ajutiselt seisma panna.

**Kui roll puudub.** Viga jõuab kliendini enne, kui keegi seda näeb — hallutsinatsioon (mudeli kindlalt öeldud, aga vale vastus) muutub välja saadetud kirjaks või süsteemi kantud numbriks. Kuidas kinnitusahelat ja piire süsteemis üles ehitada, õpetab põhjalikumalt [3.5 Ohutus: piirid ja inimene kinnitusahelas](../03-susteemi-ulesehitus/05-ohutus.md).

### 5. Haldaja — hoiab süsteemi käigus pärast käikuandmist

**Mida teeb.** Jälgib, et süsteem töötaks ka siis, kui projekt on ammu lõppenud: kulud, kvaliteet ja muudatused — ka tööriistades ja mudelites, mida keegi tema teadmata uuendab.

**Milliseid otsuseid teeb.** Kas süsteem töötab veel nii hästi, nagu lubatud; millal sekkuda; millal osa välja vahetada või süsteemi maha võtta.

**Kui roll puudub.** Süsteem laguneb vaikselt: kulud roomavad üles, kvaliteet libiseb ja keegi märkab seda alles siis, kui klient kaebab.

Ühe reaga:

| Roll | Küsimus, millele ta vastab |
|---|---|
| Tellija / omanik | Miks ja mille jaoks? |
| Ehitaja / tehnik | Kuidas tehniliselt teha? |
| Sisukujundaja | Mis on hea tulemus? |
| Kontrollija | Kas see konkreetne tulemus läheb edasi? |
| Haldaja | Kas see töötab ka homme? |

## Väikeettevõte vs suurem meeskond

> **Lihtsalt öeldes:** rollid ei ole ametikohad, vaid vastutused. Väikeses firmas kannab üks inimene korraga mitut mütsi — aga mütsid ei kao: iga rolli peab keegi ikkagi täitma. Suures meeskonnas saab iga müts oma pea.

Kolmeinimeselises ettevõttes: omanik on korraga tellija ja haldaja (ta otsustab ja jälgib kulusid-kvaliteeti), tehnikast huvitatud töötaja on ehitaja, kõige kogenenum spetsialist on sisukujundaja — ja kõik on vahetevahel kontrollijad.

Suuremas meeskonnas jagunevad rollid ametikohtadeks: tooteomanik (tellija), arendaja (ehitaja), sisukujundaja ehk promptide eest vastutav inimene, kvaliteedikontrolli vastutaja ja operatsioonide haldaja. Mida rohkem inimesi, seda tähtsamad kokkulepped, kes tohib kelle prompti muuta ja kes kinnitab — meeskonna standarditest räägib [5.2 Meeskonnatöö ja standardid](../05-suurte-projektide-tase/02-meeskonnatoo-ja-standardid.md).

## Kes vastutab, kui AI eksib?

> **Lihtsalt öeldes:** vabanduse, mida klient kuuleb, ei ütle mudel — ütleb sinu ettevõte. Mudelil pole lepingut, allkirja ega registrit — ei saa olla ka vastutust. Vastutav on inimene, kes süsteemi kasutusele võttis, ja ettevõte, kelle nime all tulemus käib.

See pole hirmuütlemine, vaid süsteemiehituse lähtekoht. Kui automatiseeritud kiri lubab kliendile midagi, mida lubada ei tohtinud, või vale summa jõuab raamatupidamisse, vastavad selle eest ikka inimene ja ettevõte — klient, pank ja seadus pöörduvad sinu poole, mitte mudeli poole. Just seepärast on kontrollpunkt süsteemis: kuna vastutus jääb sinule, peab tulemus enne väljumist läbima kontrolli — inimese või automaatse reegli, olenevalt sellest, kui kallis viga võib olla. Kontrollija ja haldaja pole „lisainimesed“, vaid kohad, kus vastutus konkreetselt elab.

Teine põhitõde: rollid, otsused ja nende põhjendused tasub kirja panna — kes otsustas, mida automatiseeritakse; kes kinnitas selle prompti; mis juhtub, kui süsteem eksib. Kui keegi lahkub või süsteemi hakatakse muutma, ei pea keegi otsuseid nullist tegema ega arvama, miks midagi just nii tehtud on. Prompt on selles mõttes ettevõtte vara, mida versioonida ja hallata — kuidas, õpetab [2.6 Promptide haldus kui vara](../02-praktika/06-promptide-haldus.md); [5.7 Vastutus, eetika ja governance](../05-suurte-projektide-tase/07-vastutus-ja-eetika.md) vaatleb süsteemi juhtimise ja eetika — ingl k *governance* (reeglite kogu, mis määrab, kes tohib mis otsuseid teha) — küsimust.

Veel üks vastutuse küsimus: kui süsteem saadab kliendiandmeid välisele AI-mudelile, vastutab ettevõte ka selle andmete eest — seda vaatame dokumendis [3.7 Turvalisus: võtmed, andmed, pahatahtlikud juhised](../03-susteemi-ulesehitus/07-turvalisus.md).

## Näide samm-sammult: raamatupidamisbüroo rollidel põhinev otsus

Väljamõeldud **„Nummerbüroo“** — väike raamatupidamisbüroo: omanik Anu, viis raamatupidajat ja Jaak — ametilt kontorihaldur, tegelikult alati ettevõtte tarkvara juures olnud.

Probleem: klientidelt tuleb e-posti arveid, mille andmed tuleb enne raamatupidamise tarkvarasse kandmist välja selgitada — kes, mis, summa, käibemaks. Üks arve võtab umbes kolm minutit, arveid tuleb päevas paarsada.

Otsus sünnib rollide kaupa:

1. **Anu, tellijana, määrab ulatuse.** Automatiseeritakse ettevalmistus — arve andmete väljaselgitus ja sisestuse kavand —, mitte otsused. Ta paneb kirja ka selle, mida **ei** automatiseerita: maksekorralduste kinnitamine kliendi pangas ja maksudeklaratsioonide esitamine. Raha liikumine ja seadusele esitatavad andmed — vastutus on liiga suur, et seda delegeerida, isegi kui see tehniliselt võimalik oleks.
2. **Jaak, ehitajana, paneb workflow (töövoog — automatiseeritud sammude jada) kokku valmis tööriistadega.** Uus arve e-postis on käivitaja (ingl k *trigger* — sündmus, mis töövoo käima paneb); süsteem loeb manuse, kogub väljad ja paneb kavandi raamatupidaja ülevaatuse järjekorda. Ebaselge arve läheb kohe käsitsi töötlusse.
3. **Raamatupidajad, sisukujundajatena, kirjutavad promptid ise.** Ainult nemad teavad, milline sisestus on korrektne: nad panevad juhendisse näited õigetest sisestustest ja reeglid — näiteks „kui arvel puudub käibemaksumäär, ära paku, vaid märgi ülevaatuseks“.
4. **Raamatupidaja, kontrollijana, vaatab iga arve üle.** Kavand on ekraanil arve kõrval; kinnitamine on üks klõps, tagasilükkamine kaks klõpsu. Erandlikud juhtumid jäävad täielikult inimesele.
5. **Jaak, haldajana, jälgib nädalas kulusid ja kvaliteeti** — mitu kavandit lükati tagasi ja miks. Anu vaatab kord kuus kokkuvõtet ja otsustab, kas ulatust laiendada (näiteks müügiarvedele).

Tulemus: ühe arve inimese aeg langeb kolmelt minutilt umbes poolele minutile ning raamatupidajate päev vabaneb selle töö jaoks, mida masin teha ei tohi — kliendiga rääkimiseks ja erandite lahendamiseks. Ja kui Jaak kunagi lahkub, ei kao süsteem koos temaga: kirjas on, kes mis otsustas, kus promptid asuvad ja kes haldab.

## Kokkuvõte

- **Viis rolli: tellija/omanik, ehitaja/tehnik, sisukujundaja, kontrollija ja haldaja** — igaüks vastab ühele küsimusele: miks, kuidas, mis on hea, kas see läheb edasi ja kas see töötab ka homme.
- **Rollid ei kao ka siis, kui isikud korduvad** — väikeses firmas kannab üks inimene mitut mütsi, aga iga rolli peab keegi täitma; suures meeskonnas jagunevad need ametikohtadeks.
- **Vastutus AI vea eest jääb alati inimesele ja ettevõttele** — klient ja seadus pöörduvad sinu poole, mitte mudeli poole. See pole hirmuütlemine, vaid põhjus, miks kontrollpunkt süsteemis on.
- **Rollid, otsused ja promptid tasub kirja panna** — kui keegi lahkub või süsteemi muudetakse, on selge, kes otsustas mida ja miks.

## Mis edasi?

- eelmine → [1.5 AI-automatiseeritud süsteemi anatoomia](05-susteemi-anatoomia.md)
- järgmine tase → [2.1 Head promptini: struktuur, roll, näide, väljundi vorming](../02-praktika/01-hea-prompt.md) — 1. tase on nüüd läbi; siit algab järgmine tase
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
