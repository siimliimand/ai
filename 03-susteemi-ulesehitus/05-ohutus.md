# 3.5 Ohutus: piirid ja inimene kinnitusahelas

> **Sihtpublik:** kõik | **Eeltingimused:** [3.4 Vead ja veakäsitlus](04-vead-ja-veakasitlus.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada ohutuse põhiprintsiipi: mida rohkem iseseisvust süsteemile annad, seda suurem on vea kahjum;
- paigaldada kaitsekihid neljas kihis: mida tohib, mis vajab enne kinnitust, mille peal süsteem peatub, mis jääb jäljele;
- nimetada tundlikud valdkonnad ja põhjendada, miks inimene kinnitusahelas on seal alati;
- kirjutada ohutusreeglid süsteemivestluse juhisesse ja teada, miks juhis üksi ei kaitse;
- enne käikuandmist läbi viia ohutuse test kahe küsimusega.

## Lihtsalt öeldes

> Küsimus pole „kas süsteem eksib“ — eksib, näitas juba 3.4. Küsimus on: kui ta eksib, kui kaugele viga jõuab, enne kui keegi selle kinni paneb? Hästi ehitatud süsteemis peatub viga ekraanil, halvasti ehitatud süsteemis jõuab viga kliendi e-posti või pangaarvele. Vahe pole mudelis — vahe on disainis. **Ohutus on disaini küsimus, mitte lootus.**

## Ohutuse põhiprintsiip: iseseisvus ja kahjum kasvavad koos

Iga uus iseseisvus, mille süsteemile annad, suurendab kasu ja kahju korraga. Süsteem, mis ainult loeb andmeid ja koostab visandeid, eksib ohutult: raisatud minut. Süsteem, mis ise kirju saadab, eksib klientide silmis. Süsteem, mis ise makseid teeb, eksib rahas — ja rahaline viga on tihti pöördumatu.

| Süsteemi iseseisvus | Vea maksimumkahjum | Miinimum-kaitse |
|---|---|---|
| loeb ja koostab visandeid | raisatud aeg | inimese ülevaatus enne kasutust |
| saadab ja muudab ise | viga jõuab kliendini | kinnituspunktid reeglite järgi + jälge |
| teeb tehinguid ise | otsene ja pöördumatu kahjum | inimese kinnitus alati + summapiirid + peatusnupp |

Üks asi ei muutu ühelgi astmel: vastutus süsteemi vea eest jääb inimesele ja ettevõttele — klient ja seadus pöörduvad sinu, mitte mudeli poole ([1.6 Rollid ja vastutus projektis](../01-alused/06-rollid-ja-vastutus.md)). Inimese koht süsteemis on seepärast disaini osa, mitte silmapilk.

Märk ette: kui ülesannet ise keelavad [1.2](../01-alused/02-voimalused-ja-piirid.md) punased lipud, pole lahendus piirangud, vaid otsus mitte automatiseerida.

## Kaitsekihid: mida tohib, mis vajab kinnitust, mille peal peatub

Ohutus pole üks reegel, vaid kaitsekihid (ingl k *safeguard* — piirang süsteemi disainis), mis koos töötavad.

### 1. kiht — mida süsteem tohib teha

Kõige kindlam kaitse on teha keelatu võimatuks, mitte üksnes keelatuks. Pane süsteemi õigused tehniliselt paika: mida ta loeb, milliseid toiminguid ta saab käivitada. Süsteemil, kellel pole kustutamise õigust ega saatmise teed, pole neid asju võimalik teha — ükskõik mida ta arvab. Kõik, mida pole eraldi lubatud, on keelatud.

### 2. kiht — mida peab enne inimene kinnitama

Mõned tegevused pole keelatud, aga vajavad allkirja — mida pöördumatum tegevus, seda kindlamalt seisab enne inimene. Tüüpilised kinnituspunktid:

- **rahalised toimingud** — makse, hinnakinnitus, allahindlus: alati inimese allkiri;
- **saatmine kliendile esimest korda** — esimene kiri on kohtumine, millest sõltub usaldus;
- **andmete muutmine** — kirje parandamine, kustutamine või ülekanne teise süsteemi.

Kinnitamine peab olema kiire — üks kriteerium, üks klõps. Kui ülevaatamine võtab rohkem aega kui töö ise, hakkavad inimesed varsti pimesi kinnitama.

### 3. kiht — mille peale süsteem peab peatuma

Vaja on peatusnuppu (ingl k *kill switch* — võimalus süsteem ühe sammuga maha võtta), kasutatavat enne kriisi, mitte selle ajal. Kolm nõuet:

- **üks samm.** Peatamine on üks klõps — mitte sätete otsimine kümnes menüüs.
- **keegi teab, kus ta on.** Peatusnupp kuulub haldajale (rollidest 1.6); kui ta on puhkusel, teab teine inimene, kus nupp on.
- **süsteem peatub ka ise.** Kümme järjestikust viga panevad töövoo seisma, inimene vaatab enne jätkamist üle.

### 4. kiht — jälge

Iga automaatne tegevus jääb jälge (ingl k *audit trail* — iga automaatse tegevuse kirje): mis, millal ja miks (milline reegel või sisend selle käivitas). Jälg on ainus viis pärast viga välja selgitada, mis juhtus — ja enne viga näidata, et süsteem käitus reeglite järgi.

> **Lihtsalt öeldes:** neli kihti on nagu panga kaart: kaart avab ainult sinu konto (mida tohib), suur ülekanne vajab allkirja (kinnitus), kahtlase tehingu peal blokeerib pank kaardi (peatus) ja kõik jääb väljavõttesse (jälg). Ükski kiht üksi ei kaitse — kaitseb nende koosmõju.

## Tundlikud valdkonnad: kus inimene peab alati ahelas olema

Enamikus töövoogudes võib kinnitus kergeneda, kui süsteem end tõestab. Tundlikus valdkonnas — kus üks viga puudutab tervist, raha või inimese õigusi — ära seda tee: seal on inimene kinnitusahelas (ingl k *human-in-the-loop* — töövoog, kus inimene kinnitab tulemuse enne kasutust) kogu ajaks, mitte ainult eranditel.

| Valdkond | Miks riskantne | Miinimum-ohutus |
|---|---|---|
| Tervisenõuanded | süsteem ei tunne inimese seisundit; vale soovitus võib tervist kahjustada | vastab ainult kataloogi andmetega; nõuanded suunab inimesele; erandid logitakse |
| Rahalised kinnitused | viga on otse ja pöördumatult rahas | rahalisi kinnitusi süsteem iseseisvalt ei tee — kõik rahalised toimingud läbivad inimese allkirja |
| Lepingulised otsused | üks sõna muutub õiguslikuks kohustuseks | süsteem koostab visandi; kinnitab ja allkirjastab inimene |
| Värbamisotsused | otsus puudutab inimese elu; mudel võib kajastada eelarvamusi | süsteem sorteerib ja kokku võtab; otsustab ja põhjendab inimene |

Muster: süsteem tohib kokku võtta, otsida ja visandada — mitte otsustada ega nõustada. Nii jääb risk inimese kätte, kes vastutab.

## Ohutusreeglid süsteemi juhises — ja nende piirid

Süsteemivestluse juhis (ingl k *system prompt*) on prompt (mudelile antav juhis), mis töötab iga vestluse taustal. Ohutusreeglid kuuluvad siia kirja, sõnastatuna kahel poolel:

1. **Keeld selgelt.** „Ära anna meditsiinilisi ega ravimite koostoimete nõuandeid.“
2. **Asendus sama selgelt.** „Kui klient küsib tervise kohta nõu, ütle viisakalt, et see kuulub apteekrile, ja paku apteekri kontakti.“ Keeld ilma asenduseta jätab mudelile tühiku, mille ta tihti ikkagi täidab.

> **HOIATUS:** juhises kirjas reegel pole garanteeritud kaitse — pika vestluse või osava sisendi juures võib mudel reeglist mööda minna. Tehniline piirang (tegevus on võimatu) on tugevam kui sõna (tegevus on keelatud): pane reeglid juhisesse, aga tugine disainile. Kuidas keegi juhiseid ümber lüüa võib — nn pahatahtlikud juhised ehk prompt injection —, vaatab [3.7 Turvalisus](07-turvalisus.md).

## Ohutuse test enne käikuandmist

Enne, kui süsteem klientide või päris andmeteni pääseb, käi kirjalikult läbi kaks küsimust:

**1. „Mis on HALVIMASI asi, mida see süsteem teha saab?“**

Küsi mitte tõenäolise, vaid võimaliku kohta. Konkreetselt: „saadab kirju“ → „saadab sada kirja vale sõnastusega“; „muudab andmeid“ → „kustutab kirje, mida tagasi võtta ei saa“.

**2. „Kas süsteemi disain hoiab selle ära?“**

Nõua vastuseks konkreetset kihti: „võimatu, sest saatmise õigust pole“; „peatub, sest kinnituspunkt on enne“. Kui vastuseks on „loodame, et mudel on tubli“ või „juhises on kirjas“, pole see disain — see on lootus. Ilma kihita süsteem käiku ei saa.

> **Lihtsalt öeldes:** nagu autos: küsid mitte „kas sõidan täna ettevaatlikult“, vaid „kas turvavöö on olemas“. Ettevaatlikkus on lootus, vöö on disain.

## Näide samm-sammult: Ravikoda kolme variandiga

Väljamõeldud **„Ravikoda“** — e-apteegi vestluses. Klient kirjutab: „Kas saan ibuprofeeni koos oma vererõhurohuga võtta?“

**Variant A — halb: süsteem vastab ise.** Süsteem vastab kindlalt: „Jah, saab.“ Risk: AI-mudel ei tea kliendi tervist ja annab üldise teabe kindla nõuandena; kui infot pole, täidab ta tühiku usutava pakkumisega — hallutsinatsioon (mudeli kindlalt öeldud, aga vale vastus). Viga jõuab kliendini nägematult — ja vastutama jääb apteek, kes vastust ei andnud.

**Variant B — piiratud: juhis keelab.** Süsteemivestluse juhises: „Ära anna tervisenõuandeid — suuna apteekrile ja anna apteekri kontakt.“ Klient saab: „Selleks küsi apteekrilt — siin on kontakt ja tööaeg.“ Süsteem koostab lisaks kirja apteekrile, mille töötaja vastab. Oluliselt parem — aga piir on sõna: teistsuguse sõnastusega küsimusele („kas need kaks on kokkusobivad?“) ei pruugi juhis enam aidata.

**Variant C — täielik: juhis + tehniline piirang + jälg.** Süsteemil on tehniliselt ainult kataloogi andmete ligipääs: ta saab öelda „Ibuprofeen on laos, hind 4,20 €, retseptivaba“ — ja midagi muud. Tervise- või koostoimeküsimusel suunab süsteem apteekrile nagu variandis B; kõik suunatud küsimused logitakse jäljele: mis, millal, kuhu. Apteekri nädala kokkuvõte näitab, milliseid küsimusi tekib ja kus suunamisega eksiti.

| | A — halb | B — piiratud | C — täielik |
|---|---|---|---|
| Piir | puudub | juhises (sõna) | juhis + tehniline ligipääsupiirang + jälg |
| Kliendile | „Jah, saab“ | suunamine apteekrile + kiri apteekrile | kataloogi faktid; terviseküsimus apteekrile |
| Risk | vale nõuanne kliendini | juhis ei pruugi aidata | väike: faktid on õiged, nõuanne jääb inimesele |
| Kui süsteem eksib | keegi ei märka | apteek näeb kirja | jälg näitab, mida ja millal |

Variant C pole „vähem automatiseerimist“ — klient saab laoseisu ja hinnad ikka sekunditega. Automatiseeritud on see, mis on ohutu; inimese kinnitusahelas on see, mis on tundlik.

## Kokkuvõte

- **Iseseisvus ja kahjum kasvavad koos** — iga uus õigus suurendab vee maksimumi, aga vastutus jääb alati inimesele ([1.6](../01-alused/06-rollid-ja-vastutus.md)).
- **Neli kaitsekihti:** mida tohib (tehniline piirang), mis vajab kinnitust (inimene enne), mille peal peatub (peatusnupp ühe sammuga) ja mis jääb jäljele (mis, millal, miks).
- **Tundlikes valdkondades — tervis, raha, lepingud, värbamine — on inimene kinnitusahelas alati**, mitte ainult eranditel.
- **Juhis süsteemivestluses on vajalik, aga mitte piisav** — sõna ei pea vastu, tehniline piirang peab.
- **Ohutuse test:** „Mis on halvim, mida see süsteem teha saab?“ ja „Kas disain hoiab selle ära?“ — kui vastus on lootus, pole süsteem valmis.

## Mis edasi?

- eelmine → [3.4 Vead ja veakäsitlus](04-vead-ja-veakasitlus.md)
- järgmine → [3.6 Kulude haldamine: tokenid, hinnad, eelarve](06-kulude-haldamine.md)
- Pahatahtlikud juhised → [3.7 Turvalisus](07-turvalisus.md)
- Süsteemi juhtimine ja eetika → [5.7 Vastutus, eetika ja governance](../05-suurte-projektide-tase/07-vastutus-ja-eetika.md)
- Andmekaitse ja auditeeritavus → [5.3 Turvalisus ja andmekaitse](../05-suurte-projektide-tase/03-turvalisus-ja-andmekaitse.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
