# 5.3 Turvalisus ja andmekaitse (GDPR, audit)

> **Sihtpublik:** juhtiv mitte-tehniline + tehniline | **Eeltingimused:** [5.2 Meeskonnatöö ja standardid](02-meeskonnatoo-ja-standardid.md)

> **TEADE LUGEJALE:** See dokument on **hariduslik ülevaade, MITTE juriidiline nõuanne**. Ta õpetab, milliseid küsimusi esitada ja millised protsessid paika panna — ta ei asenda seadust ega asjatundjat. **Enne reaalse süsteemi käikuandmist küsi andmekaitse eksperti käest.**

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- öelda, mis on GDPR (ingl k *GDPR* — Euroopa Liidu isikuandmete kaitse määrus) ja mida loeb AI-süsteemis isikuandmeteks (inimese tuvastamist võimaldavad andmed);
- nimetada viis põhinõuet ja näidata, kus igaüks sinu süsteemis elab;
- esitada AI-pakkujale viis küsimust andmete kohta ja teada, et vastutus jääb ettevõttele;
- panna paika turvalisusjuhtumi (ingl k *incident* — andmekaotus või -leke) reaktsiooniplaan neljas sammus;
- alustada auditi (auditeeritavus — ettevalmistus kontrolliks) ettevalmistust: inventuur, jälge, kirjad.

## Lihtsalt öeldes

> Kuni nüüd vaatles käsiraamat turvalisust tehniku silmadega — võtmed, andmete hulk, ümberlülitus ([3.7](../03-susteemi-ulesehitus/07-turvalisus.md)). See dokument vaatab samale majale seaduse aknast: mis on kohustuslik ja keda küsitakse, kui midagi läheb valesti. Põhimõte: **kõik andmed, mis süsteemist välja lähevad, on sinu vastutus ka siis, kui nad töötlemiseks teise firmasse sõidavad.** Pakkuja annab tööjõu, mitte vastutuse. Vastutust saab kanda ainult siis, kui tead, millised andmed sul on, kus nad on ja kui kaua neid hoitakse — selleks on inventuur ja jälge, mitte hea tahtmine.

## Isikuandmed ja GDPR AI-süsteemis

GDPR on reeglite kogu selle kohta, kuidas tohib inimeste andmeid koguda, hoida ja kasutada — ja ta kehtib ka sinu AI-süsteemile, kui see puutub kliente. Esimene kontroll on tihti kogu vastus: **kas süsteemis on üldse isikuandmed?**

AI-süsteemi kontekstis on reeglina isikuandmed: kliendi nimi, tellimuste ajalugu, kirjade ja vestluste sisu. Ka kliendinumber on seotav inimesega. Kui klient mainib vestluses oma tervist või rahalist seisundit, on andmed veel tundlikumad — nagu nägime e-apteegi näites [3.5](../03-susteemi-ulesehitus/05-ohutus.md).

Viis põhinõuet:

| Kohustus | Mida see tähendab | Kus sinu süsteemis elab |
|---|---|---|
| **Teavitamine (läbipaistvus)** | klient teab, et andmeid töödeldakse, milleks ja kellele edastatakse | privaatsustingimused + silt „vastust annab AI“ |
| **Andmete vähendamine** (ainult vajalik) | pakkujale ja mällu läheb ainult see, mida ülesanne vajab | sisendi ettevalmistus ([2.4](../02-praktika/04-sisendid-ja-andmed.md), [3.7](../03-susteemi-ulesehitus/07-turvalisus.md)) |
| **Salvestuspiir** (kui kaua hoida) | igal andmeliigil on kirjas eluiga ja automaatne kustutus | logide, mälu ja varukoopiate seadistused |
| **Õigus unustada** (kustutamise võimalus) | „unusta mind“ tuleb täita kõikjal: andmebaasis, mälus, logides | kustutamise tegevus ([4.3](../04-agendid-ja-mootmine/03-pikaaegne-malu.md)) |
| **Turvaline töötlus** | juurdepääs ainult rolli järgi; võtmed sahtlis | ligipääsud ja võtmed ([3.7](../03-susteemi-ulesehitus/07-turvalisus.md)) |

Mitte-tehnilisele juhile: need viis rida on vastus küsimusele „mis on kohustuslik?“. Küsi iga rea kohta, kas see on meil paigas ja kelle nimi selle eest vastutab — rollidest [1.6](../01-alused/06-rollid-ja-vastutus.md).

## Andmed välisele pakkujale: leping ja vastutus

Kui süsteem saadab andmeid välisele AI-pakkujale, pole see „lihtsalt API kutse“ — õiguslikult on see andmete edastamine **andmetöötlejale** (ingl k *processor* — kes andmeid ettevõtte ülesandel töötleb, nt AI-pakkuja). Otsuse, millised andmed välja lähevad, tegi sinu ettevõte — ja vastutus jääb sinna ([1.6](../01-alused/06-rollid-ja-vastutus.md)).

Viis küsimust, mida iga AI-pakkuja kohta üle küsida:

| # | Küsimus | Miks oluline |
|---|---|---|
| 1 | Kas on andmetöötluse kokkulepe? | leping, mis jätab pakkujale ainult ettenähtud kasutuse |
| 2 | Kus serverid asuvad (milline riik)? | riigist sõltuvad lisatingimused — tõlgenda koos ekspertiga |
| 3 | Kas päringuid kasutatakse mudeli treeninguks? | tundlike andmete puhul vali seadistus, kus ei ([3.7](../03-susteemi-ulesehitus/07-turvalisus.md)) |
| 4 | Kui kaua pakkuja logisid hoiustab ja kellele näitab? | pakkuja logi on samuti sinu klientide andmete hoidla |
| 5 | Kuidas andmed kustutatakse — kas ka alamtöötlejatel? | kustutamiskohustus peab kehtima kogu ahelas |

Mittetäitmine on sinu vastutus: kui pakkuja kasutab andmeid treeninguks, kuigi kokkulepitud polnud, või kaotab andmed, pöörduvad klient ja järelevalveasutus sinu poole — vastus „see oli pakkuja viga“ ei veena, sest pakkuja valis sinu.

> **Lihtsalt öeldes:** andmetöötluse kokkulepe on nagu üleandmisleht autoga remonti minnes: kirjas, kes auto vastu võtab, mida tohib parandada ja mis juhtub kahju korral. Ilma paberita on su sõna nende sõna vastu — ja kui auto tuleb tagasi kriimustatuna, maksab sinu kindlustus.

## AI-spetsiifilised küsimused: mälu, automaatotsused, jälge

**1. Kas vastustest saab välja lekkida teiste klientide andmed?** Pikaaegne mälu ja logid ([4.3](../04-agendid-ja-mootmine/03-pikaaegne-malu.md)) on uus risk: kui mälu ei lõiga kliente selgelt, võib ühe kliendi info jõuda teise kliendi vastusesse. Kaitse on disainis: mälu ja logid on kliendi järgi eraldatud ning logidesse näeb ainult see, kellel rolli järgi õigus.

**2. Kas süsteem teeb automaatsed otsused inimeste kohta?** Automaatse otsuse põhimõte (GDPR-i art. 22): inimene peab saama puhtautomaatset otsust — näiteks soodustuse keeldu — vaidlustada ja jõudma inimeseni. Seega kaks nõuet: tundlikes otsustes on inimene kinnitusahelas ([3.5](../03-susteemi-ulesehitus/05-ohutus.md)) — süsteem koostab visandi, otsustab ja vastutab inimene — ning kliendile on nähtav vaidlustamise tee: kes inimene vastab ja millal.

**3. Kuidas näidata, et tegevus oli korrektne?** Jälge (ingl k *audit trail* — iga tegevuse kirje) on [3.5](../03-susteemi-ulesehitus/05-ohutus.md) kaitsekihidest tuttav: mis, millal, miks. Jälg pole ainult vee uurimiseks — ta on tõend: küsimusele „miks süsteem andis just sellise vastuse?“ saab vastuse kirjetest, mitte mälestustest. Üks piir: jälge on ise andmekogu — ka logil peab olema salvestuspiir.

## Turvalisusjuhtum: kui midagi läheb valesti

Turvalisusjuhtum (ingl k *incident* — andmekaotus või -leke) on küsimus „millal“, mitte „kas“: lekkiv võti, fail, mis läks valele aadressile, pakkuja teade vahejuhtumist. Reaktsioon on neli sammu:

| Samm | Mida tehakse | Kes ([1.6](../01-alused/06-rollid-ja-vastutus.md)) |
|---|---|---|
| 1. Mis juhtus? | faktid kirja: millised andmed, kelle andmed, kuhu läksid, kui palju, millal | haldaja |
| 2. Piirata | leke peatatud: juurdepääs ära, võtmed vahetatud, vajadusel süsteem maha ([3.5](../03-susteemi-ulesehitus/05-ohutus.md) peatusnupp) | haldaja + ehitaja |
| 3. Teavitada | asjakohasel juhul järelevalveasutusele (Eestis Andmekaitse Inspektsioon) **72 tunni jooksul**; suure riski korral ka klientidele | tellija |
| 4. Õppida | mis juhtus, miks, mis muutub — kirjas ja kontrollitud, et muudatus tehtud | kogu meeskond |

Sõna „asjakohasel juhul“ tähendab, et mitte iga juhtum ei vaja teavitusi — aga otsustamine võtab ise aega, just seepärast peab plaan valmis olema enne juhtumit: stressis ei arutata, kes helistab kellele.

> **Lihtsalt öeldes:** reaktsiooniplaan on nagu tulekustuti — teda vaadatakse alles siis, kui põleb, ja just seepärast riputatakse ta üles enne põlemist. Nelja sammuga plaan — selgita, peata, teavita, õpi — peab olema kirjas nii, et iga sammu juures seisab nimi, mitte „keegi“.

## Näide samm-sammult: Ravikoda inventuur

[3.5](../03-susteemi-ulesehitus/05-ohutus.md) tundis e-apteegi vestlussüsteemi „Ravikoda“: süsteem vastab laoseisu ja hindade kohta, terviseküsimused suunab apteekrile. Nüüd lisandub rõhk: klientide küsimused puudutavad üha enam toimeid ja tervisseisundit — eriti tundlikke andmeid.

**1. Andmete inventuur** (mis andmed, kus, kui kaua). Esimene samm pole parandamine, vaid teadasaamine:

| Andmeliik | Kus | Kui kaua | Kellele vaja |
|---|---|---|---|
| Kliendi nimi ja tellimuste ajalugu | kaubandussüsteem | seadustest tulenevalt (nt arvepidamine) | apteeker, raamatupidamine |
| Vestluse tekst (küsimus + vastus) | AI-pakkuja logid | praegu 2 aastat | ainult veauurimiseks |
| Tervisedetailid vastustes | vestluse logid | — | ei tohiks logisse jõuda |
| Süsteemi tegevuse jälge (mis, millal, miks) | oma logisüsteem | 1 aasta | haldaja, auditeerimiseks |

**2. Nõrgad kohad, mida inventuur näitas** — kolm leitud viga, ükski pole „häkkeri töö“:

- vestluse logid hoiti 2 aastat — salvestuspiir (kui kaua hoida) puudus;
- apteeker kirjutas vastustesse kliendi tervisedetaile — andmete vähendamine (ainult vajalik) jäi pidama;
- AI-pakkujaga polnud andmetöötluse kokkulepet — andmed väljas ilma lepinguta.

**3. Parandused:**

- vestluse logid 90 päeva, seejärel automaatne kustutus;
- vastuse mallid ilma tervise detailideta: terviseinfo läheb apteekrile tema enda süsteemi, mitte vestluse logi;
- andmetöötluse kokkulepe pakkuja alla — seal sees: ei treeningut, kustutamise korraldus, serveri riik;
- kliendile nähtav tekst: „Vestlust töötleb AI; vestlusekiri hoitakse 90 päeva.“

**4. Turvalisusjuhtumi harjutus.** Juht küsib ülevalt: „Kui logifail läks valele adressaadile, mis nüüd?“ Meeskond harjutab plaani läbi:

| Samm | Ravikoda teeb |
|---|---|
| Mis juhtus | mis fail, kelle andmed, mitu kirjet, kellele läks — faktid kirja poole päeva jooksul |
| Piirata | adressaadile kirjalik kustutamise ja edasilevitamisest hoidumise palve; automaatse aruande saatmine peatatud |
| Teavita | tõsiduse hindamine: tervisedetailidega fail — asjakohasel juhul Andmekaitse Inspektsioonile 72 tunni jooksul |
| Õpi | aruanded edaspidi ainult nimetatud aadressile, ligipääsud rollide järgi, harjutus kirja |

> **Lihtsalt öeldes:** inventuur ei leidnud ründajat — leidis kolm arglikku viga, mis kõik tulid ühest puudusest: keegi polnud kirja pannud, mis andmetega juhtub. Ja plaan on harjutatud: esimesel päris juhtumil pole hea hetk otsida, kus tulekustuti ripub.

## Kokkuvõte

- **Isikuandmed on AI-süsteemis reeglina olemas** — kliendi nimi, tellimused, kirjade ja vestluste sisu. Viis põhinõuet (läbipaistvus, andmete vähendamine, salvestuspiir, õigus unustada, turvaline töötlus) vastavad küsimusele „mis on kohustuslik“.
- **Pakkuja on andmetöötleja:** küsi kokkulepe, riik, treening, logid, kustutamine — ja pea meeles, et mittetäitmine jääb ettevõtte vastutusele ([1.6](../01-alused/06-rollid-ja-vastutus.md)).
- **AI lisab kolm küsimust:** mälu ja logid ei tohi kliente segada ([4.3](../04-agendid-ja-mootmine/03-pikaaegne-malu.md)); automaatses otsuses inimese kohta peab otsust saama vaidlustada — kinnitusahel ([3.5](../03-susteemi-ulesehitus/05-ohutus.md)) ja nähtav vaidlustamise tee; jälge teeb korrektsuse näidatavaks.
- **Turvalisusjuhtum (ingl k *incident*):** mis juhtus → piirata → teavitada (asjakohasel juhul 72 tunni jooksul) → õppida. Plaan kirjas ja harjutatud enne juhtumit.
- **Audit (auditeeritavus — ettevalmistus kontrolliks)** on kolme asja kogum: inventuur, jälge, kirjad. Kui järelevalveasutaja või klient küsib „kuidas te andmeid töötlete?“, peab vastus olema kirjas — ja tõene.
- Tehnilised kaitsekihid jäävad naaberdokumentidesse: [3.5](../03-susteemi-ulesehitus/05-ohutus.md) ja [3.7](../03-susteemi-ulesehitus/07-turvalisus.md).

## Mis edasi?

- eelmine → [5.2 Meeskonnatöö ja standardid](02-meeskonnatoo-ja-standardid.md)
- järgmine → [5.4 Kulustrateegia suures mahus](04-kulustrateegia.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
