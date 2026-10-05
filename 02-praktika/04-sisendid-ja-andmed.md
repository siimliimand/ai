# 2.4 Sisendid ja andmete ettevalmistamine

> **Sihtpublik:** kõik | **Eeltingimused:** [2.3 Workflow algtasandil](03-workflow-algtasandil.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- nimetada neli omadust, mis eristavad head sisendandmete hulka halvast;
- ära tunda viis tüüpilisemat andmete murekohta ja parandada neid enne mudelisse andmist;
- vormindada andmed ühtsesse struktuuri koos selgete märgenditega;
- otsustada, kui palju andmeid päringusse panna, ja pidada privaatsuse põhimõttest kinni juba ettevalmistusel;
- käia terve protsess läbi ühe näite varal: kõnede transkriptsioonid (kõne kirjalik ülestõus) struktuurseks sisendiks.

## Lihtsalt öeldes

> Sama AI-mudel annab korralike sisendandmetega usaldusväärse vastuse — ja sama mudel segase, vigase andmehulgaga kehv vastuse või leiutab puuduva ise juurde. Kui vastused on halvad, vaata esimese asjana, mis sisendisse läks. Kolm sammu — puhastamine, formaatimine ja mõistlik, turvaline hulk — tehakse enne, kui mudel asjaga üldse tegelema saab.

## Mis hea sisend on

Dokumendist 1.3 tead, et prompt (mudelile antav juhis) on kogu sisu, mida mudel korraga näeb — sealhulgas kõik sisendandmed. Nõrgad andmed annavad sama ettearvamatu tulemuse kui nõrk juhis. Neli omadust, mida igalt sisendi osalt kontrollida:

1. **Asjakohane.** Sisaldab seda, mida antud ülesanne vajab — ja muud mitte. Kõrvale jääv info pole kahjutu lisand: see on müra, mis raskendab mudelil tähtsat üles leida, ja iga liigne sõna kulutab tokeneid (ingl k *token* — sõnatükk, milleks mudel teksti lõikab).
2. **Täielik.** Kõik faktid, mida korrektne vastus vajab, on sisendis olemas. Puuduv info ei kao kuhgi — mudel täidab lüngad üldteadmistega või leiutab midagi juurde (hallutsinatsioon — mudeli kindlalt öeldud, aga faktidel mitte põhinev vastus; vt 1.1).
3. **Ühtse vormiga.** Sama asja kirjutatakse kõikjal samal viisil: kuupäevad ühel kujul, numbrid ühe eraldajaga, nimi alati sama. Erinevad kirjaviisid sunnivad mudelit järeldama, kas „Kätelen“ ja „Kätlin“ on sama inimene.
4. **Õigel ajal.** Andmed on ajakohased: tänane hinnakiri, mitte eelmise aasta oma. Vananenud andmed on kehvemad kui andmete puudumine — neile toetutud vastus näeb õige välja, aga pole seda.

## Puhastamine: viis tüüpilist murekohta

> **Lihtsalt öeldes:** puhastamine (andmete veaparandus enne mudelisse andmist) on nagu roogade pesemine enne kokkamist — keegi ei taha seda teha, aga vahelejätmine maitseb igas toidus. Alltoodud viis murekohta hõlmab suurema osa reaalsetest andmevigadest.

| Murekoht | Tüüpiline näide | Parandus enne mudelisse andmist |
|---|---|---|
| **Kordused ja samasisulised kirjed** | Sama klient andmebaasis kaks korda eri nimede all; sama kõne kaks transkriptsiooni | Võta duplikaadid kokku, jäta alles ajakohasem versioon — duplikaadid paisutavad sisendit ja tekitavad vastuolusid |
| **Poolikud kirjed** | Tellimus ilma koguta; kontakt ilma telefoninumbrita | Täida puuduv usaldusväärsest allikast või märgi välja „info puudub“ — ära jäta lünka mudelile äraarvamiseks |
| **Erinevad kuupäeva- ja numbriformaadid** | „2.10“, „2026-10-02“, „esmaspäev“; hinnad nii „2,50“ kui „2.5“ | Ühtlusta ühele vormile (nt kuupäevad kujul 2026-10-02) ja numbrid sama eraldajaga juba enne sisendisse panemist |
| **Transkriptsioonivead** | Kõne tuvastus kirjutab nime korraga „Kätlin“, siis „Kätelen“, siis „Kätlynn“ | Hoia nimede ja toodete parandustabelit ning asenda kõik variandid õige kirjaviisiga |
| **Mitme keele segadus** | Eestikeelne kiri, kus kogused ja tootenimed on inglise keeles | Sööda sisend võimalikult ühes keeles või teata juhises, et sisend võib segakeelne olla |

Puhastus ei pea olema põhjalik remont — nende viie vea kinnipüüdmine *enne* sisendisse panemist tõstab kvaliteeti juba märgatavalt, sest vastuse parandamine *pärast* on alati kallim kui sisendi parandamine *enne*.

## Kuidas andmeid formaati mudeli jaoks

> **Lihtsalt öeldes:** formaatimine (ühtse kuju andmine) tähendab, et andmed pannakse paika nii, nagu neid loetakse — mitte nii, nagu neid koguti. Korraldatud tabelist leiab vastuse kohe, märkmiku leheküljendist peaaegu mitte kunagi.

Kolm reeglit:

1. **Ühtne struktuur.** Iga kirje samade väljadega ja samas järjekorras — klient, tellimus, kuupäev, staatus. Siis oskab mudel iga kirje juures täpselt sama asja otsida.
2. **Selgelt eraldatud sektsioonid.** Kasuta märgendeid (märgend — selgelt eraldatud sektsioonisisendi osa, nt [KÜSIMUS]), mis teevad sisendi sisse pealkirjadega ruumid: siin on küsimus, siin andmed, siin ajalugu.
3. **Andmed loendina, ilma kõnekeelse ümbruseta.** „kogus: 2 tk“ — mitte „no ta siis ütles, et võtab ehk kaks tk, ütleme nii“.

Sama info kahel kujul:

**Korraldamata sisend:**

```text
No see Kätlin Tamm siis küsis, et kus nüüd see tema tellimus 312
on jäänud, ta tellis seda puhastusvahendit 2 tk ja tellimus tehtud
28. septembril, viimase korraga sai ta 10% allahindlust.
```

**Sama info korraldatult:**

```text
[KLIENDI_KÜSIMUS] Kus on mu tellimus nr 312?
[TELLIMUS]
- klient: Kätlin Tamm
- sisu: 2 tk puhastusvahendit
- esitatud: 2026-09-28
[VARASEM_LAHENDUS] 3-päevane hilinemine → 10% soodustus
```

Sisu on identne, aga korraldatud versioonis teab mudel täpselt, mis on küsimus, mis andmed ja mis ajalugu. Veel tähtsam: seda vormi saab iga järgmine kirje täita — inimese vabalt kirjutatud lõiku keegi korrata ei saa.

## Kui palju andmeid anda

> **Lihtsalt öeldes:** kontekstiaken on nagu reisikohver — mahutab palju, aga mitte kogu majapidamist, ja mida rohkem surud sisse, seda raskem on olulisemat üles leida. Anna kõik, mida mudel vajab, ja mitte midagi muud.

- **Kontekstiaken (ingl k *context window* — tekstihulk, mida mudel korraga näeb) on piiratud.** Iga sisend kulutab tokeneid — seega ka raha ja aega; eestikeelne tekst rohkem kui ingliskeelne (vt 1.1).
- **Prioriseeri.** Küsi iga andmehulga kohta: „Kas see andmehulk on vastuse jaoks tõeliselt vajalik?“ Kuu kokkuvõte saajale on sageli täpselt nii hea, kui ta põhineb viiel olulisemal kirjel, mitte viiekümnel — ainult odavam ja täpsem.
- **Järjekord loeb.** Pane kõige oluline algusse ja lõppu: pikas sisendis saab keskmine osa kõige vähem tähelepanu. See on üks 1.3 viiest sagedamast veast — oluline info maetud pikasse keskele (vt [1.3 Promptide põhitõed](../01-alused/03-promptide-pohitoed.md)) — ja kehtib ka siis, kui sisend on andmetest täidetud.
- **Kui andmeid on liiga palju, et neid üldse sisendisse panna**, pole lahendus „suruda ikkagi rohkem“, vaid otsing: süsteem toob küsimuse järgi välja ainult asjakohased lõigud ja annab need mudelile — seda nimetatakse RAG-iks (ingl k *retrieval-augmented generation* — otsinguga täiendatud genereerimine: süsteem, mis otsib vastused ettevõtte enda dokumentidest; vt [4.2](../04-agendid-ja-mootmine/02-rag.md)).

## Privaatsus juba ettevalmistusel

> **Lihtsalt öeldes:** andmeid, mida sa pole kunagi välja saatanud, ei saa ka kunagi lekkida. Kõige odavam andmekaitse on see, et seda pole vaja teha — see saavutatakse juba sisendi kokkupanemisel, mitte süsteemi turvaga pärast.

Iga välja kohta küsi: kas ülesanne vajab seda tõeliselt? Sageli mitte. Klient võib sisendis olla „Klient #142“ — mudelile piisab sellest täielikult, sest ta peab kirjeid eristama, mitte inimest tundma. Sama kehtib isikukoodide, aadresside ja muude isikuandmete kohta: asenda või eemalda need enne saatmist, mitte pärast.

Kui sisendandmed voolavad süsteemi automaatselt — iga kõne ja iga kiri läheb päriselt sisendisse —, ei jõua keegi neid käsitsi üle vaadata. Siis peab puhastamine ja anonümiseerimine olema süsteemi automatiseeritud samm; kuidas selliseid samme workflow-sse paigutada, räägib [2.3](03-workflow-algtasandil.md). Turvalisust tervikuna — võtmed, andmed, pahatahtlikud juhised — käsitleb [3.7 Turvalisus](../03-susteemi-ulesehitus/07-turvalisus.md).

## Näide samm-sammult: Kõneabi transkriptsioonid

Olukord: telefonitugi „Kõneabi“. Iga öö jooksul jääb 15–30 kliendikõne, mida töötaja hommikul läbi kuulata ei jõua. Süsteem teeb kõnedest transkriptsiooni, aga tekstid on nii koledad, et otse mudelile andes on vastused juhuslikud.

**1. samm — algne kaos.** Üks päris transkriptsioon:

```text
terve, olin midagi tellind, aga pole kohal nr kolm-üks-kaks.
Mina olen Kätelen, teist korda juba küsin, esimest korda ütlesite
et homme tuleb, see oli siis esmaspäev... muidu viimase korraga
asi lahendus sai, kui andsite 10% maha. hääl: Kätlynn Tamme
```

**2. samm — identifitseeri kolm murekohta** (puhastamise tabelist ülal):

1. **Transkriptsioonivead** — kliendi nimi kirjas kahel eri viisil („Kätelen“, „Kätlynn“) ja tellimuse number sõnadega („kolm-üks-kaks“);
2. **Erinevad formaadid** — kuupäev ainult sõnadega („esmaspäev“), millal see oli?
3. **Poolikud andmed ja kõnekeelne ümbrus** — „olin midagi tellind“: mis toode, kui palju, millal?

**3. samm — puhasta.** Töötaja koostab nimede parandustabeli ja täidab lüngad süsteemi andmetest:

| Kõnes tuvastatud kirjaviis | õige kirjaviis |
|---|---|
| Kätelen, Kätlynn | Kätlin Tamm |

Kuupäevad ühtlustatud: „esmaspäev“ → 2026-09-28 (tellimuse andmetest). Number sõnadest numbriteks: „kolm-üks-kaks“ → 312. Poolikud andmed tellimuskirjest: 2 tk puhastusvahendit.

**4. samm — formaadi struktuurseks sisendiks:**

```text
[ROLL] Sa oled Kõneabi klienditeeninduse assistent — asjalik ja sõbralik.
[KLIENDI_NIMI] Kätlin Tamm (Klient #142)
[KLIENDI_KÜSIMUS] Kus on mu tellimus nr 312? Klient küsib teist korda.
[TELLIMUS]
- number: 312
- esitatud: 2026-09-28
- sisu: 2 tk puhastusvahendit
- staatus: teel, prognoos 2026-10-06
[VARASEM_LAHENDUS] 2026-09-15: 3-päevane hilinemine → 10% soodustus
[ÜLESANNE] Koosta kliendile vastuse mustand kuni 80 sõna, eesti keeles.
```

**5. samm — tulemus.** Otse toorel transkriptsioonil põhinevad vastused olid juhuslikud: kord pakkus mudel toote ära, kord kirjutas nime valesti, kord arvas kuupäeva. Puhastatud ja struktureeritud sisendiga tulevad kõik faktid andmetest, mitte äraarvamisest — vastused on usaldusväärsemad ja korduvad. Kuna sammud on igal hommikul samad, saab selle jada süsteemis iseseisvalt käima panna; kuidas see workflow-na üles ehitada, õpetab [2.3](03-workflow-algtasandil.md).

## Kokkuvõte

- **Hea sisend on asjakohane, täielik, ühtse vormiga ja õigel ajal** — kõik neli on kontrollitavad enne mudelisse andmist.
- **Puhastamine hõlmab viit tüüpilist murekohta:** kordused, poolikud kirjed, erinevad formaadid, transkriptsioonivead ja keelte segadus — igaühe parandus on lihtne, kui teha see enne, mitte pärast.
- **Formaatimine teeb töö korduvaks:** ühtne struktuur, selged märgendid ja puhas loend ilma kõnekeelse ümbruseta.
- **Kontekstiaken on piiratud ja tähelepanu ebavõrdne** — anna ainult vajalik, hoia oluline alguses ja lõpus, suurte andmehulkade puhul kasuta otsingut (RAG).
- **Privaatsus algab ettevalmistusel:** vähem andmeid tähendab vähem riske — „Klient #142“ on sageli piisav.

## Mis edasi?

- eelmine → [2.3 Workflow algtasandil](03-workflow-algtasandil.md)
- järgmine → [2.5 Esimene automatiseeritud workflow otsast lõpuni](05-esimene-workflow.md)
- Kui andmeid on liiga palju → [4.2 RAG: oma andmete kasutamine](../04-agendid-ja-mootmine/02-rag.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
