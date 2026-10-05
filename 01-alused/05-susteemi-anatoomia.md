# 1.5 AI-automatiseeritud süsteemi anatoomia

> **Sihtpublik:** kõik | **Eeltingimused:** [1.4 Kus AI-automatiseerimine juba töötab: kasutujuhtumid](04-kasutujuhtumid.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- nimetada AI-automatiseeritud süsteemi seitse osa ja selgitada igaühe ülesannet;
- lugeda süsteemi skeemi ja näha, kus konkreetse töövoo sammud aset leiavad;
- eristada kolme süsteemi kuju — üksik prompt, workflow ja agendipõhine süsteem — ning hinnata, milline sinu ülesandeks piisab;
- kaardistada tavaline kasutujuhtum anatoomia skeemile.

## Lihtsalt öeldes

> AI-automatiseeritud süsteem on nagu kokk köögis. Tellimus tuleb sisse — see on käivitaja. Retsept ütleb, mida ja kuidas teha — see on juhis. Kokk valmistab — see on AI-mudel. Enne taldrikule panemist keegi maitset kontrollib — see on kontrollpunkt. Ja ainult siis läheb taldrik kliendile — see on väljund. Registris jääb kirja, mis telliti ja mis serveeriti — see on andmete hoidmine. Ükski osa ei sööda külalist üksi; tulemus sünnib kogu rea koostöös.

Dokumendist [1.1](01-mis-on-ai-mudel.md) teame, et mudeli osa on ainult üks samm — kogu ümbritsev töö (andmete toomine, kontroll, saatmine) on tavaline, usaldusväärne tarkvara. See dokument võtab selle mõtte laiali ja paneb kogu süsteemi ühele pildile — enne, kui hakkame osi ükshaaval ehitama.

## Süsteemi seitse osa

Iga AI-automatiseeritud süsteem — olgu see lihtne e-posti voog või ettevõtte suur töötlusliin — koosneb samadest seitsmest osast:

1. **Käivitaja (ingl k *trigger*)** — sündmus, mis süsteemi elustab: klient saadab e-kirja, keegi täidab veebivormi, kell lööb iga päev kella kaheksa. Ilma käivitajata on süsteem magav masin.
2. **Sisendandmed** — tekst, mida töödelda, ja andmed süsteemist, mida juurde võtta. Dokumendist 1.1 teame: mudel ei otsi fakte üles — kui ostu kuupäeva kaasa ei anna, arvab ta selle välja (see on hallutsinatsioon — mudeli kindlalt öeldud, aga vale vastus). Kõik koos peab mahuma kontekstiaknasse (ingl k *context window* — tekstihulk, mida mudel korraga näeb). Halvasti vormindatud või poolik sisend on sage nurjumispõhjus — andmete ettevalmistamist vaatleme dokumendis [2.4](../02-praktika/04-sisendid-ja-andmed.md).
3. **Juhis ehk prompt (mudelile antav juhis)** — reeglid, roll ja väljundi vorming, mille järgi mudel töötab. Kuidas head prompti kirjutada, õpetas [1.3](03-promptide-pohitoed.md); süsteemi jaoks tähtis on see, et juhis on eraldi, hoitav ja muudetav osa — mitte ainult kellegi peas.
4. **AI-mudel** — ainus koht voos, kus töös on juhuslikkus: mudel klassifitseerib kirja, eraldab andmed, koostab kavandi. Ta on võimekas, aga garantiita — seepärast ei lase teda kunagi üksi väljundini.
5. **Kontrollpunkt** — koht, kus tulemust kontrollitakse enne välismaailma: kas inimene kinnitusahelas (ingl k *human-in-the-loop* — inimene vaatab tulemuse enne kasutust üle ja kinnitab selle) või automaatne reegel („kas vastuses on arve number ja kas see on andmetes olemas?“).
6. **Tegevus / väljund** — hetk, mil süsteem puudutab välismaailma: vastus läheb kliendile, andmed sisestatakse tabelisse, teavitus saadetakse. Selle sammu viga on kõige kallim — seepärast eelneb sellele alati kontrollpunkt.
7. **Andmete hoidmine** — kus tulemus ja ajalugu püsivad: saadetud vastused, tehtud otsused, töötlemise kulg. Ilma ajaloota ei näe, mis süsteemis juhtus, ega saa hiljem vigu uurida.

Kogu skeem ühel pildil — loe seda ülevalt alla, nagu retsepti:

```text
   1. KÄIVITAJA (trigger)
      uus e-kiri · vormi esitlus · ajakava
                │
                ▼
   2. SISENDANDMED
      kliendi kiri + andmed süsteemist
                │
                ▼
   3. JUHIS (prompt)
      reeglid · roll · väljundi vorming
                │
                ▼
   4. AI-MUDEL
      klassifitseerib · eraldab andmed · koostab kavandi
                │
                ▼
   5. KONTROLLPUNKT
      inimene kinnitusahelas või automaatne reegel
      ← kahtlane juhtum (liigitus või kontroll) pöördub inimesele
                │
                ▼
   6. TEGEVUS / VÄLJUND
      vastus kliendile · andmete sisestus
                │
                ▼
   7. ANDMETE HOIDMINE
      tulemus ja ajalugu jäävad püsima
```

> **Lihtsalt öeldes:** mudel on ainus osa, mis arvab — kuus teist osa on korralikku tööd tegev tarkvara. Seepärast on süsteemi kvaliteet suurem küsimus kui mudeli kvaliteet: hea mudel halvas süsteemis annab halva tulemuse ja vastupidi. Uue idee puhul käi seitsmesse ossa läbi: kust vool tuleb, mis andmed kaasa, mis juhis, kes kontrollib, kuhu tulemus läheb ja kus ajalugu jääb.

Kaks tehnilist teemat jäävad siin lühikeseks: kuidas neid osi programmeeritult kokku panna, seda selgitab API (programmiline liides mudeli kutsumiseks koodist) — vt [3.1 API integratsioonid](../03-susteemi-ulesehitus/01-api-integratsioonid.md); mis juhtub, kui mõni osa ebaõnnestub — vt [3.4 Vead ja veakäsitlus](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md).

## Kolm kuju: üksik prompt, workflow, agent

Sama anatoomia saab ellu astuda kolmes vormis — mida suurem kuju, seda rohkem osi on süsteemi sisse ehitatud:

**a) Üksik prompt.** Inimene teeb kõik ülejäänu: avab mudeliga tööriista, kleebib teksti, loeb vastuse läbi ja kopeerib sinna, kuhu vaja. Seitsmest osast on olemas ainult juhis ja mudel — käivitaja, kontroll ja hoidmine on inimese õlgadel. Piisab, kui ülesanne esineb päevas mõni kord.

**b) Workflow (töövoog — automatiseeritud sammude jada).** Sammud on ette kirjutatud ja käivitaja paneb need iseseisvalt käima: e-kiri saabub, süsteem liigitab, võtab andmed, koostab kavandi, suunab inimesele kinnituseks. Tingimused otsustavad, millist teed pidi süsteem läheb („kui kirja pole võimalik kindlalt liigitada, mine otse inimesele“). Kõik seitse osa on nähtavad ja täpselt määratud — see on 2. tasandi peamine teema.

**c) Agendipõhine süsteem.** Tööd juhib AI-agent — mudel otsustab ise, mis on järgmine samm (seda käsitleb [4.1 Agendi süsteemid: mis need on ja millal vaja](../04-agendid-ja-mootmine/01-agendi-susteemid.md)) — aga pea meeles põhireegel: lihtsam lahendus on sageli parem ja agent on enamasti viimane, mitte esimene valik.

| Kuju | Sobib kui | Näide |
|---|---|---|
| **Üksik prompt** | ülesanne esineb paar korda päevas ja inimene teeb ülejäänud sammud ise | müügikirja visandi kirjutamine tööriistaaknas |
| **Workflow** | sammud on alati samasugused, kordus suur ja kontrollikoht määratud | tagastustaotluste töötlus e-poes |
| **Agendipõhine süsteem** | teed ei saa ette kirjutada — ja lihtsam kuju on juba proovitud ning ei piisanud | uurimuslik ülesanne, kus järgmine samm sõltub sellest, mida eelmine leidis |

> **Lihtsalt öeldes:** liigu alati lihtsamast keerulisema poole. Kui inimene teeb kõik peale kirjutamise, piisab üksikust promptist; kui töö kordub tundide kaupa, ehita workflow; agent tuleb kõne alla vaid seal, kus sammud ise kindlat teed ei järgi.

## Näide samm-sammult: tagastustaotluse voo anatoomia

Võtame tagastustaotluse voo, mida eelmises dokumendis nägime, ja vaatame, mis selle all tegelikult töötab. [1.4](04-kasutujuhtumid.md) näitas nelja sammu — nüüd samad sammud anatoomia keeles:

| 1.4 samm / element | Anatoomia osa | Mis seal tegelikult juhtub |
|---|---|---|
| klient kirjutab: „Tahaksin toote X tagasi saata — see ei sobinud.“ | **1. käivitaja** | uus e-kiri elustab süsteemi |
| (kirja sisu) | **2. sisendandmed** | kirja tekst + ostu andmed süsteemist: kuupäev, summa |
| (taustal alati olemas) | **3. juhis** | reeglid: „tagastusaeg 14 päeva, kulu 5 eurot, raha tagasi 3 tööpäevaga“ |
| AI klassifitseerib kirja ja koostab vastuskavandi | **4. AI-mudel** | kaks tööd järjest: liigitamine ja kavandi kirjutamine |
| klienditeenindaja kontrollib ja kinnitab | **5. kontrollpunkt** | inimene kinnitusahelas: süsteem näitab ostu kuupäeva ja reegli täitmise seisundit; pahased juhtumid võtab inimene ise üle |
| vastus läheb ühe klõpsuga klienti | **6. tegevus / väljund** | kiri jõuab kliendini — siit edasi keegi enam ei kontrolli |
| (taustal alati) | **7. andmete hoidmine** | tagastuste register: mis otsustati, millal ja kelle poolt |

Kaks detaili, mida skeem eriti hästi näitab:

- **Varutee pole erand, vaid osa skeemist.** Kui kirja pole võimalik kindlalt liigitada, pöördub voo otse inimesele — nagu 1.4 juba ütles, on see normaalne tee, mitte ebaõnnestumine. Kuidas selliseid olukordi süstemaatiliselt käsitleda, on [3.4 veakäsitluse](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md) teema.
- **„Lihtne automatiseerimine“ on tegelikult selge masin.** Väljast paistab: „AI vastab klientidele“. Seest paistab seitse nimetatud osa, igaühel täpselt üks ülesanne. Just selge jaotus — mitte võluv mudel — teeb voo usaldusväärseks ja parandatavaks.

> **Lihtsalt öeldes:** kui süsteem mõnikord eksib, näitab anatoomia, *mis osas* ta eksis — valed andmed sammus 2, nõrk juhis sammus 3, liiga lodev kontroll sammus 5. Ilma skeemita on viga „AI-i süü“; skeemiga on viga aadress, mille juurde minna ja mis ära parandada.

Igal osal peab muidugi olema ka keegi, kes selle eest vastutab — kes need inimesed on ja kuidas töö jagatakse, käsitleb [1.6 Rollid ja vastutus projektis](06-rollid-ja-vastutus.md); siin jääb see lahti.

## Kokkuvõte

- **Igal AI-automatiseeritud süsteemil on seitse osa:** käivitaja, sisendandmed, juhis, AI-mudel, kontrollpunkt, tegevus/väljund ja andmete hoidmine.
- **AI-mudel on ainus osa, mis arvab** — ülejäänud kuus on tavaline tarkvara, mis teeb süsteemi usaldusväärseks ja parandatavaks.
- **Kolm kuju:** üksik prompt (inimene teeb kõik ülejäänu), workflow (ette kirjutatud sammud ja tingimused), agendipõhine süsteem (mudel otsustab järgmise sammu ise). Liigu alati lihtsamast — agent on viimane valik.
- **1.4 tagastustaotluse voo sammud kaardistuvad punktihaaval seitsmele osale** — „lihtne automatiseerimine“ on tegelikult korralikult kokku pandud masin.

## Mis edasi?

- eelmine → [1.4 Kus AI-automatiseerimine juba töötab: kasutujuhtumid](04-kasutujuhtumid.md)
- järgmine → [1.6 Rollid ja vastutus projektis](06-rollid-ja-vastutus.md)
- Praktiline ehitamine → 2. tase (töövoogude (workflow-de) ehitus: [2.3](../02-praktika/03-workflow-algtasandil.md), esimese süsteemi juhend: [2.5](../02-praktika/05-esimene-workflow.md))
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
