# 4.3 Pikaaegne mälu ja oleku haldus

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [4.2 RAG](02-rag.md), [3.2 Konteksti haldamine](../03-susteemi-ulesehitus/02-konteksti-haldamine.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- eristada vestlusesene mälu ja pikaaegset mälu (info üle seansside) ning selgitada, miks mõlemad elavad süsteemi andmetes, mitte mudelis;
- otsustada, millised faktid tasub mälus hoida ja millised mitte;
- ehitada lihtsa kliendiprofiili (struktureeritud püsifaktide kogu) ja panna selle olulised väljad igale päringule kaasa;
- kavandada mälu uuendamist, unustamist ja inimese kontrolli muudatuste üle;
- hoida järjekindlalt olekut (ingl k *state* — kus protsess pooleli on) ning tunda kahte põhiriski: vananenud infot ja konteksti saastumist.

## Lihtsalt öeldes

> Mudel ei mäleta sind kunagi — mäletab su süsteem. Kui klient tuleb tagasi päevade pärast, teab mudel vaid seda, mis süsteem talle selle kõne jaoks märkmikust ette kirjutab. Pikaaegne mälu on see märkmik: sinna lähevad ainult olulised püsifaktid, need kirjutatakse üle, kui klient parandab, ja pannakse iga uue kõne alguses kaasa.

## Kaks mälu liiki: vestlus sees ja seansside vahel

Dokumendist [3.2](../03-susteemi-ulesehitus/02-konteksti-haldamine.md) tead põhitõde: mudelil pole mälu, iga päring on terve vestlus algusest peale ja „mäletamine“ on süsteemi töö. Seal vaadati ühte vestlust; siin vaatame selle piiri üle.

**Vestlusesene mälu.** Ühe seansi (ingl k *session* — ühenduse periood) jooksul hoiab süsteem vestluse ajalugu ja „asjade seis“ (oluliste faktide kirja, [3.2](../03-susteemi-ulesehitus/02-konteksti-haldamine.md) põhimõte). Selle mälu eluiga lõpeb seansiga: kõne lõppes, aken tühjenes, ja midagi jäi alles ainult siis, kui süsteem selle ise kirja pani.

**Pikaaegne mälu** on info üle seansside: kliendi eelistused, varasemad juhtumid, kokkulepped. Klient tuleb tagasi päevade pärast ja süsteem peab teda tundma — mitte seepärast, et mudel midagi mäletaks, vaid seepärast, et süsteem hoiab fakte oma andmebaasis ja paneb need enne iga uut seanssi kontekstiaknasse (ingl k *context window* — tekstihulk, mida mudel korraga näeb).

Kumbki mälu ei ela seega mudelis — mõlemad on süsteemi andmed, mida aknasse kantakse. Vahe on üksnes elueas: vestlusesene mälu kestab ühe seansi, pikaaegne mälu aastaid.

## Mida mälus hoida ja mida mitte

> **Lihtsalt öeldes:** mälu ei ole salvestusruum, kuhu kõik mahub — see on valik, mis järgmises kõnes abiks on. Mida vähem kirjeid, seda kindlam, et oluline ei lähe müra all kaotsi.

Hoida tasub kolme liiki kirjeid:

1. **Püsifaktid**, mis harva muutuvad: kliendi number, keele-eelistus, haldatav seade.
2. **Kokkulepped**, millest mõlemad pooled on selgelt rääkinud: „soovib e-posti teavitust“, „ei soovi reklaami“, „õhtuti ei pruugi telefoni vastu võtta“.
3. **Olulised ajaloolised juhtumid**, mis mõjutavad edasist teenindust: „28. septembril vahetati kanal, probleem jäi“ — järgmine kõne peab seda teadma.

Pole mõtet hoida:

- **Igapäevane müra.** Iga kõne sõnasõnaline tekst pole mälu, vaid logi. Kui kõik üles kirjutada, kasvab mälu müraks, mis surub olulise alla.
- **Tundlikud andmed ilma põhjuseta.** Igale faktile küsi: kas see aitab järgmises kõnes paremat vastust anda? Kui ei, ära salvesta; kuidas andmeid vähendada ja kaitsta, õpetab [3.7 Turvalisus](../03-susteemi-ulesehitus/07-turvalisus.md).

## Kuidas hoida: profiil enne keerulist

> **Lihtsalt öeldes:** kliendiprofiil on nagu arstivastuvõtu kaart — iga külastuse alguses vaadatakse kaart üle, mitte ei küsita patsiendi elulugu nullist. Kaart on lühike, struktureeritud ja hoolikalt peetud; täpselt niisugune piisab.

Alusta kõige lihtsamast: **struktureeritud kliendiprofiil** andmebaasis või isegi failis — kindlate väljadega (keel, eelistused, märkused), mitte vaba teksti hunnikuna. Süsteem paneb profiili olulised väljad igale päringule konteksti: see on „asjade seisu“ pikaajaline versioon, mis jätkub seal, kus 3.2 lõpetas. Kuidas selline päring välja näeb, näitab allpool Marti näide.

Sellest piisab enamikul juhtudel. Suurem lahendus tuleb pildile, kui varasemaid juhtumeid on tuhandeid ja need ei mahu ühessegi profiili: siis hoiab süsteem juhtumid eraldi ja otsib uue küsimuse järgi asjakohased välja — see on otsinguga täiendatud genereerimine, RAG (vt [4.2](02-rag.md)).

### Olek: kus protsess pooleli on

Faktide kõrval hoiab süsteem olekut. Tellimusel on olek: uus → saadetud → tagastatud. Kui olek läheb sassi, küsitakse klient uuesti või tehakse toiming kaks korda.

Seansside vahel on oleku hoidmine eriti tähtis poolelijäänud voogude puhul: klient alustas tagastust esmaspäeval, täitis vormi poole ja tuli tagasi neljapäeval. Kui süsteem hoiab voo olekut, jätkub see sealt, kus jäi; kui ei hoi, peab klient kõik uuesti tegema. Reegel on lihtne: igal mitmeastmelisel voolul on oma olek, millel igal hetkel on üks kehtiv väärtus — ja seda väärtust hoitakse järjekindlalt süsteemi andmetes, mitte „mudeli mälus“, mida pole olemas.

## Mälu uuendamine, unustamine ja riskid

> **Lihtsalt öeldes:** mälu ei ole tõde, vaid hüpotees. Kirja pandud fakt võib homme valeks osutuda — inimene muudab meelt, seade vahetub, kokkulepe lõpeb. Seepärast: uuenda kirjeid, kinnita muudatused ja üle küsi, kui oluline.

**Uuendamine.** Kirjed aeguvad. Kui klient teatab uue eelistuse, kirjutab süsteem vana kirje üle — ega lisa uut kõrvale. Kaks vastuolulist kirjet on hullem kui üks aegunud: süsteem ei tea enam, kumb kehtib.

**Kes tohib muuta.** Mälu muudatus ei tohi toimuda vaikselt: mida olulisem kirje, seda selgemalt peab inimene muudatust kinnitama või vähemalt nägema. Inimene kinnitusahelas (ingl k *human-in-the-loop* — töövoog, kus inimene kinnitab tulemuse enne kasutust) on mälu puhul mõistlik just muudatuste juures (vt [3.5 Ohutus](../03-susteemi-ulesehitus/05-ohutus.md)). Tavapärane jaotus: kergemad faktid (seadme mudel) uuenevad automaatselt, tundlikumad (kontaktviisid, kokkulepped) lähevad inimese kinnitusele.

**Unustamine.** Klient võib öelda „unusta mind“ — ja süsteem peab suutma tema kirjed kustutada. Selle aluseks on kliendi õigus oma andmetele ning andmekaitse nõuded (vt [5.3 Turvalisus ja andmekaitse](../05-suurte-projektide-tase/03-turvalisus-ja-andmekaitse.md)).

Kaks riski, mida silmas pidada:

1. **Vana info saab vale.** Mälu kirja pandud „tõde“ võib olla vana: inimene muutis meelt. Seepärast pole pikaajalised faktid vastuse jaoks täitmiseks antud käsud, vaid taust, mida olulise puhul üle küsida: „Kas teavitused ikka veel e-postile?“
2. **Konteksti saastumine.** Kui aknasse satub paar vale kirjet, mürgitavad need kõik järgmised vastused — mudel toetub igal päringul kindlalt valele faktile. Vigane mälu on seega veel ohtlikum kui hallutsinatsioon (mudeli enesekindel, aga faktidel mitte põhinev vastus, vt [1.1](../01-alused/01-mis-on-ai-mudel.md)): vea allikas pole siin mudel, vaid sinu enda andmed, ja viga kordub süstemaatiliselt.

## Näide samm-sammult: Marti kolmas kõne

Olukord: telefonitugi „Kõneabi“ (näite järjestamine [2.4](../02-praktika/04-sisendid-ja-andmed.md)-ga). Klient Mart helistab kolmandat korda sama probleemi asjus: internet kaob iga paari päeva tagant.

**a) Ilma pikaaegse mäluta (enne).** Süsteemil pole midagi Marti kohta; iga kõne algab nullist:

> „Tere! Palun öelge oma kliendinumber ja kirjeldage probleemi.“

Mart seletab kolmandat korda sama lugu: mis modem, millal internet kaob, mida juba proovitud. Kliendile tundub, et ta räägib seina vastu — halb kogemus, mis pole kellelegi kasulik.

**b) Kliendiprofiiliga (pärast).** Süsteem tuvastab numbri järgi profiili ja paneb olulised väljad päringu alguses „asjade seisu“ kujul:

```json
{
  "asjade_seis": {
    "klient": "Mart Tamm (Klient #207)",
    "keel": "et",
    "modem": { "mudel": "KTX-200", "aasta": 2023 },
    "varasemad_kõned": [
      { "kuupäev": "2026-09-18", "teema": "internet kaob õhtuti", "lahendus": "modemi taaskäivitus" },
      { "kuupäev": "2026-09-28", "teema": "internet kaob uuesti", "lahendus": "kanali vahetus, probleem jäi" }
    ],
    "lubatud_kontaktviis": "e-post",
    "märkused": "kolmas kõne sama teemaga — tõsta prioriteediks"
  },
  "uus_sõnum": "Jälle see internet! Kolmas kord nüüd."
}
```

Vastus:

> „Tere jälle, Mart! Näen, et see on kolmas kõne interneti kadumise asjus — 18. septembril aitas modemi taaskäivitus, 28. septembril jäi probleem kanali vahetusest hoolimata. Teie KTX-200 on 2023. aasta seade; kahtlustan nüüd modemi vanust. Saadan e-postile pakkumise uue seadme vahetuseks — teateviisina jääme e-posti juurde, nagu kokkulepitud.“

Kõik faktid tulid profiilist — mitte Marti mälust ega mudeli õnnest.

**c) Uuendamine.** Mart ütleb, et tal on nüüd uus modem NXR-50. Süsteem kirjutab kirje üle:

```json
{ "modem": { "mudel": "NXR-50", "aasta": 2026, "uuendatud": "2026-10-05" } }
```

Pange tähele: vanast mudelist ei jää profiilis jälgegi. Kui KTX-200 kirje jäetaks alles, arvaks süsteem ka järgmises kõnes, et kõne all on vana seade — täpselt konteksti saastumine, mida ülal kirjeldasime.

## Kokkuvõte

- **Kaks mälu liiki:** vestlusesene mälu (ajalugu ja „asjade seis“ ühe seansi jooksul) ja pikaaegne mälu — info üle seansside, mida süsteem hoiab oma andmetes ja paneb iga päringu alguses aknasse.
- **Hoia vaid olulist:** püsifakte, kokkuleppeid ja olulisi ajaloolisi juhtumeid; igapäevane müra ja tundlikud andmed ilma põhjuseta jäävad välja.
- **Lihtne lahendus esmalt:** struktureeritud kliendiprofiil kindlate väljadega; kui juhtumeid on palju, tuleb appi RAG ([4.2](02-rag.md)).
- **Olek hoitakse järjekindlalt:** mitmeastmeline voog jätkub seal, kus jäi — ka seansside vahel.
- **Mälu vajab haldust:** kirjed kirjutatakse üle, mitte ei kuhjata; olulisemad muudatused kinnitab inimene; klient saab unustamist nõuda.
- **Mälu on hüpotees, mitte tõde:** vana kirje võib olla vale, ja paar vale kirjet saastavad kõik järgmised vastused — seepärast mälu uuendada, üle küsida ja kontrollida.

## Mis edasi?

- eelmine → [4.2 RAG](02-rag.md)
- järgmine → [4.4 Mitme agendi arhitektuurid](04-mitme-agendi-arhitektuurid.md)
- Vestlusesene konteksti haldamine → [3.2 Konteksti haldamine](../03-susteemi-ulesehitus/02-konteksti-haldamine.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
