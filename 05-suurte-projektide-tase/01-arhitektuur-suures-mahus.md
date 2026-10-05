# 5.1 Arhitektuur suures mahus

> **Sihtpublik:** tehniline + juhtiv mitte-tehniline | **Eeltingimused:** [4.7 Jõudlus ja latentsus](../04-agendid-ja-mootmine/07-joudlus-ja-latentsus.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- nimetada, mis kasvades korrutub: süsteemid, inimesed, võtmed ja logid;
- selgitada neli arhitektuurimustrit ja öelda iga kohta, millal ta tasub;
- mõista AI-omast küsimust: mitu süsteemi kasutavad sama mudelit — üks mudeli vahetus ([5.5](05-mudelite-vahetamine.md)) või hinnamuutus mõjutab kõiki korraga;
- pidada arhitektuurilehte — ühte lehte kõigi AI-komponentide ja sõltuvustega;
- küsida skaleerimise küsimus „mis juhtub, kui kasutajad ×10?“ ja teada, mille peale vastates vaadata.

## Lihtsalt öeldes

> [3.1 API integratsioonid](../03-susteemi-ulesehitus/01-api-integratsioonid.md) lõpus oli üks süsteem, mis kutsus üht mudelit — ja see töötas. Suures mahus küsitakse teist asja: mitte „kas süsteem töötab?“, vaid „kas keegi suudab süsteeme käigus hoida, kui neid on tosin ja neid puudutab mitu inimest?“. Arhitektuur on otsuste kogum: kuidas töö tükkideks jaotatakse, kuidas tükid räägivad ja kust läheb läbi kõik, mis maksab.

## Mis muutub, kui süsteem kasvab

Kasv pole ainult „rohkem sama“: neli asja korrutub ja vahetab loomust.

| | Ühes süsteemis | Suures mahus |
|---|---|---|
| Süsteemid | üks tagastusvoog | mitu teenust (ingl k *service* — eraldiseisvalt hooldatav süsteemi osa): klassifitseerija, otsing, saatmine — igaüks elab oma elu |
| Inimesed | üks arendaja, kes teab kõike peast | meeskond — keegi ei tea enam kõike peast, teadmine peab olema kirjas |
| Võtmed | üks API-võti (ingl k *API key* — süsteemi pass ja kassasüsteem) | iga teenuse oma võti — ja küsimus, kust neid üle vaadata |
| Logid | üks logi, mille läbi loeb üks inimene | palju logisid — tarvis arusaadavat ülevaadet, mitte kümme faili |

Iga rida tekitab küsimuse — ja neli mustrit allpool ongi nende vastused.

> **Lihtsalt öeldes:** kasv ei lisa üksikasju — ta korrutab neid. Kaks süsteemi tähendab kahte võtit, kahte logi, kahte kohta, kus viga peituda võib. Seepärast pole suure mahus retsept „töötab ju ka rohkemaga“, vaid kord: jaota tükkideks, lase tükkidel rääkida kokkulepitud viisil ja pane kõik, mis maksab, ühe koha taha.

## Neli arhitektuurimustrit

| Muster | Mis see on | Millal kasu |
|---|---|---|
| **Teenuste eraldus** | süsteem jaotatakse teenusteks: iga funktsioon on oma osa — klassifitseerija, otsing, saatmine —, mida hooldatakse eraldi | kui osi uuendatakse eri rütmis või eri inimeste käes; kui ühe osa viga ei tohi kõiki peatada |
| **Sündmustepõhisus** (ingl k *event-driven*) | teenused räägivad sündmuste (ingl k *event* — teade, mis käivitab tegevuse) kaudu: „uus kiri saabus“ käivitab voo — keegi ei kutsu kedagi otse | kui saatjaid ja kuulajaid on või tuleb juurde: uue teenuse võib panna sündmust kuulama ilma olemasolevaid muutmata |
| **Keskne juurdepääsu värav** (ingl k *gateway* — keskne koht kõigile mudelikutsetele) | kõik mudelikutsed lähevad ühe koha kaudu: kulude nähtavus, võtmete haldus ja limiitide kontroll ühes kohas | alati, kui mudelit kutsub rohkem kui üks teenus — suures mahus praktiliselt alati |
| **Keskkondade eraldus** | keskkond (arendus/test/tootmine): katsed arendus- ja testkeskkonnas, tootmine teenib päris kasutajaid | alati, kui on midagi kaitsta: tootmisandmetega ei katsetata ja tootmisvõtmega ei testita |

> **Lihtsalt öeldes:** mustrite põhimõtted on igapäevased. Teenuste eraldus on firma osakondadeks jaotamine — igaüks teeb oma asja ja kui üks haigestub, ei seisa kogu maja. Sündmustepõhisus on kuulutustahvel: saatja riputab teate üles, uus osakond loeb samu teateid kaasa. Juurdepääsu värav — lühidalt keskne värav — on ühine kassa, kust näeb kõiki kulutusi. Keskkondade eraldus on prooviruum: müügiks mõeldut proovitakse enne mujal.

Mustreid ei võeta kõiki korraga — igaüks maksab ise midagi: teenusteks jaotus toob vahelooduse, sündmused järjekorra ja hilinemise, värav on veel üks hooldatav osa. Väike süsteem, mida hoiab käigus üks inimene, ei vaja neist ühtegi; esimene märk, et aeg on käes, on lihtne lause arendajalt: „ma enam päriselt ei tea, mis kus jookseb.“

Kas arhitektuur kannab, näitab **skaleerimise test**. Skaleerimine (ingl k *scaling*) tähendab kasvuga arvestamist enne kasvu: „mis juhtub, kui kasutajad ×10?“ Vasta kolmes punktis:

- **Limiidid.** Kas pakkuja liitumislimiit kannab kümnendkordse kutsearvu — ja kas üks hooletu silmus võtab teistelt koha ära ([3.1](../03-susteemi-ulesehitus/01-api-integratsioonid.md) limiitidest)?
- **Kulud.** Kas kümnendiline arve on keegi enne näinud ja kinnitanud ([3.6 Kulude haldamine](../03-susteemi-ulesehitus/06-kulude-haldamine.md); suure pildi jaoks [5.4](04-kulustrateegia.md))?
- **Latentsus.** Kas ooteaeg jääb talutavaks ka koormuse tipus — keskmisena ja halvima juhtumina ([4.7](../04-agendid-ja-mootmine/07-joudlus-ja-latentsus.md))?

Kui mõni vastus on „ei tea“, leidis test nõrga koha enne kasutajat.

## Üks mudel, palju süsteeme: keskne värav

AI-l on suures mahus üks eriti terav küsimus: **paljud süsteemid kasutavad sama mudelit.** Kui pakkuja kuulutab välja mudeli vahetuse ([5.5](05-mudelite-vahetamine.md)) või tõstab hinda, mõjutab see mitte ühte süsteemi, vaid kõiki korraga. Ilma korralduseta avastatakse see siis, kui süsteemid üksteise järel lakkavad töötamast.

Vastuseks on keskne värav ja kirjas olevad sõltuvused:

```
Ilma väravata:                     Väraga:

liigitaja ──────► mudel            liigitaja ──┐
vestlusbot ────► mudel            vestlusbot ┼─► keskne värav ─► mudel
kuuraport ──────► mudel            kuuraport ──┘        │
                                                        └─► üks logi: kes, millist, palju
kolm võtit, kolm kohta muuta       üks võti, üks koht muuta
```

Värava kõrvale käib nimekiri sõltuvustest — milline süsteem millist mudelit kasutab ja miks:

| Teenus | Mudel | Miks just see |
|---|---|---|
| Kirjade liigitaja | väike | lihtne samm — kiirus ja hind ([4.7](../04-agendid-ja-mootmine/07-joudlus-ja-latentsus.md)) |
| Vestlusbot | suur | klient ootab — kvaliteet loeb |
| Kuuraport | suur | kord kuus — kiirus ei loe |

Kui vahetus või hinnamuutus ees ootab, on nimekiri juhile vastus küsimusele „keda puudutab?“ ja tehnikule tööplaan: väravaga on muudatus ühe koha asja, ilma väravata korduv retk süsteemist süsteemi.

> **Lihtsalt öeldes:** keskne värav on ühine kassasüsteem ja lülitilaud ühes: kõik, mis maksab, läbib ühe kassa — ja ühe lülitiga lülitad ümber kogu maja. Aga lülitilaud aitab ainult siis, kui kirjas on, milline juhe kuhu läheb; selleks on sõltuvuste nimekiri.

## Arhitektuurileht: üks leht, mis peab olema õige

**Arhitektuurileht** (süsteemide ja sõltuvuste üheleheküljeline skeem) on üks leht, kus on kõik AI-komponendid ja nende sõltuvused: mis teenused on olemas, mis käivitab mida, mis läbib väravat, millist mudelit keegi kasutab.

```
KODUTOA POOD — AI-SÜSTEEMID (arhitektuurileht, uuendatud 2026-10)

e-pood ──(„uus kiri“)──► tagastusteenus ──► keskne värav ──► mudelid (vt sõltuvuste tabelit)
                             │
                             └──► kavand ► Piret kinnitab ► saatmine
Võtmed: väravas | Logid: värava ühislogi | Keskkonnad: arendus / test / tootmine
Omanik: Tanel
```

Kolm reeglit, mis lehe õigeks teevad:

- **üks leht** — kui ei mahu, on süsteem muutusteks juba liiga keeruline;
- **uuendatakse muudatuse päeval** — vana skeem on hullem kui puuduv, sest ta sööb usaldust;
- **omanik kirjas** — iga süsteemi juures üks nimi: vastutus on alati inimesel, mitte süsteemil ([1.6 Rollid ja vastutus](../01-alused/06-rollid-ja-vastutus.md)).

Lehe põhjus on inimlik: kui arendaja lahkub, saab järgmine aru.

## Näide samm-sammult: Kodutoa pood kaupluste ketiks

**Lähtepunkt.** Mari Kodutoa pood — Tanel arendab, Piret kinnitab kavandeid — on kasvanud 12 kauplusega ketiks. Tagastusvoog, mis [2.5](../02-praktika/05-esimene-workflow.md)-s ehitati ja [3.1](../03-susteemi-ulesehitus/01-api-integratsioonid.md)-s süsteemi viidi, töötas ühes kaupluses suurepäraselt.

**1. Kuidas sinna jõuti.** Iga uue kaupluse juures kopeeris Tanel voo: oma võti, oma logi, sama prompt. Kiire — midagi uut ei ehitatud.

**2. Mis läks katki.**

- **12 võtit.** Küsimus „kumma võtme all see kulu?“ jäi vastuseta.
- **12 logi.** Kliendi kiri kauplusest 7 jäi kavandita; põhjus leiti kümne faili läbi lugedes.
- **Üks mudeli vahetus murdis KÕIK.** Pakkuja teatas vana mudeli toetuse lõpust. Tanel parandas ühe voo, siis teise — kolm päeva olid osad kauplused kavanditeta ja Piret tegi tööd käsitsi.

**3. Lahendus: üks keskne tagastusteenus.**

- **Üks teenus, mitte kaksteist kloon:** üks klassifitseerija, üks kavandaja, üks API-võti.
- **Kauplused kutsuvad sündmusel:** „uus kiri saabus kaupluses 7“ — kauplus ei pea teadma, mis taga juhtub; uue kaupluse lisamine on uus saatja, mitte uus süsteem.
- **Kõik mudelikutsed värava kaudu:** kulu ja limiidid nähtavad ühest kohast.
- **Keskkonnad eraldi:** Tanel proovib uut prompti arenduskeskkonnas ilma päris kliendita; tootmine teenib korraga kõiki 12.
- **Arhitektuurileht ühel lehel:** teenused, mudelid, võtmed alati käekõrval; värava ühislogist sai kõrvalsaadusena monitooringu ([4.6](../04-agendid-ja-mootmine/06-monitooring.md)) ühtne allikas.

**4. Tulemus.**

| | Enne: 12 kloon | Pärast: üks teenus |
|---|---|---|
| API-võtmed | 12 | 1 (väravas) |
| Mudeli vahetus | 12 kohta, päevad | 1 koht, tunnid |
| „Kes kulutas?“ | arvamus | värava logist täpselt |
| Uus kauplus | uus voo koopia | üks kirje — teenus on juba olemas |

> **Lihtsalt öeldes:** Mari ei tea, mis väravas toimub, ja tal pole vajagi — ta teab ainult, et küsimus „mis juhtub, kui mudel vahetub?“ on nüüd üks otsus, mitte kaksteist remonti. Kui Tanel homme lahkub, loeb järgmine ühe lehe ja jätkab.

## Kokkuvõte

- **Kasv korrutab:** üks süsteem → mitu teenust, üks arendaja → meeskond, üks API-võti → mitu, üks logi → vajadus ülevaate järele.
- **Neli mustrit vastavad neljale küsimusele:** teenuste eraldus (kuidas tükkideks jaotada), sündmustepõhisus (kuidas räägitakse), keskne juurdepääsu värav (kust läheb läbi kõik, mis maksab), keskkondade eraldus (kus katsetatakse).
- **Üks mudel, palju süsteeme:** mudeli vahetus ([5.5](05-mudelite-vahetamine.md)) või hinnamuutus mõjutab kõiki korraga — keskne värav ja kirja pandud sõltuvused teevad muudatuse ühe koha asjaks.
- **Arhitektuurileht** on üks leht, alati õige ja omanikuga: kui arendaja lahkub, saab järgmine aru.
- **Skaleerimise test** — „kasutajad ×10?“ — vaatab limiite ([3.1](../03-susteemi-ulesehitus/01-api-integratsioonid.md)), kulusid ([3.6](../03-susteemi-ulesehitus/06-kulude-haldamine.md), [5.4](04-kulustrateegia.md)) ja latentsust ([4.7](../04-agendid-ja-mootmine/07-joudlus-ja-latentsus.md)); „ei tea“ on vastus, mida parem kuulda enne kasutajaid.
- Kokkulepped selle üle, kes tohib muuta ja kuidas kinnitatakse, on [5.2 Meeskonnatöö ja standardid](02-meeskonnatoo-ja-standardid.md) teema.

## Mis edasi?

- eelmine → [4.7 Jõudlus ja latentsus](../04-agendid-ja-mootmine/07-joudlus-ja-latentsus.md) (4. tase on läbi)
- järgmine → [5.2 Meeskonnatöö ja standardid](02-meeskonnatoo-ja-standardid.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
