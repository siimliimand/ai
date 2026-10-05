# 4.4 Mitme agendi arhitektuurid

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [4.1 Agendi süsteemid](01-agendi-susteemid.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- otsustada, kas ülesanne vajab ühte AI-agenti või mitme agendi arhitektuuri (ingl k *multi-agent architecture* — süsteem, kus ülesande töö on jaotatud mitme spetsialiseerunud agendi vahel);
- nimetada kolm märki, et üks agent enam ei piisa;
- eristada ja valida kolme põhimustrit: järjestikune ahel, juht + töötajad, teineteise kontrollijad;
- selgitada, miks juhib agentide tööd tihti lihtne workflow (töövoog), mitte teine agent;
- näha, kuidas kulud, vead ja testimise vaev agentide arvuga korrutuvad, ning paigaldada piirid igale agendile eraldi.

## Lihtsalt öeldes

> Mitu agenti pole „rohkem jõudu“ — see on rohkem keerukust. Enamiku töid teeb ära üks agent hea juhisega. Mitu agenti tulevad jutuks siis, kui töö on tõesti jaotunud ja tulemus vajab sõltumatut kontrolli. **Mitme agendi arhitektuur on keerulise ülesande lahendus, mitte eesmärk — lihtne jääb alati eelistatuks.**

## Millal üks agent ei piisa

Dokument [4.1](01-agendi-susteemid.md) andis agendi põhitõed: kolm osa, agent vs workflow, hübriid. Aga mis siis, kui isegi üks agent hea juhisega üksi ei saa hakkama? Kolm märki:

1. **Ülesande osad vajavad erinevat spetsialiseerumist.** Kui üks osa tööst nõuab teistsugust juhist, teistsuguseid tööriistu või isegi teistsugust mudelit, ei saa ühe agendi juhis mõlemat hästi öelda. Analüütiku juhis ütleb „ole lühike ja täpne“, kirjaniku juhis „hoia lugu voolavana“ — ühes juhises need üksteist ära segavad.
2. **Tulemus vajab sõltumatut kontrolli — kirjutaja ei ole kontrollija.** Kes on teksti kirjutanud, on oma teksti paha kriitik — loeb seda, mida tahtis öelda. Sama kehtib agendi kohta. Eraldi kontrollagent näeb tulemust värske pilguga ja käib oma nimekirja järgi — mitte kirjutaja eneserahuldamise järgi.
3. **Töö on loomulikult jaotunud.** Viis postitust on viis sõltumatut tükki: need saab teha eraldi, vajadusel paralleelselt, ja igaüht eraldi kontrollida. Siin pole üks hiigelülesanne, vaid hunnik väikeseid ülesandeid.

Üks kontrollküsimus enne otsust: kas osad vajavad erinevat **juhist** või lihtsalt erinevat **sisendit**? Kui sama agent saab sama juhisega teha viis korda viis erinevat sisendit, pole vaja viit agenti — piisab ühest agendist viie kutsega.

## Kolm põhimustrit

| Muster | Kuidas töötab | Millal sobib |
|---|---|---|
| **Järjestikune ahel** (ingl k *pipeline* — iga agent teeb oma osa ja annab tulemuse edasi, nagu tootmisliin) | agent 1 → agent 2 → agent 3; igaühe väljund on järgmise agendi sisend | kui töö on loomulikult järjestikune ja etapid on näha juba paberil: teema → tekst → kontroll |
| **Juht + töötajad** (ingl k *orchestrator + workers* — juhtagent jagab töö, kutsub töötajad ja kogub tulemused) | juht otsustab, kes mida teeb; töötajad teevad oma osa ja annavad tulemused juhile tagasi | kui alamülesannete arv ja kuju pole ette teada ning „kes mida teeb“ on ise otsus, mis vajab mõtlemist |
| **Teineteise kontrollijad** (ingl k *generator-critic* — üks teeb, teine kriitiseerib, tegija parandab) | teos → kriitika → parandus → uus kriitika, kuni kontrollija rahuldub | kui kvaliteet on kriitiline ja viga kallis: faktid, aruanded, kliendini minev tekst |

```
(a) JÄRJESTIKUNE AHEL (pipeline)

[agent 1] ──▶ [agent 2] ──▶ [agent 3] ──▶ tulemus
 teemad         tekstid        kontroll
iga agent teeb oma osa ja annab tulemuse edasi

(b) JUHT + TÖÖTAJAD (orchestrator + workers)

              [juhtagent]
                    │  jagab töö, kogub tulemused
      ┌─────────────┼─────────────┐
      ▼             ▼             ▼
  [töötaja]     [töötaja]     [töötaja]

(c) TEINETEISE KONTROLLIJAD (generator-critic)

[teeb] ────────── teos ──────────▶ [kriitiseerib]
  ▲                                   │
  └──────── paranduskäsk ◀────────────┘
       kordab, kuni kontrollija rahuldub
```

> **Lihtsalt öeldes:** kolm mustrit on nagu köök. Järjestikune ahel on tootmisliin: supp → pearoa → magustoit. Juht + töötajad on peakokk, kes jagab tellimused kokkade vahel ja paneb taldriku kokku. Teineteise kontrollijad on kokk ja maitseproovija — kliendile ei lähe taldrik enne, kui proovija kiidab heaks.

## Orkestratsioon: kes juhib

Orkestratsioon (ingl k *orchestration* — agentide töö jagamine, suunamine ja tulemuste kokkuvõtmine) on juhi töö. Esimene küsimus pole „kuidas agendid ühendada“, vaid „kes juhib“.

Kiusatus on ehitada juhtagent: lase ühel agendil otsustada, kes mida teeb. Aga siis on juhi positsioonis jälle mudel — tema otsused on etteaimamatud ja tema vead kanduvad kõigile allapoole. Siin töötab [2.3 põhimõte](../02-praktika/03-workflow-algtasandil.md): **tihti juhib agente lihtne workflow**. Workflow orkestraatorina on:

- **etteaimatav** — samad sammud iga kord; kui viga tuleb, tead, kus otsida;
- **odav** — juhtimine ise maksab null tokenit;
- **kontrollitav** — sina otsustad, millised agendid millal käivituvad, mitte mudel.

Juhtagent on põhjendatud ainult siis, kui töö jaotus ise nõuab mõtlemist — „kes mida teeb“ on ise otsus. Sellegipoolest pane ka juhtagendile piirid: maksimaalne sammude arv, fikseeritud tulemuse vorm ja tööriistade nimekiri (tööriistakutse põhitõed, [3.3](../03-susteemi-ulesehitus/03-tooriistad-ja-tegevused.md)).

## Kulud, riskid ja piirid korrutuvad

> **Lihtsalt öeldes:** üks agent on üks tasuline assistent; kaks agenti on kaks assistenti, kes peavad omavahel kokku leppima — kulud ja eksimisvõimalused kasvavad kiiremini kui kasu.

Mitme agendi süsteemis korrutub kõik — ka hea, aga eelkõige halb:

- **Kulud korrutuvad.** Igal agendil on oma päringud: oma juhis, oma tööriistakutsed, oma mõtlemistokenid. Kolm agenti pole kolm korda ühe agendi hind — see on kolm korda kõike, pluss info liikumine nende vahel ([3.6 Kulude haldamine](../03-susteemi-ulesehitus/06-kulude-haldamine.md)). Kontrollija tsükkel „paranda ja proovi uuesti“ on kalleim osa, kui talle kordade piiri ei pane.
- **Vead kanduvad ahelas.** Järgmine agent töötleb eelmise tulemust usaldavalt. Kui esimene agent annab vale teema, kirjutab teine sellest veenva teksti ja kolmas kontrollib läbi juba vale sisendit ([3.4 Vead ja veakäsitlus](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md)).
- **Selgus kadub.** Ühe agendi süsteemis on viga juhises, sisendis või mudeli vastuses. Viie agendi süsteemis on viis juhist, viis sisendit ja nende vahelised ülekanded — kui lõpptulemus halb, on oluliselt raskem öelda, **kus** viga juhtus.
- **Testimine keeruliseks.** Ühe agendi test on „sisend sisse, väljund välja“. Mitme agendi süsteemi test vajab kombinatsioone: mis juhtub, kui üks agent annab ootamatu väljundi, ja kuidas käitub järgmine? Kuidas seda kõike mõõta, vaatab [4.5 Hindamine](05-hindamine.md).
- **Piirid igale agendile eraldi.** Kaitsekiht (ingl k *safeguard* — piirang, mis on süsteemi disaini sisse ehitatud) kehtib igale agendile eraldi, mitte süsteemile korraga: teemade leidjal ainult lugemisõigus, koostajal pole avaldamisvõimalust, kontrollagendil pole saatmisõigust. Kui ühel agendil on kõik õigused, on kõik teised piirid ainult sõnad ([3.5 Ohutus](../03-susteemi-ulesehitus/05-ohutus.md)).
- **Inimene kinnitusahelas jääb lõppotsustel.** Viis agenti, kes on nõus, ei ole viis allkirja: agentide konsensus pole inimese kinnitus. Kliendini minev, rahaga ja avalikkusega seotu läbib alati inimese.

## Näide samm-sammult: Sõnarohu nädalasisu

Väljamõeldud **„Sõnarohi“** — sisuturunduse agentuur. Ülesanne: kliendile iga nädal 5 sotsiaalmeedia postitust ja 1 uutiskiri. Töö on jaotunud (teema → tekst → kontroll), tulemus vajab sõltumatut kontrolli (tekstid lähevad kliendi avalikule kanalile), rütm on fikseeritud. Järeldus: **kolm agenti, juhiks lihtne workflow.**

```
SÕNAROHU NÄDALASISU — juht on LIHTNE WORKFLOW (fikseeritud sammud, mitte juhtagent)

 ESMASPÄEV                KOLMAPÄEV                 NELJAPÄEV                REEDE
─────────────────────────────────────────────────────────────────────────────────────
 [1] teemade leidja  ──▶  [2] teksti koostaja ──▶   [3] kontrollagent  ──▶  INIMENE
     tööriist:                tööriist:                kontroll-loend:         kinnitab
     kliendi varasemad        kaubamärgi               faktid | stiil |        ja avaldab
     postitused + vaated      stiilijuhend             keelatud väited
                                                        │
                                                        ▼ kui punkt läbi kukub
                                               paranduskäsk tagasi [2]-le
```

| Agent | Eesmärk | Tööriistad | Piirid |
|---|---|---|---|
| Teemade leidja | 5 postituse teemat + 1 uutiskirja teema, igaühel lühike põhjendus (miks just see) | kliendi varasemad postitused ja vaadete andmed — ainult lugemine | ei kirjuta teksti; ei põhjenda midagi, mida andmetest ei leia; väljund fikseeritud vormis |
| Teksti koostaja | tekstid stiilijuhendi järgi | kaubamärgi stiilijuhend | ei leiuta fakte ega hinnasid — kasutab ainult antud teemat ja faktide nimekirja; ei saa midagi avaldada |
| Kontrollagent | iga teksti läbivaatus kontroll-loendi järgi | kontroll-loend + kliendi faktide nimekiri (hinnad, kuupäevad) | ei paranda ise — kirjutab paranduskäsu koostajale; pole avaldamisõigust |

Kontrollagendi kontroll-loend:

| Kontrollpunkt | Mida kontrollitakse | Näide veast |
|---|---|---|
| Faktid | iga number, hind, kuupäev ja nimi vastab kliendi faktide nimekirjale | postitus nimetab hinda, mida hinnakirjas pole |
| Stiil | stiilijuhendi hääl, pikkus ja sõnavara | müügiline toon, kus juhend nõuab nõustavat |
| Keelatud väited | lubadused, mida klient ei tohi teha | „garanteerime tulemuse 30 päevaga“ |

**Mis juhtus ilma kontrollijata.** Enne kontrollagendi lisamist käis ahel otse: teemad → tekstid → inimene kinnitab. Reedel kinnitas inimene viis teksti korraga, ühe pilguga — ja üks postitus läks välja hinnaga „29 €“, mida kliendi hinnakirjas pole: koostaja oli täitnud teabeaugu. Kliendi jälgijad hakkasid „soodushinnale“ viitama ja Sõnarohu usaldus värises. Kanduv viga sünnib ühes agendis, aga maksab kõigi järgmiste sammude pealt — kontrollagent oleks hinna hinnakirjaga võrrelnud juba neljapäeval, mitte klient alles reedel.

Täna ei liigu workflow neljapäeval edasi, enne kui kontroll-loend on täis — ja reedel jääb viimane sõna inimesele: agent ei avalda kunagi ise midagi.

## Kokkuvõte

- **Mitu agenti tulevad jutuks kolmel põhjusel:** osad vajavad erinevat spetsialiseerumist, tulemus vajab sõltumatut kontrolli (kirjutaja ≠ kontrollija), töö on loomulikult jaotunud.
- **Kolm mustrit:** järjestikune ahel (tootmisliin), juht + töötajad (juht jagab ja kogub), teineteise kontrollijad (teeb ja kriitiseerib, kuni rahuldus).
- **Tihti juhib agente lihtne workflow, mitte juhtagent** — etteaimatav, odav ja vigade leidmisel selge.
- **Kulud, vead, selguse kadu ja testimise vaev korrutuvad** agentide arvuga; kaitsekihid kehtivad igale agendile eraldi ja lõppotsustel on inimene kinnitusahelas.
- **Põhimõte:** mitme agendi arhitektuur on keerulise ülesande lahendus, mitte eesmärk. Alusta ühe agendi või workflow-ga ja lisa agent ainult siis, kui sul on konkreetne põhjus.

## Mis edasi?

- eelmine → [4.3 Pikaaegne mälu ja oleku haldus](03-pikaaegne-malu.md)
- järgmine → [4.5 Hindamine: kuidas teada, kas süsteem on hea](05-hindamine.md)
- Agendi põhitõed ja agent vs workflow → [4.1 Agendi süsteemid](01-agendi-susteemid.md)
- Kaitsekihid ja inimene kinnitusahelas → [3.5 Ohutus](../03-susteemi-ulesehitus/05-ohutus.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
