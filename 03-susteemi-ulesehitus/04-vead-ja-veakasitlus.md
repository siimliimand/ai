# 3.4 Vead ja veakäsitlus

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [3.3 Tööriistad ja tegevused](03-tooriistad-ja-tegevused.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- nimetada viis vealiiki ja igaühe tavalise lahenduse;
- panna kordusele (ingl k *retry* — proovi uuesti) piir ja selgitada, miks lõputu kordus olukorda halvendab;
- planeerida varutee (ingl k *fallback* — ettenähtud B-tee), mis võtab vastu töö, mida peatee teha ei jõudnud;
- otsustada, mida vigadest kirja panna (ingl k *logging* — sündmuste kirjapanek);
- öelda, millal inimene hoiatatakse ja millal piisab ühest kirjest logis.

## Lihtsalt öeldes

> Vead ei ole süsteemi rike, vaid osa selle tavapärasest päevast. Nii nagu poes võib kassasüsteem hanguda ja müüja võtab raha käsitsi vastu, on ka voos ette kirjutatud: mis juhtub, kui AI-mudel ei vasta õigel ajal, kui vastus tuleb vales vormis või kui e-posti teenus seisab. Kellel teed valmis on, kellel jääb veast kirje logisse — mitte hädaolukord.

Selle dokumendi põhimõte ongi: **vead on normaalsed ja ette planeeritud — nad ei ole hädaolukord.** [2.5](../02-praktika/05-esimene-workflow.md) nimetas veatüüpe ja lubas, et igal veal on aadress — nüüd vaatame, mida nendega ette võtta.

## Veeliigid: mis võib paigast minna

Enamik workflow’de tootmisvigadest langeb ühte viiest liigist:

| Vea liik | Mis juhtub | Tavaline lahendus |
|---|---|---|
| **Aegumine (ingl k *timeout*)** | mudel ei vasta õigel ajal — päring (ingl k *request*) jääb vastust ootama | kordus piiratud arv kordi; kui ei aita, varutee |
| **Liitumislimiidi ületus (ingl k *rate limit*)** | liiga palju päringuid lühikese ajaga — teenus lükkab need tagasi | kordus ootusega; päringute aeglustamine — tehniline pool [3.1-s](01-api-integratsioonid.md) |
| **Rikutud väljund** | vastus on formaadist kõrvale läinud — JSON-i (struktureeritud andmevorming) ei saa lahti lugeda, sest mudel lisas vestava lause | automaatne kontroll leiab, et välju pole — kirje varuteele ([2.2](../02-praktika/02-struktureeritud-valjund.md) kontroll) |
| **Sisuviga** | vorm on õige, aga sisu vale: hallutsinatsioon (mudeli kindlalt öeldud, aga vale vastus; [1.1](../01-alused/01-mis-on-ai-mudel.md)) või vale andmebaasi vastus | ennetamine (faktid andmetest, mitte mudeli mälust) ja kontrollpunkt enne väljundit ([1.5](../01-alused/05-susteemi-anatoomia.md) viies osa) |
| **Süsteemiväline viga** | teine teenus ei tööta: e-posti teenus on maas, andmebaas ei vasta | töö ülesalvestus ja jätk, kui teenus taastub |

Aegumine ja liitumislimiit on **ajutised** — sama päring võib hetke pärast läbida. Rikutud väljund ja sisuviga on **juba toimunud** — uus vastus eksib sarnaselt. Süsteemiväline viga pole selle voo oma — ta kuulub teisele masinale.

Kuidas rikutud väljund välja näeb? Süsteem ootab sellist vastust (lühendatult näidatud):

```json
{
  "liik": "tagastus",
  "tellimuse_nr": "1187"
}
```

Kui mudel vastab hoopis: „Muidugi! Kirja liik on tagastus…“ — ei leia kontroll ühtegi välja: sisu on peaaegu õige, aga vorm ei ole. Seepärast vaatab kontroll enne vormi ja alles siis sisu.

> **Lihtsalt öeldes:** aegumine ja liitumislimiit on uksekell, mille peale keegi hüüab „oota, tulen!“ — helista hetke pärast ja kõik läheb. Rikutud väljund ja sisuviga on juba juhtunud — neid parandab kontroll ja varutee, mitte teine proovimine.

## Kordus: millal ja mitu korda

Kordus on lihtsaim vahend: sama päring saadetakse veel kord. Ta sobib ainult ajutistele vigadele — aegumisele ja liitumislimiidile. Põhireegel:

> **Ajutise vea puhul kordus peatumisajaga — näiteks kolm katset ja katsete vahele jääv ootus — mitte igavesti.**

Miks järjest kiire kordus olukorda halvendab:

1. **Kordus lisab koormusele koormust.** Kui teenus on aeglane, hoiavad kõik iga sekundi tagant kordavad teenused ta maas hoopis kauem — kordusest saab viivituse põhjus.
2. **Liitumislimiidi juures saab kordusest ise viga.** Ilma ootuseta on iga uus päring just see, mida limiit keelab: mida rohkem proovitakse, seda rohkem lükatakse tagasi.
3. **Lõputu kordus hoiab töö rippumas.** Kiri jääb poolikuks tööks, keegi ei tea, kus ta on, ja klient ootab.

Seepärast on peatumisaeg süsteemi osa: näiteks kolm katset, iga järgmine ootab pikem kui eelmine; kui piir saab täis, ei proovita neljandat korda — võetakse varutee. (Aegumislimiidi seadmine päringule — API-kutse detail, vt [3.1](01-api-integratsioonid.md).) Kordus on ka uus mudelikutse ja uus raha — kulude poole vaatab [3.6](06-kulude-haldamine.md).

> **Lihtsalt öeldes:** kordus on uksekella teine ja kolmas vajutus — viisakas ja sageli piisav. Vajutamine iga sekundi tagant ei tee ust kiiremini lahti, aga naabrimees ärkab — seepärast on katsete arv ja ootus ette kirjutatud.

## Varutee: ettenähtud tee, kui peatee ei tööta

Varutee on [1.5](../01-alused/05-susteemi-anatoomia.md) anatoomiast tuttav pöördumine inimesele, aga pole ainus kuju — tavavoos kasutatakse kolme, sageli kõrvuti:

1. **Reeglitepõhine staandardvastus.** Kui päris vastust ei saa kindlalt koostada, saadab süsteem ette kirjutatud ja välja proovitud teksti: „Täname kirjutamast — me võtame ühendust ühe päeva jooksul.“ — otse või inimese kinnituse kaudu. Lubadus on täidetav, sest kiri on juba inimese loendis. Mudel võis eksida, aga eksitus ei jõudnud kliendini.
2. **Töö ülesalvestus ja käsitsi jätk.** Kui süsteemiväline osa on ajutiselt maas, salvestab süsteem poolelioleva töö järjekorda ja jätkab, kui teenus taastub — töö ei kao, ta liigub hiljem.
3. **Inimesele suunamine.** Kui ükski automaatne tee pole usaldusväärne, läheb töö inimese ülevaatuse loendisse — nii nagu „muu“ pöördus Piretile 2.5-s. See pole ebaõnnestumine, vaid disaini osa (1.5) — ebakindel hinnang ei pea ise otsustama.

Nende taga on üks põhimõte: **varutee on alati parem kui vale vastus kliendile.** Korrektse „võtame ühendust“ kirja saanud klient jääb rahule; kindla tooni, aga vale sisuga vastuse saanud klient kaotab usalduse — seda ei taasta kolm õiget kirja hiljem. Mida süsteem tohib iseseisvalt ette võtta ja mis vajab inimese kinnitust (inimene kinnitusahelas, ingl k *human-in-the-loop*), on ohutusotsus — vt [3.5](05-ohutus.md).

> **Lihtsalt öeldes:** varutee on takso ette tellimine enne, kui esimene katkeb: peatee ummikus, töö liigub ettenähtud kõrvuteele, mitte suvalises suunas. Kliendile jääb mulje, et kõigil oli plaan — sest tal oligi.

## Vigade logimine: mida ja miks kirja panna

Kui viga on lahendatud, on kliendi jaoks asi tehtud — arendaja jaoks ta alles algab. 1.5 sõnul on igal veal aadress ja aadressi näitab ainult kirje: ilma logita jääb juhtunuks „miski läks valesti“; logiga on see koht, mille juurde minna.

Mida iga lahendatud viga kohta kirja panna:

| Kirje osa | Näide | Milleks |
|---|---|---|
| aeg | 13. oktoober, 14:32 | kas vead kuhjuvad kindlal ajal (tiputunnil)? |
| sisend | kirja liik ja tellimuse number | milline sisend viis vea peale? |
| viga | aegumine pärast 20 sekundit | mis liiki viga see oli? |
| tehtud tee | kordus — teine katse õnnestus | kas piisas peateest, kordusest või varuteest? |

**Lihtne reegel inimese hoiatamiseks:** hoiata, kui varutee käivitus, ja kui sama viga kordub ette määratud arv kordi (näiteks viis korda päevas). Üks kordus ei vaja inimest; varutee ja korduv viga tähendab, et midagi muutus — seda peab keegi teadma. Põhjalikumalt vaatab [4.6 Monitooring tootmises](../04-agendid-ja-mootmine/06-monitooring.md).

> **Lihtsalt öeldes:** logi on sõidupäevik: kolm aegumist kolmapäeval, kõik kordusega lahendatud, on kolm kirjet — mitte kolm kriisi. Kui päevik näitab, et aegumised kogunevad keskpäeva paiku, on sul otsustamiseks fakt, mitte tunne.

## Näide samm-sammult: kolm juhtumit e-poes

[2.5](../02-praktika/05-esimene-workflow.md) ehitas Kodutoa poe tagastustaotluste voo ja jälgis esimest nädalat kolme numbriga. Sama nädala kolm juhtumit:

| # | Juhtum | Mis juhtus | Mis tee süsteem võttis | Tulemus kliendile |
|---|---|---|---|---|
| **a** | aegumine | mudel vastas 30 sekundiga — päring ületas 20-sekundilise ooteaja | kordus: süsteem proovis uuesti ja teisel korral tuli vastus mõne sekundiga | kliendi kiri ei kadunud: kavand jõudis Pireti kinnituseks minutiga hiljem — kliendile tavaline kinnitusahel, lihtsalt väike viivitus |
| **b** | rikutud väljund | mudel vastas vabateksti („Muidugi! Kirja liik on tagastus…“) — automaatne reegel ei leidnud välju | varutee: kontroll püüdis rikkumise kinni, süsteem võttis staandardvastuse ja pani kirja Pireti loendisse | klient sai kohe korrektse vastuse, päris vastuse lubatud päeva jooksul |
| **c** | süsteemiväline viga | e-posti teenus oli 10 minutit maas — vastuskirjad ei läinud välja | töö ülesalvestus: kirjed ootasid järjekorras ja läksid välja, kui teenus taastus | klient sai vastuse umbes 10 minutit hiljem — ei tea asjast midagi |

Kolm viga — ja ükski ei jõudnud kliendini veana. Tööks jäi kolm kirjet logis ja üks otsus: aegumise ooteaeg pikendati, sest aegumised kogunesid samale poolepäevale — otsus tehti päeviku põhjal tööajal, mitte ööl.

> **Lihtsalt öeldes:** esimene nädal andis kolm viga ja null hädaolukordi — ette planeeritud vead jäävad kirjeks logis ja vahel üheks väikeseks otsuseks, mitte kliendi probleemiks ega meeskonna öeks.

## Kokkuvõte

- **Viis vealiiki, igal ühel tavaline lahendus** — mida pole kriisi ajal vaja leiutada.
- **Kordus ainult ajutistele vigadele ja peatumisajaga** (näiteks kolm katset, vahe pikeneb): kiire järjestikkune kordus lisab ülekoormusele koormust ja muutub ise veaks.
- **Varutee on ettenähtud B-tee** — staandardvastus, töö ülesalvestus või pöördumine inimesele — ja alati parem kui vale vastus kliendile.
- **Logi teeb veast õppetunni:** aeg, sisend, viga ja tehtud tee; inimene hoiatatakse varutee käivitusel ja korduva vea puhul.
- **Vead on normaalsed ja ette planeeritud — mitte hädaolukord:** süsteem, kellel on teed valmis, elab iga vea üle ühe kirjena.

## Mis edasi?

- eelmine → [3.3 Tööriistad ja tegevused](03-tooriistad-ja-tegevused.md)
- järgmine → [3.5 Ohutus: piirid ja inimene kinnitusahelas](05-ohutus.md)
- Monitooring tootmises → [4.6](../04-agendid-ja-mootmine/06-monitooring.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
