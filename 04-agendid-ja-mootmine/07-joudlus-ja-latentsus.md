# 4.7 Jõudlus ja latentsus

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [4.6 Monitooring tootmises](06-monitooring.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, mis on latentsus (ingl k *latency* — ooteaeg päringust vastuseni) ja millal kiirus oluline on;
- nimetada viis põhjust, mis vastust aeglustavad, ja viis võimalust neid leevendada;
- hinnata iga kiirendusvahendi tegelikku hinda — seda, mille arvel kiirus osteti;
- mõõta latentsust nii keskmisena kui ka halvima juhtumina (ingl k *p95* — 95% juhtudest kiirem kui see);
- käia iga kiirendusmuudatus läbi hindamisega (vt [4.5](05-hindamine.md)), et kvaliteet ei kukuks.

## Lihtsalt öeldes

> Kiirus pole omadus, vaid nõue, mille piir sõltub sellest, kes ootab: klient vestlusaknas ootab sekundeid, öine e-kiri võib oodata minuteid. Kiirendada saab viiel viisil — aga ükski pole tasuta: igaüks on ostetud kvaliteedi või süsteemi lihtsuse arvel. Seepärast mõõdetakse enne ja pärast ning otsustatakse numbrite, mitte mulje põhjal.

## Latentsus: millal kiirus oluline on

Latentsus on aeg sisendi saatmisest vastuse kättesaamiseni — üks kuuest põhimõõdikust, mida [4.5](05-hindamine.md) juba nimetas. Siin vaatame, millal ta oluline on.

Talutav piir pole tehniline konstant — ta sõltub sellest, kas keegi seisab vastuse ootel või teeb vahepeal midagi muud:

| Kasutusjuhtum | Talutav latentsus | Miks |
|---|---|---|
| Vestlus kliendiga (chat) | mõni sekund (2–5 s) | klient ootab ekraani ees — ootab pisut, siis sulgeb akna |
| Mustand töötajale (kavand, kokkuvõte) | 10–30 s | töötaja saab vahepeal teisi asju teha |
| Öine e-kirjade klassifitseerimine | minutid | töö peab valmis olema hommikuks — keset ööd kiirus ei loe |
| Nädala- või kuuraport | minutid–tunnid | keegi ei seisa ootel — loeb ainult kokkulepitud valmimisaeg |

Reegel on lihtne: **mida otsesemalt inimene vastust ootab, seda lühem peab latentsus olema** — kaheksa sekundit vestluses on katastroof, öises klassifitseerimises märkamatult kiire.

Latentsust jälgitakse kahe mõõdikuga (ingl k *metric*):

- **Keskmine** ütleb, kui kiire on tüüpiline päring — aga varjab äärmusi: üksik 30-sekundiline juhtum kaob keskmises nähtamatuks.
- **Halvim juhtum (p95)** ütleb, kui halb on halvima õnnega kasutaja kogemus: p95 20 s tähendab, et iga kahekümnes klient ootab 20 sekundit või kauem — seda keskmine näidata ei oska.

## Mis vastust aeglustab

Aeg koguneb viiest kohast. Enne kiirendamist selgita välja, millised neist sinu süsteemis loevad — muidu kiirendad õiget asja valest otsast.

1. **Suur mudel.** Võimsam mudel vastab põhjalikumalt, aga aeglasemalt — [1.1](../01-alused/01-mis-on-ai-mudel.md) pilt: spetsialist mõtleb põhjalikult, praktikant vastab kohe. Lihtsa sammu jaoks on suur mudel lihtsalt aeglane.
2. **Pikk sisend.** Mida rohkem tokeneid (ingl k *token* — sõnatükk) päringuga kaasa läheb, seda kauem kulub nende läbilugemiseks. Kogu vestlusajalugu, mis täidab kontekstiakent (ingl k *context window* — tekstihulk, mida mudel korraga näeb; vt [3.2](../03-susteemi-ulesehitus/02-konteksti-haldamine.md)), kasvatab mitte ainult kulusid, vaid ka ooteaega — [2.4](../02-praktika/04-sisendid-ja-andmed.md) põhimõte „anna ainult vajalik“ kehtib ka kiiruse kohta.
3. **Pikk väljund.** Vastust kirjutatakse token kaupa — iga lisasõna võtab oma aja. Kolm numbrit struktureeritud väljundis ([2.2](../02-praktika/02-struktureeritud-valjund.md)) on mitu korda kiiremad kui kolmelõiguline kokkuvõte; kärbi väljundist, mida keegi ei loe.
4. **Agendi mitu käiku.** Nagu [4.1](01-agendi-susteemid.md) näitas, otsustab agent ise, mitu sammu ta teeb — iga käik on uus päring oma ooteajaga, ja kümme käiku tähendab kümne aja liitumist.
5. **Pakkuja koormus.** Aeg-ajalt aeglustub pakkuja teenus või mõni päring läbikukkub. Mõjutada ei saa, aga arvestada tuleb: uued proovikatsed ja tagavarakäitumine ([3.4](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md)) pikendavad ooteaega veelgi.

> **Lihtsalt öeldes:** vastus valmib kolmes tegevuses — mudel loeb sisendi, mõtleb ja kirjutab väljundi. Pikk sisend on pikk ettelugemine, suur mudel on aeglane mõtleja ja pikk väljund on pikk kirjutamine. Agent teeb seda kolmikut mitu korda järjest — ja aeg liitub.

## Viis võimalust kiirendada

| Vahend | Mis teeb | Mille arvel |
|---|---|---|
| **Väiksem või kiirem mudel lihtsateks sammudeks** | klassifitseerimine ja muud lihtsad sammud käivad märgatavalt kiiremini ja odavamalt | kvaliteedi risk — iga mudelivahetus tuleb läbi hindamise ([4.5](05-hindamine.md)) |
| **Lühem prompt ja väljund** | vähem tokeneid lugeda ja kirjutada (vt 2.4 ja 2.2) | lühem väljendus — kontrolli, et midagi olulist ei kärbitud |
| **Vahemälu** | sama sisend → salvestatud vastus saadakse kohe | vastus võib vananeda — vahemälu tuleb värskendada |
| **Voogedastus** | kasutaja näeb vastust juba kirjutamise ajal | koguaeg ei muutu — kasu on ainult kasutaja taju |
| **Sõltumatu töö paralleelsus** | mitu sõltumatut päringut korraga, mitte järjest | keerukam süsteemi ülesehitus — tulemused tuleb uuesti kokku panna |

Kolm vahendit väärivad paari lisalauset.

**Vahemälu (ingl k *cache* — salvestatud vastus sama sisendi jaoks).** Kui tihti korduv küsimus annab alati sama vastuse, pole põhjust mudelilt uuesti küsida: esmakordne vastus salvestatakse, järgmised saavad selle kohe — nii lahendatakse näiteks sageli küsitavate küsimuste lehekülg. Boonus: arveldatavat päringut pole, seega säästab vahemälu ka kulusid — [3.6](../03-susteemi-ulesehitus/06-kulude-haldamine.md) põhimõtted toimivad siin kaks korda.

**Voogedastus (ingl k *streaming* — vastuse kuvamine kirjutamise ajal).** Vastust ei näidata korraga, vaid kirjutatakse kasutajale silme all: esimesed sõnad ilmuvad sekunditega, ülejäänud tulevad järele. Koguaeg jääb samaks, aga kogemus muutub — süsteem „vastab kohe“ selle asemel, et vaikida ja siis kogu vastuse korraga välja pühkida. Vestluse jaoks peaaegu hädavajalik, taustatöö jaoks kasutu.

**Paralleelsus.** Kui ülesandes on osad, mis üksteisest ei sõltu (näiteks sama kirja kokkuvõte ja keelekontroll), saadetakse need välja korraga: ooteaeg on siis pikima osa aeg, mitte kõigi summa.

> **Lihtsalt öeldes:** vahemälu on „juba vastatud“ vihik — sama küsimusele loetakse vastus üles, mitte ei arvutata uuesti. Voogedastus ei tee süsteemi kiiremaks, aga kasutaja tunneb nii: esimesed sõnad ütlevad „vastan“, kuni ülejäänud tulevad järele. Mis üksteisest ei sõltu, tehakse korraga, mitte üksteise järel.

## Kolmnurk: kiirus, kvaliteet, kulu

Iga kiirendus on ost mõne teise väärtuse arvel. Kolm väärtust moodustavad kolmnurga, mille külgi ei saa korraga kõiki maksimumini venitada:

- tahad **kiiremini** → kas maksad rohkem (kiirem teenusetase, vahemälu — kulu tõuseb) või võtad riski (väiksem mudel, lühem juhis — kvaliteet võib langeda);
- tahad **kvaliteetsemalt** → suurem mudel või põhjalikum sisend — aeg ja kulu kasvavad;
- tahad **odavamalt** → väiksem mudel, lühem sisend ja väljund, vahemälu — säästad aega kindluse arvel.

Kõik kolm ei saa korraga maksimaalsed olla — iga kasutusala valib rõhuasetuse ise: vestluses on esikohal kiirus (klient ootab), öises paketttöös kulu (keegi ei oota, aga arve tuleb).

Ja üks kindel reegel: **iga kiirenduskatse käib läbi hindamise** — mõõdikutega enne ja pärast ([4.5](05-hindamine.md)). Kui aeg lõi poole ja kvaliteet jäi samaks, on muudatus hea; kui kvaliteet lõi, pööratakse tagasi — kiirus, mis osteti klientide rahulolematuse arvel, pole ost. Kulusääst ja kiirussääst käivad käsikäes: samad vahendid langetavad mõlemat ([3.6](../03-susteemi-ulesehitus/06-kulude-haldamine.md)).

Pärast käikuandmist jälgib numbreid tootmises monitooring ([4.6](06-monitooring.md)). Ja kui süsteem kasvab mitmeteks töovoogudeks ja agentideks, muutub kolmnurga hoidmine arhitektuuriotsuseks — seda käsitleb [5.1](../05-suurte-projektide-tase/01-arhitektuur-suures-mahus.md).

## Näide samm-sammult: Kõneabi vestlusbot

Sama telefoni-tugi Kõneabi, kus [2.4](../02-praktika/04-sisendid-ja-andmed.md) puhastas transkriptsioone ja [4.3](03-pikaaegne-malu.md) ehitas kliendiprofiili, lisas saidile vestlusboti: klient kirjutab küsimuse, süsteem paneb päringusse suure mudeli ja kogu vestlusajaloo ([3.2](../03-susteemi-ulesehitus/02-konteksti-haldamine.md) põhimõttel), mudel vastab. Vastuseks kulus **8–12 sekundit** — kliendid sulgesid akna enne vastust.

**1. samm — mõõtmine.** Üks nädal andmeid: keskmine latentsus **9,5 s**, halvim juhtum (p95) **~20 s** — iga kahekümnes klient ootas vähemalt 20 sekundit. Enamik ajast läks pika sisendi (~4 000 tokenit ajalugu igal päringul) läbilugemisele ja pika vastuse kirjutamisele suure mudeliga. Numbrid paberil enne, muudatused pärast.

**2. samm — kolm muudatust.**

1. **Klassifitseerimine väiksele mudelile (0,8 s).** Iga küsimus käib kõigepealt läbi väikse ja kiire mudeli, kes määrab teema (tellimuse staatus, tagastus, hinnakiri) ja valib vajalikud kliendiandmed. Selleks lihtsaks sammuks pole suurt mudelit vaja ([1.1](../01-alused/01-mis-on-ai-mudel.md): praktikant) — 0,8 s ja samm läbi.
2. **Pika ajaloo asemel kliendiprofiil.** Klient tuvastatakse ja süsteem paneb päringusse ainult struktureeritud profiili — „asjade seis“ ([4.3](03-pikaaegne-malu.md)): nimi, tellimused, varasemad lahendused. Sisend on ~4 000 tokeni ajaloo asemel ~400 tokenit — samad faktid, kümnendik lugemist.
3. **Voogedastus chat-i.** Vastus kandub kliendile token kaupa: esimesed sõnad ilmuvad juba ~1 s juures, kuigi täielik vastus valmib mõne sekundiga nagu enne.

**3. samm — hindamine ja tulemus.** Enne kasutuselevõttu käis uuendatud süsteem läbi testikomplekti ([4.5](05-hindamine.md)): kvaliteet **ei kukkunud** — profiil sisaldas samu fakte, mida pikk ajalugu. Kuu hiljem:

| Mõõdik | Enne | Pärast |
|---|---|---|
| Keskmine latentsus | 9,5 s | 2,8 s |
| Halvim juhtum (p95) | ~20 s | ~5 s |
| Esimesed sõnad kliendile | 9,5 s (kogu vastus korraga) | ~1 s |
| Kulu päevas | baas | −35% |
| Kvaliteet (testikomplekt, 4.5) | baas | ei kukkunud |

Keskmine aeg lühenes rohkem kui kolm korda, halvim juhtum peaaegu neli — ja kliendid jäävad nüüd vastust ootama. Kulu langes 35% lühema sisendi ja väiksema mudeli arvel: kulusääst ja kiirussääst kõndisid käsikäes.

## Kokkuvõte

- **Latentsus on ooteaeg sisendist vastuseni;** talutav piir sõltub sellest, kas keegi ootab — vestluses mõni sekund, taustatöös minuteid.
- **Aeg koguneb viiest kohast:** suur mudel, pikk sisend, pikk väljund, agendi mitu käiku ja pakkuja hetkeline koormus.
- **Viis kiirendajat:** väiksem mudel lihtsateks sammudeks, lühem prompt ja väljund, vahemälu, voogedastus ja paralleelsus — igaüks on ostetud millegi arvel.
- **Kolmnurk:** kiirus, kvaliteet ja kulu ei saa korraga maksimaalsed; iga kasutusala valib rõhuasetuse ja iga muudatus käib läbi hindamise (4.5).
- **Mõõda keskmist ja halvimat juhtumit (p95):** keskmine ütleb tavapärase kogemuse, p95 näitab halvima õnnega kasutaja oma — keskmine varjab äärmusi.

## Mis edasi?

- eelmine → [4.6 Monitooring tootmises](06-monitooring.md)
- järgmine tase → [5.1 Arhitektuur suures mahus](../05-suurte-projektide-tase/01-arhitektuur-suures-mahus.md) (4. tase on läbi)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
