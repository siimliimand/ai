# 3.6 Kulude haldamine: tokenid, hinnad, eelarve

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [3.1 API integratsioonid](01-api-integratsioonid.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, kust AI-kasutuse hind tuleb — sisendist, väljundist ja vestluse ajaloo kasvust;
- teha enne käikuandmist lihtne kulude hinnang (ingl k *estimate* — kuluprognoos enne käikuandmist) ühe valemiga;
- leida pakkuja juhtpaneelilt (ingl k *dashboard* — pakkujate veebiportaal, kus tegelik kasutus ja kulud on nähtavad) reaalsed numbrid;
- nimetada viis praktikat, millega kulut langetada ilma kvaliteeti ohverdamata;
- seada hoiatuskünnis (ingl k *alert threshold* — kulu piir, mille ületamisest tuleb teade) ja kululagi (ingl k *budget limit* — ette seatud kuulimiit).

## Lihtsalt öeldes

> AI-mudeli kasutamise eest makstakse iga küsimuse ja iga vastuse eest, tükikaupa. Kulud ei sõltu aga ainult hinnakirjast — neid dikteerivad kaks asja, mida sina kontrollid: kui palju sa igal päringul kaasa annad ja kui tihti sa küsid. Hinnang enne käikuandmist, üks pilk juhtpaneelile kord kuus ja mõistlik kululagi hoiavad ära suured üllatused.

## Kuidas hind tekib: tokenid sisenemisel ja väljumisel

Dokumendist [1.1 Mis on AI-mudel ja kuidas ta „mõtleb“](../01-alused/01-mis-on-ai-mudel.md) tead, mis on token (ingl k *token* — sõnatükk). API-kasutuses on ta ka arveldusühik: [3.1 API integratsioonides](01-api-integratsioonid.md) näidatud päringul maksad kahes punktis.

**Sisend (ingl k *input*) — kõik, mis päringuga kaasa läheb.** Juhised, andmed, ajalugu — kõik, mis kontekstiaknasse (ingl k *context window* — tekstihulk, mida mudel korraga näeb) pannakse, arveldatakse.

**Väljund (ingl k *output*) — mudeli vastus.** Enamikus hinnakirjadest on väljundi token kallim kui sisendi oma (tüüpiliselt mõni kord) — vastuse tükikaupa kirjutamine on arvutuslikult kallim kui lugemine.

Kolmas tegur on süsteemiehitaja jaoks kõige tähtsam: **vestluse ajalugu (ingl k *conversation history* — varasemate käikude kogu, mida süsteem igal päringul uuesti kaasa saadab)**. Nagu [3.2 Konteksti haldamine](02-konteksti-haldamine.md) selgitab, ei mäleta mudel midagi — igal päringul läheb kogu senine ajalugu taas kaasa ja maksab taas. Kümne käigu pika vestluse viies küsimus on seega arveldatud kümne käigu ulatuses sisendit — kontrollimatult kasvav ajalugu kasvatab kulud samas tempos.

> **Lihtsalt öeldes:** maksad nii selle eest, mis sa ütled, kui selle eest, mis vastu öeldakse. Ja kuna mudel midagi ei pea meeles, tuleb tal iga kord kogu varasem jutt ette lugeda — ja selle eest uuesti maksta.

Üks varjatud kulu veel: vead. Kui süsteem päringu uuesti teeb, maksab ka kordus (ingl k *retry*) — mitu korda kordunud päring on kordades kallim; veakäsitluse põhimõtted on [3.4 Vead ja veakäsitlus](04-vead-ja-veakasitlus.md) teema.

## Kulude hinnang enne käikuandmist

> **Lihtsalt öeldes:** enne käikuandmist arvuta kulu paberil — nagu enne ehitust eelarve. Tulemus ei pea olema sentitäpne; see peab näitama suurusjärku: kas siin on jutt ühe eurost kuus või sajast.

Kulude hinnang käib ühe valemiga:

```text
päevane kulu = päringud × (sisendi tokenid × sisendi hind)
             + päringud × (väljundi tokenid × väljundi hind)
```

Läbiarvatud näide:

```text
  Päringuid päevas:               12
  Keskmine sisend:             2 500 tokenit
  Keskmine väljund:              400 tokenit
  Näidishind, sisend:   0,002 € / 1 000 tokenit
  Näidishind, väljund:  0,006 € / 1 000 tokenit (kallim!)

  Sisend:  12 × 2 500 = 30 000 tk → 30 × 0,002 € = 0,06 €
  Väljund: 12 ×   400 =  4 800 tk =  4,8 × 0,006 € = 0,029 € ≈ 0,03 €
  Päev kokku:                          ≈ 0,09 €
  Kuu (22 tööpäeva):                   ≈ 2 €
```

Hinnang käib siin ainult päevakokkuvõtete kohta; arvete sisestuskavandite voos korrutame sama valemi oma numbritega.

Kaks ausat märkust. **Esiteks: hinnad on näitlikud ja muutuvad.** „0,002 € iga tuhande sisendtokeni eest“ on õpetuslik näitarv — reaalsed hinnad leiad pakkujate hinnakirjadest, need sõltuvad mudelist (suur kallim, väike odavam) ja muutuvad aja jooksul. **Teiseks: ümarda julgelt — eesmärk on suurusjärg, mitte sent.** Keskmine tokenite arv päringu kohta selgub ühest proovikäigust või juhtpaneelilt; sellele suurusjärgule toetub ka tellija otsus (vt [1.6 Rollid ja vastutus projektis](../01-alused/06-rollid-ja-vastutus.md)).

## Järelevalve: juhtpaneel, ülevaade, hoiatus

> **Lihtsalt öeldes:** järelevalve (ingl k *monitoring* — süsteemi käigu ja kulude pidev jälgimine) ei pea olema keerukas. Kolm harjumust: pilk juhtpaneelile iga nädal, numbrid üle kord kuus ja üks teavitusseadistus. Enamat alguses vaja pole.

**Kus kulust näha: juhtpaneel.** Iga suurem pakkuja pakub juhtpaneeli, kus on päris kasutuse kulu — päevade ja mudelite kaupa. Tehnilisele lugejale: sealt näeb ka tokenite jagunemist sisendi ja väljundi vahel ning üksikute päringute kulusid.

**Kord kuus numbrid üle — tellija otsuste alus.** 1.6 järgi jälgib haldaja (Nummerbüroos Jaak) kulutempot; tellija Anu vaatab kord kuus numbrid hinnanguga üle.

**Hoiatuskünnis.** Juhtpaneelidesse saab paigaldada hoiatuskünnise: näiteks „kui kuu kulu ületab 7 €, saadab süsteem Jaagule e-kirja“. Teade on varajane märk: kasutuskäitumine on muutunud — enne, kui arve üllatab.

## Viis võimalust kulu vähendada

> **Lihtsalt öeldes:** kõik viis võimalust on kolme põhimõtte variandid — anna vähem kaasa, küsi vähem kordi, saa lühem vastus. Ükski ei nõua programmeerimist, kõik nõuavad järele mõtlemist.

1. **Väiksem mudel lihtsamate ülesannete jaoks.** Dokumendi 1.1 põhimõte: spetsialist on kallis, praktikant odav — kirja liigitamine („arve“ / „kaebus“ / „tellimus“) ei vaja suurt mudelit; jäta suur keerukateks ülesanneteks.
2. **Lühem prompt (mudelile antav juhis).** Püsijuhis kulutab tokeneid igal päringul — [2.1 Head promptini](../02-praktika/01-hea-prompt.md) prompti püsivad osad peavad olema täpsed, mitte pikad. Ära saada korduvalt suurt juhendit, kui lühike piisab.
3. **Vähem päringuid.** Süsteem ei tohiks mudelit igal klahvivajutusel välja kutsuda — oota sisestuse lõppu, ühenda mitu väikest küsimust üheks. Iga kutse on arveldatav sündmus.
4. **Lühem väljund.** Küsi ainult vajalikud väljad — [2.2 Struktureeritud väljund](../02-praktika/02-struktureeritud-valjund.md) aitab: kümne lause asemel kolm numbrit, ja lühem vastus on odavam.
5. **Lühem ajalugu.** Kui süsteem kannab igal päringul kogu ajaloo kaasa, maksab ta iga kord uuesti — „asjade seis“ lahendus (vt [3.2](02-konteksti-haldamine.md)) hoiab olulised faktid eraldi lühikirjas ja akna lühikesena.

Üks reegel, mis kõigis viies kehtib: odavam ei tohi tähenda halvemat kvaliteeti — pärast iga säästusammu kontrolli, et kvaliteet püsis. Sügavamat optimeerimist — vahemälu, pakettkäsitus, latentsuse ja jõudluse mõõtmine — vaatab [4.7 Jõudlus ja latentsus](../04-agendid-ja-mootmine/07-joudlus-ja-latentsus.md).

## Eelarve ja kululagi

> **Lihtsalt öeldes:** kululagi on nagu krediitkaardi limiit — kogu kuu saab kulutada, aga kui limiit täis, kulutamine lakkab ja sellest teavitatakse. Üllatusarve jääb tulemata.

Hinnang ja hoiatuskünnis annavad arusaamise; kululagi (ingl k *budget limit*) lisab kaitse. Kuulimiitu saab paigaldada kolmel tasemel — ja valik on tellija otsus, sest see on eelarveküsimus (vt [1.6](../01-alused/06-rollid-ja-vastutus.md)):

- **Teavitab.** Kõige pehmem variant: künnise ületamisel saadetakse teade, süsteem töötab edasi.
- **Vähendab.** Süsteem lülitub odavamale mudelile või peatab mittekriitilised tööd — näiteks kokkuvõtted tehakse väiksemaga, klienditöö jääb muutumatuks.
- **Peatab.** Karmim variant: kululagi täis — uued päringud enam ei lähe. Sobib ainult selliste tööde jaoks, kus paus kannatub: öine raport võib oodata, kliendi vastus ei saa oodata.

Mõistlik kooskõla: kululagi umbes kaks korda suurem kui hinnang (jätab ruumi kasvule), hoiatuskünnis umbes 70–80% kululagist. Kui kasutus on suure mahu ja pideva kasvuga, on kulude planeerimise strateegia [5.4 Kulustrateegia suures mahus](../05-suurte-projektide-tase/04-kulustrateegia.md) teema.

## Näide samm-sammult: Nummerbüroo kuukulu

Jällegi **„Nummerbüroo“** (vt [1.6](../01-alused/06-rollid-ja-vastutus.md)): omanik Anu, haldaja Jaak. Süsteem koostab arvetest sisestuse kavandeid ja lisaks igal päeval 12 kliendile päevakokkuvõtte nende arvetoimingutest. Anu küsib Jaagilt: „Mis see AI meile maksma läheb?“

**1. Hinnang paberil.** Jaak teeb ülaltoodud arvutuse päevakokkuvõtete osas ja korrutab sama valemi kavandite vooga — kokku jõuab **~4 € kuuni** (neist ~2 € päevakokkuvõtted). „Paar eurot kuus,“ ütleb Jaak. Anu rahul.

**2. Esimene nädal juhtpaneelil.** Nädal hiljem näitab Jaak Anule pakkuja juhtpaneeli möödunud nädala andmeid:

```text
Nädala kulu:      ~1,20 €   (kuutempo ~5,20 €)
Sisendtokenid:    ~490 000
Väljundtokenid:   ~24 000
```

Summad on väiksed, aga suhe on vale — miks?

**3. Üks prompt sööb üle poole.** Jaotuses nähtub Anule üllatus: **umbes 60% nädala kulust (~0,70 €) tuleb ühestainsast päevase promptist** — ühe kliendi kokkuvõttest. Põhjus leitakse kiiresti: selles töövoos kannab süsteem igal päringul kaasa kogu nädala vestluse ajaloo — ja nagu 3.2 õpetab, arveldatakse kogu ajalugu iga päringul uuesti. Jaak rakendab 3.2 kolmandat strateegiat — „asjade seis“: süsteem hoiab olulised faktid (klient, periood, summad) eraldi lühikirjas (~1 500 tokenit) ja pikka ajalugu kaasa enam ei lähe.

Nädal hiljem juhtpaneel: nädala kulu **~0,60 €**, kuutempo ~2,50–3 € — **kulu kukub poole võrra** ja on jälle hinnanguga kooskõlas. Kvaliteet jääb samaks: faktid on nüüd täpselt kirjas, mitte pika ajaloo hooleks jäetud (kuidas seda kontrollida, õpetab [4.5 Hindamine](../04-agendid-ja-mootmine/05-hindamine.md)).

**4. Kaitse paika.** Lõpetuseks paigaldab Jaak **hoiatuskünnise 4,50 € kuus** (teade Jaagule ja Anule) ja **kululagi 6 € kuus** (see on pandud kõrgemale kui kaks korda hinnangust, sest tegelik tempo lähenes hinnangule). Kui kulu ületaks, lülitaks süsteem kõigepealt päevakokkuvõtted väiksemale mudelile ja teavitaks — sisestuste kavandid jääksid muutumatuks.

Hinnang andis suurusjärku, juhtpaneel näitas tegelikku, üks ülevaatus leidis ühe liiga pika prompti ja kaitse hoiab tulevased üllatused ära. Kogu see tegevus võttis aega vähem kui tund.

## Kokkuvõte

- **Hind tuleb tokenitest — nii sisendis kui väljundis**, väljund on tavaliselt kallim; vestluse ajalugu on peamine kasvutegur, sest igal päringul arveldatakse kogu ajalugu uuesti.
- **Tee enne käikuandmist kulude hinnang** — lihtne valem annab suurusjärku; hinnad on näitlikud ja muutuvad, reaalsed leiad pakkuja hinnakirjast.
- **Järelevalve on kolm lihtsat harjumust:** pilk juhtpaneelile, kuune ülevaatus haldajal ja hoiatuskünnis.
- **Kulu vähendab viis asja:** väiksem mudel lihtsamateks ülesanneteks, lühem prompt, vähem päringuid, lühem väljund, lühem ajalugu.
- **Kululagi on kaitse kolme variandiga:** teavitab, vähendab või peatab — valik on tellija eelarveotsus.

## Mis edasi?

- eelmine → [3.5 Ohutus: piirid ja inimene kinnitusahelas](05-ohutus.md)
- järgmine → [3.7 Turvalisus: võtmed, andmed, pahatahtlikud juhised](07-turvalisus.md)
- Sügavam optimeerimine → [4.7 Jõudlus ja latentsus](../04-agendid-ja-mootmine/07-joudlus-ja-latentsus.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
