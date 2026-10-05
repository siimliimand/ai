# 5.6 Pidev täiustamine: mõõtmisest otsusteni

> **Sihtpublik:** juhtiv mitte-tehniline + tehniline | **Eeltingimused:** [5.5 Mudelite vahetamine ja drift](05-mudelite-vahetamine.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada põhimõtet „mõõtmine ise ei paranda midagi — parandab otsus“ ja kirjeldada täiustamise tsüklit (andmed → analüüs → muudatus → mõõtmine);
- viia läbi kuine täiustamise tsükkel viies etapis — tabeli järgi, iga etapi väljund silmis;
- prioriseerida ideid mõju × vaevus kujundiga ja kirjutada julgelt ka „ära tee“;
- leida täiustusideid viiest allikast, millest väärtuslikum on inimese sekkumiste loend;
- langetada ka otsust lõpetada ([5.4](04-kulustrateegia.md)) ja hoida tsüklile väikest regulaarset rütmi.

## Lihtsalt öeldes

> Monitooring ([4.6](../04-agendid-ja-mootmine/06-monitooring.md)) annab numbreid, aga number ei paranda midagi — midagi parandab inimene, kes pärast numbrite vaatamist otsustab midagi teisiti teha. Pidev täiustamine on kokkulepe: korra kuus istub meeskond numbrite taha, valib ühe muudatuse, kirjutab üles oodatava mõju ja kontrollib kuu hiljem, kas see täitus. Kaks tundi kuus annab aasta jooksul kaksteist mõõdetud parandust — rohkem kui üks suur ülevaatamine aastas.

## Mõõtmine ei paranda — parandab otsus

[4.6](../04-agendid-ja-mootmine/06-monitooring.md) lõpetas nädalarutiiniga: 15 minutit nädalas kolme küsimusega logi üle. See rutiin kogub andmeid ja tabab hoiatusi, aga veel pole midagi parandanud. Mõõdik (ingl k *metric* — [4.5](../04-agendid-ja-mootmine/05-hindamine.md)-s ja 4.6-s tutvustatud mõõdetav näitaja) on ainult teade; parandus sünnib otsusest: **mis me muudame, mida sellest ootame ja kuidas me teame, et see toimus.**

Kui meeskond vaatab numbreid kuu kaupa ja mitte midagi ei muutu, pole monitooring enam tööriist — on rituaal. Seepärast lõpeb iga mõõtmine otsusega, ja otsus viib uue mõõtmiseni — seda nimetatakse täiustamise tsükliks:

```
   ┌─────────────────────────────────────────────┐
   ▼                                             │
 andmed ──► analüüs ──► muudatus ──► mõõtmine ───┘
```

- **andmed** — kuuaruanne; **analüüs** — mis kordus; **muudatus** — üks korraga, testitud; **mõõtmine** — kas mõõdik liikus?

Tsükkel käib niikaua, kui süsteem elab — aga ainult siis, kui iga ring lõpeb otsusega. Ilma otsuseta jääb tsükkel jooniseks.

> **Lihtsalt öeldes:** kaal ei langeta — kaal näitab ainult, kas see, mida teed, töötab; mõõtmine ütleb „paranes või ei“, aga liikuma paneb otsus midagi muuta.

## Kuine täiustamise tsükkel viies etapis

Nädalarutiin ([4.6](../04-agendid-ja-mootmine/06-monitooring.md)) jääb alles — tema töö on andmete kogumine ja kiire tabamine; suuremaks tööks kasvab sama mõte kuineks tsükliks: korra kuus 1–2 tundi, fikseeritud päev ja osalejad. Rollide jagamine on meeskonna kokkulepe — õpetab [5.2 Meeskonnatöö ja standardid](02-meeskonnatoo-ja-standardid.md).

| Etapp | Mis tehakse | Kes | Väljund |
|---|---|---|---|
| **1. Andmete kokkukogumine** | kuuaruanne: [4.6](../04-agendid-ja-mootmine/06-monitooring.md) kuus mõõdikut, [3.4](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md) logikirjed, inimese sekkumise juhtumite loend | haldaja | kuu pilt numbrites + sekkumiste nimekiri |
| **2. Analüüs** | kaks küsimust: mille peal kulub kõige rohkem inimese aega? millised eksimused korduvad? | sisukujundaja + kontrollija | korduvate probleemide loend sagedustega |
| **3. Otsuse loend** | iga idee kirja: mis muudame ja mida oodatakse — oodatav mõju mõõdikule | kogu meeskond, otsustab tellija | otsuse loend — ideed koos oodatava mõjuga, prioriteeritult |
| **4. Täiendamine** | üks muudatus korraga ([2.1](../02-praktika/01-hea-prompt.md)), enne tootmisse minekut regressioonitest ([4.5](../04-agendid-ja-mootmine/05-hindamine.md)) | ehitaja | uuendatud prompt või teadmusbaas + testitulemus |
| **5. Kontroll kuu hiljem** | kas mõõdik liikus oodatud suunas? | haldaja + tellija | otsus: jäi / võeti tagasi / järgmine idee |

Kaks kohta, kus tsükkel kõige sagedamini katki läheb:

- **Etapil 3 ei kirjutata mõju üles.** „Parandame kvaliteeti“ pole oodatav mõju; „sekkumise määr 14% → alla 10%“ on. Ilma numbrita on kuu hiljem võimatu öelda, kas muudatus õnnestus.
- **Etapil 4 tehakse korraga mitu muudatust.** [2.1](../02-praktika/01-hea-prompt.md) põhimõte kehtib ka siin: korraga üks muudatus, muidu ei tea kuu hiljem, kumb neist aitas. Regressioon (ingl k *regression* — varem töötanu läbikukkumine, [4.5](../04-agendid-ja-mootmine/05-hindamine.md) põhimõte) tabatakse testikomplektiga enne kliente.

Kui oled juht, kes kuu ülevaatust juhib, siis sinu töö pole tabeleid ette lugeda, vaid küsimuste esitamine:

1. Milline mõõdik likus sel kuul kõige rohkem — ja miks just see?
2. Mitu tundi kulus inimestel sekkumiseks? Kas samad juhtumid kordusid?
3. Eelmise kuu muudatus: kas mõõdik liikus, nagu otsuse loendis lubati? Kui ei — miks?
4. Mis on klientide juures uut, mida süsteem veel ei tunne?
5. Millised ideed jäid sel kuul tegemata — ja kas nad on veel olulised?

### Kust ideed tulevad: viis allikat

1. **Inimese sekkumised — kõige väärtuslikum allikas.** Iga käsitsi parandatud vastus on õpetus: kui kontrollija muutis vastuse enne saatmist, teab ta täpselt, mis oli valesti. Sekkumiste loend annab konkreetsed kohad, mida parandada — tasuta eksperthinnang, mida mujalt osta ei saa.
2. **Kliendi tagasiside.** Korduvad küsimused ja valupunktid otse kliendilt: mis jäi arusaamatuks, mis vastus aitas, mis ei.
3. **Hoiatused ([4.6](../04-agendid-ja-mootmine/06-monitooring.md)).** Iga käivitunud hoiatus on pooleldi analüüsitud juhtum — jääb üle leida põhjus ja otsustada, kas parandada.
4. **Promptipanga ajalugu ([2.6](../02-praktika/06-promptide-haldus.md)).** Versioonide ajalugu näitab, mis on juba proovitud ja mis tollal juhtus — palju „uusi“ ideid on tegelikult vanad ideed, mida keegi ei mäleta.
5. **Testijuhtumid, mis kukkusid ([4.5](../04-agendid-ja-mootmine/05-hindamine.md)).** Iga läbikukkunud juhtum kirjeldab täpselt olukorda, mida süsteem veel ei oska — ja jääb pärast parandust kontrolliks, et regressiooni ei tule.

### Otsus lõpetada

Pidev täiustamine hõlmab ka julgust lõpetada: kui kasu/kulu võrdlus ([5.4](04-kulustrateegia.md)) näitab mitu kuud järjest, et süsteemi hooldamine maksab rohkem, kui see säästab, on õige otsus võtta süsteem maha — mitte täiustada asja, mida keegi enam ei vaja.

### Rütm: väike regulaarne laseb suure maha

Kuine tsükkel on hambaravi: väike regulaarne samm, mitte aastane plomeerimine. Kord aastas tehtav suur ülevaatus kogub ette kaksteist kuu jagu muutusi: andmed on vahepeal mitu korda muutunud ja numbrid pole enam eraldatavad — keegi ei tea, mis mõjus ja mis mitte. Kaks tundi kuus hoiab iga muudatuse väikese ja pööratavana.

## Prioriseerimine: mõju × vaevus

Mõju × vaevus on prioriteerimise lihtsuskujund. Iga otsuse loendi rea kohta kaks küsimust: **kui palju see mõõdikut muudab?** ja **kui palju töötunde see maksab?**

| | **Väike vaevus** | **Suur vaevus** |
|---|---|---|
| **Suur mõju** | **tee kohe** — sel kuul | **planeeri** — võta järgmise kuu tsüklisse ja jaga suurem töö osadeks |
| **Väike mõju** | tee kõrvalt, kui aega jääb — või jäta | **ära tee** |

Kõige tähtsam ruut on „ära tee“. Iga tsükkel toodab ideesid, mis tunduvad mõistlikud, aga ei muuda ühtegi mõõdikut. Selle ruudu kirjapanek on sama oluline kui tegemine — kuu hiljem ei pea keegi küsima, miks seda ei tehtud.

> **Lihtsalt öeldes:** küsi igalt ideelt kaht asja — „kas mõõdik liigub?“ ja „kui palju see maksab töötunde?“ — ja pool nimekirjast langeb ise ära. Neli ruutu hoiab ära selle, et kuu töö kulub valel asjal.

## Näide samm-sammult: Kõneabi kolm kuud

Kõneabi vestlusbot — sama telefonitugi, kus [2.4](../02-praktika/04-sisendid-ja-andmed.md) puhastas transkriptsioone ja [4.3](../04-agendid-ja-mootmine/03-pikaaegne-malu.md) ehitas kliendiprofiili — on olnud tootmises kolm kuud ([4.7](../04-agendid-ja-mootmine/07-joudlus-ja-latentsus.md) kiirendas boti vahepeal kolm korda). Peamine mõõdik on inimese sekkumise määr — mitu % vestlustest läheb klienditeenindajale ([4.6](../04-agendid-ja-mootmine/06-monitooring.md) põhimõte).

**Kuu 1 — andmed ja otsuse loend.** Sekkumise määr on **14%**. Sekkumiste loendist kerkivad kolm korduvat eksimust:

1. **modemite mudelid** — bot segab seadmete mudelid omavahel ja annab valesid juhiseid;
2. **kuupäevad** — klient küsib „kas kolmapäevaks on lahendatud?“, aga bot ei tea, milline kuupäev kolmapäev on, ja lubab midagi muud;
3. **tervituse formaat** — vastused algavad kord nii, kord teisiti; osa on kohmakalt pikad.

Kolm korduvat eksimust, kolm rida otsuse loendis:

| Muudatus | Oodatav mõju | Prioriteet |
|---|---|---|
| märgendite täpsustus: mudelite ja kuupäevade jaoks selged reeglid promptis + 5 uut testjuhtumit | sekkumise määr 14% → alla 10% | suur mõju / väike vaevus → **kohe** |
| kliendiprofiili väljade uuendus: profiili tuleb hetkekuupäev ja paigalduse andmed | sama eksimuse vähendamine, aga vajab arendustööd | keskmine mõju / keskmine vaevus → **planeeri** |
| vastuse malli lühendus | tervitus on vorm, mitte sisu — ei muuda, mida klient lahendusest arvab | väike mõju → **ära tee** |

**Kuu 2 — muudatus ja kontroll.** Ehitaja tegi ainult esimese rea — üks muudatus korraga ([2.1](../02-praktika/01-hea-prompt.md)) — ja jooksutas enne tootmisse minekut testikomplekti läbi ([4.5](../04-agendid-ja-mootmine/05-hindamine.md)): regressiooni ei ilmnenud. Kuu lõpuks oli sekkumise määr **9%** — oodatud mõju täitus.

**Kuu 3 — uus nähtus.** Nädalane rutiin ([4.6](../04-agendid-ja-mootmine/06-monitooring.md)) märkas uut: kliendid küsivad nüüd **kiudinterneti paigaldust** — teema, mida teadmusbaasis pole, mistõttu need vestlused läksid inimesele. Teadmusbaasi lisati paigalduse tükk ([4.2](../04-agendid-ja-mootmine/02-rag.md)) ja viis testjuhtumit kontrolliks. Kas uus teema jääb, näitab järgmise kuu etapp 5.

Sama oluline on see, mida Kõneabi **ei** teinud: malli lühendus jäi tegemata — ja keegi ei küsinud selle järele; profiili uuendus elab otsuse loendis ja tuleb planeeritult. Kolm kuud, üks mõõdetud parandus, üks planeeritud töö ja üks kirjalik „ei“.

## Kokkuvõte

- **Mõõtmine ei paranda — parandab otsus:** mõõdik on teade, muudatus sünnib otsusest „mis me muudame ja mida ootame“.
- **Tsükkel on viieetapiline:** andmed → analüüs → otsuse loend → üks muudatus (regressioonitestiga) → kontroll kuu hiljem.
- **Iga otsuse loendi rida peab kandma numbrilist oodatavat mõju** — muidu ei saa kuu hiljem öelda, kas see õnnestus.
- **Mõju × vaevus prioriteerib:** suur mõju ja väike vaevus kohe, suur mõju ja suur vaevus planeeri, väike mõju ja suur vaevus — ära tee.
- **Parim allikas on inimese sekkumiste loend:** iga käsitsi parandatud vastus on konkreetne õpetus, mida mujalt osta ei saa.
- **Väike regulaarne laseb suure maha:** kaks tundi kuus hoiab muudatused väikesteks ja pööratavateks — ja hõlmab ka julgust lõpetada ([5.4](04-kulustrateegia.md)).

## Mis edasi?

- eelmine → [5.5 Mudelite vahetamine ja drift](05-mudelite-vahetamine.md)
- järgmine → [5.7 Vastutus, eetika ja governance](07-vastutus-ja-eetika.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
