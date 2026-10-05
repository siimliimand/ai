# 2.2 Struktureeritud väljund: tabelid, mallid ja JSON

> **Sihtpublik:** kõik | **Eeltingimused:** [2.1 Head promptini](01-hea-prompt.md)

## Mis sa sellest õpid

Pärast seda dokumenti oskad sa:

- selgitada, miks vabatekst sobib inimesele, aga mitte süsteemile, kes tulemust edasi töötleb;
- valida kolme vormi vahel — loend, tabel ja JSON — olenevalt sellest, kes vastust kasutab;
- kirjutada prompt (mudelile antav juhis) nii, et väljundi formaat on välja nõutud ja vastused jäävad ühesuguseks;
- parandada olukorda, kus AI-mudel formaadist kõrvale läheb — kordusreegli, tugevdatud näite ja automaatse kontrolliga.

## Lihtsalt öeldes

> Kui vastust loeb inimene, piisab hästi kirjutatud tekstist. Kui vastus peab aga edasi minna tabelisse, arvutusele või teisele programmile, peab igal andmejaam olema kindel nimi ja kindel koht — siis teab igaüks, kus hind ja kus pindala seisab. Sellist kindlas, masinloetavas vormis vastust nimetame struktureeritud väljundiks. See dokument õpetab sellise vastuse mudelilt välja küsima — ja mida teha, kui mudel reeglit ei pea.

## Miks vabatekst ei tööta süsteemis

Dokumendist [1.5](../01-alused/05-susteemi-anatoomia.md) tead, et AI-süsteemis on väljund sageli mõne teise osa sisend. Inimene jaksab lause seest hinna üles otsida; programm ei jaksa — tal peab iga info koht paigas olema. Kolm probleemi, mis vabatekstilisel vastusel süsteemis peaaegu alati tekivad:

1. **Info on maetud lausesse.** „Karlova korter südalinna lähedal, 58 m², hind 149 000 €, kohe müüa“ — kõik on olemas, aga iga väärtus peidus teistsuguses kohas. Programm, kes peaks hinnast ruutmeetrihinna arvutama, ei teada, kumb arv on hind ja kumb pindala.
2. **Iga kord teistsugune sõnastus.** Ühel päeval kirjutab mudel „kolm tuba“, järgmisel „2 tuba ja köök“, kolmandal „3-toaline“. Inimene mõistab kõiki kolme; tabelisse aga vajab süsteem sama kujuga väärtust, muidu veerud pole võrreldavad.
3. **Tellimata lisad.** Küsid JSON-i ja mudel vastab hea tahtmisega: „Muidugi! Siin on soovitud andmed:“ — ja alles siis tulevad ise andmed. Inimene naeratab; programm, kes JSON-i ootas, saab teksti, mis tema reeglite järgi vale on.

> **Lihtsalt öeldes:** vabatekst on inimese keel — me loeme ja mõistame. Süsteem vajab kohta-kirja: iga väli nimega ja oma kindlal kohal, iga kord ühtmoodi.

## Kolm vormi: loend, tabel, JSON

Kõik kolm on struktureeritud väljundid — vahe on selles, kelle jaoks nad mõeldud on.

| Vorm | Kelle jaoks | Millal sobib |
| --- | --- | --- |
| Loend | inimene | Lihtsad asjad: sammud, ülesannete nimekiri, kus iga rida on üks lühike asi |
| Tabel | inimene | Võrdlemine: mitu kirjet ja mitu tunnust, silm peab ridu läbi vaatama |
| JSON | masin | Vastus läheb edasi tabeliprogrammile või teisele süsteemile |

Loend piisab, kui igast kirjest vaja on üks lühike väli; kui aga vastuse peab üle võtma programm, on kindel valik JSON.

### JSON lihtsalt seletatuna

JSON (struktureeritud andmevorming — võti-väärtus paarid) näeb esmapilgul tehniline välja, aga põhimõte on argine: iga info on paar, kus üks pool ütleb, mis väli see on (võti), ja teine pool, mis selles seisab (väärtus). „hind: 149000“ ongi JSON-i idee täies ulatuses.

```json
{
  "aadress": "Pärnu mnt 12, Tartu",
  "hind_eur": 149000,
  "pindala_m2": 58,
  "tubade_arv": 3,
  "linn": "Tartu"
}
```

Loe seda nagu tabeli ühte rida: vasakul veeru pealkiri, paremal väärtus. Loogsulud `{` ja `}` hoiavad read kokku üheks tervikuks — üheks kirjeks (näiteks üks korter). Kui kirjeid on mitu, pannakse nad nurksulude `[` ja `]` vahele nimekirja. Sulud kannavad struktuuri: isegi kui sina JSON-i programmeerida ei oska, teab iga tabeli- või arvutusprogramm, et „hind_eur“ on alati samal kohal ja tähendab sama. Sellepärast on JSON masinale edastamise vormiks — süsteemi piiri ületava väljundi tehnilist poolt selgitab [3.1](../03-susteemi-ulesehitus/01-api-integratsioonid.md).

> **Lihtsalt öeldes:** JSON on nagu vorm, mille veerud on sõnadega üles kirjutatud. Võti on veeru pealkiri, väärtus on ruutu kirjutatud vastus, sulud hoiavad read ühe kirje kokku.

## Kuidas formaati promptis nõuda

Hea formaadinõue koosneb neljast osast:

1. **Täpne kirjeldus.** Nimeta iga väli ja ütle, mis kujuga väärtus peab olema: „hind_eur täisarvuna eurodes, nt 149000“, „tubade_arv täisarvuna; köök ei loe tubadeks“.
2. **Üks täielik näide oodatud väljundist.** Näita ära kogu vastus, mida ootad — mitte „midagi taolist“, vaid päris kirje päris väljadega. Mudel jäljendab näidatud mustrit palju kindlamalt kui pikka kirjeldust (nii soovitas 1.3 viienda osana).
3. **Reeglid puuduvate andmete jaoks.** Lisa alati: „Kui mõni andmejaam allikas puudub, kirjuta selle väärtuseks tühi — ÄRA leiuta.“ Ilma selleta täidab mudel lüngad äraarvamisega ja tee hallutsinatsioonini (mudeli kindlalt öeldud, aga vale vastus) on lühike.
4. **Mitmeseisundi reeglid.** Ütle välja, mis peab juhtuma eri olukordades: „Vasta AINULT JSON-iga, ilma sissejuhatava ja lõpetava tekstita“ ning „kui ülesanne osutub lahendamatuks, vasta kujul `{"viga": "põhjus"}`“.

Ja veel: pane kirja aktsepteerimiskriteerium — tingimus, mille täitmisel vastus läbi pääseb, näiteks „kõik 30 objekti esindatud, igas kirjes täpselt viis välja, ükski väärtus pole mudeli enda lisatud“. See muudab testimise võimalikuks — kuidas prompti testimise tsüklis arendada, on teema dokumendis [2.1](01-hea-prompt.md).

> **Lihtsalt öeldes:** nõua formaati nagu pank dokumendilt: mis leht, mis väljad, mis teha, kui midagi puudub — ja ühtegi lisalehte kaasa ära pane.

## Mida teha, kui mudel formaadist kõrvale läheb

Isegi korraliku promptiga juhtub: üks kirje kolmekümnest jääb pooleli või mudel lisab viisakusfraasi. Kolm sammu, tähtsuse järjekorras:

1. **Kordusreegel.** Kõige sagedasem põhjus on, et formaadinõue seisab prompti keskel ja läheb tähelepanu alt ära (vt 1.3 kolmandat sagedamat viga). Pane nõue algusesse ja korda kriitilist osa lõpus: „Meenutus: ainult JSON, ilma muu tekstita.“
2. **Tugevda näidet.** Kui mudel ikka lisab sissejuhatusi, näita näites ära ka kontrast: „mitte nii: „Siin on andmed! { … }“, vaid täpselt nii: { … }“. Hea ja halva näide kõrvuti õpetab kiiremini kui järelepaldumine.
3. **Kontroll automaatse reegliga.** Kui vastust töötleb programm, lase programmil enne kasutamist lihtne reegel üle kontrollida: kas vastus on üldse JSON ja kas kõik vajalikud väljad on olemas? Kui ei — küsi mudelilt parandust või saada kirje inimesele. Kuidas selline kontroll ja parandussükkel suuremas süsteemis üles ehitada, selgitab [3.4 Vead ja veakäsitlus](../03-susteemi-ulesehitus/04-vead-ja-veakasitlus.md).

Kuni süsteem pole oma töökindlust tõestanud, jääb lõplik heakskiit inimesele — nii hoiame inimese kinnitusahelas (ingl k *human-in-the-loop*): masin teeb, inimene kiidab heaks.

## Näide samm-sammult: Annika kuulutuste võrdlustabel

Annika on kinnisvaramaakler. Tal on 30 objekti kirjeldust vabatekstina („Karlova korter südalinna lähedal, 58 m², kaks tuba ja köök, hind 149 000 € …“) ja ta tahab neist kliendile võrdlustabelit.

**1. samm — vaba küsimus, tulemus kasutuskõlbmatu.**

```text
Siin on 30 kinnisvarakuulutust. Tee neist võrdlus.
```

Mudel kirjutab pika ja ladusa kokkuvõtte: mõne hinna mainib, mõne jätab välja, järjekord iga käivitusega erinev, tubade arv mõnikord sõnadega, mõnikord numbriga. Inimesele meeldib lugeda; tabelisse kleebida ei saa.

**2. samm — formaadi nõue.**

```text
Loe allpool olevad 30 kinnisvarakuulutust ja esita iga objekti kohta
üks kirje järgmisse JSON-i vormingusse (üks nimekiri, 30 kirjet):

[
  {
    "aadress": "Pärnu mnt 12, Tartu",
    "hind_eur": 149000,
    "pindala_m2": 58,
    "tubade_arv": 3,
    "linn": "Tartu"
  }
]

Reeglid:
- hind_eur: täisarv eurodes, ainult kuulutuses kirjas olev hind
- tubade_arv: täisarv; köök ja vannituba ei loe tubadeks
- Kirjeid peab olema täpselt 30, kuulutuste järjekorras.
- Vasta AINULT JSON-i nimekirjaga, ilma sissejuhatava ja
  lõpetava tekstita.
```

Nüüd on kõik 30 kirjet ühesuguse ehitusega — veerud langevad tabeliprogrammis kohale.

**3. samm — puuduvate andmete reegel.**

Üks kuulutus ei maini pindalat. Esimesel käivitusel seisis tulemuses `"pindala_m2": 55` — mudel „aitas“ ja arvas pindala tubade arvu järgi ära. See ongi hallutsinatsioon: kindel number, mida üheski allikas pole. Annika lisab reeglite hulka ühe rea:

```text
- Kui mõni andmejaam kuulutuses puudub, kirjuta selle väärtuseks ""
  (tühi). ÄRA arva ära ja ÄRA täida üldteadmistest.
```

Uus käivitus annab selle objekti jaoks `"pindala_m2": ""` — ja Annika näeb täpselt, millise kirje juurde peab ta ise maakleri andmetest tagasi minema. Tühi väärtus on aus; äraarvatud number tekitaks hiljem valesid järeldusi.

**4. samm — lõpptulemus tabelis.**

Kuna kõik väljad on alati olemas ja samas järjekorras, läheb JSON-i ümbervormimine tabeliks lihtsalt — Annika kleebib vastuse tabeliprogrammi:

| aadress | hind_eur | pindala_m2 | tubade_arv | linn |
| --- | ---: | ---: | ---: | --- |
| Pärnu mnt 12, Tartu | 149 000 | 58 | 3 | Tartu |
| Kaubamaja tn 4, Tartu | 210 000 | — | 4 | Tartu |
| Rannahoone tn 8, Pärnu | 310 000 | 96 | 4 | Pärnu |

Tühja väärtuse kujutamiseks tabelis kasutame märget „—“.

Teine rida näitab täpselt seda, mida me tahtsime: puuduv andmejaam on nähtavalt tühi, mitte vaikselt ära arvatud. Kui Annika tahab sammule juurde ehitada — võrdlus tehtud, järgmine samm koostab sellest kliendikirja — on järgmine tase workflow: kuidas struktureeritud väljund saab ühe sammu väljundist järgmise sammu sisendi, sellest [2.3](03-workflow-algtasandil.md).

## Kokkuvõte

- **Vabatekst on inimesele, struktuur süsteemile.** Info maetuna lausesse, iga kord erinev sõnastus ja tellimata lisad teevad vabatekstilisest vastusest süsteemis kasutamatu.
- **Kolm vormi:** loend lihtsa nimekirja jaoks, tabel inimese võrdlemiseks, JSON masinale edastamiseks. JSON on võti-väärtus paaride kogu, mille struktuuri hoiavad sulud.
- **Formaat nõutakse välja neljaga:** täpne kirjeldus, üks täielik näide, puuduvate andmete reeglid ja mitmeseisundi reeglid. Aktsepteerimiskriteerium ütleb, millal vastus läbi pääseb.
- **Kõrvalekalle pole katastroof:** korda reeglit, tugevda näidet ja lase automaatse reegliga kontrollida.

## Mis edasi?

- eelmine → [2.1 Head promptini](01-hea-prompt.md)
- järgmine → [2.3 Workflow algtasandil: sammud ja tingimused](03-workflow-algtasandil.md)
- Masinloetavuse tehniline külg → [3.1 API integratsioonid](../03-susteemi-ulesehitus/01-api-integratsioonid.md)
- tagasi → [README index](../README.md)

---
*Viimati uuendatud: 2026-10-05*
