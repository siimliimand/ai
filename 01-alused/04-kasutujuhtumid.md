# 1.4 Kus AI-automatiseerimine juba töötab: kasutujuhtumid

> **Sihtpublik:** kõik | **Eeltingimused:** [1.2 Võimalused ja piirid: mida automatiseerida tasub](02-voimalused-ja-piirid.md) ja [1.3 Promptide põhitõed](03-promptide-pohitoed.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- nimetada viis valdkonda, kus organisatsioonid AI-automatiseerimist juba igapäevaselt kasutavad;
- igas valdkonnas eristada, mida teeb AI-mudel ja mida inimene — ja miks just selline jaotus töötab;
- lihtsa arvutusega hinnata, kui palju aega üks automatiseeritud ülesanne kokku hoida võib;
- eristada realistlikku kasu liigsetest lubadustest: mis on tüüpiline esimene võit ja kus imesid pole.

## Lihtsalt öeldes

> Enamik ettevõtteid ei alusta millegi futuristlikuga. Nad alustavad sealt, kus inimene kulutab tunde samasuguste tekstide kirjutamisele ja andmete sisestamisele: vastused klientidele, müügi- ja turundustekstid, arvete andmed, pikkade dokumentide kokkuvõtted. AI-mudel teeb kavandi, inimene vaatab üle ja kinnitab. See lihtne jaotus — **mudel valmistab ette, inimene kinnitab** — on tänapäeva töötava AI-automatiseerimise selgroog.

## Kasutujuhtumite kaart

Ülevaade valdkondadest — iga neist lähemalt allpool.

| Valdkond | Mida AI automatiseerib | Mida inimene teeb | Realistlik kasu |
|---|---|---|---|
| **Klienditeenindus** | Küsimuste klassifitseerimine, vastuskavandid, korduvate küsimuste vastused | Vaatab üle ja kinnitab, võtab üle keerulised juhtumid | Vastamise ettevalmistus minutitest sekunditeks |
| **Müük ja turundus** | Tekstide kavandid, e-kirjade personaliseerimine, sisu loomine (ingl k *content creation*) | Hoiab brändi häält, kontrollib fakte, valib lõpliku | Päevade pikkune ettevalmistus mahub tundidesse |
| **Haldus ja finantsid** | Arvete andmete eraldamine, andmesisestus, dokumentide kokkuvõtted | Kontrollib numbreid, kinnitab, otsustab ebaselge korral | Mitu tundi nädalat rutiini maha |
| **Sisemine teadmine** | Vastused ettevõtte enda juhendite põhjal | Hoiab juhendid ajakohased, lahendab erandjuhtud | Kiirem sisseelamine, vähem katkestatud kolleegide tööd |
| **Tarkvaraarendus** | Koodi- ja testikavandid, dokumentatsioon | Koodi ülevaade, arhitektuuriotsused | Arendaja aeg rutiinilt loova töö juurde |

## Klienditeenindus

Kõige levinum koht, kus AI-d juba kasutatakse, on kliendikirjade töötlus.

**Mida AI teeb.** Iga sissetulev kiri liigitatakse — kas see on arveküsimus, tagastustaotlus, tellimuse staatuse päring või kaebus — ja suunatakse õigesse kohta. Mudel koostab vastuse kavandi, kasutades kliendi andmeid süsteemist: tellimuse number, saatmise kuupäev, arve staatus. Täiesti korduvatele küsimustele („kui pikalt kehtib tagastusõigus?“) võib süsteem iseseisvalt vastata, aga ainult tekstiga, mille inimene on eelnevalt kinnitanud.

**Mida inimene teeb.** Klienditeenindaja vaatab kavandid üle ja kinnitab need — seda nimetatakse inimese kinnitusahelaks (ingl k *human-in-the-loop* — töövoog, kus inimene kinnitab süsteemi tulemuse enne, kui see jõuab kliendini). Keerulised, pahased ja erandlikud juhtumid lähevad alati inimesele.

> **Lihtsalt öeldes:** mudel, kellele fakte kaasa ei anta, ei keeldu vastamast — ta leiutab usutava vastuse. See on hallutsinatsioon (ingl k *hallucination* — mudeli kindlalt öeldud, aga vale vastus), ja klienditeeninduses tähendab see võõrast hinnakirja kliendile saatmist. Seepärast annab süsteem mudelile alati andmed kaasa ja inimene kinnitab enne saatmist.

**Realistlik kasu.** Vastuse ettevalmistus lüheneb minutitest sekunditeks ja klienditeenindaja päev vabaneb selle töö jaoks, mida masin teha ei oska — rahulolematu kliendi rahustamiseks ja erandite lahendamiseks.

## Müük ja turundus

**Mida AI teeb.** Kirjutab esimese versiooni: müükirjad, tootekirjeldused, uudiskirjad, sotsiaalmeedia postitused, blogiplaanid (sisu loomine, ingl k *content creation*). Personaliseerib e-kirju — sama põhisõnum kohandatakse iga saaja konteksti järgi. Samuti teeb mudel kümmekond varianti samast sõnumist, et testides leida parem.

**Mida inimene teeb.** Hoiab brändi häält: mudel kirjutab tubli keskmise, aga sinu ettevõtte kõla on sinu vara. Kontrollib fakte — hinnad, tingimused, lubadused — ja teeb lõpliku valiku.

**Realistlik kasu.** Kampaania ettevalmistus, mis varem võttis päeva, mahub nüüd poole päeva. See on sageli kõige kiiremini tasuv koht, sest töö on puhast tekstitööd — täpselt see, milles keelemudelid kõige tugevamad on. Hea kavandi saamine sõltub aga selgest juhisest: kuidas prompt (ingl k *prompt* — mudelile antav juhis) üles ehitada, õpetab [1.3 Promptide põhitõed](03-promptide-pohitoed.md).

## Haldus ja finantsid

**Mida AI teeb.** Eraldab dokumentidest struktureeritud andmed — arvenumbri, kuupäeva, summa, tasuja — ja paneb need vormi, millest raamatupidamissüsteem aru saab (andmesisestus). Teeb pikkadest dokumentidest kokkuvõtted: leping, hangetingimused, kvartaliaruanne. Kirjutab rutiinsete e-kirjade kavandeid: kinnitus, meeldetuletus, vastus „sai kätte“.

**Mida inimene teeb.** Kontrollib numbreid enne kinnitamist — raha juures pole „ligilähedast õigust“. Kui dokument on ebaselge, poolik või ebatavaline, suunab süsteem selle inimesele, mitte ei paku.

**Realistlik kasu.** Ühe dokumendi käsitlemine lüheneb minutitest sekunditeks. Kümnete arvete või kirjade korral päevas on kogus mitu tundi nädalas — ilma et keegi kontrolli kaotaks, sest iga sisestuse kinnitab ikkagi inimene.

## Sisemine teadmine (RAG)

Siin peab vastus põhinema *teie enda* teadmistel, mitte mudeli üldisel mälul. Töötaja küsib: „mitu päeva puhkust mul jäänud on?“, „kuidas läpakat tellida?“ — ja saab vastuse otse ettevõtte juhenditest. Tavamudel neid juhendeid ei tea: ilma nendeta ta arvab. Lahendust, kus süsteem enne vastamist otsib vastused ettevõtte enda dokumentidest, nimetatakse RAG-iks (ingl k *retrieval-augmented generation* — otsinguga täiendatud genereerimine).

**Mida AI teeb.** Otsib süsteemi abil vastused ettevõtte enda juhenditest ja koostab nende põhjal vastuse.

**Mida inimene teeb.** Hoiab juhendid ajakohased ja vastab eranditele.

**Realistlik kasu.** Uued töötajad harjuvad kiiremini, kogenud kolleegide tööd katkestatakse vähem. See on üks väärtuslikumaid kasutusviise — seda käsitletakse põhjalikult dokumendis [4.2 RAG](../04-agendid-ja-mootmine/02-rag.md).

## Tarkvaraarendus

Arendajad kasutavad AI-d juba igapäevaselt. Jaotus on sama mis kõikjal — mudel kirjutab kavandi, arendaja loeb läbi, parandab ja vastutab lõpptulemuse eest. Tegu on tavapraktikaga, mitte eksperimendiga.

**Mida AI teeb.** Koostab koodi- ja testikavandeid, dokumentatsiooni ning esimesi parandusversioone vigade otsimisel.

**Mida inimene teeb.** Vaatab koodi üle ja teeb arhitektuuriotsused.

**Realistlik kasu.** Rutiinne koodikirjutus (mallid, testid, dokumentatsioon) nihkub masinale, arendaja aeg läheb arhitektuurile ja ülevaatusele.

---

Kuidas hinnata, kas konkreetne ülesanne on üldse automatiseerimist väärt, käsitleb [1.2 Võimalused ja piirid](02-voimalused-ja-piirid.md) — siin seda ei korrata. Kuidas üksikkäsk muutub süsteemiks, näitab [1.5 AI-automatiseeritud süsteemi anatoomia](05-susteemi-anatoomia.md); kuidas valitud ideed reaalselt ehitada, õpetab 2. tase — alates töökindlatest juhistest kuni esimese valmis töövooni (ingl k *workflow* — töövoog, automatiseeritud sammude jada).

## Mida oodata realistlikult

Arvutus on lihtne ja aus: **säästud = ühe tegevuse kestus × korduste arv**. Näide: kliendikirjale vastamine võtab keskmiselt 8 minutit ja kirju tuleb 40 päevas — 5 tundi 20 minutit päevas. Kui kavand lühendab inimese osa kahe minutini, on teoreetiline sääst neli tundi päevas. Reaalsuses süüakse osa säästust ülevaatamise ja parandamisega, aga isegi pooled säästust on päris raha: umbes kümme töötundi nädalas ühe tööprotsessi juures.

Kolm reaalsuse reeglit:

1. **Tüüpiline esimene võit on korduvad tekstitööd** — kavandid, kokkuvõtted, andmete eraldamine. Need on massilised, aeganõudvad ja veahind on madal, sest inimene kinnitab tulemuse enne kasutamist.
2. **Ära oota imesid keerukate otsuste juures.** Lepingu lõppotsus, vaidluskäsitlus, strateegiline valik — mudel võib olla ettevalmistaja, aga mitte otsustaja. Seal, kus vea hind on suur, jääb viimane sõna inimesele.
3. **Alusta ühest konkreetsest ülesandest ja mõõda.** Üks valitud tegevus, ajaarvestus enne ja pärast — nii põhineb otsus arvudel, mitte tunnetel.

> **Lihtsalt öeldes:** kasumiarvutus mahub taskuarvutisse: minutid × kordused = tunnid. Kui välja tuleb vähemalt paar tundi nädalas, on teema enamikus ettevõtetes juba tasuv — ja selliseid kohti on tavaliselt mitu.

## Näide samm-sammult: tagastustaotluse töötlus

Olukord: e-poes saabub päevas umbes 40 kliendikirja, millest suur osa on tagastustaotlused. Varem luges klienditeenindaja iga taotluse, kontrollis ostu kuupäeva ja tagastustingimusi ning kirjutas vastuse — keskmiselt 5 minutit, rohkem kui kolm tundi päevas (40 × 5 min = 3 h 20 min, realistlikult pooled).

Voog käib nii:

1. **Klient kirjutab:** „Tahaksin toote X tagasi saata — see ei sobinud.“
2. **AI klassifitseerib kirja:** tagastustaotlus, mitte info küsimus. Kui süsteem ei oska kirja kindlalt liigitada, suunab ta selle otse inimesele — see on normaalne varutee, mitte ebaõnnestumine.
3. **AI koostab vastuskavandi:** ostu andmed võetakse süsteemist kaasa, juhised määravad reeglid (tagastusaeg 14 päeva, tagastuskulu 5 eurot, raha tagasi 3 tööpäeva jooksul) ja mudel kirjutab vastuse.
4. **Klienditeenindaja kontrollib ja kinnitab:** kas ost vastab tingimustele (nt pole tagastustähtaega ületanud) — kui jah, läheb vastus ühe klõpsuga klienti. Pahased ja erandlikud juhtumid võtab inimene ise üle.

**Kus võib minna valesti.** Mudel võib jätta märkamata, et ost on tagastustähtaja ületanud, ja koostada siiski positiivse vastuse — seepärast näitab süsteem kontrolliks ostu kuupäeva ja reegli täitmise seisundit ning inimene kinnitab enne saatmist. Kui ostu andmeid kaasa ei anta, võib mudel tingimused välja mõelda. Ja kiri, mis puudutab midagi, mida andmetest ei näe (näiteks juriidiline nõue), läheb alati inimesele.

**Kui palju kokku hoitakse.** Inimese osa langeb 5 minutilt umbes 1–2 minutini. 40 taotlust päevas tähendab teoreetiliselt kuni ca kolm vabastatud tundi päevas; reaalsuses süüakse osa säästust ülevaatamisega, aga isegi pooled — umbes poolteist tundi päevas — on enam kui seitse tundi nädalas.

> **Lihtsalt öeldes:** parim kontroll on ühe omaenda korduva tekstiülesande kirjapanek: mitu minutit see võtab, mitu korda nädalas juhtub, kes kavandi üle vaataks. Kolm vastust — ja otsus on pooleldi tehtud.

## Kokkuvõte

- **Kõige levinum muster on kavand + kinnitus:** AI-mudel valmistab ette, inimene kinnitusahelas vaatab üle ja kinnitab.
- **Viis valdkonda, kus see juba töötab:** klienditeenindus, müük ja turundus, haldus ja finantsid, sisemine teadmine (RAG), tarkvaraarendus.
- **Kasu arvutus on lihtne:** ühe tegevuse aeg × korduste arv; tüüpiline esimene võit on korduvad tekstitööd.
- **Keerukad otsused jäävad inimesele** — mudel valmistab ette, aga ei otsusta.

## Mis edasi?

- eelmine → [1.3 Promptide põhitõed](03-promptide-pohitoed.md)
- järgmine → [1.5 AI-automatiseeritud süsteemi anatoomia](05-susteemi-anatoomia.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
