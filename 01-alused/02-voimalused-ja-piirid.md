# 1.2 Võimalused ja piirid: mida automatiseerida tasub

> **Sihtpublik:** kõik | **Eeltingimused:** [1.1 Mis on AI-mudel ja kuidas ta „mõtleb“](01-mis-on-ai-mudel.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- hinnata tööülesannet nelja otsustusküsimuse abil ja põhjendada, kas AI-automatiseerimine (töö delegeerimine AI-mudelile) tasub;
- nimetada kategooriaid, kus AI-mudel on tugev, ja kohti, kus inimene jääb asendamatuks;
- tunda ära „punased lipud“ — olukorrad, kus automatiseerimine toob rohkem kahju kui kasu;
- mõista, et otsus pole jah/ei valik, vaid küsimus, kuhu inimene jääb.

## Lihtsalt öeldes

> AI-mudel on nagu väga tark, aga sinu ettevõtet veel mitte tundev assistent — usaldusväärne tüütu rutiini juures, aga vajab ülevaatust seal, kus viga maksab. Ta on lugenud tohutu hulga tekste, kirjutab sujuvalt, ei tüüda rutiinist ja töötab ööpäev läbi. Aga sa ei anna assistendile volitust kliendiga hinda kokku leppida — sama kehtib AI-automatiseerimise puhul: mida kallim on vea hind, seda kindlamalt peab inimene tulemuse enne kasutust üle vaatama.

AI on tööjõud kontrolli all, mitte otsustaja. Järgnevalt: milliseid ülesandeid tal paluda tasub, milliseid mitte ja kuidas otsustada.

## Kust AI-l põhjalikult kasu on: 4 ülesandekategooriat

Dokumendist 1.1 teame: mudel ennustab teksti jätku, mitte ei otsi fakte üles. Sellest järgneb tema tugevus — tekstiga saab ta töötada kiiresti ja paindlikult ning koostada sõnastuse, mis kõlab nagu hea töö. Kõige paremini õnnestuvad tal neli kategooriat.

| Kategooria | Näide | Miks AI-l õnnestub |
|---|---|---|
| **Tekstide kavandid** | vastused kliendikirjadele, tootekirjeldused, koosoleku kutse | mudel on treeningul „lugenud“ miljoneid sarnaseid tekste ja koostab ladusa visandi sekunditega; inimene viimaseerib |
| **Klassifitseerimine** | e-kirja jaotamine rühmadesse „müük“, „tugi“, „arve“ | vastuseks paar sõna; viga on kergesti märgata ja täpsustada saab näidistega |
| **Kokkuvõtete tegemine** | 30-leheküljeline leping viie punktini, kohtumise kokkuvõte | pika teksti tihendamine on just see, milleks ennustusloogika kõige paremini sobib; allikale saab nõuda viitamist |
| **Info eraldamine ja vormingu teisendus** | arvete andmed tabelisse, punktloendist e-kirja visand | struktuuri muutmine on talle loomulik; kontroll on kiire, võrreldes välju allikaga (eeldus: dokument mahub kontekstiaknasse — ingl k *context window*, tekstihulk, mida mudel korraga näeb) |

Ühine nimetaja: tegu on tekstiga, tulemus peab olema *hea* — mitte sõna-sõnalt etteantud — ja inimene saab seda kiiresti kontrollida.

> **Lihtsalt öeldes:** kõiki nelja ühendab muster — mudel teeb kaheksakümmend protsenti tööst minutiga, inimene paneb peale pitseri. Päris ettevõtete näiteid siin ei loetle: tüüpilised kasutusalad valdkondade kaupa loetleb dokument [1.4](04-kasutujuhtumid.md).

## Kus inimene jääb asendamatuks

> **Lihtsalt öeldes:** inimene on asendamatu seal, kus tegu pole enam ülesandega, vaid vastutusega. Mudel oskab pakkumuse sõnastada, aga ei saa selle eest vastata; oskab vabandust kirjutada, aga ei tunne, kui palju kaalub maine, mille nimel iga sõna öeldakse.

Neli kohta, kus otsustab ja kirjutab inimene:

1. **Vastutusotsused.** Otsused, mille eest keegi ametlikult vastutab — palkamine, partneri valik, õiguslik seisukoht. Mudel võib tuua välja variante, aga otsus ja tagajärjed jäävad inimesele.
2. **Kriisikommunikatsioon.** Kui midagi on juhtunud ja avalikkus jälgib, sõnastab teksti inimene, kes tunneb olukorda ja osapooli. Automaatne vastus võib asja süvendada — mudel ei tunne konteksti, kus ta räägib.
3. **Hinnapakkumiste lõplik kinnitus.** Mudel võib visandi koostada, aga lõplik hind ja tingimused on äriline otsus — üks vale number on otse kahjum. Kinnitus on inimese allkiri.
4. **Kliendisuhte olulised hetked.** Klient kaebab avalikult, pikaajaline klient on lahkumas — need on suhted, mitte kirjad. Vale toon maksab siis rohkem, kui automatiseerimine kokku hoidis.

## Kuidas otsustada: 4 küsimust enne automatiseerimist

Kui kahtled, kas mõni oma ülesanne AI-le anda, käi läbi need neli küsimust. Mida rohkem jaatavaid vastuseid, seda kindlam on automatiseerimine.

**1. Korduvus — kas ülesanne kordub regulaarselt?**
Automaatika vaev tasub ära ainult siis, kui töö tuleb ette ikka ja jälle. Näide: kakskümmend kliendikirja nädalas — võimalus; aastaaruanne kord aastas — kirjutad ise.

**2. Tekstipõhisus — kas sisend ja väljund on tekst?**
Keelemudel (ingl k *large language model*, LLM — suur keelemudel) töötab tekstiga. Kui sisend on füüsiline paber ja väljund allkiri, jääb töö niigi inimesele. Näide: PDF-arved e-postis — sobib; kaupade loendamine laos silmaga — ei.

**3. Vea talutavus — kui halb on, kui vastus on 90% õige?**
Kõige tähtsam küsimus. Kui kliendikirja visandid on 90% korrektsed ja parandad need enne saatmist, säästad aega. Kui arvete summad on 90% õiged, on iga kümnendal arvel viga — ja keegi peab selle üles leidma. Küsi endalt: kas 90% õige on veel kasu (visand, mida parandan) või juba kahju (number, mida enam keegi ei kontrolli)?

**4. Näidiste olemasolu — kas on näiteid, kuidas head tööd tehtakse?**
Mudel jäljib eeskuju. Kümme head vastust klientidele või kolm head pakkumust on kuld: annad need kaasa promptile (mudelile antav juhis) ja tulemus langeb sinu stiili. Kui head tööd pole kusagil näitel, ei tea seda mudelki — palve „kirjuta meile müügisõnum“ annab ilma näideteta ainult keskpärase. Kuidas juhiseid ja näidiseid hästi sõnastada, õpetab dokument [1.3 Promptide põhitõed](03-promptide-pohitoed.md).

Vastused annavad kohe otsustusreegli:

| Kui vastuseks on… | siis mõistlik samm on… |
|---|---|
| neli „jah“ | automatiseerida terviklikult — ehita workflow (töövoog — automatiseeritud sammude jada), kus kontrollid on sisse ehitatud — praktikas alusta kavanditest inimese ülevaatusega ja liigu aja jooksul täieliku töövoo suunas |
| kolm „jah“ ja üks „ei“ | automatiseerida osaliselt ja jätta inimene kinnitusahelasse (ingl k *human-in-the-loop* — inimene vaatab tulemuse enne kasutust üle) |
| kaks või enam „ei“ | veel mitte automaatiseerida — korrasta ülesanne ja kogu näited ise |

Kuidas selliseid töovoogu — ja kui sammude jada kasvab mitmeastmeliseks, AI-agenti (süsteemi, mis täidab ülesannet iseseisvalt samm-sammult) — üles ehitatakse, kirjeldab dokument 1.5 ning 2. taseme juhendid.

## Punased lipud: millal EI tasub automatiseerida

Need on märgid, et ülesanne pole valmis automatiseerimiseks — või ei saa seda kunagi olema:

- **Head tulemust pole, millele toetuda.** Kui keegi ei oska öelda, mis eristab head halvast, ei saa seda ka mudel teada.
- **Fakte pole kusagil kirja pandud.** Kui õige vastus sõltub infost, mida süsteemile kaasa ei saa — andmed on kellegi mälus või suulises kokkuleppes —, täidab mudel tühjad kohad usutavate pakkumistega. Seda nimetatakse hallutsinatsiooniks (mudeli kindlalt öeldud, aga vale vastus); põhjused on põhjalikult lahatud dokumendis [1.1](01-mis-on-ai-mudel.md).
- **Vea hind on kõrge, aga kontrollpunkti pole.** Kui vale vastus jõuab inimesest mööda väljapoole — makse läheb teele, pakkumus kliendile —, pole risk enam kontrolli all.
- **Iga juhtum on erinev.** Kui reeglid muutuvad iga korraga, tuleb juhiseid pidevalt ümber kirjutada — rohkem tööd, mitte vähem.
- **Kontroll on kallim kui töö ise.** Kui tulemuse ülevaatamine võtab rohkem aega kui ülesande ise tegemine, pole mõtet automaatiseerida.

## Näide samm-sammult: väikese ettevõtte 4 ülesannet läbi otsustusmaski

Võtame väljamõeldud näiteks **„Kodutoa pood“** — Mari müüb e-poos käsitöökaupu ja teeb kõik muu ise. Tema neli tüütu tööd läbi otsustusmaski:

**1. Kliendikirjade vastuste kavandid.** Kirju tuleb 15–20 nädalas ja enamik küsib sama: kui kaua saatmine kestab ja kas toode on laos. *Korduvus:* jah. *Tekstipõhisus:* jah. *Vea talutavus:* keskmine — vale vastus on pahandus, mitte katastroof, ja Mari vaatab visandi enne saatmist üle, nii et veaga vastused jõuavad kliendini ainult harva. *Näidised:* jah — eelmistest aastatest üle saja hea vastus. **Otsus: jah, kavanditena** — süsteem koostab visandi, Mari vaatab sekunditega üle ja saadab.

**2. Arvete andmete sisestus.** Igas kuus umbes 40 müügiarvet PDF-ina; andmed lähevad raamatupidamise programmi. *Korduvus:* jah. *Tekstipõhisus:* jah. *Vea talutavus:* madal — vale summa tekitab muret nii raamatupidamises kui kliendis; 90% täpsus tähendab umbes nelja viga kuus. *Näidised:* jah — arvete väljad on alati samad. **Otsus: jah, aga kontrollpunktiga** — mudel tõmbab andmed, süsteem kontrollib, et arve number ja summa ühtivad tellimusega; ebaselged juhtumid lähevad Marile ülevaatusele.

**3. Hinnapakkumiste lõplik kinnitus.** Suuremad tellijad küsivad paar korda kuus pakkumust. *Korduvus:* jah. *Tekstipõhisus:* jah. *Vea talutavus:* puudub — pakkumus on lubadus: liiga madal hind on otsene kahjum, liiga kõrge ajab kliendi ära. *Näidised:* jah. **Otsus: ei.** AI võib visandi varasemate näidiste põhjal koostada, aga lõpliku kinnituse teeb Mari ise — see on vastutusotsus, mida ükski korduvus ei õigusta.

**4. Turundustekstide koostamine.** Toodete kirjeldused ja uudiskiri kord kuus. *Korduvus:* jah. *Tekstipõhisus:* jah. *Vea talutavus:* keskmine — üks halb postitus ei maksa kohe midagi, aga ebaühtlane toon kulutab brändi. *Näidised:* osaliselt — Mari tunneb sobivat toont, aga pole kirja pannud, milline on „meie hääl“. **Otsus: jah, pärast ettevalmistust** — Mari paneb kõigepealt kokku viis enda parimat teksti näidiseks; siis koostab mudel variante, mille hulgast ta valib ja viimaseerib. Ilma näideteta oleks otsus olnud „pole veel valmis“.

Pane tähele mustrit: samad neli küsimust andsid kolm jaatavat otsust ja ühe keeldu. Automatiseerimine pole lüliti vahetamine, vaid valik, kus inimene protsessis seisab.

## Kokkuvõte

- **Neli otsustusküsimust:** korduvus, tekstipõhisus, vea talutavus, näidiste olemasolu — mida rohkem jaatavaid vastuseid, seda kindlam on automatiseerimine.
- **AI tugevad alad:** tekstide kavandid, klassifitseerimine, kokkuvõtted ning info eraldamine ja vormingu teisendus — tekstitöö, kus hea tulemus piisab ja inimene viimaseerib.
- **Inimene jääb asendamatuks:** vastutusotsused, kriisikommunikatsioon, pakkumuste kinnitus ja kliendisuhte olulised hetked.
- **Punased lipud:** kui head tulemust pole näidata, fakte pole kusagil või kontroll maksab rohkem kui töö, ära automatiseeri.
- **Otsus on harva binaarne:** kõige sagedasem õige vastus on „jah, aga inimene kinnitusahelas“.

## Mis edasi?

- eelmine → [1.1 Mis on AI-mudel ja kuidas ta „mõtleb“](01-mis-on-ai-mudel.md)
- järgmine → [1.3 Promptide põhitõed](03-promptide-pohitoed.md) — kuidas juhiseid ja näidiseid hästi sõnastada
- Kus AI-automatiseerimine juba töötab — konkreetsete näidete kataloog: [1.4](04-kasutujuhtumid.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
