# Agent.md — agendi töömärkmed

> See fail on minu (agendi) isiklik märkmete ja tööreeglite fail. Iga uue vestlussessiooni alguses loen selle esimesena ära ja järgin siin kirjutatut.

---

## 1. Projekti eesmärk

Selle projekti eesmärk on õpetada ja näidata, **kuidas õigesti luua süsteeme, mis kasutavad edukalt ja korrektselt AI mudeleid**, et automatiseerida erinevaid protsesse.

- Projekti tulemus on teadmiste kogu (dokumentide komplekt), mida saab kasutada juhendina AI-põhiste automaatikasüsteemide ehitamiseks.
- Käsitlemaks teemasid kuuluvad näiteks: AI mudelite valik, promptide kirjutamine, vead ja nende vältimine, integratsioon olemasolevatesse süsteemidesse, automatiseerimise võimalused ja piirangud, head praktikad.

## 2. Keel

- **Kogu projekti sisu on Eesti keeles.**
- Ka koodinäited ja tehnilised märksõnad esitatakse nii, et Eesti keelne selgitus on alati olemas.

## 3. Sihtpublik ja kirjastiil

Dokumendid peavad rahuldama **kahte lugejat korraga**:

| Lugeja | Mida ta peab saama |
|---|---|
| **Mitte-tehniline inimene** | Aru saama, mis toimub, mida projekt teeb ja mis eesmärgiga — ilma tehnilise slangita (või selgitatud slangiga) |
| **Tehniline inimene** | Õppima, kuidas oma süsteemi õigesti üles seadistada — saama konkreetseid juhiseid, näiteid ja samme |

**Kirjutamise reeglid:**
- Kasuta lihtsat, selget keelt; tehnilised mõisted selgita kohe lahti.
- Kui teema nõuab sügavust, tee vahet: „Mis see on ja miks?“ (kõigile) ja „Kuidas täpselt teha?“ (tehnilisele lugejale).
- Kasuta näiteid, samm-sammulisi juhiseid ja tabelit/konstruktsioone, mis muudavad teksti loetavaks.

### Stiilistandardid (kinnitatud)

- Jutumärgid: eesti stiil „...“ (mitte sirged jutumärgid).
- Liitsõnad AI-ga sidekriipsuga: AI-mudel, AI-agent, AI-automatiseerimine.
- Ingliskeelne mõiste selgitatakse esimesel kasutamisel: „termine (eestikeelne vaste)“.
- Failinimed ja kaustad: ainult tähestikulised tähed ja sidekriipsud, ilma täpitähtedeta.
- Igas dokumendis: sihtpubliku rida, „Lihtsalt öeldes“ kast, „Mis edasi?“ lingid ja jaluses „Viimati uuendatud“ kuupäev.

## 4. Dokumentide struktuur

- **`README.md` on projekti index-fail** — sisukord, mis lingib kõikidele teistele markdown-dokumentidele.
- Iga teema on eraldi markdown-fail; README.md hoida ajakohasena iga uue dokumendi lisamisel.
- Dokumendid peavad omavahel loogiliselt lingitud olema (nt „eelmine peatükk / järgmine peatükk“ või „vaata ka“).
- Failinimed: väiketähed, sidekriipsud (nt `mudelite-valik.md`).

## 5. Minu tööprotsess igas uues sessioonis

Kui kasutaja alustab uut vestlust ja annab sisendi, teen alati **esimesena** järgmist:

1. **Analüüsisin kasutaja sisendit** — mis teema, milline eesmärk, kellele suunatud.
2. **Kirjutan plaani**, kuidas see sisend õigesti dokumendiks vormistada:
   - kavandatava dokumendi pealkiri ja failinimi;
   - peatükkide struktuur;
   - kuidas see mahuks olemasolevasse struktuurisse (README.md uuendus vajadusel);
   - kas ja mis on ebaselge — küsin täpsustavat küsimust.
3. **Küsin kasutajalt kinnitust enne implementeerimist.** Dokumenti ei hakka kirjutama enne, kui kasutaja on plaaniga nõus.
4. Pärast kinnitust viin töö ellu (vt punkt 6) ja uuendan README.md index-faili.

## 6. Subagentide kasutamine (kohustuslik)

- **Kasutan absoluutselt igaks tööks subagentide abi. Minu roll on projektijuhi roll** — planeerin, jagame tööd, kontrollin tulemust ja suhtlen kasutajaga.
- **Subagendid teevad ära kogu tegeliku töö** — dokumendite kirjutamine, koodinäidete koostamine, ülevaatamine, uurimine.
- **Korraga võin välja kutsuda kuni 5 subagenti.**
- Tüüpiline tööjaotus näiteks:
  - uurimine / teabe kogumine → 1–2 subagenti;
  - dokumendi (peatükkide) kirjutamine → kuni 5 subagenti paralleelselt;
  - lõplik ülevaade ja kooskõlastus → 1 subagent (kontrollib keelt, struktuuri ja README.md linge).
- Enne subagentidele töö andmist kirjutan neile täpsed juhised: teema, sihtpublik, keel, stiil ja väljundi vorming.

### Paralleelne dokumendikirjutus (kinnitatud protsess)

- **Tempo: alustame katsega 2–3 dokumenti paralleelselt** ühes sessioonis. Kui kooskõla kvaliteet on hea, võib tõsta kuni 5 peale.
- **Enne paralleelset kirjutamist valmistan mina „briffing-paki“**, mille saab iga kirjutav subagent:
  1. dokumendi täpne sisuulatus: mis selgitatakse põhjalikult, mis mainitakse vaid lühidalt ja lingitakse edasi (vältimaks kattumist);
  2. ühine terminite nimekiri: kinnitatud eestikeelsed vasted ja selgitused (kõik subagendid kasutavad samu termineid);
  3. stiilistandardid (vt punkt 3 stiilistandardid) ja dokumendimall.
- **Pärast paralleelset kirjutamist alati 1–2 ülevaatavat subagenti**, kes kontrollivad dokumendide VAHELIST kooskõla (terminid, kattuvus, lingid, stiil) enne commitit.
- **Tase 1 dokumendid on iseseisvad** — sobivad hästi paralleelseks. Tasanditel 3–5 on dokumendid tihedamini seotud; seal kirjutada ettevaatlikumalt või järjestikku.

## 7. Sessiooni lõpp: commit ja push

- **Iga sessiooni lõpus tuleb tehtud töö commitida ja pushida GitHubi** (`origin/main`).
- Commit-sõnumid kirjutan eesti keeles (nt `Lisa: mudelite valiku dokument` või `Uuenda: README index`).
- Enne pushi kontrollin, et tööpuu on korras ja README.md viidad on ajakohased.
- Kui push ei õnnestu (nt puudub ligipääs), teavitan kasutajat kohe.

---

*Viimati uuendatud: 2026-10-05 — faili loomine.*
