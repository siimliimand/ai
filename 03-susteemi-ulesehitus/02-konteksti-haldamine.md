# 3.2 Konteksti haldamine: kuidas mudel „mäletab“

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [3.1 API integratsioonid](01-api-integratsioonid.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, miks AI-mudelil mälu pole ja mida „mäletamine“ sel juhul tähendab;
- näha, kuidas vestluse ajalugu kasvab ja miks pikk ajalugu muudab süsteemi kalliks ja unustavaks;
- valida ja kombineerida kolme strateegiat: piiratud aken, kokkuvõte, „asjade seis“;
- paigutada süsteemivestluse juhise õigesse kohta kontekstis;
- öelda, millal teadmine aknasse ei mahu ja millal tulevad appi RAG ning pikaaegne mälu.

## Lihtsalt öeldes

> AI-mudel ei mäleta midagi. Iga päring on talle uus vestlus nullist — varasemast teab ta ainult seda, mis süsteem talle iga kord uuesti ette kirjutab. „Mäletamine“ pole seepärast mudeli, vaid süsteemi töö: enne iga päringut panna aknasse kõik, mida hea vastuse jaoks vaja on — juhised, olulised faktid, varasemad käigud — ja mitte midagi muud.

## Mudelil pole mälu — kogu info tuleb kaasa anda

Dokumendist [1.1](../01-alused/01-mis-on-ai-mudel.md) tead, mis on kontekstiaken (ingl k *context window* — tekstihulk, mida mudel korraga näeb). Siin läheb vaja üht selle järeldust:

**Mudelil pole vestlusest mälu. Iga API-päring on terve vestlus algusest peale.**

Kui klient kirjutas eile midagi ja täna saadab uue sõnumi, ei tea mudel esimesest midagi — välja arvatud juhul, kui süsteem saadab eilse teksti päringus kaasa. Ka vestlusrakenduste „mälu“ on just see: süsteem hoiab varasemad sõnumid enda andmetes ja paneb need iga kord aknasse tagasi.

## Vestluse ajalugu ja miks seda kontrollimatult kasvada ei tohi

> **Lihtsalt öeldes:** vestluse ajalugu on nagu koosoleku protokoll, mida iga uue punkti juures tervikuna ette loetakse — mida pikem koosolek, seda pikem ettelugemine iga kord ja seda suurem võimalus, et keskmine leht jääb tähelepanuta.

Vestluse ajalugu (ingl k *conversation history* — varasemate käikude kogu, mida süsteem igal päringul uuesti kaasa saadab) on lihtsaim viis mudelile „mälu“ anda. Käik (üks vestluse voor — kasutaja küsimus + mudeli vastus) lisandub ajaloo lõppu, ja igal järgmisel päringul läheb kogu ajalugu jälle kaasa.

Kolm põhjust, miks seda kontrollimatult kasvada lasta ei tohi:

1. **Kulud kasvavad iga käiguga.** Iga token (sõnatükk) on tasuline ja pikk ajalugu kulutatakse igal päringul uuesti. Kuidas kulusid arvestada ja piirata, õpetab [3.6 Kulude haldamine](06-kulude-haldamine.md).
2. **Tähelepanu on ebavõrdne.** Pika sisendi keskmine osa saab kõige vähem tähelepanu: tellimuse number, öeldud vestluse alguses ja nüüd pika ajaloo keskel, on just info, mis kõige kergemini kaob (vt [1.3 Promptide põhitõed](../01-alused/03-promptide-pohitoed.md)).
3. **Aken saab täis.** Kui ajalugu ületab akna mahu, peab midagi välja jääma — kas süsteem lõikab mehaaniliselt algusest ära, või otsustad sina ise, mis säilib. Teine võimalus on alati parem.

## Kolm strateegiat pika vestluse haldamiseks

> **Lihtsalt öeldes:** akna täitumine sarnaneb lauale kogunevatele paberitele. Kolm tegevust: (a) viska vanemad ära ja hoia viimased, (b) pane vanemate sisu ühele kokkuvõtelehele, (c) hoia oluline eraldi märkmikus.

### 1. Piiratud aken

Hoia ajaloost ainult viimased N käiku (näiteks kümme) — vanemad lõikab süsteem välja. Kõige lihtsam strateegia: üks piirangu number seadetes, lisatööd pole.

Risk on otsene: kõik, mis varakult öeldi, kaob vaikimisi. Tellimusenumber, öeldud esimeses käigus, on pärast kümmet käiku kadunud — mudel kas küsib uuesti või arvab halvimal juhul ise välja (hallutsinatsioon — mudeli kindlalt öeldud, aga vale vastus).

### 2. Kokkuvõte

Kokkuvõte (ingl k *summarization* — vanema vestlusosa lühikokkuvõte, mida laseb süsteem mudelil ära teha) hoiab ajalugu pidevalt kasvamast: kui käike on liiga palju, asendab süsteem vanema osa ühe lühikokkuvõttega. Aknasse läheb edaspidi kokkuvõte + viimased käigud.

Varajane info säilib kokkuvõttes, aga kokkuvõte on alati valik — mudel võib just selle detaili välja jätta, mis hiljem oluliseks osutub (näiteks tellimuse number).

### 3. „Asjade seis“ — oluliste faktide eraldamine

Süsteem hoiab ajaloo kõrval olulisi fakte eraldi kirjas — „asjade seis“ (oluliste faktide kiri): kes klient on, mis tellimus, mis eesmärk, mis on otsustatud. Neid fakte ei laske süsteem mudelil kokku võtta ega ajaloo hooleks jätta — need tulevad süsteemi enda andmetest ja lähevad igal päringul kaasa. Ühe päringu kuju:

```json
{
  "süsteemivestluse_juhis": "Sa oled e-poe klienditeeninduse assistent. Vasta lühidalt ja eesti keeles.",
  "asjade_seis": {
    "klient": "Kadri Kask",
    "tellimus": "1224",
    "toode": "lauvalamp Nordica",
    "soov": "vahetada must tumerohelise vastu"
  },
  "viimased_käigud": [
    { "roll": "kasutaja", "tekst": "Kas vahetus on ikka veel võimalik?" },
    { "roll": "mudel", "tekst": "Jah, 14 päeva jooksul ostust on vahetus võimalik." }
  ],
  "uus_sõnum": "Nagu ma ütlesin, tahaksin ikkagi teist värvi."
}
```

| Strateegia | Kuidas | Eelis | Risk |
|---|---|---|---|
| **Piiratud aken** | Hoia ainult viimased N käiku | Kõige lihtsam, lisatööd pole | Varajane info kaob vaikimisi |
| **Kokkuvõte** | Vanem osa kokku võtta; hoia kokkuvõte + viimased käigud | Ajalugu säilib, aken ei kasva pidevalt | Kokkuvõte võib olulise detaili välja jätta |
| **„Asjade seis“** | Olulised faktid eraldi kirjas, igal päringul kaasas | Faktid täpsed ja alati olemas | Vajab haldust: kes ja millal fakte uuendab |

Praktikas kombineeritakse: süsteem hoiab „asjade seisu“ + viimased käigud ja laseb vahel vanema osa kokku võtta.

### Kui teadmine on aknast suurem

Kolm strateegiat eeldavad, et vajalik teave on kusagil olemas — küsimus on ainult selles, mis aknasse läheb. Kui teadmine ise on aknast suurem (nt kogu ettevõtte dokumentide baas), aitab RAG: süsteem otsib küsimuse järgi asjakohased lõigud välja ja annab need mudelile (vt [4.2 RAG](../04-agendid-ja-mootmine/02-rag.md)). Kui info peab meelde jääma seansside vahel — klient tuleb nädala pärast ja süsteem peaks teda tundma —, salvestatakse faktid süsteemi andmetesse ja tuuakse vajadusel konteksti tagasi; seda nimetatakse pikaaegseks mäluks (vt [4.3 Pikaaegne mälu ja oleku haldus](../04-agendid-ja-mootmine/03-pikaaegne-malu.md)).

## Süsteemivestluse juhis: alati ees olevad reeglid

> **Lihtsalt öeldes:** süsteemivestluse juhis on nagu esimesel tööpäeval antud tööjuhend — üks kord kirja pandud reeglid, mida assistent igal ülesandel meeles peab.

Süsteemivestluse juhis (ingl k *system prompt* — alati ees olev püsijuhis, mis defineerib rolli ja reeglid) on konteksti esimene ja kõige püsivam osa: ta ei muutu iga käiguga, vaid kehtib kogu vestluse jooksul. Sinna kuulub roll („Sa oled e-poe klienditeeninduse assistent“), reeglid („vasta lühidalt, ära leiuta hindu“) ja piirid (mille üle assistent ei tohi otsustada).

Selle koht pole juhus — tüüpiline päring ehitatakse üles nii:

1. süsteemivestluse juhis — roll ja reeglid, sest need kehtivad alati;
2. „asjade seis“ ja asjakohased andmed;
3. ajalugu või viimased käigud;
4. uus küsimus — viimaseks, sest just sellele vastatakse.

Nii jääb oluline info akna algusse ja lõppu, kus tähelepanu kõige tugevam on (vt [2.4 Sisendid ja andmete ettevalmistamine](../02-praktika/04-sisendid-ja-andmed.md)).

Üks hoiatus: püsijuhis on osa promptist (mudelile antav juhis) ja kulutab tokeneid igal päringul. Seepärast hoia see täpne ja lühike — iga sinna pandud lause maksab iga kord uuesti.

## Näide samm-sammult: klient tuleb järgmisel päeval tagasi

Olukord: e-poe kliendivestlus. Eile käis klient vestluses tellimuse 1224 (lauvalamp Nordica) ümber — 12 käigu jooksul selgus, et klient soovib musta asemel tumerohelist. Öö jooksul vestlus lõppes. Homme avab klient vestlusakna uuesti:

> „Nagu ma ütlesin, tahaksin ikkagi teist värvi.“

**a) Ilma konteksti haldamata (enne).** Uus vestlus koosneb ainult sellest lausest. Mudel ei tea, millisest tootest jutt, milline on tellimus ja mis värvi klient silmas peab:

> „Kindlasti aitame! Palun öelge, millist toodet ja millise värvi te silmas peate.“

Tulemus halb: klient peab kõik uuesti seletama — või saab halvimal juhul usutavalt kõlava, aga asjasse mitte kuuluva vastuse.

**b) Piiratud aken 10 käigu pikkusega.** Süsteem hoiab viimased 10 käiku, aga eilne vestlus oli 12: esimesed kaks — just need, kus tellimus ja soovitud värv kõne all olid — jäid aknast välja. Mudel näeb küll, et vestlus puudutab vahetust, aga ei tea enam toodet ega värvi:

> „Muidugi! Ütlege palun, milline toode teil silmas on ja mis värvi te sooviksite — kontrollin kohe, kas vahetus on võimalik.“

Info on süsteemi andmetes olemas, aga aknasse ei mahtunud — varajane info jäi välja.

**c) „Asjade seis“ lahendusega (pärast).** Süsteem hoiab eilse vestluse käigus koostatud faktide kirja (toode, tellimus, soov) ja paneb selle iga päringuga kaasa — ülal näidatud JSON-i kujul. Uus küsimus jõuab mudelini koos kõigi faktidega:

> „Tere jälle! Jätkame sealt, kus jäid: tellimuse 1224 lauvalamp Nordica saame musta asemel tumeroheliseks vahetada, vahetus on tasuta. Kinnitage palun, ja uus lamp läheb kohe teele.“

Enne pidi klient kogu eilse info uuesti öeldama; pärast on vastus isikupärane ja õige — faktid tulid süsteemi kirjast, mitte mudeli õnnest.

Pange tähele: variant c ei vajanud pikka ajalugu üldse — kogu „mälu“ on mõnesõnaline faktide kiri, mida süsteem hoiab. Sama kehtib töövoogude kohta: iga töövoo jooks on iseseisev ja tema „mälu“ tuleb andmetest (vt [2.3 Workflow algtasandil](../02-praktika/03-workflow-algtasandil.md)).

## Kokkuvõte

- **Mudelil pole mälu — iga päring on terve vestlus algusest peale.** „Mäletamine“ on süsteemi töö: panna aknasse kõik, mis hea vastuse jaoks vajalik.
- **Vestluse ajalugu kasvab iga käiguga ja läheb iga kord täielikult kaasa** — seepärast kasvavad ka kulud ja info kadumise risk.
- **Kolm strateegiat:** piiratud aken (lihtne, aga varajane info kaob), kokkuvõte (ajalugu säilib lühendatult) ja „asjade seis“ (faktid täpsed ja alati kaasas). Praktikas kombineeritakse.
- **Süsteemivestluse juhis on konteksti kõige püsivam osa:** roll ja reeglid ees, siis faktid, siis ajalugu, viimaseks uus küsimus.
- **Kui teadmine on aknast suurem, aitab otsing ja mälu:** RAG suurte dokumentibaaside jaoks, pikaaegne mälu seansside vahel.

## Mis edasi?

- eelmine → [3.1 API integratsioonid](01-api-integratsioonid.md)
- järgmine → [3.3 Tööriistad ja tegevused: lase mudelil tegutseda](03-tooriistad-ja-tegevused.md)
- Kui teadmine on kontekstiaknast suurem → [4.2 RAG](../04-agendid-ja-mootmine/02-rag.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
