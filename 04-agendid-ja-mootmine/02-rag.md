# 4.2 RAG: oma andmete kasutamine vastuste allikana

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [4.1 Agendi süsteemid](01-agendi-susteemid.md), [3.2 Konteksti haldamine](../03-susteemi-ulesehitus/02-konteksti-haldamine.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- nimetada kolm teed, kuidas mudelile sinu teadmised kätte saada;
- kirjeldada RAG-i (ingl k *retrieval-augmented generation* — otsinguga täiendatud genereerimine) voogu neljas sammus: ettevalmistus, otsing, kontekst, vastus allikaga;
- seletada mittetehniliselt, kuidas otsing (ingl k *retrieval* — küsimusele vastava tüki leidmine) sarnasuse kaudu töötab;
- öelda, miks allikas (ingl k *source* — kust vastus tuli) on vastuse kohustuslik osa;
- hooldada teadmusbaasi (ettevõtte enda dokumentide kogu) elava süsteemina ja tunda, millal RAG-i pole vaja.

## Lihtsalt öeldes

> Mudel õppis oma teadmised maailmast, mitte sinu kontorist — sisereeglid, hinnakiri ja arvestusjuhend on talle võõrad. RAG-i põhiidee: ära ürita mudelit asju pähe õpetada, vaid anna talle enne igat vastamist õige leht ette. Süsteem otsib dokumentidest küsimusele vastava tüki, paneb selle koos küsimusega mudeli ette ja vastuses on kirjas, kust see tuli.

## Kolm teed, kui mudel peab teadma sinu asju

Kui mudel peab vastama sinu ettevõtte asjade kohta — sisereeglid, hinnakiri, reeglite failid —, on kolm teed:

**1. Kõik prompti panna.** [2.4](../02-praktika/04-sisendid-ja-andmed.md) näitas, kuidas andmed päringuga kaasa anda — ja kui teadmine on vähe ning muutumatu, on see õige tee. Piir tuleb aga ette: kontekstiaken (ingl k *context window* — tekstihulk, mida mudel korraga näeb) saab täis. Suure teadmise puhul ei tasu seda prompti panna: 200-leheküljeline käsiraamat on ~150 000 tokenit, mis arveldataks iga küsimuse juures uuesti (vt [3.6](../03-susteemi-ulesehitus/06-kulude-haldamine.md)), ja suures hulgas infost jääks suur osa tähelepanuta (vt [3.2](../03-susteemi-ulesehitus/02-konteksti-haldamine.md)) — isegi kui suur aken ta tehniliselt mahutaks.

**2. Mudel ümber treenida.** Kallis, aeganõudev ja enamasti mõttetu: faktid muutuvad ja treenimine tuleb iga uuenduse järel uuesti teha, ning vastusel pole allikat. Treening sobib stiili ja vormi õpetamiseks, mitte muutuvate faktide hoidmiseks.

**3. RAG — kolmas tee.** Teadmine jääb sinu dokumentidesse; iga küsimuse puhul otsitakse sealt õige tükk, pannakse konteksti ja mudel vastab selle põhjal. Mudelit ei õpetata ümber — uuendatakse ainult dokumendikogu.

> **Lihtsalt öeldes:** kaks esimest teed on „pane kõik lauale“ ja „õpi pähe“ — esimene läheb suure teadmisega täis ja kalliks, teine vananeb kohe, kui hind muutub. Kolmas on arhiiv ja abiline, kes leiab sealt hetkel vajaliku lehe.

Millal kolmas tee **pole** vaja? Kolm juhtu: teadmisi on vähe ja need on püsivad — siis mahub kõik prompti (vt [2.4](../02-praktika/04-sisendid-ja-andmed.md)); vastus peab olema sajaprotsendiliselt täpne arvutus — siis aitab tööriistakutse, mitte RAG (vt [3.3](../03-susteemi-ulesehitus/03-tooriistad-ja-tegevused.md)); andmed on salajased — siis pole lahendus RAG, vaid andmete vähendamine ja hoolikas teenusepakkuja valik (vt [3.7 Turvalisus](../03-susteemi-ulesehitus/07-turvalisus.md)).

## RAG-i voog neljas sammus

### 1. Dokumentide ettevalmistus: tükeldamine

Esimene samm ei puuduta mudelit üldse: dokumendid lõigatakse otsitavateks tükkideks — tükeldamine (ingl k *chunking*). Kaks reeglit:

- **Üks teema ühes tükis.** Lõikamine käib pealkirjade järgi: peatükk või alapeatükk koos oma pealkirjaga. Liiga suur tükk toob hunniku müra; liiga väike jääb tähendusetuks — lõik „tohib maha arvata 40 protsenti“ ilma pealkirjata ei ütle, mis kulust jutt on.
- **Ühtne struktuur.** Iga tükk teab, mis dokument ja peatükk ta on ning millal kehtib — just see info jõuab hiljem vastusesse allikana.

### 2. Otsing: kuidas õiget tükki leitakse

Vastus elab tavaliselt mõnes tükis tuhandete hulgast — kuidas süsteem selle leiab? Sarnasuse kaudu.

Iga tükk kujutatakse arvuliselt — vektor (ingl k *embedding* — teksti arvuline kujutis, mis laseb sarnaseid tekste leida). Kujutle, et iga tükk saab raamatukogus kindla riiulikoha ja sarnase sisuga tekstid satuvad riiulil üksteise kõrvale. Küsimus pannakse samasugusesse kujutisse ja otsing toob selle lähimad naabrid. Küsimus ja tükk võivad sõnade poolest erineda, aga tähenduse poolest jäävad riiulil kõrvuti.

Kui otsing aga õiget tükki ei leia — jämedad tükid, ebamäärane küsimus —, jääb mudel faktideta ja hakkab arvama; selliseid juhte käsitleb [3.4 Vead ja veakäsitlus](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md).

### 3. Tükkide panek konteksti koos küsimusega

Süsteem paneb kontekstiaknasse kokku juhise („vasta ainult allikatükkide põhjal; kui siin vastust ei ole, ütle seda“), 2–5 parimat leitud tükki ja küsimuse. Edaspidi on olukord sama, mis iga tavalise päringu puhul (vt [3.2](../03-susteemi-ulesehitus/02-konteksti-haldamine.md)) — ainult et teadmistega tükk tuleb aknasse otsingult, mitte sinu käest.

### 4. Vastus allikaga

Mudel sõnastab vastuse ja näitab, kust see tuli. Struktureeritud väljundina (vt [2.2](../02-praktika/02-struktureeritud-valjund.md)) näeb see välja umbes nii:

```json
{
  "küsimus": "Kas kaugtöö sidekulu tohib tööjõumaksudest maha arvata?",
  "vastus": "Jah, kui kaugtöö kokkulepe on kirjalik ja kulu dokumendiga tõendatud.",
  "allikad": [
    { "dokument": "Arvestusreeglite käsiraamat", "tükk": "4.2", "kehtib": "2026-09-01" }
  ]
}
```

Kogu voog ühel pildil:

```text
   200-leheküljeline reeglite käsiraamat
        │
        ▼
   1. TÜKELDAMINE: ~600 tükki → iga tükk arvulisse kujutisse („riiulikoht“)
        │
        ▼
   KÜSIMUS: „kas sidekulu kaugtööl tohib maha arvata?“
        │
        ▼
   2. OTSING: küsimus samasse kujutisse → lähimad naabrid (tükk 4.2, tükk 2.3)
        │
        ▼
   3. KONTEKST: juhis + tükk 4.2 + tükk 2.3 + küsimus
        │
        ▼
   4. VASTUS + ALLIKAS: „Jah, kui ... — käsiraamat, lõik 4.2“
```

## Allikas kohustuslik: vastus, mida saab kontrollida

Allikas pole kaunistus — kolm praktilist põhjust, miks allikata vastust lubada ei tohi:

1. **Usaldus.** Kasutaja näeb, kust vastus tuli, ega pea uskuma pimesi.
2. **Kontroll.** Inimene kinnitusahelas (ingl k *human-in-the-loop* — töövoog, kus inimene kinnitab tulemuse enne kasutust) saab lõigu 4.2 avada ja võrrelda — hinnangust saab kontrollitav väide.
3. **Hallutsinatsioonide tuvastus.** Hallutsinatsioon (mudeli kindlalt öeldud, aga vale vastus, vt [1.1](../01-alused/01-mis-on-ai-mudel.md)) ei kao RAG-is ära: kui õige tükk otsingult ei tulnud, võib mudel ikkagi midagi usutavalt välja mõelda. Allikas teeb selle nähtavaks: kui allikad puuduvad või ei toeta öeldut, on tegemist arvamisega. Seepärast seisab juhises ka reegel „kui tükkides vastust ei ole, ütle seda“.

> **Lihtsalt öeldes:** allikata vastus on nagu tsitaat ilma leheküljenumbrita — usalda ainult siis, kui kontrolli pole vaja. Raha ja reeglite juures on kontrollida alati vaja.

## RAG on elus süsteem: hooldus

Dokumendid muutuvad: hind uueneb, reegel tühistatakse, tingimus lisandub. RAG-süsteem ei muutu nendega automaatselt kaasa — ja just siin elab kõige ohtlikum viga:

**Vananenud tükk on hullem kui puuduv.** Puuduv vastus toob ausa „ei leidnud“ ja inimene otsib ise; vananenud tükk toob enesekindla, allikaga varustatud vana vastuse — ja keegi ei kontrolli, sest allikas näeb usaldusväärne välja.

Selle vältimiseks kolm hooldusreeglit:

- **Uuendus = vana välja, uus sisse.** Kui reegel muutub, tuleb vana tükk teadmusbaasist eemaldada — mitte jätta uue kõrvale, muidu elab baasis kaks vastust ja süsteem valib nende vahel ettearvamatult.
- **Üks tõde ühe teema kohta.** Üks kehtiv versioon iga dokumendi kohta, kehtivuse kuupäev iga tüki küljes.
- **Regulaarne kontroll.** Iga dokumendimuudatuse järel esita süsteemile paar tuntud küsimust ja vaata, kas allikad on ajakohased. Kas süsteem vastab õigesti — kuidas seda mõõta, on hindamise teema (vt [4.5 Hindamine](05-hindamine.md)).

RAG pole ühekordne ehitus, vaid elus süsteem: teadmusbaasi hooldamine on süsteemi hooldamine.

## Näide samm-sammult: Nummerbüroo reeglite teadmusbaas

Väljamõeldud **Nummerbüroo** (vt [1.6](../01-alused/06-rollid-ja-vastutus.md)) — omanik Anu, viis raamatupidajat ja Jaak, kes süsteeme ehitab (ja kelle käe alt tuli [3.3](../03-susteemi-ulesehitus/03-tooriistad-ja-tegevused.md)-s arve-oleku tööriist). Järgmine korduv tüütus: raamatupidajad küsivad arvestusreeglite kohta — „kas telefoni- ja internetikulu tohib veel tööjõumaksudest maha arvata, kui inimene töötab ka kodus?“ — ja keegi peab vastuse 200-leheküljelisest sisefailist üles otsima.

Seda faili aknasse panna ei tasu: ~150 000 tokenit arveldataks iga küsimuse juures uuesti (vt [3.6](../03-susteemi-ulesehitus/06-kulude-haldamine.md)) ja suures hulgas infost jääks suur osa tähelepanuta (vt [3.2](../03-susteemi-ulesehitus/02-konteksti-haldamine.md)) — isegi kui suur aken ta tehniliselt mahutaks. Ümber treenima keegi ei hakka. Jaak ehitab RAG-i:

**1. Ettevalmistus.** Käsiraamat lõigatakse peatükkide järgi umbes 600 tükiks; iga tükiga pealkiri (nt „4.2 Kulud, mida tööjõumaksudest maha arvata tohib“), dokumendi nimi ja kehtivuse kuupäev.

**2. Otsing.** Iga tükk saab arvulise kujutise; raamatupidaja küsimus pannakse samasse kujutisse ja otsing toob kaks lähemat tükki: „4.2 Mahaarvatavad kulud“ ja „2.3 Kaugtöö tingimused“.

**3. Kontekst.** Juhis („vasta ainult tükkide põhjal; kui vastust pole, ütle seda“), kaks tükki, küsimus.

**4. Vastus allikaga:**

> „Jah — kaugtöö korral on sidekulud mahaarvatavad, kui kokkulepe on kirjalik ja kulud tõendatud. *(Allikas: Arvestusreeglite käsiraamat, lõik 4.2, kehtiv alates 2026-09-01.)*“

Enne: iga küsimus kümme minutit kaustade vahel. Pärast: sekundid — ja raamatupidaja kontrollib allika järgi vastuse enne kliendile ütlemist ise üle.

**Ja siis juhtub see, mis kindlasti juhtub.** 1. oktoobril muutub reegel. Jaak paneb uuendatud faili baasi, aga üks vana versiooni tükk jääb alles. Raamatupidaja küsib sama küsimust ja saab vastuse vana reegli järgi — enesekindlalt ja allikaga. Lahendus: vana tükk välja, uus sisse, kehtivuse kuupäevad eristavad neid — ühekordsest parandusest saab fikseeritud uuendusprotsess.

> **Lihtsalt öeldes:** ehitamine võttis Jaagult nädala; hooldamine võtab igal muudatuse korral viis minutit. Süsteem on hea täpselt nii kaua, kui keegi hoiab dokumendid ajakohased — see „keegi“ peab olema kirjas juba enne esimest küsimust.

## Kokkuvõte

- **Kolm teed:** kõik prompti, ümber treenimine (kallis, muutuvate faktide jaoks mõttetu), RAG (otsi õige tükk ja anna ta konteksti) — suure ja muutuva teadmise jaoks on kolmas ainus töötav.
- **Voog neljas sammus:** tükeldamine (üks teema ühes tükis, ühtne struktuur) → otsing (sarnasuse kaudu, vektori abil) → tükkide panek konteksti koos küsimusega → vastus allikaga.
- **Allikas on kohustuslik:** usaldus, kontroll inimese kinnitusahelas ja hallutsinatsioonide tuvastus.
- **RAG on elus süsteem:** uuendusel peab vana tükk välja minema, sest vananenud tükk annab enesekindla vale vastuse.
- **Millal RAG-i pole vaja:** teadmine mahub prompti; vastus peab olema sajaprotsendiliselt täpne arvutus (tööriistakutse, [3.3](../03-susteemi-ulesehitus/03-tooriistad-ja-tegevused.md)); andmed on salajased (andmete vähendamine, [3.7](../03-susteemi-ulesehitus/07-turvalisus.md)).

## Mis edasi?

- Kui teadmine on suurem, kui aknasse mõistlik panna → [3.2 Konteksti haldamine](../03-susteemi-ulesehitus/02-konteksti-haldamine.md)
- Kui otsing õiget tükki ei leia → [3.4 Vead ja veakäsitlus](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md)
- Kas RAG vastab õigesti → [4.5 Hindamine](05-hindamine.md)
- eelmine → [4.1 Agendi süsteemid](01-agendi-susteemid.md)
- järgmine → [4.3 Pikaaegne mälu ja oleku haldus](03-pikaaegne-malu.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
