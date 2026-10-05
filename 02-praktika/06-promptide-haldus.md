# 2.6 Promptide haldus kui vara

> **Sihtpublik:** kõik | **Eeltingimused:** [2.5 Esimene automatiseeritud workflow otsast lõpuni](05-esimene-workflow.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, miks prompt (ingl k *prompt* — mudelile antav juhis) on ettevõtte vara, mitte ühe töötaja märkmed;
- luua promptipank (ühine korraldatud hoidla) ja kirja panna iga prompti juures kuus põhiandmeid;
- nummerda prompte versioonideks ja kirja panna iga muudatuse juures, mida muudeti, miks ja kes kinnitas;
- taaskasutada ühte promptimalli mitmes kasutuses, teades, mida tohib muuta ja mida mitte.

## Lihtsalt öeldes

> Hea prompt on ettevõtte kogemus kirja pandud kujul: millised õppetunnid tulid kallil, kuidas kliendid räägivad, millised reeglid kehtivad. Kui need laused elavad ühe töötaja e-kirjades või sülearvutil, ei oma neid ettevõte — omab inimene ja võtab lahkudes kaasa. Promptipank tähendab, et kõik promptid on ühes kokkulepitud kohas, kus igaüks neid leiab, ja iga tähtsam muudatus on kirjas. Rohkem pole vaja: ühine dokument peab vastu.

## Miks promptid on vara

[1.6 Rollid ja vastutus projektis](../01-alused/06-rollid-ja-vastutus.md) ütles, et prompt on ettevõtte vara. Miks? Sest hea prompti sisse on kirjutatud teadmisi, mis pole kusagil mujal olemas:

- **Õppetunnid.** „Kui ühel päeval on mitu tehingut, lähevad kokkuvõttes igaüks eraldi reale, mitte üheks kirjeks“ — selle lause taga on tõenäoliselt valed kuu kokkuvõtted, millest keegi õppis.
- **Kliendikeel ja toon.** Kuidas sinu klientidega räägitakse — mis kõlab tuttavalt, mis liiga ametlikult — teab ainult sinu ettevõte.
- **Reeglid ja piirid.** Mida tulemus tohib sisaldada ja mida mitte; kus peab inimene vahele sekkuma.

Need kirjed on kalli hinnaga ostetud — testimise tsükli (testi, paranda, testi uuesti) ja töötundidega. Ja need on kaduvad: prompt, mis seisab ainult selle inimese arvutis, kes selle kirjutas, kaob temaga kaasa — haigus või lahkumine piisab.

Piirjoon: teadmised on ettevõtte varaks alles siis, kui nad on kirjas kohas, kuhu ka teistel ligipääs. Promptide hoidmine on sama tavaline töö nagu aruannete ja lepingute hoidmine.

## Kus prompte hoida: promptipank

> **Lihtsalt öeldes:** promptipank on ettevõtte retseptikogu — mitte iga koka peas, vaid köögiraamatus, mille järele igaüks vaatama saab. Iga retsepti juures märge: kes teeb, millal viimati prooviti, kas kasutusel.

Lihtsaim variant, millest väikesele ettevõttele piisab: **ühine dokument või kaustastruktuur**, kuhu kõigil on ligipääs. Iga prompti juures on pankas kuus põhiandmeid:

- **nimi** — nii, et teine inimene aru saab, millega tegu;
- **eesmärk** — üks lause: mida ta teeb;
- **kellele / kus kasutatud** — kes kasutab ja millises töövoos;
- **viimati testitud** — kuupäev, mitte mälu;
- **olek** — kasutusel, testimisel või arhiivis; arhiivi ei kustutata, sest ka vana variant on ajalugu;
- **versiooninumber** — nt v3: mitmes testitud seis on praegu kasutusel.

Kuupäev „viimati testitud“ on tähtsaim: [2.1](01-hea-prompt.md) õpetab, et kvaliteedi kinnitab testimise tsükkel — kuupäev ütleb teadmise vanuse.

Tehnilisele lugejale üks lause: git sobib, sest iga muudatuse ajalugu jääb iseenesest alles — aga mitte-tehniline meeskond saab ühise dokumendiga hakkama, kohustuslik pole.

Kui meeskond kasvab, lähevad muutmisõigused ja ülevaatuse vood standarditesse — vt [5.2 Meeskonnatöö ja standardid](../05-suurte-projektide-tase/02-meeskonnatoo-ja-standardid.md).

## Versioonihalduse põhitõed

> **Lihtsalt öeldes:** prompt käsitletakse nagu vormi variante: v1, v2, v3. Iga uue numbri juures on kirjas, mida muudeti, miks ja kes kinnitas. Uut versiooni ei tehta tuju järgi, vaid siis, kui testimine näitab, et vana enam ei tööta.

Versioon (ingl k *version* — üks fikseeritud seis, mida eristatakse numbriga, nt v1, v2) tähendab seda: muudatus ei kirjutata vanale üle, vaid tehakse uue numbriga. Vanad jäävad alles — kui uus läbi kukub, saab ühe sammuga tagasi.

Kolm kirjet iga uue versiooni juures:

1. **mida muudeti** — täpselt, millist lauset või reeglit;
2. **miks** — ei „natuke paremaks“, vaid konkreetne põhjus: milline testjuhtum (kindel sisend, mille puhul tead, milline vastus peab tulema) läbi kukkus;
3. **kes kinnitas** — kes uue versiooni käiku andis. Ka siin kehtib inimene kinnitusahelas (ingl k *human-in-the-loop* — töövoog, kus inimene kinnitab tulemuse enne kasutust): muudatus jõuab kasutusse ainult inimese otsusega.

Millal uuendada? Ainult siis, kui testimine näitab vajadust — mitte iga tuju järgi. „Mulle tundub, et saaks paremini“ pole muudatuse põhjus, vaid test: näitab vajadust — versioon põhjendatud; ei näita — vana jääb. Kui prompte on sadu ja kasutajaid palju, kasvavad need põhitõed standarditeks ja ülevaatuse voogudeks — vt [5.2](../05-suurte-projektide-tase/02-meeskonnatoo-ja-standardid.md) ning vastutus- ja juhtimisküsimusi [5.7 Vastutus, eetika ja governance](../05-suurte-projektide-tase/07-vastutus-ja-eetika.md).

## Taaskasutus ja kohanemine

> **Lihtsalt öeldes:** promptimall on nagu vorm lüngadega: lüngad täidad iga kord uute andmetega, aga vormi ennast ei lihvi. Lüngad on sinu — vorm on kogu meeskonna.

Ühte ja sama promptimalli (taaskasutatav prompt, kus muutuvad andmed on eraldatud) saab kasutada mitmel juhul: „kuu tehingute kokkuvõte“ töötab kõigi klientidega, kui andmed vahetuvad. Reegel on kaheosaline.

**Tohib muuta:**

- **kohatäiteid** (ingl k *placeholder* — mallis olev koht, mida kasutamisel asendatakse, nt [KLIENDI_NIMI] või [SUMMA]) ja nende sisu;
- **konteksti** — kliendi andmed, konkreetne arve, olukorra kirjeldus.

**Ei tohi muuta ilma uut testimise tsüklit läbimata** (vt [2.1](01-hea-prompt.md)):

- **testitud tuuma** — ülesannet, reegleid ja näiteid, mille põhjal prompt on läbi testitud;
- **formaadinõudeid** — väljundi kuju, pikkust ja struktuuri.

Loogika tuleb testimise tsüklist: mis on AI-mudelile sisendiks, on vahetatav; mis suunab, kuidas mudel vastab, on prompti osa ja selle muutus on uus prompt. Nii ei kasutata prompti, mida keegi „väikese paranduse“ järel enam testinud pole.

## Näide samm-sammult: Nummerbüroo promptipank

Väljamõeldud **Nummerbüroo** — sama ettevõte, kelle rollid [1.6](../01-alused/06-rollid-ja-vastutus.md)-s läbi käisime: omanik Anu, viis raamatupidajat ja Jaak.

### Lähteseis: 12 prompti, 4 inimest, ükski koht

Arve automatiseerimine tõi kaasa 12 prompti, mis on tekkinud nelja inimese peal. Kaks on pooleli raamatupidaja Kaja e-kirja mustandites, viis Jaagi sülearvutil, ülejäänud hajutatud vestlustes ja märkmetes. Kaja küsib sama prompti juba teist korda; Jaak parandab üht versiooni, aga Merle kasutab veel vana; keegi ei tea, kas „Kuu tehingute kokkuvõte kliendile“ on see, mida eile testiti.

### Promptipank üles

Jaak loob ühe ühise dokumendi ja kõik 12 prompti saavad põhiandmed. Väljavõte:

| Nimi | Eesmärk | Kasutaja | Viimati testitud | Olek | Versioon |
|---|---|---|---|---|---|
| Arve andmete väljaselgitus | Võta arve manusest välja kes, mis, summa ja käibemaks | kõik raamatupidajad | 28.09.2026 | kasutusel | v3 |
| Ebaselge arve küsimuste loend | Koosta kliendile küsimused puuduvate andmete kohta | kõik raamatupidajad | 22.09.2026 | kasutusel | v2 |
| Kuu tehingute kokkuvõte kliendile | Koosta kliendile kokkuvõte kuu tehingutest | Kaja, Merle | 30.09.2026 | kasutusel | v2 |
| Kuu kokkuvõtte e-kirja tekst | Vormista kokkuvõttest kliendikirja sissejuhatus | Kaja | 30.09.2026 | kasutusel | v1 |
| Arve andmed tabelisse | Vormista arve andmed sisestustabelisse | Jaak | 05.06.2026 | arhiivis | v1 |

Ülejäänud seitse samas vormis.

### Üks versioonihalduse juhtum

3. oktoobril leiab Kaja testimisel, et üks testjuhtum ei toimi: kuu, mil ühel päeval on mitu tehingut, annab kokkuvõttes vale tehingute arvu ja kogusumma — mall koondab päeva tehingud üheks kirjeks. Kaja, kes teab, milline kuu kokkuvõte on korrektne, parandab malli ja paneb muudatuse kirja — mida, miks, millal. Anu, omanikuna, kinnitab:

| Versioon | Kuupäev | Mida muudeti | Miks | Kes kinnitas |
|---|---|---|---|---|
| v1 | 12.05.2026 | Esimene kasutuskõlblik variant | — | Anu |
| v2 | 14.08.2026 | Formaadinõue: kokkuvõte tabelina, mitte lõiguna | Testjuhtum T3: klient tahtis ridasid kontrollida | Anu |
| v3 | 03.10.2026 | Uus reegel: ühel päeval mitu tehingut lähevad kokkuvõttes igaüks eraldi reale, mitte üheks kirjeks | Testjuhtum T7 kukkus läbi: mitme tehinguga päev andis vale kogusumma | Anu |

Väike juhtum, aga täpselt see, mida vara puhul oodata: aastate pärast näeb igaüks, miks kokkuvõte ridade kaupa tehakse ja kes kinnitas. Selguseks: dokumendis 2.1 testitud variandid (A, B, C) on katsetused; versiooninumbri v-number saab pankas ainult heaks kiidetud võitja.

### Jaak lahkub

Aasta pärast saab Jaak teiselt tööandjalt pakkumise. [1.6](../01-alused/06-rollid-ja-vastutus.md) meenutas: kui keegi lahkub, ei tohi teadmised temaga koos lahkuda — nüüd näitab pank, mida see tähendab:

- kõik 12 prompti on pankas — mitte ükski sülearvutil, mis kaasa läheb;
- kirjas, kes mida kasutab, kes mida kinnitas ja millal viimati testitud;
- versioonide ajalugu näitab, miks iga reegel just nii kirjas on — sülearvutile jääks vaid „küsi Kajalt“ või äraarvamine.

Lahkub inimene — mitte teadmised: uus ehitaja loeb panka ja jätkab sealt.

## Kokkuvõte

- **Prompt on ettevõtte vara** — õppetunnid, kliendikeel ja reeglid on tema sisse kirjutatud; hoidmata kaovad need inimesega.
- **Promptipank on lihtne** — ühine dokument või kaust; iga prompti juures nimi, eesmärk, kasutaja, viimati testitud, olek ja versiooninumber. Git sobib, aga pole kohustuslik.
- **Versioonid hoiavad ajaloo** — iga muudatuse juures mida, miks (milline testjuhtum kukkus läbi) ja kes kinnitas; uuenda ainult siis, kui testimine nõuab.
- **Taaskasuta läbi malli** — muuda kohatäiteid ja konteksti, ära puuduta testitud tuuma ega formaadinõudeid ilma uue testimise tsüklita.

## Mis edasi?

- eelmine → [2.5 Esimene automatiseeritud workflow otsast lõpuni](05-esimene-workflow.md)
- järgmine tase → [3.1 API integratsioonid](../03-susteemi-ulesehitus/01-api-integratsioonid.md)
- Testimise tsükkel → [2.1 Head promptini](01-hea-prompt.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
