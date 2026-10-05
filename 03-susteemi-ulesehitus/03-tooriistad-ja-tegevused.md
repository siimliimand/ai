# 3.3 Tööriistad ja tegevused: lase mudelil tegutseda

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [3.2 Konteksti haldamine](02-konteksti-haldamine.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, mis on tööriistakutse (ingl k *function calling / tool use* — võimalus, kus mudel saab süsteemilt küsida välise tegevuse sooritamist) ja kes selles tegelikult tegevuse sooritab;
- kirja panna tööriista kirjelduse (nimi + kirjeldus + parameetrid) — selle, mille põhjal mudel otsustab, millal tööriista kasutada;
- jälgida voo küsimusest vastuseni samm-sammult ja näha, kus töötab mudel ning kus sinu süsteem;
- otsustada, millal tööriistakutse õigustab end ja millal piisab ette kirjutatud workflow (töövoog — automatiseeritud sammude jada) sammust ([2.3](../02-praktika/03-workflow-algtasandil.md)).

Dokumendist [1.5](../01-alused/05-susteemi-anatoomia.md) teame süsteemi kolme kuju. Workflow’is on kogu tee ette kirjutatud — siin avame ukse, mis muudab midagi olulist: AI-mudel otsustab ise, millal tööriista kutsuda. Aga nagu kohe näeme, jääb tegutseja ikkagi sinu süsteemiks.

## Lihtsalt öeldes

> Tööriistakutse on nagu sekretär kontoris, kellel on sisetelefonide nimekiri. Külastaja küsib: „Mis on tellimuse nr 8812 olek?“ Sekretär (AI-mudel) ei jookse ise arhiivi — ta täidab lipiku „too mulle tellimuse nr 8812 kaust“ ja annab selle laduhoidjale (süsteemile). Laduhoidja toob kausta, sekretär loeb selle ja sõnastab külastajale vastuse. Sekretär ei saa ise ühtegi ukse lahti teha — ja enne kausta saabumist ei tea ta, mis seal kirjas on.

## Mis tööriistakutse täpselt on (ja kes tegelikult teeb)

Mudel oskab keelt ja teeb järeldusi tekstist, aga tal pole silmi sinu andmetesse ega käsi välismaal: ta ei tea, kas arve nr 2026-47 on makstud, ega saa ise kirja saata. Tööriistakutse on kokkulepe, mis selle lünga täidab. Kolm asja, mis selles kokkuleppes kirjas:

1. **Sina kirjeldad tööriista.** Enne esimest vestlust annad sa süsteemis üles tööriista kirjelduse: nimi, ühe lausega kirjeldus ja parameetrid (andmed, mida tegevus vajab).
2. **Mudel otsustab, kas ja millal kutsuda.** Iga kasutaja päringu (ingl k *request*) korral loeb mudel nii küsimust kui ka kõigi olemasolevate tööriistade kirjeldusi ja otsustab: kas selleks, et vastata, tuleb tööriista kasutada, millist ja mis andmetega.
3. **Kutsumus tuleb sinu süsteemile, mitte mudelile.** Mudel ei soorita tegevust ise — ta annab vastuse struktureeritud väljundina (ennustatavas vormis vastus, vt [2.2](../02-praktika/02-struktureeritud-valjund.md)): „kutsu tööriist nimega X andmetega Y“. Seda kutsumust loeb sinu süsteem (sinu programm, mis tegevuse sooritab), paneb tegeliku tegevuse käima — otsib andmebaasist, arvutab, saadab — ja annab tulemuse mudelile tagasi. Alles siis sõnastab mudel inimesele loetava vastuse.

Kolmas punkt on turvalisuse seisukohalt kriitiline: **mudel ei tee midagi iseseisvalt — süsteem teeb.** Mudel võib ainult soovitada; iga ukse avamise ja iga andmete otsimise sooritab programm, mis käitub ainult nii, nagu sina oled lubanud. Kuidas seda püsti panna, on API (programmiliides) teema ([3.1](01-api-integratsioonid.md)); mis juhtub, kui süsteemi osa ebaõnnestub, vaatab [3.4 Vead ja veakäsitlus](04-vead-ja-veakasitlus.md).

## Kuidas tööriista kirjeldada

Tööriista kirjeldus on kolmeosaline:

- **Nimi** — lühike ja täpne: `arve_olek`, `tellimus_otsing`, `email_kavand`. Nime järgi mudel tööriistu eristab, seega kaks tööriista ei tohi sama nime saada.
- **Kirjeldus** — üks lause, mis ütleb kaks asja: mida tööriist teeb ja millal teda kasutada. Just kirjelduse põhjal mudel valib — ebaselge kirjeldusega tööriista kutsutakse valel hetkel või jäetakse kutsumata, isegi kui süsteemi pool on veatu.
- **Parameetrid** — JSON-skeemi laadis struktuur (andmekirjeldus, mis ütleb, mis andmed ja mis kujul vajalikud on): iga parameetri nimi, liik (tekst, number) ja kas see on kohustuslik. Võta ainult see, mida tegevus tõesti vajab — mitte kogu vestluse koopia.

Näidis: tööriist, mis otsib arve oleku.

```json
{
  "nimi": "arve_olek",
  "kirjeldus": "Tagastab arve makseoleku raamatupidamisprogrammist. Kasuta, kui kasutaja küsib konkreetse arve numbri järgi, kas see on makstud.",
  "parameetrid": {
    "arve_number": {
      "liik": "tekst",
      "kohustuslik": "jah",
      "kirjeldus": "Arve täisnumber, näiteks „2026-47“"
    }
  }
}
```

> **Lihtsalt öeldes:** tööriista kirjeldus on nagu menüü kirje heas kohvikus: nimi ütleb, mis toit on, kirjeldus ütleb, millal seda tasub tellida, ja parameetrid on küsimus „kuidas soovid?“ — udune kirjeldus toob midagi muud, kui küsiti.

## Voog: küsimusest vastuseni

Kogu vood läbib viis sammu:

```text
   1. INIMENE küsib
      „Kas arve nr 2026-47 on makstud?“
                │
                ▼
   2. MUDEL loeb küsimust ja tööriistade kirjeldusi
      otsus: kutsu arve_olek (arve_number = „2026-47“)
      → struktureeritud kutsumus SÜSTEEMILE
                │
                ▼
   3. SÜSTEEM sooritab tegevuse
      päring raamatupidamisprogrammile
      → programm vastab: „makstud 2026-09-30“
                │
                ▼
   4. TULEMUS LÄHEB MUDELILE TAGASI
      süsteem lisab tulemuse vestluse juurde
                │
                ▼
   5. MUDEL SÕNASTAB inimesele
      „Jah — arve nr 2026-47 maksti 30. septembril 2026.“
```

Pane tähele, kes igas sammus töötab: inimene küsib, mudel otsustab ja sõnastab, süsteem otsib ja täidab — punktid 1, 3 ja 4 on sinu vastutus, mudelile jäävad 2 ja 5.

## Head ja halvad kasutusalad

Tööriistakutse tasub, kui mudelil on vaja juurdepääsu faktidele või tegevustele, mida tal endal pole. Kolm head tüüpi:

| Tüüp | Näide | Miks tööriistaga |
|---|---|---|
| **Andmeotsing** | „Mis on tellimuse nr 8812 olek?“ | fakt elab sinu andmetes, mitte mudeli treeningteadmistes — tööriist on ainus viis õigele vastusele ilma hallutsinatsioonita (mudeli kindlalt öeldud, aga vale vastus) |
| **Arvutus** | „Kui palju jääb pärast käibemaksu?“ | mudel annab ligikaudse arvu, arvutustööriist täpse — arvutamine kuulub programmile, sõnastus mudelile |
| **Väljaspool süsteemi tegevus** | e-kirja kavandi saatmine | mudel võib kavandi koostada, aga saatmise sooritab süsteem ainult siis, kui inimene on kinnitanud — inimene kinnitusahelas (ingl k *human-in-the-loop* — inimene vaatab tegevuse enne toimumist üle ja kinnitab selle) |

Kolmas rida on ka ohutustundlikum: välismaailma puudutav tööriist vajab süsteemi ehitatud kinnitusahelat — mitte lootust, et mudel „ise ei tee viga“. Kuidas piiri tõmmata, näitab [3.5 Ohutus](05-ohutus.md).

Millal aga **mitte** kasutada? Kui sama töö saab teha ette kirjutatud workflow-sammuga ([2.3](../02-praktika/03-workflow-algtasandil.md)), eelista sammu. Erinevus kummaski suunas:

- **Workflow-samm käib alati** — iga kord, kindlas kohas, sama tee pidi. Ta on ettearvatav, kontrollitav ja odavam.
- **Tööriistakutse on paindlikum** — mudel otsustab vestluse käigus, millal ja kuidas teda kasutada — aga just seepärast vähem ettearvatav: sarnased vestlused võivad erinevat teed käia.

Seega: ette teada küsimused ja alati sama tee — ehita workflow; vabas vormis küsimused ja küsimusest sõltuv tee — tööriistakutse. Lihtsuse reegel jääb samaks: agendilaadne käitumine — kus mudel otsustab järjest mitu sammu ette — on viimane, mitte esimene valik (vt [4.1](../04-agendid-ja-mootmine/01-agendi-susteemid.md)).

> **Lihtsalt öeldes:** workflow on bussiliin — kindel tee, kindlad peatused; tööriistakutse on takso — läheb sinna, kus reisija palub, aga mitte alati sama tee pidi. Kui kõik sõidud käivad ühte kohta, pane buss käima.

## Näide samm-sammult: Nummerbüroo tööriist arve_olek

Väljamõeldud **Nummerbüroo** ([1.6](../01-alused/06-rollid-ja-vastutus.md)) — väike raamatupidamisbüroo: omanik Anu, viis raamatupidajat ja Jaak, kes süsteeme ehitab. [2.1](../02-praktika/01-hea-prompt.md)-s said kirjadest ühtlased kokkuvõtted; nüüd on järgmine korduv tüütus: raamatupidajad ja kliendid küsivad pidevalt „kas arve nr 2026-47 on makstud?“ ja keegi peab vastuse üles otsima. Jaak ehitab tööriista `arve_olek`.

Esmalt tööriista kirjeldus — täpselt see JSON, mida ülal nägime: nimi `arve_olek`, ühe lausega kirjeldus ja üks parameeter `arve_number`. Siis voo samm-sammult:

```text
   1. RAAMATUPIDAJA küsib vestlusaknas:
      „Kas arve nr 2026-47 on juba makstud?“
                │
                ▼
   2. MUDEL: arvete baasist ma midagi ei tea,
      aga kirjelduste hulgast leian tööriista arve_olek —
      kutsun ta parameetriga arve_number = „2026-47“
      → kutsumus läheb JAAKU SÜSTEEMILE
                │
                ▼
   3. SÜSTEEM: loeb kutsumuse, paneb päringu
      raamatupidamisprogrammile
      → programm vastab: „makstud 2026-09-30“
                │
                ▼
   4. SÜSTEEM ANNAB TULEMUSE MUDELILE TAGASI:
      „arve_olek tulemus: makstud 2026-09-30“
                │
                ▼
   5. MUDEL SÕNASTAB:
      „Jah — arve nr 2026-47 maksti 30. septembril 2026.“
```

Miks see hästi on:

- **Fakt ei tulnud mudeli peast.** Kuupäev 2026-09-30 tuli raamatupidamisprogrammist — midagi polnud vaja ära arvata, seega pole hallutsinatsioonil kuhugi hiilida.
- **Mudel tegi ainult kaks asja, mida ta hästi oskab:** valis õige tööriista ja panes numbri õigesse kohta ning sõnastas vastuse loetavalt. Otsimise tegi süsteem.
- **Parameetreid on täpselt nii palju kui vaja.** Üks number on kogu vajalik teave — iga lisaväli (kliendi nimi, kuupäev) annaks mudelile ainult rohkem võimalusi eksida. Mis saab siis, kui küsitakse vale numbriga ja programm midagi ei leia — selle käsitlemist vaatab [3.4](04-vead-ja-veakasitlus.md).
- **Saatmine, kustutamine ja maksmine poleks siin tööriist.** See tööriist ainult loeb; kui Jaak hiljem tahab, et süsteem klientidele ka meeldetuletusi saadaks, saab see ainult kinnitusahelaga — [3.5 Ohutus](05-ohutus.md) näitab, kuidas seda piirata.

> **Lihtsalt öeldes:** Jaak ei õpetanud mudelit arveid tundma — ta andis mudelile lipiku, kuhu numbri kirjutada, ja programmi, mis lipiku ära toob. Mudel ei saa kunagi makstud arvet „leida“, sest ta ei otsi — otsib süsteem.

## Kokkuvõte

- **Tööriistakutse tähendab, et mudel küsib, süsteem teeb:** mudel otsustab vestluse käigus, kas ja millal tööriista kasutada, aga tegevuse sooritab alati sinu programm ja tulemus läheb mudelile tagasi sõnastamiseks.
- **Tööriista kirjeldus on kolmeosaline:** nimi, ühe lausega kirjeldus (mida teeb ja millal kasutada) ning parameetrid — andmed, mida tegevus vajab. Kirjelduse kvaliteet määrab, kui hästi mudel valib.
- **Mudel ei tee midagi iseseisvalt** — see on kriitiline turvalisuse punkt; kõik, mis välismaailma puudutab, käib läbi süsteemi ja vajaduse korral inimese kinnituse ([3.5](05-ohutus.md)).
- **Head tööriistad:** andmeotsing, täpne arvutus ja väljaspool süsteemi tegevus — viimane ainult kinnitusahelaga.
- **Kui piisab ette kirjutatud workflow-sammust, kasuta sammu:** tööriistakutse on paindlikum, aga vähem ettearvatav; agendilaadne käitumine on viimane valik ([4.1](../04-agendid-ja-mootmine/01-agendi-susteemid.md)).

## Mis edasi?

- eelmine → [3.2 Konteksti haldamine](02-konteksti-haldamine.md)
- järgmine → [3.4 Vead ja veakäsitlus](04-vead-ja-veakasitlus.md) — mis juhtub, kui tööriista kutsumine nurjub
- Ohutus enne tegutsemist → [3.5 Ohutus](05-ohutus.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
