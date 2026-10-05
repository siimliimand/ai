# AI-automatiseerimise käsiraamat

See käsiraamat õpetab, kuidas õigesti luua süsteeme, mis kasutavad AI-mudeleid protsesside automatiseerimiseks. Kogu sobib nii mitte-tehnilisele lugejale, kes soovib mõista võimalusi ja teha arukaid otsuseid, kui ka tehnilisele lugejale, kes vajab konkreetsesse süsteemi ülesehitusse suunatud juhendeid. Dokumendid on järjestatud tasanditeks, mis viivad sujuvalt alusteadmisest professionaalse suurprojekti tasemeni.

## Kuidas lugeda

Kogu on järjestatud **5 tasandiks**. Alusta alati 1. tasemest — iga järgmine tase eeldab eelmise tundmist.

- **Mittetehniline lugeja:** iga dokumendi alguses on kast **„Lihtsalt öeldes“**, mis annab dokumendi põhitõe ilma tehnilise detailita. Sellest piisab arusaamiseks ja otsustamiseks.
- **Tehniline lugeja:** leiad iga dokumendi sisemusest sügavama selgituse ja samm-sammulised juhendid süsteemi ülesehituseks.

## Olekud

| Olek | Tähendus |
| --- | --- |
| ✅ | Dokument on valmis ja loetav |

---

### 1. tase — Alusteosed (`01-alused/`)

| # | Dokument | Kirjeldus | Olek |
| --- | --- | --- | --- |
| 1.1 | [Mis on AI-mudel ja kuidas ta „mõtleb“](01-alused/01-mis-on-ai-mudel.md) | Mis on keelemudel, kuidas ta töötab ja mida tema „mõtlemine“ tegelikult tähendab | ✅ |
| 1.2 | [Võimalused ja piirid: mida automatiseerida tasub](01-alused/02-voimalused-ja-piirid.md) | Millised ülesanded sobivad AI-le ja millised mitte, kuidas otsustada | ✅ |
| 1.3 | [Promptide põhitõed](01-alused/03-promptide-pohitoed.md) | Kuidas kirjutada mudelile selgeid ja töökindlaid juhiseid | ✅ |
| 1.4 | [Kus AI-automatiseerimine juba töötab: kasutujuhtumid](01-alused/04-kasutujuhtumid.md) | Tüüpilised kasutusalad ja näited sellest, mida organisatsioonid AI-ga automatiseerivad | ✅ |
| 1.5 | [AI-automatiseeritud süsteemi anatoomia](01-alused/05-susteemi-anatoomia.md) | Süsteemi seitse osa: käivitaja, sisendandmed, juhis, mudel, kontrollpunkt, väljund ja andmete hoidmine | ✅ |
| 1.6 | [Rollid ja vastutus projektis](01-alused/06-rollid-ja-vastutus.md) | Viis rolli (tellija, ehitaja, sisukujundaja, kontrollija, haldaja) ja vastutus AI vea eest | ✅ |

### 2. tase — Praktika (`02-praktika/`)

| # | Dokument | Kirjeldus | Olek |
| --- | --- | --- | --- |
| 2.1 | [Head promptini: struktuur, roll, näide, väljundi vorming](02-praktika/01-hea-prompt.md) | Süvenemine: promptimallid, versioonid ja testimine pikemas tsüklis | ✅ |
| 2.2 | [Struktureeritud väljund: tabelid, mallid ja JSON](02-praktika/02-struktureeritud-valjund.md) | Kuidas saada mudelilt ennustatavas vormis vastuseid | ✅ |
| 2.3 | [Workflow algtasandil: sammud ja tingimused](02-praktika/03-workflow-algtasandil.md) | Mitmeosalised protsessid, otsustuspunktid ja tingimused | ✅ |
| 2.4 | [Sisendid ja andmete ettevalmistamine](02-praktika/04-sisendid-ja-andmed.md) | Kuidas andmeid koguda, puhastada ja vormindada nii, et mudel saaks hästi töötada | ✅ |
| 2.5 | [Esimene automatiseeritud workflow otsast lõpuni](02-praktika/05-esimene-workflow.md) | Täielik näide ühe automatiseeritud töövooga algusest lõpuni | ✅ |
| 2.6 | [Promptide haldus kui vara](02-praktika/06-promptide-haldus.md) | Promptide salvestamine, versioonihaldus ja taaskasutamine | ✅ |

### 3. tase — Süsteemi ülesehitus (`03-susteemi-ulesehitus/`)

| # | Dokument | Kirjeldus | Olek |
| --- | --- | --- | --- |
| 3.1 | [API integratsioonid: mudel programmist välja kutsuda](03-susteemi-ulesehitus/01-api-integratsioonid.md) | Kuidas kutsuda mudelit koodist ja ehitada mudel oma süsteemi sisse | ✅ |
| 3.2 | [Konteksti haldamine: kuidas mudel „mäletab“](03-susteemi-ulesehitus/02-konteksti-haldamine.md) | Kontekstiakna kasutamine ja info hoidmine vestluse jooksul | ✅ |
| 3.3 | [Tööriistad ja tegevused: lase mudelil tegutseda](03-susteemi-ulesehitus/03-tooriistad-ja-tegevused.md) | Funktsioonide kutsumine ja väliste süsteemide kasutamine mudeli poolt | ✅ |
| 3.4 | [Vead ja veakäsitlus](03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md) | Mida teha, kui mudel eksib või süsteemi osa ebaõnnestub | ✅ |
| 3.5 | [Ohutus: piirid ja inimene kinnitusahelas](03-susteemi-ulesehitus/05-ohutus.md) | Kuidas piirata mudeli tegevust ja kaasata inimene otsustusse | ✅ |
| 3.6 | [Kulude haldamine: tokenid, hinnad, eelarve](03-susteemi-ulesehitus/06-kulude-haldamine.md) | Kulude mõõtmine, prognoosimine ja juhtimine | ✅ |
| 3.7 | [Turvalisus: võtmed, andmed, pahatahtlikud juhised](03-susteemi-ulesehitus/07-turvalisus.md) | API-võtmete haldus, andmekaitse ja prompt injectioni tõrje | ✅ |

### 4. tase — Agendid ja mõõtmine (`04-agendid-ja-mootmine/`)

| # | Dokument | Kirjeldus | Olek |
| --- | --- | --- | --- |
| 4.1 | [Agendi süsteemid: mis need on ja millal vaja](04-agendid-ja-mootmine/01-agendi-susteemid.md) | Agendid, kes plaanivad ja tegutsevad iseseisvalt, ning nende sobivus | ✅ |
| 4.2 | [RAG: oma andmete kasutamine vastuste allikana](04-agendid-ja-mootmine/02-rag.md) | Otsinguga täiendatud genereerimine ja oma teadmusbassi kasutamine | ✅ |
| 4.3 | [Pikaaegne mälu ja oleku haldus](04-agendid-ja-mootmine/03-pikaaegne-malu.md) | Kuidas hoida infot ja olekut seansside vahel | ✅ |
| 4.4 | [Mitme agendi arhitektuurid](04-agendid-ja-mootmine/04-mitme-agendi-arhitektuurid.md) | Kuidas jagada keeruka ülesande töö mitme agendi vahel | ✅ |
| 4.5 | [Hindamine: kuidas teada, kas süsteem on hea](04-agendid-ja-mootmine/05-hindamine.md) | Hindamismeetodid, testikomplektid ja kvaliteedi mõõtmine | ✅ |
| 4.6 | [Monitooring tootmises](04-agendid-ja-mootmine/06-monitooring.md) | Jälgimine, hoiatused ja kvaliteedi uurimine reaalses kasutuses | ✅ |
| 4.7 | [Jõudlus ja latentsus](04-agendid-ja-mootmine/07-joudlus-ja-latentsus.md) | Kiiruse optimeerimine ja kasutajakogemuse parandamine | ✅ |

### 5. tase — Suurte projektide professionaalne tase (`05-suurte-projektide-tase/`)

| # | Dokument | Kirjeldus | Olek |
| --- | --- | --- | --- |
| 5.1 | [Arhitektuur suures mahus](05-suurte-projektide-tase/01-arhitektuur-suures-mahus.md) | Arhitektuurilised otsused ja mustrid suurtes süsteemides | ✅ |
| 5.2 | [Meeskonnatöö ja standardid](05-suurte-projektide-tase/02-meeskonnatoo-ja-standardid.md) | Standardid, töövoog ja dokumentatsioon meeskonnas | ✅ |
| 5.3 | [Turvalisus ja andmekaitse (GDPR, audit)](05-suurte-projektide-tase/03-turvalisus-ja-andmekaitse.md) | Õigusnõuded, andmekaitse ja auditeeritavus | ✅ |
| 5.4 | [Kulustrateegia suures mahus](05-suurte-projektide-tase/04-kulustrateegia.md) | Kulude planeerimine ja optimeerimine suurtes projektides | ✅ |
| 5.5 | [Mudelite vahetamine ja drift](05-suurte-projektide-tase/05-mudelite-vahetamine.md) | Kuidas elada üle mudelite muutumist ja vahetust | ✅ |
| 5.6 | [Pidev täiustamine: mõõtmisest otsusteni](05-suurte-projektide-tase/06-pidev-taiustamine.md) | Kuidas viia mõõtmise andmed arukate otsusteni | ✅ |
| 5.7 | [Vastutus, eetika ja governance](05-suurte-projektide-tase/07-vastutus-ja-eetika.md) | Eetilised põhimõtted ja süsteemi juhtimine | ✅ |
| 5.8 | [Mallide teek: kontroll-loendid ja näidised](05-suurte-projektide-tase/08-mallide-teek.md) | Valmis mallid ja kontroll-loendid praktiliseks kasutamiseks | ✅ |

---

Käsiraamat on valmis: 34 dokumenti viies tasandis. Muudatuste ajalugu leiad [CHANGELOG.md](CHANGELOG.md)-ist.
