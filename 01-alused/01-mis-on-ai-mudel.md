# 1.1 Mis on AI-mudel ja kuidas ta „mõtleb“

> **Sihtpublik:** kõik | **Eeltingimused:** puuduvad — see on teekonna esimene dokument.

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada lihtsalt, mis on AI-mudel ja millest tema töö koosneb;
- seletada, miks mudel mõnikord vastab veenvalt, aga eksib — ja mida selle vastu teha;
- eristada tavalist programmi AI-mudelist ja mõista, miks see muudab süsteemide ehitamise põhimõtteid;
- kirjeldada samm-sammult, kuidas mudel töötleb kliendi päringut e-arve kohta.

## Lihtsalt öeldes

> AI-mudel on nagu assistent, kes on elu jooksul läbi lugenud tohutu raamatukogu. Küsimuse saades ei otsi ta vastust ühestki failist ega andmebaasist — ta ennustab, milline tekst kõige tõenäolisemalt järgneks. Seepärast on ta väga paindlik: oskab vastata, kokku võtta, tõlkida, kirjutada. Aga sama omaduse tagapool on nõrkus: ta *ei tea*, ta *arvab* — ja võib arvata veenvalt, öelda kindlal häälel välja asja, mis pole tõsi.

Kui seda meeles pidada, on kogu käsiraamat lihtsam mõista: me ei ehita süsteemi, mis täidab käske täpselt, vaid süsteemi, mis annab *väga hea pakkumise* — ja meie ülesanne on see enne kasutamist kontrollida.

## Kuidas AI-mudel tegelikult töötab

### Sisend → ennustus → väljund

> **Lihtsalt öeldes:** mudel võtab vastu teksti ja otsib selle jätkuks kõige tõenäolisemat järgmist sõnatükki — kordades. See on kogu tema „mõtlemine“.

Tehnilisele lugejale: keelemudel (ingl k *large language model*, LLM — suur keelemudel) on närvivõrk (arvutusmudel, mille parameetrid — nn kaalud — on miljardid numbrid), mis igas sammus arvutab iga võimaliku järgmise sõnatüki tõenäosuse lähtuvalt kontekstist. Vastus sünnib osade kaupa: mudel loeb seni kirjutatu, valib järgmise tüki, lisab selle konteksti ja kordab tsüklit. Sinu tekst on sisend (ingl k *input*), tükikaupa koostatud vastus on väljund (ingl k *output*).

Sellest oluline järeldus: mudelil pole andmebaasi, kust vastuseid tõmmata. Ta ei otsi sinu e-arvet üles ega loe kliendiregistrit — kui tahad, et ta sinu andmetega töötaks, anna andmed talle ise sisendisse.

### Treenimine vs kasutamine

> **Lihtsalt öeldes:** treenimine on aastatepikkune kool — lugeda tohutul hulgal tekste ja õppida, milline tekst kuidas jätkub. Kasutamine on see, mida sina igapäevaselt teed — küsid ja saad vastuse. Mudel ei õpi sinu vestlusest midagi: homme alustab ta jälle samade teadmistega.

Treenimise (ingl k *training*) käigus näidatakse mudelile tohutuid tekstihulkasid ning tema kaalud häälestatakse nii, et ennustus muutuks täpsemaks. Sinu osa on kasutamine (ingl k *inference* — järeldus, mudeli töökäik): saadad sisendi ja saad vastuse.

Tähtis tagajärg: mudeli teadmised „külmutatakse“ treeningu hetkel — ta ei tea pärast treeningut juhtunust midagi ega õpi sinu andmetest iseseisvalt. Vajadusel tuleb ajakohane teave tuua konteksti.

### Token — mudeli sõnatükk

> **Lihtsalt öeldes:** mudel ei loe tähti ega terviklikke sõnu — ta lõikab teksti tükkideks, mida nimetatakse tokeniteks (ingl k *token* — sõnatükk, teksti lõik mõne tähemärgi ja ühe sõna vahel). Token on mudeli jaoks üks „sõna“.

Näide. Lause „Arve nr 2026-14 on tasumata“ võib mudeli silmis jaguneda nii:

```text
["Arve", " nr", " 2026", "-", "14", " on", " tasu", "mata"]
```

Pange tähele: pikemad eestikeelsed sõnad lõhutakse tükkideks („tasumata“ → „tasu“ + „mata“). Eesti keel kulutab seepärast sageli rohkem tokeneid kui inglise keel — ja paljud teenused arvestavad hinda tokenite järgi.

### Kontekstiaken — kui palju mudel korraga „mäletab“

> **Lihtsalt öeldes:** kontekstiaken (ingl k *context window* — tekstihulk, mida mudel korraga näeb) on nagu töölauapind: kõik, mis pinnale ei mahu, jääb nähtamatuks, isegi kui see on mujal sahtlis olemas.

Võrdluseks: raamatu lehele mahub umbes 500 sõna, aga mõned tänapäeva mudelid mahutavad aknasse kümneid tuhandeid kuni miljoneid sõnatükke — kümneid raamatuid korraga. Kogu maailma siiski mitte. Kaks praktilist järeldust:

1. Kui mudel „unustab“ midagi vestluse algusest, pole tegemist tahtega — teave on aknast väljas või mudeli tähelepanu (ingl k *attention* — mehhanism, mis jaotab kaalu sisendi osade vahel) on hõivanud muu info.
2. Iga sisend ja väljund kulutab konteksti — pikk automaatne töötsükkel täidab akna kiiresti.

### Suured ja väiksed mudelid — miks valik oluline

> **Lihtsalt öeldes:** suur mudel on kogenud spetsialist — võimekas, aga aeglane ja kallis. Väike mudel on tubli praktikant — kiire ja odav, aga vajab täpsemaid juhiseid ja teeb nõudlevate ülesannete juures rohkem vigu.

Suured mudelid saavad hakkama keerukama loogika ja pikkade juhistega, aga maksavad rohkem ja vastavad aeglasemalt. Väiksemad sobivad lihtsateks, massilisteks töödeks — näiteks kirja liigitamine („arve“, „kaebus“, „tellimus“). Süsteemiehitaja reegel: **alusta väiksemaga ja tõusu suuremale seal, kus väiksem testimisel ei saa hakkama.**

## Miks mudel ei ole alati õige (hallutsinatsioon)

> **Lihtsalt öeldes:** mudel on suurepärane veenja. Ta võib kindla häälega ja hästi üles ehitatud tekstina öelda midagi, mis on väljamõeldis — seda nimetatakse hallutsinatsiooniks (ingl k *hallucination* — mudeli enesekindel, aga faktidel mitte põhinev vastus).

**Miks see juhtub?** Kuna mudel ennustab järgmist sõnatükki tõenäosuse alusel, on tema jaoks vastus, mis *kõlab õigesti*, ja vastus, mis *on õige*, sarnased ülesanded. Kui fakt on tema teadmistes nõrk või puudub, ei ütle ta „ma ei tea“ — ta jätkab kõige tõenäolisemat teksti. Nii võib ta luua näiteks e-posti aadressi, mis kõlab usutavalt, aga pole kellegi oma.

Klassikaline ärinäide: küsid müüginumbreid ilma numbreid kaasa andmata — mudel võib pakkuda ümarad ja usutavad summad, mis ei vasta ühelegi tegelikule andmele.

**Kolm praktilist reeglit, kuidas riski vähendada:**

1. **Anna mudelile faktid kaasa.** Ära toetu mudeli mälule — pane vajalik info (dokument, tabel, andmebaasi väljad) otse sisendisse. Siis põhineb vastus sinu andmetel, mitte statistilisel mälul.
2. **Nõua allikat.** Kritilise teabe puhul palu mudelil tugineda olemasolevale tekstile ja näidata, kust teave pärineb. Süsteem peab lubama ka vastust „ei leia allikast“.
3. **Kriitilised otsused jäävad inimesele.** Raha, lepingud, õiguslikud seisukohad — inimene vaatab üle enne, kui midagi väljub süsteemist: mida suurem on vea hind, seda tugevam on kontrollpunkt.

## Programme ja AI-mudeli erinevus

| Tavaline programm | AI-mudel |
|---|---|
| **Deterministlik** — sama sisend annab alati sama tulemuse | **Statistiline** — sama sisend võib anda eri korradel eri vastuse |
| Töötab programmeerija kirjutatud reeglite järgi | On õppinud näidetest — reeglid on peidus sisemistes numbrites |
| Viga on prognoositav: programm krahhib (ingl k *crash* — lakkab töötamast) või annab veateate | Viga on veenev: vale vastus näeb välja nagu õige |
| Ei oska midagi, mida pole programmeeritud | Oskab üldistada olukordadesse, mida pole näidatud |

Sellest järgneb kõige tähtsam: AI-süsteemi ehitamise põhimõtted on teistsugused — AI-põhises süsteemis on juhuslikkus loomulik osa, mitte viga. Seepärast:

- ehitame sisse **automaatseid kontrolle** (nt „kas vastuses on arve number ja kas see on andmebaasis olemas?“), mis püüavad halvad vastused kinni;
- paneme **inimese ülevaatuse** sinna, kus automaatne kontroll ei saa hakkama;
- kavandame **varutee** (ingl k *fallback* — ettevalmistatud alternatiiv, mida süsteem kasutab, kui põhitee ebaõnnestub): vale vastus pole erand, vaid olukord, millega süsteem peab oskama toime tulla.

See vaatenurk — mudel kui ebakindel, aga võimekas komponent, mida ümbritseb korrapärane süsteem — on kogu käsiraamatu tuum.

## Näide samm-sammult: kuidas mudel töötleb kliendipäringut

Oletame, et klient kirjutab: *„Tere! Kas mu e-arve nr 2026-14 on juba makstud?“*

1. **Kliendi kiri jõuab süsteemi.** Süsteem (mitte mudel) loeb kirja ja otsustab, et sellele tuleb vastata.
2. **Tekst tükeldatakse tokeniteks.** Kiri jaotub sõnatükkideks ja muudetakse numbriteks, mida mudel oskab töödelda.
3. **Süsteem paneb konteksti kokku.** Oluline samm: kirjale lisatakse kliendi andmed (nt „arve 2026-14: makstud 12.03.2026“) ja juhised („vasta lühidalt, viita arve numbrile“). Nüüd on sisendis kõik, mida korrektne vastus vajab.
4. **Mudel ennustab vastuse tükikaupa.** Mudel loeb kogu konteksti ja kirjutab vastust üks sõnatükk korraga.
5. **Vastus jõuab kliendini.** Näiteks: „Tere! Jah, arve nr 2026-14 on makstud — makse laekus 12.03.2026.“
6. **Kus võib minna valesti ja mida inimene näeb.** Kui sammus 3 andmeid kaasa ei antud, võib mudel hallutsineerida — luua maksekuupäeva, mis kõlab usutavalt, aga pole tõsi. Seepärast näitab süsteem inimesele, kust andmed tulid ja millel vastus põhineb; vale numbriga vastus suunatakse inimesele ülevaatusele.

Pange tähele: mudeli roll on ainult samm 4 — kogu muu (andmete toomine, konteksti kokkupanek, vastuse kontroll) on tavaline, usaldusväärne tarkvara. Selline tööjaotus (nn orkestratsioon — süsteem, mis korraldab osade koostööd) on hästi toimiva AI-süsteemi võti.

## Kokkuvõte

- **AI-mudel ei otsi vastuseid üles, vaid ennustab järgmist sõnatükki** — seepärast on ta paindlik, aga garantiita õige.
- **Token on mudeli tööühik** — sõnatükk, millesse tekst lõigatakse; tokenite hulk mõjutab hinda ja seda, kui palju kontekstiaknasse mahub.
- **Hallutsinatsioon ei ole jama, vaid ennustusloogika loomulik tagajärg** — selle vastu aitab faktide kaasaandmine, allika nõudmine ja inimese kontrollpunkt.
- **Tavaline programm krahhib, kui eksib; AI-mudel annab veenva vale vastuse** — seepärast peab mudelit ümbritsema süsteem, mis kontrollib, piirab ja vajadusel inimesele suunab.

## Mis edasi?

- Järgmine dokument → [1.2 Võimalused ja piirid: mida automatiseerida tasub](02-voimalused-ja-piirid.md)
- Tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
