# 3.1 API integratsioonid: mudel programmist välja kutsuda

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [2.6 Promptide haldus kui vara](../02-praktika/06-promptide-haldus.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, mis on API (programmiliides) ja millal workflow (töövoog) enam ei piisa;
- käsitleda API-võtit (ingl k *API key*) — süsteemi passi ja kassasüsteemi — vastutustundlikult;
- kokku panna päring (ingl k *request*): API-aadress, mudeli nimi, prompt (mudelile antav juhis) ja parameetrid, sealhulgas temperatuur;
- lugeda vastuse (ingl k *response*) struktuuri: kus on mudeli tekst ja kui palju tokeneid kulus;
- viia [2.5](../02-praktika/05-esimene-workflow.md) tagastusvoog API-le ja testida esimest väljakutset enne tootmist.

## Lihtsalt öeldes

> 1. tasandil rääkisid AI-mudeliga sina ise: kopeerisid kirja, lugesid vastuse. API tähendab, et sama töö teeb sinu süsteem: programm saadab mudelile kindlas vormis kirja — juhised ja andmed — ja saab kindlas vormis vastuse. [2.5](../02-praktika/05-esimene-workflow.md) workflow tegi seda küll juba, aga klikkide varjus: tööriist saatis päringud sinu eest. Kui aru saad, et edasi antakse viis asja — aadress, mudeli nimi, juhised, parameetrid ja võti (viimane päiseriidal) — ja vastusest loetakse tagasi tekst ning kulu, on API-loogika selge.

## Mis on API ja miks mudelit sealt kutsuda

**API (programmiliides)** on kokkulepe, kuidas programmid omavahel räägivad: mis aadressile, mis kujul saata ja mis kujul vastus tuleb. Kõlaline võrdlus: teenindusaken. Sa ei lähe kööki: jäta tellimus aknast ja saad toidu kindlas vormis tagasi. Köök võib asuda teises linnas — akna kord jääb. Mudeli „köök“ on teenusepakkuja serverites; sinu süsteem vajab ainult akent.

Miks mitte jääda visuaalse tööriista juurde? Sest [2.5](../02-praktika/05-esimene-workflow.md) lubas: „kui vool tuleb hiljem käivitada oma süsteemi seest, on järgmine tase API“. Tööriist (n8n, Make, Zapier) on hea paik ehitamiseks, aga kui voo peab käivitama sinu programm — uus tellimus, nupuvajutus rakenduses —, on tööriist vahendaja, kelle vahele jätta: programm kutsub mudelit otse. Reegel: klikitav protsess jääb [workflow](../02-praktika/03-workflow-algtasandil.md) tasandile; sinu programmi osa läheb API-le.

Siin kutsub programm mudelit; vastupidine suund — mudel ise kutsub tööriistu — on [3.3 Tööriistad ja tegevused](03-tooriistad-ja-tegevused.md) teema.

## API-võti: sinu süsteemi pass ja kassasüsteem

> **Lihtsalt öeldes:** API-võti on tööluba ja kassasüsteem ühes: ta ütleb pakkujale, kes tuleb ja kelle arvele kulud lähevad. Võti on salajane nagu pangakaardi PIN — miks ja kuidas hoida, õpetab [3.7 Turvalisus](07-turvalisus.md).

Kui sinu programm seisab akna ees, peab ta end ära tundma andma. Selleks on API-võti (ingl k *API key*) — salajane kood, mis tuvastab sinu süsteemi ja arvab kulu sinu kontole. Kaks tagajärge:

- **Pass.** Kõik, mis sinu võtmega kutsub, loetakse sinu süsteemi tehtuks. Seepärast iga süsteem oma võtme: poe süsteemil üks, testil teine — kui midagi juhtub, näed, kumb süsteem vea tekitas, ja sulged ainult selle.
- **Kassasüsteem.** Iga kutse maksab (tokenid, vt allpool) ja arve läheb võtme omanikule.

Kolm reeglit: ära pane võtit sinna, kuhu teised saavad (avatud kood, e-kiri); vaheta kohe, kui võti on sattunud sinna, kuhu ei tohtinud; kustuta kasutamata võti. Kus võtit hoida ja kuidas leket parandada, õpetab [3.7 Turvalisus](07-turvalisus.md).

## Päring ja vastus: kuidas väljakutse välja näeb

**Päring (ingl k *request*)** on sinu süsteemi kiri mudelile. Neli osa:

1. **API-aadress** — kuhu kiri läheb; pakkuja annab selle kirjas.
2. **Mudeli nimi** — kumb mudel: pakkujatel on neid palju, oskus ja hind erinevad.
3. **Sisendjuhised** — prompt (mudelile antav juhis): [2.1](../02-praktika/01-hea-prompt.md) testimise tsükli läbinud mall koos selle kirja andmetega.
4. **Parameetrid** — sätted, millega kutse häälestad. Tuntuim on temperatuur: kui palju mudel tohib vastuses varieeruda. Nulli juures valib mudel peaaegu alati sama, kõige tõenäolisema jätku; kõrgemal lubab endale rohkem varieerumist. Liigitajale madal, kirjutajale kõrgem. Päringule saab seada ka aegumislimiidi (ingl k *timeout* — kui vastust selle ajaga ei tule, katkestab süsteem ootamise); mida siis edasi teha, vaatab [3.4](04-vead-ja-veakasitlus.md).

Nende osade ühine pseudokuju (päriselt lisandub veel paar tehnilist rida — põhimõte jääb samaks):

```json
{
  "aadress": "https://api.pakkuja.ee/v1/kutsed",
  "mudel": "keelemudel-v3",
  "temperatuur": 0.2,
  "juhised": "Sa oled e-poe kliendikirjade liigitaja … KLIENDI_KIRI: [kirja tekst]",
  "võti": "sk-…"
}
```

(Päriselt liigub võti enamasti eraldi päiseriidal (HTTP-päises) — põhimõte sama: iga kutse viib selle kaasa.)

**Vastus (ingl k *response*)** tuleb tagasi samas vaimus:

```json
{
  "kutse_id": "req_7f31c",
  "tekst": "{\"liik\": \"tagastus\", \"toode\": \"villane sall\", \"tellimuse_nr\": \"1187\", \"põhjus\": \"sall jäi liiga kitsaks\"}",
  "tokenid": { "sisend": 486, "väljund": 52 }
}
```

Vastusest loeb programm kolm asja:

1. **Mudeli tekst** välja „tekst“ sees. Pange tähele: sisu on jälle JSON — [2.2 struktureeritud väljund](../02-praktika/02-struktureeritud-valjund.md) tuleb kätte tekstina, mille programm lahti loeb.
2. **Tokenite kasutus.** token (sõnatükk) on tekstikild, mille põhjal mudel loeb ja kirjutab; sisend + väljund on kulu alus — kuidas tokenarv eurodeks saab, õpetab [3.6 Kulude haldamine](06-kulude-haldamine.md).
3. **Kutse ID** — iga väljakutse on aadressitav kirje: hiljem näed, mis selle juures juhtus (2.5 kolme numbri jälgimine).

> **Lihtsalt öeldes:** päring ja vastus on kirjavahetus kokkulepitud kujul: sinu süsteem kirjutab oma kõrvale, pakkuja oma — kumbki ei pea teise siseelu tundma, ainult vormi.

## Liitumislimiit: mitu korda minutis

> **Lihtsalt öeldes:** liitumislimiit on bussi uks: korraga läheb läbi kindel arv reisijat. Pole selleks, et sind tagasi hoida, vaid et kõik oma piletiga sisse saaks.

**Liitumislimiit (ingl k *rate limit*)** on pakkuja kokkulepitud piir: mitu päringut minutis (tihti ka päevas) ühe võtme kohta tohib saata. Miks on see olemas? Mudelit teenindatakse jagatud võimsuse pealt — üks vales kohas jooksev silmus võtaks võimsuse teistelt ära. Limiit kaitseb kõiki, ka sind: ilma selleta jääks su süsteem mõne teise vea tõttu ootama.

Kui limiit ületatakse, jäetakse päring täitmata ja tuleb veateade — süsteemi töö on oodata ja uuesti proovida; kuidas seda (aegumine, kordused, varuteed) korralikult teha, õpetab [3.4 Vead ja veakäsitlus](04-vead-ja-veakasitlus.md).

Kodutoa poe mõõdupuu: 40 kirja päevas on 8-tunnise tööpäeva juures keskmiselt umbes 5 kirja tunnis — kordades all limiidist. Piir muutub oluliseks, kui süsteem töötleb korraga sadu kirju või mõni silmus kutseid kordab: enne suuremat mahtu kontrolli limiit üle.

## Esimene väljakutse: testimine enne tootmist

Esimene päring ei ole 40 päris kliendikirja, vaid üks testkiri — sama distsipliin kui [2.5](../02-praktika/05-esimene-workflow.md) testimise tabelis:

1. Võta testjuhtumitest kõige lihtsam: tüüpiline tagastus koos toote ja numbriga.
2. Saada üks päring testvõtmega ja loe vastus ise läbi: kas „liik“ on „tagastus“, kas väljad täitsid, kas tokenite arv on mõistlik?
3. Saada sama päring teist korda. Kas vastus jäi samaks? (Miks see tähtis on, näitab allpool näide.)
4. Käi läbi veel kolm-neli juhtumit — ka „puudub“ ja „muu“ variandid.
5. Alles siis lülita tootmisse — ja isegi seal jääb inimese kinnitusahelasse (ingl k *human-in-the-loop*): kavandit kinnitab edasi Piret. API muudab seda, kes mudelit kutsub, mitte seda, kes otsustab.

Suurte mahtude kohta üks reegel: palju korraga pole test, vaid tootmine ilma turvavõrguta.

## Näide samm-sammult: tagastusvoo viimine API-le

Kodutoa poe arendaja Tanel viib [2.5](../02-praktika/05-esimene-workflow.md) tagastusvoo visuaalsest tööriistast poe enda süsteemi: iga uue kirja puhul paneb programm päringu kokku ise.

**1. Mis jääb puutumatuks.** Liigitaja prompti ei kirjutata koodi sisse — programm laadib selle [promptipangast](../02-praktika/06-promptide-haldus.md) kehtiva versioonina (v2). Kui prompt parandatakse ja kinnitatakse, uueneb süsteem ilma koodi puudutamata — see ongi põhjus, miks promptid on vara.

**2. Käivitaja.** Uus kiri poe aadressile käivitab voo nagu enne; ainult käivitaja on nüüd poe süsteemi sündmus, mitte tööriista plokid.

**3. Päring.** Programm paneb kokku viis osa — API-aadress, mudeli nimi, 2.5 liigitaja prompt (selle kirja tekst [KLIENDI_KIRI] alla), parameetrid (temperatuur 0,2) ja poe süsteemi võti (viimane päiseriidal):

```json
{
  "mudel": "keelemudel-v3",
  "temperatuur": 0.2,
  "juhised": "Sa oled e-poe kliendikirjade liigitaja. Määra liik, toode, tellimuse_nr ja põhjus … KLIENDI_KIRI: „Tere! Soovin tagastada villase salli (tellimus 1187) — jäi liiga kitsaks. Pille K.“",
  "võti": "sk-…"
}
```

**4. Vastus.**

```json
{
  "kutse_id": "req_7f31c",
  "tekst": "{\"liik\": \"tagastus\", \"toode\": \"villane sall\", \"tellimuse_nr\": \"1187\", \"põhjus\": \"sall jäi liiga kitsaks\"}",
  "tokenid": { "sisend": 486, "väljund": 52 }
}
```

Programm loeb väljad: „liik“ läheb tingimusse („muu“ → Pireti loend), „tellimuse_nr“ otsingusse. Edasi kõik nagu 2.5-s: kavand päris tellimuse andmetega, Piret kinnitab enne saatmist.

**5. Ja kui päring kordub?** Tanel saadab sama kirja veel kord — see on testimise samm 3 ülalpool:

```json
{
  "kutse_id": "req_7f31d",
  "tekst": "{\"liik\": \"tagastus\", \"toode\": \"villane sall\", \"tellimuse_nr\": \"1187\", \"põhjus\": \"sall jäi liiga kitsaks\"}",
  "tokenid": { "sisend": 486, "väljund": 52 }
}
```

Sama tulemus. Just seda madal temperatuur tagab: sama sisend annab praktiliselt sama liigituse — liigitus on korduskatsetatav ja vead uuritavad. Sada protsenti garantiid mudel siiski ei anna; seepärast jääb Piret ahelasse ja logisse jäävad mõlemad kutse ID-d.

> **Lihtsalt öeldes:** väljastpoolt pole midagi muutunud — kiri tuleb, kavand valmib, Piret kinnitab. Muutus on seespool: plokkide klikkimise asemel saadab süsteem päringuid.

## Kokkuvõte

- **API (programmiliides)** on tee, kuidas programm mudelit kutsub: workflow-tööriist tegi seda klikkide varjus, API-ga teeb sinu süsteem ise — käivitaja on sinu programm.
- **API-võti (ingl k *API key*)** on pass ja kassasüsteem: iga süsteem oma võti, hoitakse nagu pangakaarti, vahetatakse kohe, kui kahtlus.
- **Päring (ingl k *request*)** = API-aadress + mudeli nimi + prompt (mudelile antav juhis) + parameetrid; temperatuur näitab, kui palju mudel tohib vastuses varieeruda.
- **Vastuse (ingl k *response*) struktuurist** loeb programm kolm asja: mudeli teksti (struktureeritud väljund kui tekst), tokenite kasutuse (kulu alus — detail [3.6](06-kulude-haldamine.md)) ja kutse ID.
- **Limiit, kordus ja testimine:** liitumislimiit (ingl k *rate limit*) kaitseb kõiki; madal temperatuur teeb liigituse korduskatsetatavaks; esimene väljakutse tehakse ühe testkirjaga, mitte tootmises — ja inimese kinnitus jääb.

## Mis edasi?

- eelmine → [2.6 Promptide haldus kui vara](../02-praktika/06-promptide-haldus.md)
- järgmine → [3.2 Konteksti haldamine: kuidas mudel „mäletab“](02-konteksti-haldamine.md)
- Kulude detail → [3.6 Kulude haldamine](06-kulude-haldamine.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
