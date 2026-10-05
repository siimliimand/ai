# 2.5 Esimene automatiseeritud workflow otsast lõpuni

> **Sihtpublik:** tehniline + süvenev mitte-tehniline | **Eeltingimused:** [2.3 Workflow algtasandil](03-workflow-algtasandil.md), [2.4 Sisendid ja andmete ettevalmistamine](04-sisendid-ja-andmed.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- panna workflow (töövoog — automatiseeritud sammude jada) eesmärk ühele lehele ja kirja panna, mida süsteem EI tee;
- kaardistada süsteem 1.5 anatoomia seitsme osaga enne esimest klikki;
- ehitada voo samm (ingl *step*) haaval no-code tööriistaga ja põhjendada iga valikut;
- käia valmis vool läbi testimise tsükli ja jälgida esimest nädalat kolme numbriga.

## Lihtsalt öeldes

> 2.1–2.4 andsid osad: promptimall, struktureeritud väljund, sammud ja tingimused, puhastatud sisend. Nüüd paneme need üheks masinaks: ehitame Kodutoa poes (käsitöökaup) tagastustaotluste voo otsast lõpuni — juhend on piisavalt täpne, et selle järgi sama süsteemi ehitada.

## Samm 0: eesmärk ja mahuvalik

Enne ehitamist pane eesmärk ühele lehele — see on tellija otsus (1.6). **Maht** on teada: umbes 40 kliendikirja päevas, igaüks 5 minutit — rohkem kui kolm tundi rutiini (40 × 5 min = 3 h 20 min; 1.4 arvutus).

**Süsteem teeb:** liigitab iga uue kliendikirja („tagastus“, „info“ või „muu“), võtab tagastusel ja infol tellimuse andmed e-poe süsteemist, koostab vastuse kavandi ja paneb selle poe klienditeenija Pireti kinnituseks.

**Süsteem EI tee:** ei saada kliendile midagi ilma Pireti kinnituseta; ei käsitle kaebusi ega juriidilisi nõudeid; ei otsusta raha asjus.

**Mida jätsime välja ja miks.** Kaebused jäävad inimesele — 1.2 järgi on kliendisuhte olulised hetked inimese töö: vale toon maksab rohkem, kui automatiseerimine kokku hoidis. Ka iseseisev saatmine jäeti välja: esimene versioon jääb inimese kinnitusahelasse (ingl *human-in-the-loop* — inimene kinnitab tulemuse enne kasutust); kergem kontroll tuleb ainult näitajate peale (2.3 paigutustabel). Kolm väärtust („tagastus“, „info“, „muu“) on siin teadlikult karskem kui 2.3 viiesammasuline näide — kaebus ja „arusaamatu“ teevad mõlemad tihti sama asja: viivad kirja inimesele. Ja esimeses versioonis saab iga väljaminev kiri inimese kinnituse, isegi info — usalduse kasvades saab info-haru avada (vt 2.3 paigutustabelit).

> **Lihtsalt öeldes:** eesmärk mahub ühte lausesse: „süsteem loeb kirja, koostab vastuse ja Piret vajutab nuppu.“ Kõik, mis lausesse ei mahu, jääb välja — väljajäetute nimekiri on sama tähtis kui eesmärk: ilma selleta kasvab projekt käigus.

## Süsteemi osade nimekiri (anatoomia tabel)

1.5 anatoomia on ehitusplaan: iga ehitussamm täidab ühe rea.

| Anatoomia osa | Mis selles süsteemis | Kus teeme |
|---|---|---|
| 1. käivitaja (ingl *trigger*) | uus e-kiri poe aadressile | ehituse samm 1 |
| 2. sisendandmed | kirja tekst + tellimuse andmed, märgenditega vormis | sammud 2 ja 5 |
| 3. juhis | kaks promptimalli: liigitaja ja kavandaja | sammud 3 ja 5 |
| 4. AI-mudel | liigitab kirja ja koostab kavandi | sammud 3 ja 5 |
| 5. kontrollpunkt | tingimus „muu → inimesele“ + kinnituse samm | sammud 4 ja 6 |
| 6. tegevus / väljund | kinnitatud vastuse saatmine | samm 7 |
| 7. andmete hoidmine | kirje igast kirjast: liik, otsus, kellaaeg | samm 7 |

## Ehitus samm-sammult

Ava no-code tööriist (visuaalne keskkond, kus süsteemi kokku klõpsitakse) — n8n, Make või Zapier — ja ehita seitse plokki. Tehnilisele lugejale: kui vool tuleb hiljem käivitada oma süsteemi seest, on järgmine tase API — vt [3.1](../03-susteemi-ulesehitus/01-api-integratsioonid.md).

**1. Käivitaja (ingl *trigger*): uus kiri e-posti.** Seadista vool elustuma, kui poe teeninduse aadressile saabub uus kiri. *Miks nii:* käivitaja teeb voo iseseisvaks — süsteem ärkab iga kirja juures samamoodi, Piret ei kopeeri midagi käsitsi.

**2. Sisendi ettevalmistus: kiri märgenditega vormi.** Pane kirja tekst [KLIENDI_KIRI] märgendi (selgelt eraldatud sisendi osa) alla — nagu 2.4 õpetas. *Miks nii:* formaatimata vabatekst on müra; märgendiga vorm on iga kirja juures identne.

**3. Liigitaja prompt (mudelile antav juhis) JSON-iga.** Loo samm, mis kutsub AI-mudeli selle promptimalliga:

```text
Sa oled e-poe kliendikirjade liigitaja. Sinu ainus ülesanne on kiri liigitada.

Määra kirja põhjal:
- liik: „tagastus“ (klient soovib toodet tagasi saata), „info“ (muud küsimused)
  või „muu“ (kaebus, juriidiline nõue, arusaamatu kiri)
- toode: kirjas olev tootenimi või „puudub“
- tellimuse_nr: kirjas olev tellimuse number või „puudub“
- põhjus: üks lühike lause

Reeglid:
- Kasuta ainult kirjas olevaid andmeid. Kui andmeid pole, kirjuta „puudub“ — ära leiuta.
- Vasta AINULT JSON-iga, ilma sissejuhatava ja lõpetava tekstita.

KLIENDI_KIRI:
[KLIENDI_KIRI]
```

Tulemus on struktureeritud väljund:

```json
{
  "liik": "tagastus",
  "toode": "villane sall",
  "tellimuse_nr": "1187",
  "põhjus": "sall jäi liiga kitsaks"
}
```

*Miks nii:* workflow oskab JSON-i välju edasi lugeda — „liik“ läheb tingimusse, „tellimuse_nr“ otsingusse. Reegel „kui andmeid pole, ära leiuta“ sulgeb hallutsinatsiooni (mudeli kindlalt öeldud, aga vale vastus) tee: tühi väli on nähtav, leiutatud number pole. „muu“ täidab siin „teadmata“ rolli — ka see teed viib inimesele.

**4. Tingimus (ingl *condition*): „muu“ → inimesele.** Tingimus loeb välja „liik“: kui väärtus on „muu“ — või kui automaatne reegel („kas vastus on JSON ja kõik väljad olemas?“, 2.2-st) välju ei leia —, suuna kiri Pireti ülevaatuse loendisse ja lõpeta voo. *Miks nii:* varutee on disaini osa, mitte ebaõnnestumine (1.5) — ebakindel hinnang ei pea ise otsustama. Tagastuse korral: kui „tellimuse_nr“ on „puudub“ või tellimust ei leita, suunatakse kiri Pireti ülevaatuse loendisse.

**5. Kavandaja promptimall tellimuse andmetega.** Liikidel „tagastus“ ja „info“ otsib vool tellimuse andmed number alusel ja paneb need kavandaja malli kohatäitesse [NIMI_SUURTÄHTEDEGA] (nurksulgudes ja suurtähtedes nimetatud koht, mida iga kiri täidab):

```text
Sa oled Kodutoa poe klienditeenindaja — asjalik ja sõbralik.

Koosta vastuskiri kliendi kirja põhjal.

Reeglid (poe tingimused):
- tagastusaeg 14 päeva ostust, tagastuskulu 5 eurot, raha tagasi 3 tööpäevaga
- kasuta ainult allpool olevaid andmeid; kui andmed puuduvad, ütle,
  et küsid kolleegidelt — ära paku midagi muud

Vorming: e-kiri, kuni 80 sõna, eesti keeles.

KLIENDI_KIRI:
[KLIENDI_KIRI]

TELLIMUSE ANDMED:
- number: [TELLIMUSE_NR]
- ostu kuupäev: [OSTU_KUUPAEV]
- toode: [TOODE]
- summa: [SUMMA]
```

*Miks nii:* kinnised osad (roll, reeglid, vorming) on 2.1 testimise tsükli läbinud püsisõnastus, mida voo käigus ei muudeta; muutuvad osad täituvad baasi faktidega — kavandisse ei saa numbrit, mida süsteemis pole.

**6. Kinnituse samm: Piret näeb kolme asja kõrvuti.** Vool ootab, kuni Piret on läbi lugenud põhikirja, kavandi ja tellimuse andmed ning kinnitanud (või parandanud ja kinnitanud). *Miks nii:* kavand on kliendile lubadus; kontrollpunkt enne väljundit tabab vea seal, kus see maksab minuti, mitte usaldust (1.5 viies osa).

**7. Saatmine ja kirje ajalukku.** Pärast kinnitust saadab vool vastuse ja kirjutab ajalukku: liik, otsus (saadetud / inimesele), kas kavandit muudeti, kellaaeg. *Miks nii:* ajaloota on voo käitumine arvamus — kirjetest näed, mis töötab, ja saad öelda, kes mis kinnitas (1.6).

## Testimine enne elustamist

Enne käikuandmist käi 2.1 testimise tsükkel läbi kogu voo: sisesta testkirjad käsitsi ja jälgi, kuhu vool neid viib. Aktsepteerimiskriteerium (kokkulepitud nõue, mille täitmisel tulemus läbi pääseb): iga kiri jõuab õigesse kohta ja kavandis pole fakti, mida andmetest poleks tulnud. Kasuta päris vanu kirju (andmed anonümiseeritult):

| Testjuhtum | Oodatav käitumine | Läbis |
|---|---|---|
| Tüüpiline tagastus (toode ja nr) | liik „tagastus“, väljad täituvad, kavand päris andmetega, kinnitus muutmata | ✓ |
| Tagastus ilma tellimuse numbrita | „tellimuse_nr“ on „puudub“ — süsteem ei leiuta, kiri Pireti ülevaatusele | ✓ |
| Info küsimus, ekslikult poe aadressile | liik „info“, lihtne kavand, Piret kinnitab | ✓ |
| Poolik kiri („toode ei meeldinud“) | liik määratakse, puuduvad väljad „puudub“; ilma numbrita Pireti kätte | ✓ |
| Krooniliselt vale tootenimi („sinine müts“) | kavandis on baasist tulnud õige tootenimi, mitte kliendi vale nimetus; inimene näeb kõrvutust ja parandab | ✓ |
| Pahane kaebus | liik „muu“ → otse Piretile, midagi ei koostata ega saadeta | ✓ |
| Liigitaja rikkus JSON-i (lisas tutvustuse) | automaatne reegel ei leia välju → kiri Pireti loendisse | ✓ |

> **Lihtsalt öeldes:** testid on proovisõit tühja autoga. Kui mõni juhtum ei läbi, tee üks muudatus, uus versioon ja uus ring — mitte mitut muudatust korraga, muidu ei tea, kumb aitas (2.1). Elusta alles, kui kõik olulised read on ✓: eksiv liigitaja eksib tootmises 40 korda päevas.

## Esimene nädal tootmises

Pärast elustamist loe sammu 7 kirjeid kolmeks numbriks:

1. **Mitu kirja päevas läbis vool ja mitu pöördus inimesele.** Varutee kasutamine pole viga — järsk hüpe aga jah: midagi muutus sisendis.
2. **Mitu kavandit Piret muutis enne saatmist.** Kvaliteedi peamine märk: palju muudatusi tähendab, et mõni kinnine osa vajab tööd; null viitab, et kontrolli võib hiljem kergendada (2.3 valimiskontroll).
3. **Veatüübid — kus eksitus juhtus.** Liigitamisel, andmetel või toonis? Iga viga on 1.5 järgi aadressiga: paranda õiget osa, mitte „AI-i üldse“.

Kui päev lüheneb tundide võrra ja muudatusi on vähe, on laiendamine (nt kaebuste ettevalmistus Piretile) järgmine projekt. Kulude esimene pilk: pane kirja, kui palju nädal mudelikutseid kulutas — põhjalikumalt [3.6](../03-susteemi-ulesehitus/06-kulude-haldamine.md). Kui vead korduvad, on aeg veakäsitluse ([3.4](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md)) ja monitooringu ([4.6](../04-agendid-ja-mootmine/06-monitooring.md)) järele — mõlemad on järgmiste tasandite teemad; siin piisab kolmest numbrist.

> **Lihtsalt öeldes:** esimene nädal on mõõtmise nädal: kolm arvu ütlevad, kas masin teeb Pireti päeva lühemaks ja kus ta eksib.

## Kokkuvõte

- **Eesmärk ühele lehele, väljajäetud asjad samuti:** tagastus ja info saavad kavandi, kõik muu pöördub inimesele, Piret kinnitab kõik enne saatmist.
- **1.5 anatoomia on ehitusplaan:** seitse osa = seitse ehitussammu; sammu valmides on süsteemi osa täidetud.
- **Kolm reeglit teevad masina usaldusväärseks:** faktid tulevad andmetest (JSON ja „puudub“), otsused seisavad tingimustes, lubadused läbivad inimese kinnitusahela.
- **Testimise tsükkel enne elustamist, kolm numbrit pärast seda** — otsused põhinevad näitajatel.

## Mis edasi?

- eelmine → [2.4 Sisendid ja andmete ettevalmistamine](04-sisendid-ja-andmed.md)
- järgmine → [2.6 Promptide haldus kui vara](06-promptide-haldus.md)
- Tehniline tase → [3.1 API integratsioonid](../03-susteemi-ulesehitus/01-api-integratsioonid.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
