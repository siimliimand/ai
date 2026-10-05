# 4.6 Monitooring tootmises

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [4.5 Hindamine: kuidas teada, kas süsteem on hea](05-hindamine.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, mis on monitooring (ingl k *monitoring* — tootmises jälgimine) ja miks eile hästi töötanud süsteem võib täna halvemini töötada;
- lugeda kuut põhimõõdikut (ingl k *metric* — 4.5-s „mõõdik“; siin jälgime sama numbrit pidevalt tootmises) ja öelda, milline arv on märk ja milline müra;
- seada püsti hoiatus (ingl k *alert* — automaatne teavitus, kui mõõdik läheb lubatud piirist välja), millel on konkreetne hoiatuskünnis (ingl k *alert threshold* — piir, mille ületamisel teavitus käivitub);
- pidada 15-minutilist nädalarutiini, mis toidab testikomplekti ([4.5](05-hindamine.md)) ja promptide uuendusi ([2.6](../02-praktika/06-promptide-haldus.md)).

## Lihtsalt öeldes

> Süsteem pole kord käivitatud ja igaveseks valmis: sisemised andmed muutuvad, kliendid kirjutavad uusi tüüpe kirju ja mudeli pakkuja uuendab mudeleid. Monitooring tähendab mõne arvu regulaarset vaatamist, mis sellise muutuse märkab enne kliente. Alustamiseks piisab kuuest numbrist, ühest konkreetsest hoiatusest ja 15 minutist nädalas.

## Hindamine vs monitooring: plaanitud test ja reaalsus

Kvaliteet võib aja jooksul langeda ka siis, kui sa ise midagi ei muuda. Kolm tüüpelist põhjust:

1. **Siseandmed muutuvad.** Tootenimekirja tuleb juurde, hinnad vahetuvad, väljad saavad uue vormi — vool, mis vanade andmetega läbis, eksib uutega.
2. **Kliendid kirjutavad uusi tüüpe kirju.** Uus tootekategooria toob kirjad, mille keeles klassifitseerija pole õpetatud.
3. **Pakkuja muudab mudeleid.** Uuendused tulevad taustal — vastuse kiirus, keel või vorm võib nihkuda ilma et sina midagi klõpsiksid. Selle põhjalik käsitlus on [5.5-s](../05-suurte-projektide-tase/05-mudelite-vahetamine.md).

Kõik kolm on vaiksed: süsteem ei anna veateadet, ta hakkab lihtsalt natuke halvemat tööd tegema. Monitooring on ainus viis seda märgata enne kliente.

Erinevus hindamisest mahub ühte lausesse: hindamine ([4.5](05-hindamine.md)) on plaanitud test teadaolevatel sisenditel, monitooring on süsteemi käitumine päris sisenditel päris päeval, kui keegi just ei vaata.

> **Lihtsalt öeldes:** hindamine on aastaarsti ülevaatus ajakaval kokkulepitud hetkel, monitooring on igal päeval võetud kehatemperatuur. Üks ütleb, kuidas süsteem testi hetkel oli; teine, kuidas tal päris elus läheb. Mõlemad on vaja — üks ei asenda teist.

## Mida jälgida: kuus põhimõõdikut

[2.5](../02-praktika/05-esimene-workflow.md) lõpus loeti esimest nädalat kolme numbriga. Siin skaleerub sama mõte süstemaatiliseks monitooringuks — kuus mõõdikut, millest enamikus süsteemides piisab:

| Mõõdik | Mida ütleb | Hoiatusmärk |
|---|---|---|
| **maht** | mitu päringut päevas süsteem töötles | järsk tõus või langus = midagi muutus: uus klient või kampaania — või käivitaja on katki ja kirju ei tule enam üldse |
| **veamäär** (vigade osakaal) | mitu % päringutest lõppes veaga — aegumine, rikutud väljund, süsteemiväline viga; kirjed tulevad [3.4](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md) logist | tõus üle tavalise taseme: midagi muutus sisendis, vormis või välises teenuses |
| **inimese sekkumise määr** (mitu % juhtumitest läks inimesele) | mitu % juhtumitest pöördus inimesele ja mitu % kinnitustest muudeti enne saatmist (vt [4.5](05-hindamine.md) mõõdikuid) | tõus kummalgi näol = kvaliteet on langenud või sisend on muutunud |
| **latentsus** | kui kaua sisendist vastuseni kulub | aeglustus viitab pakkuja muutusele või ummikule — tehniline pool [4.7-s](07-joudlus-ja-latentsus.md) |
| **kulu** | kui palju süsteem päevas kulutab ([3.6](../03-susteemi-ulesehitus/06-kulude-haldamine.md)) | hüpe ilma mahu kasvuta = midagi jookseb kavatsamatult |
| **erijuhud** | mitu kirja läks varuteele või inimesele | järsk tõus: [3.4](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md) reegel — hoiata, kui varutee käivitus ja viga kordub |

Esimesed kolm numbrit tulevad samadest kirjetest, mida vool juba 2.5-s ajalukku kirjutas — monitooring pole uue süsteemi ehitamine, vaid olemasolevate kirjete regulaarne lugemine. Mõõdikud võib koondada juhtpaneelile (ingl k *dashboard* — kõik arvud ühel lehel ühel pilgul), aga esimese sammuna piisab nädalasest kokkuvõttest — enamikus no-code tööriistades on see valmis mallina olemas.

> **Lihtsalt öeldes:** kuus mõõdikut on süsteemi elumärgid, nagu pulss ja temperatuur visiidil. Maht ütleb, kas patsient üldse tuli; veamäär, kas ta aru sai; sekkumise määr, kas töö hästi läks. Igaüks üksi on pool lugu — koos annavad nad pildi.

## Hoiatused: konkreetsed, mitte iga asja peale

Kui keegi peab numbreid iga päev käsitsi vaatama, jääb vahele just see päev, mil midagi juhtub. Seepärast on monitooringu südamik hoiatus: süsteem teavitab ise, kui näitaja läbib lubatud piiri.

Hoiatus elab ja sureb hoiatuskünnisega:

- **Hea hoiatus on konkreetne:** „veamäär ületab tunnis 10%“, „varutee käivitus kolm korda päevas“, „sekkumise määr nädalas üle 20%“. Iga selline teavitus tähendab reaalset muutust ja väärib vaatamist.
- **Halb hoiatus on iga asja peale:** teavitus iga vea ja iga „puudub“ välja kohta. 40 kirja päevas tähendab mõni väike viga igal päeval — esimese nädalaga tuleb hoiatusväsimus: teavitusi tuleb nii palju, et inimene lakkab neid lugemast, ja märkamata jääb just päris hoiatus.

Kellele teave läheb? Haldajale — rollile, kelle hoolde on süsteemi elusolek ([1.6](../01-alused/06-rollid-ja-vastutus.md) määrab selle); ühele kindlale inimesele, mitte jagamata aadressile, kust teade upub. Kuidas hoiatus päriselus päästab, näitab allpool e-poo teisipäev.

> **Lihtsalt öeldes:** hoiatus peab olema suitsuandur, mitte uksekell. Uksekell heliseb iga külalise peale ja varsti keegi enam uksele ei torma; suitsuandur heliseb ainult tule korral — ja siis tormatakse. Kümme täpset hoiatust aastas väärt rohkem kui sada kahetust kuus.

## Nädalane rutiin: 15 minutit, mis päästab

Hoiatused hoiavad ööd ja päevad; inimesele jääb nädalane rutiin — 15 minutit, mil [3.4](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md) logikirjeid vaadatakse kolme küsimusega:

1. **Mis liiki vead sel nädalal olid?** Aegumised, rikutud väljundid, süsteemivälised? Sama liigi kordumine viitab ühele konkreetsele kohale, mida parandada.
2. **Mis kirjed läksid inimesele ja miks?** Kas klassifitseerija eksis, andmeid ei leitud või oli kiri tõesti ebaselge? Korduv eksimus on juhise viga, mitte halb õnn.
3. **Mida klientide küsimustes on uut?** Uued tooted, uued sõnad, uued taotlused — kõik, mida süsteem veel ei tunne.

Vastused lähevad otse töösse: iga tabatud viga läheb testikomplekti uue juhtumina ([4.5](05-hindamine.md)) ja iga uus kirjatüüp saab näited prompti juurde ([2.6](../02-praktika/06-promptide-haldus.md)). Nii kasvab süsteem päris sisendist, mitte oletustest. Kui voolusid on mitu ja meeskond suurem, kasvab sama rutiin pideva täiustamise tsükliks — seda vaatab [5.6](../05-suurte-projektide-tase/06-pidev-taiustamine.md).

> **Lihtsalt öeldes:** 15 minutit nädalas on aiapidamine: üks ring üle aia, et näha, mis kasvas üle, mis kuivas ja mis on uus. Tingimus on ainult üks — ring tehakse iga nädal, mitte siis, kui midagi on juba läinud.

## Näide samm-sammult: e-poo esimene kuu

Kodutoa poe tagastusvoog ([2.5](../02-praktika/05-esimene-workflow.md)) on töös olnud kuu aega; [4.5](05-hindamine.md) hindamise järel oli inimese sekkumise määr 12% ja veamäär ~2%. Järgnev on nähtav ainult tänu logimisele (ingl k *logging* — sündmuste kirjapanek, [3.4](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md)): iga kiri jätab kirje. Esimesel kuul juhtus kolm asja.

**(a) Teisipäev: veamäär hüppas 2% → 14%.** Kõik vead olid aegumised (3.4 juhtumi tüüp): päring ületas 20-sekundilise ooteaja. Hoiatus „veamäär > 10% tunnis“ jõudis haldajani enne lõunat; logi ajast leiti põhjus — pakkuja oli öösel uuendanud vastuseaegu. Korduste arv suurendati ja nädalaks oli veamäär tagasi 2%-ni. Hoiatus päästis olukorra: ilma selleta oleks esimene märk olnud Pireti küsimus „miks kinnitused jäävad hiljaks?“ — ja kõige hiljem oleks märkanud klient.

**(b) Inimese sekkumise määr tõusis 12% → 19% kolme nädalaga.** Üksikut nädalat vaadates polnud hüpet ja hoiatus ei käivitunud — tõus oli vaikne. Märkas nädalane rutiin: klassifitseerija ei tundnud ära uut tootekategooriat (küünlajalad), mille jaoks ta polnud õpetatud, ja need kirjad läksid „arusaamatu kirjana“ Piretile. Testikomplekti lisati küünlajalgade juhtumid ja klassifitseerija juhisele lisati näited (4.5 kuldstandard; 2.6 prompti uuendus). Järgmisel nädalal langes määr 13%-ni.

**(c) Kulu tõusis 30%.** Esimene refleks: midagi jookseb kavatsamatult. Juhtpaneeli võrdlus näitas, et maht oli kasvanud täpselt sama palju — uued kliendid toovad rohkem kirju. Kulu kasvas koos mahuga, kvaliteet jäi samaks. Tegevust pole vaja: see pole viga, vaid süsteemi kasv.

| Näitaja | Enne | Pärast | Tegevus |
|---|---|---|---|
| veamäär | 2% | 14% (teisipäeval) | hoiatus → põhjus: pakkuja muutis vastuseaegu → korduste arv suurendati; nädalaga tagasi 2% |
| inimese sekkumise määr | 12% | 19% (kolme nädalaga) | nädalane rutiin tabas põhjuse: uus tootekategooria → testjuhtumid ja näited juurde; järgmisel nädalal 13% |
| kulu | baastase | +30% | võrdlus mahu kasvuga: uued kliendid — normaalne, midagi ei tehtud |

Kolm sündmust, kolm erinevat teed: üks hoiatuse kaudu, üks rutiini kaudu, üks osutus mitteveaks. Ükski ei kasvanud kliendi probleemiks, sest igaüks märgati numbritest — mitte kliendi kirjast pärast juhtunut.

> **Lihtsalt öeldes:** kuus numbrit ja üks hästi seatud hoiatus tegid esimese kuu kolm üllatust kolmeks rahulikuks otsuseks. Sama kuu ilma monitooringuta oleks andnud Pireti, kes imestab, miks kinnitused hanguvad, ja mõne kliendi, kelle kiri jäi vastamata.

## Kokkuvõte

- **Kvaliteet langeb vaikselt:** andmed muutuvad, kliendid kirjutavad uut, pakkuja uuendab mudelit ([5.5](../05-suurte-projektide-tase/05-mudelite-vahetamine.md)) — monitooring on ainus viis seda märgata enne kliente.
- **Hindamine on plaanitud test, monitooring on reaalsus** — mõlemad vajalikud, üks ei asenda teist.
- **Kuus mõõdikut piisab:** maht, veamäär, inimese sekkumise määr, latentsus, kulu, erijuhud — esimesed kolm tulevad samadest logikirjetest, mida vool juba kirjutab.
- **Hoiatus peab olema konkreetne:** hoiatuskünnis arvuga, mitte teavitus iga asja peale — muidu tuleb hoiatusväsimus; teave läheb haldajale.
- **Alustamiseks piisab 15 minutist nädalas:** kolm küsimust logi üle, tulemused testikomplekti ja promptide juurde.

## Mis edasi?

- eelmine → [4.5 Hindamine](05-hindamine.md)
- järgmine → [4.7 Jõudlus ja latentsus](07-joudlus-ja-latentsus.md)
- Pidev täiustamine suurtes süsteemides → [5.6](../05-suurte-projektide-tase/06-pidev-taiustamine.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
