# 5.4 Kulustrateegia suures mahus

> **Sihtpublik:** juhtiv mitte-tehniline + tehniline | **Eeltingimused:** [5.3 Turvalisus ja andmekaitse (GDPR, audit)](03-turvalisus-ja-andmekaitse.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, milles erineb mitme süsteemi kulustrateegia ühe süsteemi eelarvest — ja miks peamine nõue on atribueeritavus (kulude juurde viitavus);
- sildistada kulud nii, et iga arvel olev euro on seotud kindla süsteemi, kliendi ja keskkonnaga;
- panna iga süsteemi ja iga keskkonna jaoks eraldi kululagi (ingl k *budget limit* — ette seatud kuumiit, [3.6 põhimõte](../03-susteemi-ulesehitus/06-kulude-haldamine.md));
- pidada kvartaalset ülevaatust (ingl k *quarterly review* — korrapärane arutelu, mis tasus end ära ja mida lõpetada) koos juhi küsimuste nimekirjaga;
- langetada kasu/kulu võrdluse põhjal otsus, millal süsteemi optimeerida ja millal lõpetada;
- kaitsta end agendipõhise süsteemi ootamatu kulutuse eest.

## Lihtsalt öeldes

> Ühe süsteemi juures piisab hinnangust ja ühest kululagist. Kümne süsteemi ja kaheksa kliendi juures tekib uus küsimus: **kes selle arve tegi?** Suures mahus pole kulude juhtimine ühekordne ülesanne, vaid jätkuv juhtimisprotsess: iga süsteemi kulu on eraldi nähtav, igal süsteemil on oma lagi ja kord kvartalis istutakse kokku, et vaadata, mis tasus end ära — ja mida ära lõpetada.

## Ühe süsteemi eelarvest strateegiani

Dokument [3.6 Kulude haldamine](../03-susteemi-ulesehitus/06-kulude-haldamine.md) õpetas ühe süsteemi eelarvet: hinnang enne käikuandmist, pilk juhtpaneelile (ingl k *dashboard* — pakkujate veebiportaal, kus tegelik kasutus ja kulud on nähtavad), üks hoiatuskünnis ja üks kululagi. See töötab seni, kuni süsteeme on üks.

Suures mahus muutub pilt kolmel viisil:

- **Süsteeme on mitu.** Igaühel oma juhised, oma kasutustempo ja oma võimalus kulu kasvatada.
- **Ehitajaid on mitu.** Iga meeskonnaliige tekitab kulutusi, mida teised ei näe.
- **Pakkujaid on mitu.** Erinevate mudelite ja platvormide arved tulevad erinevatest kohtadest ja ükski neist ei näita tervikpilti.

Sellest tulenevad strateegia kaks peamist nõuet. **Esiteks: kulu peab olema atribueeritav** — iga euro saab viia kindla süsteemi ja kliendi juurde; muidu on arve üks arusaamatu summa. **Teiseks: juhtimine peab olema ennetav.** Ühe süsteemi juures on kuu ülevaatusega hilinemine odav viga; kümne süsteemi juures tähendab see juba päris raha. Strateegia töötab piiride ja teadetega, mis rakenduvad ise — mitte uurimisega, mis algab pärast üllatusarve lahtitõmbamist.

Eelarvepõhimõtted jäävad samaks, aga korrutatakse süsteemi kaupa:

- **Igal süsteemil oma kululagi.** 3.6 kolm taset — teavitab, vähendab, peatab — kehtivad iga süsteemi kohta eraldi. Üks suur lag kogu portfellile näitab küll üllatust, aga mitte selle põhjust.
- **Keskkondade eraldus.** Arenduse ja testimise kulu on eraldi piiriga, tootmisest eraldatud — nii ei söö katsetamine tootmise eelarvet ega vastupidi. Keskkondade ülesehitus on [5.1 Arhitektuur suures mahus](01-arhitektuur-suures-mahus.md) teema.
- **Kvartaalne ülevaatus.** Kord kvartalis vaatab juhtkond numbrid üle: mis tasus end ära, mida optimeerida, mida lõpetada. Kuidas, räägime allpool.

> **Lihtsalt öeldes:** ühe süsteemi eelarve on üks kuumiit ja üks teade. Strateegia on nende süsteem: iga süsteem ja iga keskkond oma piiriga, hoiatused sisse ehitatud — ja kalender, mis sunnib kord kvartalis numbreid ausalt üle vaatama.

## Kulude sildistamine: iga euro teab, kust ta tuli

Atribueeritavus ei sünni iseenesest: pakkujate arved näevad välja nagu üks paks number, kui midagi ette ei võta. Lahendus on kulude sildistamine (ingl k *cost tagging* — iga kulustatud euro on seotud süsteemi/kliendiga). Praktikas: iga süsteem oma võtmega, iga võti siltidega, ja juhtpaneel peab võimaldama numbreid siltide kaupa vaadata.

Kolm silti, mis iga kulu juurde viivad:

| Silt | Küsimus, millele vastab | Näide |
|---|---|---|
| Süsteem | milline automatiseering kulu tekitas? | „sisestuskavandid“ |
| Klient | kelle eest (või millise allüksuse)? | „Kliendibüroo Kask“ |
| Keskkond | arendus või tootmine? | „tootmine“ |

> **Lihtsalt öeldes:** ilma siltideta ütleb arve ainult „AI maksis selle kuu 300 €“ — ja keegi ei saa öelda, kas see oli raha hästi kulutatud. Siltidega muutub arve nimekirjaks, kus igal real on omanik: rea võib võrrelda, piirata ja vajadusel lõpetada.

Siltideta pole võimalik ka ootamatut kulutust õigeaegselt märgata — ja agendid (vt [4.1 Agendi süsteemid](../04-agendid-ja-mootmine/01-agendi-susteemid.md)) on siin eraldi risk: agent ei tee üht päringut, vaid paneb ühe ülesande lahendamiseks ette mitu käiku, ja kui mõni käik läheb tsüklisse, kasvab kulu kiiresti. Kaitse on kaheti: agendi käikude piir (max sammud ülesande kohta) igale agendile eraldi — [4.4 Mitme agendi arhitektuurid](../04-agendid-ja-mootmine/04-mitme-agendi-arhitektuurid.md) teema — ning sildi kaupa seatud hoiatused, mis teatavad enne arve saabumist (3.6 põhimõte).

## Kasu ja kulu: millal süsteem lõpetada

Sildistus näitab, kes maksab. Et otsustada, kas raha on hästi kulutatud, on vaja kasu/kulu võrdlust. Kasu lähtub 1.4 põhimõttest (säästunud aeg = ühe tegevuse kestus × korduste arv, [1.4 Kus AI-automatiseerimine juba töötab](../01-alused/04-kasutujuhtumid.md)): süsteemi väärtus on säästetud tööaeg, miinus kulu, pluss kvaliteedi tõus. Kulu näitab sildistatud arve rida. Edasi kolm sammu, selles järjekorras:

1. **Mõõda kasu.** Kui palju aega ja raha süsteem tegelikult säästab? Kuidas seda usaldusväärselt mõõta, vaatab [4.5 Hindamine](../04-agendid-ja-mootmine/05-hindamine.md).
2. **Optimeeri enne lõpetamist.** Käi läbi optimeerimise portfell: kas lihtsamad ülesanded saaks teha **väiksemal mudelil** ja korduvad vastused **vahemällu** jätta (mõlemad — [4.7 Jõudlus ja latentsus](../04-agendid-ja-mootmine/07-joudlus-ja-latentsus.md))? Kas suur ja ennustatav maht annab aluse **lepingulisele hinnale** pakkujaga — põhimõtteliselt: kindel mahutellimus on läbirääkimiste kaart odavama ühikuhinna jaoks.
3. **Lõpeta reegli järgi.** Kui pärast optimeerimist on kulu järjekindlalt suurem kui kasu ja parandus pole teel, lõpeta süsteem. Töötamise jätmine pole neutraalne otsus — see maksab iga kuu.

> **Lihtsalt öeldes:** süsteem, mis tasub end ära, vajab ainult järelevalvet. Süsteem, mille kulu ületab kasu, saab ühe võimaluse — optimeeri; kui see ei aita, lõpetatakse. Reegel on kirjas ja kehtib kõigile võrdselt, mistõttu on ülevaatuse arutelu asjalik, mitte tundeline.

Kvartaalne ülevaatus on koht, kus see kõik läbi mängitakse. Juhile piisab kolmest küsimuste blokist:

- **Numbrid:** milliste süsteemide kulu kasvas ja kas põhjus on teada? Kas mõni lähenes kululagile?
- **Kasu:** millised kaks-kolm süsteemi tasusid end kõige paremini ära? Kus jäi sääst ootustest väiksemaks?
- **Otsused:** mida optimeerida, mida lõpetada, kuhu järgmises kvartalis tähelepanu pöörata?

## Näide samm-sammult: Sõnarohu kolm korda kasvanud arve

Väljamõeldud **„Sõnarohi“** — sisuturunduse agentuur, kelle vood põhinevad [4.4 kolme agendi arhitektuuril](../04-agendid-ja-mootmine/04-mitme-agendi-arhitektuurid.md). Kaheksa klienti, igaühe jaoks sisutootmise voog: teemade leidja → teksti koostaja → kontrollagent. Tavaline kuu kulus ~100 €.

**1. Kaos.** Ühel kuul kasvas pakkuja arve ~300 €-ni — kolm korda. Juhtpaneel näitas üht kogusummat, sildistust polnud, ja keegi ei osanud öelda, **kumma kliendi** arvel see oli. Iga voog tundus töötavat nagu enne.

**2. Sildistamine.** Kõik vood said kliendi kaupa sildid ja igale kliendile oma kululagi. Järgmise kuu tabel näitas, kus kulu oligi (numbrid näitlikud ja ümarad):

| Klient | Süsteem | Kuu kulu | Kululagi |
|---|---|---|---|
| A–F (6 klienti) | sisuvoog | ~50 € kokku | 20 € / klient |
| Klient G | sisuvoog | ~200 € | 20 € |
| Klient H | sisuvoog | ~18 € | 20 € |

Üks klient andis umbes kaks kolmandikku kogu arvest — üks rida paistis tabelis silma.

**3. Põhjus.** Kliendi G „teemade leidja“ agent töötas vale juhise toel: ta otsis kliendi varasemate postituste ja andmete hulgast „piisavalt head“ näidet lõputult. Otsingutsükkel ei leidnud kunagi rahuldavat tulemust, aga iga käik maksis — üks ülesanne võis tähendada kümneid päringuid.

**4. Kaitse.** Agendile pandi agendi käikude piir: maksimaalselt 5 käiku ülesande kohta — kui piir täis, lõpetab agent ja juhtum märgitakse ülevaatuseks. Lisaks hoiatuskünnis 50% kululagist, mis saadab teate haldajale enne, kui kuu läbi on (3.6 põhimõte). Järgmisel kuul oli kogukulu tagasi ~110 € juures.

**5. Kvartaalne ülevaatus.** Kaheksa voogu käiakse üle. Kliendi H vood lõpetatakse: kulu jäi pidevalt kasust suuremaks ja optimeerimine ei aitanud. Kahe kliendi (B, E) vood viiakse üle väiksemale mudelile — kvaliteet jääb [4.5 hindamise](../04-agendid-ja-mootmine/05-hindamine.md) nõuetele vastavaks ja nende kulu langeb umbes poole võrra.

Põhimõtteline järeldus: kolmekordne arve põhines ühe agendi veal ühe kliendi juures. Sildistus tegi sellest ühe päeva päevakorra — ilma selleta oleks agentuur optimeerinud pimesi kõiki kaheksat voogu.

## Kokkuvõte

- **Strateegia erineb ühe süsteemi eelarvest (3.6) selle poolest, et skaala on teine:** mitu süsteemi, ehitajat ja pakkujat nõuavad, et kulu oleks atribueeritav ja juhtimine ennetav, mitte järelpauk.
- **Kulude sildistamine (ingl k *cost tagging*) on alus:** iga euro seotud süsteemi, kliendi ja keskkonnaga — ilma selleta on arve üks arusaamatu number.
- **Igal süsteemil ja keskkonnal on oma kululagi**, hoiatused on sildi kaupa seatud ja kvartaalne ülevaatus vaatab kord kvartalis üle, mis tasus end ära ja mida lõpetada.
- **Kasu/kulu võrdlus lähtub 1.4 põhimõttest (säästunud aeg = ühe tegevuse kestus × korduste arv):** enne lõpetamist optimeeri — väiksemad mudelid, vahemälu, lepinguline hind — aga kui kulu on pidevalt kasust suurem ja ei parane, lõpeta.
- **Agendid kulutavad ettearvamatult:** agendi käikude piir igale agendile ja sildi kaupa hoiatused hoiavad ootamatu kulu enne arvet kinni.

## Mis edasi?

- eelmine → [5.3 Turvalisus ja andmekaitse (GDPR, audit)](03-turvalisus-ja-andmekaitse.md)
- järgmine → [5.5 Mudelite vahetamine ja drift](05-mudelite-vahetamine.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
