# 2.3 Workflow algtasandil: sammud ja tingimused

> **Sihtpublik:** kõik | **Eeltingimused:** [2.2 Struktureeritud väljund](02-struktureeritud-valjund.md), [1.5 AI-automatiseeritud süsteemi anatoomia](../01-alused/05-susteemi-anatoomia.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- kirja panna workflow (töövoog — automatiseeritud sammude jada) sammu (ingl k *step*) haaval: iga sammu sisend, tegevus ja väljund;
- püsti panna tingimused (ingl k *condition*) — otsustuspunktid, kus tee hargneb — ja otsustada, kas neid lahendab reegel või AI-mudel;
- kasutada silmust (ingl k *loop*), kui sisendeid tuleb korraga palju;
- paigutada voo sisse inimese kinnitusahela samm (ingl k *human-in-the-loop* — inimene vaatab tulemuse enne kasutust üle ja kinnitab selle).

Dokumendist [1.5](../01-alused/05-susteemi-anatoomia.md) teame süsteemi seitset osa ja kolme kuju — siin pannakse osadest kokku sirge, jälgitav jada.

## Lihtsalt öeldes

> Workflow on nagu korralikult sisseseatud postkontor: kirjad tulevad sisse ühest otsast, iga laud teeb ühe asja ja annab tulemuse edasi; ristmikul suunab viit — „arved vasakule, infokirjad otse, pahased ülemuse lauale“. Postkontor ei improviseeri — just see teeb ta usaldusväärseks: kui miski läheb valesti, tead, millise laua juures otsida.

## Sisendist väljundini: sammude mõte

Workflow on sammude jada, mis käib alati samas järjekorras: iga samm võtab sisendi, teeb ühe asja ja annab väljundi — järgmise sammu sisendi — kuni vool jõuab lõppeni.

Näide: Mari arvete voog (1.2: iga kuu umbes 40 PDF-arvet raamatupidamisse):

| # | Samm | Sisend | Tegevus | Väljund |
|---|---|---|---|---|
| 1 | käivitaja (ingl k *trigger*) | uus arve e-postis | voo elustamine, faili kättesaamine | arve fail süsteemis |
| 2 | andmete eraldamine | arve fail | AI-mudel loeb väljad: number, kuupäev, summa | väljad [struktureeritud väljundina](02-struktureeritud-valjund.md) |
| 3 | kontroll | väljad + tellimuse andmed | reegel: kas number ja summa ühtivad? | „jah“ → samm 4; „ei“ → Marile ülevaatusele |
| 4 | sisestus | kinnitatud väljad | kirje raamatupidamisprogrammi | arve sisestatud ja märgitud |

Kaks reeglit, mis sammu heaks teevad:

- **Iga samm teeb ühe asja.** Kui sammu kirjelduses on sõna „ja“ („eralda andmed JA koosta vastus“), tükelda ta kaheks. Ühe asjaga sammu saab sekundiga üle vaadata ja vajadusel ümber ehitada ilma et kogu voog laguneks.
- **Iga sammu väljund on kontrollitav.** Pärast sammu 2 saad vaadata, kas väljad on õiged; pärast sammu 3, mida otsustati. Viga ei jää pimedusse.

Kus seda ehitada? **Tööriistad nagu n8n, Make ja Zapier** on visuaalsed tahvlid, kus sammud on ühendatavad plokid — koodi kirjutada ei tule. See dokument on tööriist-agnostiline: põhimõtted kehtivad kõigis; ühe voo otsast lõpuni ehitamist näitab [2.5](05-esimene-workflow.md).

## Tingimused ja harud: kui siis muidu

Vood on harva sirge joon — enamasti tuleb ette koht, kus tee hargneb. See koht on **tingimus (ingl k *condition*)**: küsimus, mille vastus on jah või ei. Iga vastuse suund on **haru (ingl k *branch*)**: tee, mida pidi vool liigub. Inimkeeles: „kui number ühtib, siis sisesta; muidu küsi Marilt“.

Tingimuse lahendamiseks on kaks teed — nende vaheline valik on üks olulisemaid otsuseid voo ehitamisel:

| Lahendus | Mis see on | Kasuta kui… |
|---|---|---|
| **Automaatne reegel** | kindel võrdlus andmetega: „summa on üle 1000“, „väli on tühi“ | teave on kindlas väljas ja reegel mahub ühte lausesse — tulemus on kiire, odav ja seletatav |
| **AI klassifitseerimine** | mudel loeb vaba teksti ja vastab struktureeritud väljundiga („liik = kaebus“) | otsus elab tekstis: „kas see kiri on vihane?“ ei mahu ühegi reeglisse |

Reegel annab fakti, mudel annab hinnangu. Lihtne test: kui otsuse saab langetada, vaadates ainult numbreid ja välju, pane reegel; kui selleks tuleb tekst sisse lugeda ja tunda, kasuta mudelit. Kuna mudeli hinnang on arvamus, mitte fakt, anna talle alati ka „teadmata“ valik, mis viib inimesele. Kui aga tingimust ei saa ette kirjutada isegi mudeliga, on see märk sellest, et vaja läheb agendi süsteemi (vt [4.1](../04-agendid-ja-mootmine/01-agendi-susteemid.md)): seal otsustab mudel iga järgmise sammu ise. Jää siiski workflow juurde, kuni see piisab — ette kirjutatud tee on kontrollitavam ja sageli ka parem.

> **Lihtsalt öeldes:** tingimus on ristmik, haru on tee, mis sealt läheb: kui ristmikul loetakse numbreid, pane sinna reegel; kui tuleb kirja lugeda, pane mudel.

## Silmused: kui kirju on palju

Korraga tuleb päevaga sisse 40 kliendikirja — nagu üks pakk. Igaüks käib läbi sama tee. Kas ehidad 40 workflow-t? Ei — ehidad ühe ja lisad **silmuse (ingl k *loop*)**: „võta järgmine kiri ja aja ta läbi sama voo, kuni kast on tühi.“

```text
   kast: 40 kirja
        │
        ▼
   ┌─► võta JÄRGMINE kiri
   │        │
   │        ▼
   │   sama workflow ühe kirjaga:
   │   klassifitseeri → tingimus → haru → väljund
   │        │
   │   nurjus? ──► märgi nurjunuks, kiri nimekirja
   │        │
   │        ▼
   └── kast veel kirju? ── jah
            │
           ei
            ▼
   kõik läbitud + nurjunute nimekiri käsitluseks
```

Kaks nüanssi silmuse juures:

1. **Ehita ja proovi läbi ühe kirjaga, siis lase silmusel korrata.** Silmus ei muuda teed — ainult korduste arvu; kui voo sees on viga, kordab ta seda 40 korda.
2. **Ühe kirja nurjumine ei tohi peatada teisi.** Kui üks kiri ei klassifitseeru, jääb ta nurjunute nimekirja ja silmus liigub edasi. Mida nurjunutega edasi teha, on [3.4 Vead ja veakäsitluse](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md) teema.

> **Lihtsalt öeldes:** silmus on konveierilint — ehitad ainult ühe tööjaama, lind toimetab iga toote sellest läbi; viga ühes tootes ei peata linti.

## Inimene kinnitusahelas workflow sammuna

Workflow-s saab kasutada ka sammu, mis pole masina töö: **kinnituse samm**. Voog jõuab kindla kohani, paneb töö inimese loendisse ja **ootab** — ei liigu edasi, enne kui inimene on ütelnud „jah“ või lükanud tagasi.

Kus kinnituse samm paigutada? [1.2](../01-alused/02-voimalused-ja-piirid.md) andis vastuse: seal, kus viga maksab — enne, kui süsteem puudutab välismaailma. Kolm tavalist paigutust:

| Paigutus | Kui sobib |
|---|---|
| **iga juhtumi kinnitus** | alguses, kui usaldust veel pole — näed iga tulemust ja õpid, kus eksitakse |
| **ainult kahtlased juhtumid** | kui süsteem on kindel, läheb automaatselt; ebakindel või kõrge panusega juhtum läheb inimesele |
| **juhuslik valim** | usaldus kasvanud — kontrollid iga kümnendat juhtumit, et kvaliteet ei libiseks |

> **Lihtsalt öeldes:** kinnituse samm on allkiri ekraanil, nagu pangatehingul: süsteem valmistab ette, aga viimane sõna, enne kui midagi väljapoole läheb, on inimesel. Kinnitus pole süsteemi nõrkus, vaid disaini osa.

## Näide samm-sammult: Kodutoa poe kolm haru

**Kodutoa pood** (1.2 maailm): Mari müüb e-poos käsitöökaupu ja saab päevas kirju kolmes liigis — tagastus, info, kaebus. Kolmeharuline workflow näeb välja nii:

```text
   KÄIVITAJA: uus e-kiri saabub poe aadressile
        │
        ▼
   SAMM 1: KLASSIFITSEERIMINE (AI-mudel)
   prompt (mudelile antav juhis):
   „loe kiri ja määra tema liik“
   struktureeritud väljund (JSON-vorming, vt 2.2):
   liik = tagastus | info | kaebus | teadmata
        │
        ▼
   TINGIMUS: kui „liik“ on…
        │
        ├── tagastus ──► HARU A: kavand + kinnitus
        │                  võta ostu andmed baasist
        │                  koosta vastuse kavand
        │                  KINNITUSE SAMM: Mari vaatab üle
        │                  kinnitatud vastus läheb kliendile
        │
        ├── info ──────► HARU B: vastus automaatselt
        │                  võta saatmise ja laoseisu andmed baasist
        │                  koosta vastus nende andmete põhjal
        │                  vastus läheb kliendile otse
        │                  (1.4 lisab nüanssi: iseseisvalt saab minna
        │                   ainult tekst, mille sisu on eelnevalt
        │                   kinnitatud andmetest)
        │
        ├── kaebus ────► HARU C: alati inimesele
        │                  kiri ja kogu kontekst lähevad Marile
        │                  süsteem kliendile ise ei vasta
        │
        └── teadmata ──► HARU C: samuti inimesele
                          (klassifitseerija ei olnud kindel)

   iga haru lõpus: KIRJE AJALUKU — liik, otsus, kellaaeg
```

AI-mudel teeb siin kaht tööd — liigitab ja koostab sõnastusi. Kõik faktid (kas toode on laos, kui kaua saatmine kestab, millal ost tehti) tulevad baasist, mitte mudeli peast — nii ei saa vastusse hiilida hallutsinatsioon (mudeli kindlalt öeldud, aga vale vastus).

Tingimused kiri kirja kohta:

| Tingimus | Kuhu läheb | Miks |
|---|---|---|
| liik = „tagastus“ | haru A — kavand, Mari kinnitab | tagastus on kliendile lubadus (raha või kaup) — viga maksab; kavand säästab Mari aega, kinnitus jätab vastutuse talle (1.2: „kolm jah, üks ei“) |
| liik = „info“ | haru B — automaatne vastus | vastus tuleb baasi faktidest, mitte arvamusest; viga on väike ja kergesti märgatav |
| liik = „kaebus“ | haru C — alati inimesele | kliendisuhte olulised hetked on 1.2 järgi inimese töö — vale toon maksab rohkem, kui automatiseerimine kokku hoidis |
| liik = „teadmata“ | haru C — inimesele | varutee: ebakindel hinnang ei arva, vaid küsib |

> **Lihtsalt öeldes:** kolm haru automatiseerivad kolmuti — info täiesti iseseisvalt, tagastus pooleldi (Mari kinnitab), kaebus üldse mitte. Iga haru on omaette otsus, just nagu 1.2 ütles: valik on see, kus inimene protsessis seisab.

## Kokkuvõte

- **Workflow on ette kirjutatud sammude jada:** iga samm võtab sisendi, teeb ühe asja ja annab kontrollitava väljundi edasi.
- **Tingimus on otsustuspunkt, haru on tee.** Numbritega otsustab reegel, tekstiga otsustab AI-mudel — ja ebakindluse korral viib „teadmata“ haru inimesele.
- **Silmus kordab sama teed kõigi sisendite jaoks:** proovi läbi ühe kirjaga, siis lase korrata; üks nurjunud sisend ei peata teisi.
- **Kinnituse samm peatab voo inimese jaoks** — seal, kus viga maksab, jääb viimane sõna inimesele.
- **Tööriistad (n8n, Make, Zapier) on ainult töölaud** — põhimõtted kehtivad kõigis.
- Kui teed ei saa ette kirjutada, on see märk sellest, et vaja läheb agendi süsteemi (vt 4.1) — aga enne proovi alati lihtsamat kuju.

## Mis edasi?

- eelmine → [2.2 Struktureeritud väljund](02-struktureeritud-valjund.md)
- järgmine → [2.4 Sisendid ja andmete ettevalmistamine](04-sisendid-ja-andmed.md)
- Täielik juhend ühe workflow ehitamiseks → [2.5 Esimene automatiseeritud workflow otsast lõpuni](05-esimene-workflow.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
