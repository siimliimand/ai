# 5.2 Meeskonnatöö ja standardid

> **Sihtpublik:** juhtiv mitte-tehniline + tehniline | **Eeltingimused:** [5.1 Arhitektuur suures mahus](01-arhitektuur-suures-mahus.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, miks standard (kokkulepe, mis teeb töö ettearvatavaks) pole bürokraatia, vaid kvaliteedi kindlustus kasvavas meeskonnas;
- nimetada viis standardit, mis peavad iga AI-automatiseerimist tegevas meeskonnas kirjas olema;
- panna igale süsteemile ja promptile omanik (ingl k *owner* — süsteemi/prompti vastutaja, üks nimi);
- korraldada olulised muudatused läbi ülevaatuse (ingl k *review* — teine inimene vaatab muudatuse üle) enne tootmisse minekut;
- sisse lülitada uus liige nii, et ta töötaks usaldusväärselt juba esimesel kuul.

## Lihtsalt öeldes

> Kuni prompte hoiavad pead, piisab vestlusest. Kümne inimese meeskonnas pead enam vastu ei pea: keegi muudab, keegi ei tea, keegi ei testi. Standardid pole kontrollitsemine — nad teevad teie töö ettearvatavaks: kõik teavad, kuidas asju muudetakse, kes tohib kinnitada ja kust otsida. Ja kui tuleb uus inimene, loeb ta selle kirjast, mitte kellegi mälust.

## Miks standardid: ettearvatavus on väärtus

On üks lause, mis elab iga kasvava automatiseerimisprojekti juures: „Kaja parandas prompti, aga keegi ei tea, miks ja kas testitud.” Keegi pole süüdi — Kaja parandas tõesti. Vigane pole inimene, vaid süsteem ümber inimese: muudatuse tee pole kirjas, seega on iga muudatus isikupärane risk, mis sõltub sellest, kes parasjagu klaviatuuri taga istub.

Väike meeskond jõuab kõige tähtsa üle suuliselt kokku leppida — kuni teatud inimeste arvuni. Peades elavad kokkulepped ei kopeeru: uus liige ei loe neid, vaid arvab — ja üks testimata muudatus jõuab otse kliendini.

Standard (kokkulepe, mis teeb töö ettearvatavaks) lahendab just seda. Ta ei aeglusta tööd, vaid muudab selle prognoositavaks: sama viga toob sama vastuse, olenemata sellest, kes ta leiab, ja igaüks teab, kus asjad seisavad ja kes otsustab. Erinevus bürokraatiast on lihtne: bürokraatia on kirja panemine, mida keegi pärast ei kasuta; standard on kirja panemine, mida iga muudatus tegelikult läbib. Sellepärast pole standardid bürokraatia, vaid kvaliteedi kindlustus.

## Viis standardit, mis peavad olema kirjas

Järgneb viis kokkulepet — igaüks mahub poolele leheküljele.

1. **Promptide muutmise voog.** Vood (ettenähtud tegevuste jada) ütleb, kuidas prompt ühest seisust teise jõuab: muudatuse vajadus → testimise tsükkel ([2.1](../02-praktika/01-hea-prompt.md)) → teise inimese ülevaatuse all → kirja pankasse ([2.6](../02-praktika/06-promptide-haldus.md)). Ilma voota muudetakse prompte jutu ja tuju järgi; vooga on muudatus sammude jada, mida igaüks läbida oskab.
2. **Kinnitamise õigused.** Mitte igaüks ise: kokkulepe ütleb, kes tohib muudatuse tootmisse lasta — ehk kasutusse, kus tulemus jõuab kliendini. Tavapärane jaotus: muuta ja testida tohib laialdaselt, käiku anda tohib prompti omanik või tema nimetatud asendaja — inimene kinnitusahelas (ingl k *human-in-the-loop* — töövoog, kus inimene kinnitab tulemuse enne kasutust) meeskonnatasandil: ka muudatus ise on tulemus, mida keegi peab kinnitama.
3. **Uue süsteemi sünniprotokoll.** Sünniprotokoll (uue süsteemi enne käikuandmist läbitav kontroll) hoiab ära olukorra, kus esimene kliendile minev tulemus on ühtlasi esimene test. Enne käikuandmist läbib iga süsteem kolm sammu: ohutuse kontroll — mis võib minema ja kus inimene vahele sekkub ([3.5](../03-susteemi-ulesehitus/05-ohutus.md)); hindamine testjuhtumite komplektil ([4.5](../04-agendid-ja-mootmine/05-hindamine.md)); dokumentatsioon — pank, omanik ja piirid kirjas (kontroll-loend [5.8](08-mallide-teek.md)-is). Täielik kontroll-loend [5.8](08-mallide-teek.md) katab lisaks sellele ka kulu hinnangu, turvalisuse ja monitooringu valmisoleku.
4. **Nimekirjade ja kirjapaneku reeglid.** Kokkulepe, kus mida otsida: üks promptipank, üks süsteemide nimekiri (omanik, olek, viimati testitud), üks otsuste logi. „Üks” tähendab siin üht: kui sama prompt elab kahes kohas, ei tea peagi enam keegi, kumb versioon tõene on.
5. **Ülevaatuse põhimõte.** Olulised muudatused ei lähe kunagi ühe inimese pealt tootmisse. Ülevaatus (vt selgitust õppeesmärkides) pole kolleegi usaldamise küsimus, vaid lihtne aritmeetika: autor näeb oma plaani, teine inimene näeb kirja nii, nagu see on — ja märkab tihtipeale just seda, mida autor enam ei näe.

Kui standardid kasvavad ettevõtte laiuseks — kes tohib mis otsuseid teha ja kellele keegi aru annab —, on see juba governance (süsteemi juhtimise reeglite) küsimus: vt [5.7 Vastutus, eetika ja governance](07-vastutus-ja-eetika.md).

### Dokumentatsioon reaalajas

> **Lihtsalt öeldes:** dokument, mis ei kirjelda enam tegelikkust, pole dokument, vaid eksitus korras vormis. Põhimõte kogu kogule: kui avad kirje ja näed, et ta valetab, veeta kaks minutit ja paranda enne, kui alustad uut tööd — mitte „hiljem, kui jõuad”.

See kehtib ühtviisi standardite endi kohta: reaalajas parandatud viga on kaks minutit tööd, kuu vanused valed kirjed aga uurimistöö, milleks keegi aega ei saa.

## Rollid kasvavas meeskonnas: iga süsteemil omanik

[1.6 Rollid ja vastutus projektis](../01-alused/06-rollid-ja-vastutus.md) defineeris viis rolli — tellija, ehitaja, sisukujundaja, kontrollija, haldaja — ja seda siin ei korrata. Kasvavas meeskonnas tekib teine küsimus: mitte „millised rollid on olemas”, vaid „kes kannab neid igal süsteemil”.

Reegel on ühe lausega: igal süsteemil ja igal promptil on omanik (vt selgitust õppeesmärkides). Üks nimi, mitte komisjon — sest komisjon vastutab vaikimisi kõige eest, mis hästi läheb, ja mitte millegi eest, mis valesti läheb.

Omanik ei pea ise iga muudatust kirjutama ega testima — tal peavad olema õigus kinnitada ja kohustus teada, kus tema süsteem seisab. Üks inimene võib omada mitut süsteemi; keelatud on ainult üks seis: keegi ei oma midagi. Kui omanik lahkub või läheb puhkusele, kirjutatakse panka asendaja nimi — vahetub inimene, mitte vastutus.

> **Lihtsalt öeldes:** „Seda hoiavad kõik silmad peal” tähendab praktikas, et ei hoia keegi. Omanik pole auaste ega süüdlane — see on aadress, kuhu küsimused minevad ja kelle allkiri muudatuse käiku annab.

## Uue liikme sisselülitus

> **Lihtsalt öeldes:** hästi kirjutatud standardid on parim sisselülitusmaterjal — uus inimene loeb ühe päeva ja teab, kuidas siin asju muudetakse. Halb standard näeb välja nii: „Küsi Jaagult, ta seletab” — ja kui Jaak lahkub, ei tea keegi enam midagi.

Sisselülitus on kolm sammu:

1. **Loe.** Kogu on järjestatud tasanditeks ([README index](../README.md)): alusteadmised 1.1–1.6, siis praktika 2. tasemelt, edasi 3.–5. tasemeni vastavalt sellele, millega inimene tööle asub. Järjekord pole stiilinõue — iga tase eeldab eelmist.
2. **Vaata kaasa.** Nädal aega jälgib uus liige, kuidas kogenud inimene läbib vood — kuidas muudatus pankasse jõuab ja mis juhtub, kui test läbi kukub.
3. **Tee ülevaatuse all.** Esimesed muudatused läbivad sama voo nagu kõik — teise inimese ülevaatuse all. Nii õpib uus inimene vood ära, ilma et klient see õppetund maksma läheks.

Tutvustaja on prompti- või süsteemiomanik või kogenud ehitaja — mitte see, kellel „parasjagu aega”. Hea standard on kirjutatud nii, et sisselülitus võtab päevad, mitte kuud.

## Näide samm-sammult: Nummerbüroo 4-st 12-ni

Sama väljamõeldud **Nummerbüroo**, mida [1.6](../01-alused/06-rollid-ja-vastutus.md) ja [2.6](../02-praktika/06-promptide-haldus.md) on kirjeldanud. Lähteseis: süsteemi kallal töötas neli inimest — Anu, Kaja, Merle ja Jaak — ja kõik 12 prompti olid kolitud promptipankadesse. Siis kasvas büroo poole aastaga 12 inimeseni: liitus uusi raamatupidajaid ja palgati kaks uut ehitajat.

Kaos saabus kiiresti. Prompti „Kuu kokkuvõtte e-kirja tekst” muutsid kolm inimest ühel ja samal nädalal — igaüks oma versiooni, teineteist üle kirjutades. Üks muudatus jõudis tootmisse testimata, ja kliendile läks vale kinnituskiri: kiri kinnitas kokkuvõtet, mille numbrid pärinesid vana malli järgi koostatud seisust. Küsimusele „kumb teie kirjadest on õige?” ei osanud büroo vastata.

Anu ja Jaak panevad kirja neli kokkulepet:

1. **Muutmise voog kirja.** Iga prompti muudatus läbib sama tee: muudatus → test → teine ülevaatus → kinnitus → pank. Tabel allpool.
2. **Omanik igale promptile ja süsteemile.** Kaksteist prompti ja üks süsteem — kolmteist nime panka. Kahele uuele ehitajale läksid müügiarvete ja palgaandmete vood; kokkuvõtte prompte hoiab endiselt Kaja.
3. **Nädalane 15-minuti ülevaatuse hetk.** Iga esmaspäev: mis muutus, mis testiti, mis jäi pooleli (kuidas selline rutiin üles ehitada, õpetab [4.6 Monitooring tootmises](../04-agendid-ja-mootmine/06-monitooring.md)).
4. **Uue töötaja leht.** Mida lugeda ja millises järjekorras — 1.1–1.6, siis 2. tase, edasi vajaduse järgi ([README index](../README.md)) — ja kes on tutvustaja.

Muutmise voog tabelina:

| Voo samm | Kes teeb | Mis kirja läheb |
|---|---|---|
| 1. Vajadus: testjuhtum näitab viga | igaüks, kes vea leiab | pank: vea kirje — mis ja millal läks valesti |
| 2. Parandus uue versioonina, vana jääb alles | vea leidja või prompti omanik | pank: uus versiooninumber, mida muudeti ja miks |
| 3. Testimise tsükkel | teine inimene, kui leidja ise testis | pank: „viimati testitud” kuupäev |
| 4. Ülevaatus — teine silm | prompti omanik või kogenud kolleeg | logi: kes vaatas üle ja mida märkis |
| 5. Kinnitus ja tootmisse | ainult omanik | pank: kes kinnitas, olek „kasutusel”, vana versioon arhiivi |

Kuu hiljem ei küsi büroos enam keegi „kumb on õige” — vastab pank.

## Kokkuvõte

- **Standard on kokkulepe, mis teeb töö ettearvatavaks** — mitte bürokraatia, vaid kvaliteedi kindlustus: sama viga toob sama vastuse, olenemata sellest, kes ta leiab.
- **Viis standardit peavad olema kirjas:** promptide muutmise vood, kinnitamise õigused, uue süsteemi sünniprotokoll, kirjapaneku reeglid ja ülevaatuse põhimõte.
- **Olulised muudatused ei lähe kunagi ühe inimese pealt tootmisse** — teine silm pole usalduse puudumine, vaid kaitse inimese eksituse eest.
- **Igal süsteemil ja promptil on omanik — üks nimi, mitte komisjon.**
- **Dokumentatsioon parandatakse reaalajas** — vale kirje enne uue töö alustamist, mitte „hiljem, kui jõuad”.
- **Hästi kirjutatud standardid teevad sisselülituse kiireks** — uus liige loeb, vaatab kaasa ja teeb esimesed muudatused ülevaatuse all.

## Mis edasi?

- eelmine → [5.1 Arhitektuur suures mahus](01-arhitektuur-suures-mahus.md)
- järgmine → [5.3 Turvalisus ja andmekaitse (GDPR, audit)](03-turvalisus-ja-andmekaitse.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
