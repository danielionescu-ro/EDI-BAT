# EDI-BAT - Instructiuni de dezvoltare

> In acest document, `BAT` inseamna **British American Tobacco**. Denumirea `EDI-BAT` identifica aplicatia si proiectul pentru BAT; nu exista si nu se va utiliza niciun fisier cu extensia `.bat`.

> Completeaza sectiunile de mai jos cu cerinte concrete. Nu este necesar sa pastrezi textul explicativ dintre paranteze; acesta are rolul de a ghida definirea aplicatiei.

## 1. Scopul aplicatiei

Descrierea pe scurt a rezultatului dorit:

- Preluare informatii din forma principala `PORDERS` din ERP-ul Priority.
- Creare fisiere XML conform unei structuri definite.
- Salvare fisiere XML intr-un folder configurabil.
- Preluare fisiere XML dintr-un folder configurabil.
- Procesare si conversie a informatiilor din fisierele XML.
- Trimitere de instructiuni prin API catre ERP-ul Priority.
- Creare NIR in Priority pe baza comenzii.

## 2. Terminologie si obiecte de business

Completeaza definitiile pentru termenii folositi:

| Termen | Definitie / observatii |
|---|---|
| `PORDERS` |  |
| `BAT` | British American Tobacco, beneficiarul / organizatia pentru care se dezvolta aplicatia. |
| Comanda |  |
| NIR | Nota de intrare receptie; precizeaza forma, statusurile si regulile. |
| EDI |  |
| API Priority |  |
| Fisier de intrare |  |
| Fisier de iesire |  |

## 3. Fluxul general al aplicatiei

Descrie ordinea exacta a etapelor si ce se intampla la succes sau eroare.

### 3.1 Export din Priority

1. Cum se identifica inregistrarile din forma `PORDERS`?
2. Ce filtre se aplica?
3. Se prelucreaza o singura comanda sau un lot?
4. Ce campuri se citesc din Priority?
5. Cum se marcheaza o comanda deja exportata?
6. Ce se intampla daca exportul esueaza?

### 3.2 Crearea si salvarea XML

1. Structura XML obligatorie:

```xml
<!-- Lipeste aici structura XML completa sau un exemplu valid. -->
```

2. Namespace-uri obligatorii:
3. Encoding-ul fisierului:
4. Reguli de denumire a fisierului:
5. Folderul de salvare:
6. Se scrie intai intr-un fisier temporar?
7. Cand este considerat fisierul finalizat?
8. Se pastreaza o arhiva?

### 3.3 Preluarea fisierelor XML

1. Folderul de intrare:
2. Masca fisierelor acceptate, de exemplu `*.xml`:
3. Ordinea de procesare:
4. Se proceseaza recursiv subfolderele?
5. Cum se evita procesarea dubla?
6. Ce se intampla cu fisierele invalide?
7. Folder pentru fisiere procesate:
8. Folder pentru fisiere cu eroare:

### 3.4 Procesarea si conversia XML

1. Validarea XML se face pe baza unui XSD sau prin reguli interne?
2. Campuri obligatorii:
3. Campuri optionale:
4. Reguli de conversie:
5. Conversii de tip numeric, data si moneda:
6. Reguli pentru cantitati, preturi si TVA:
7. Reguli pentru linii lipsa sau duplicate:
8. Ce date trebuie pastrate pentru trasabilitate?

### 3.5 Crearea NIR in Priority

1. Cum se identifica comanda de baza?
2. Endpoint-ul API:
3. Metoda HTTP: `GET`, `POST`, `PUT`, `PATCH`:
4. Structura request-ului:

```json
{
  "completeaza_aici": "payload-ul asteptat de Priority"
}
```

5. Structura raspunsului asteptat:
6. Campul care confirma crearea NIR:
7. Cum se trateaza raspunsurile `4xx` si `5xx`?
8. Cum se trateaza timeout-ul si retry-ul?
9. Cum se verifica daca NIR-ul exista deja?
10. Cand fisierul XML este mutat in folderul de procesate?

## 4. Configurare in fisiere XML

Toate valorile care difera intre medii sau clienti trebuie sa fie configurabile, nu hardcodate in cod.

### 4.1 Fisierul principal de configurare

Nume propus: `EDI-BAT.config.xml`

Exemplu de structura initiala:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ediBat>
  <application>
    <name>EDI-BAT</name>
    <environment>development</environment>
    <logLevel>INFO</logLevel>
    <dryRun>true</dryRun>
  </application>

  <folders>
    <exportFolder></exportFolder>
    <inputFolder></inputFolder>
    <processedFolder></processedFolder>
    <errorFolder></errorFolder>
    <archiveFolder></archiveFolder>
    <logFolder></logFolder>
  </folders>

  <files>
    <inputMask>*.xml</inputMask>
    <encoding>utf-8</encoding>
    <temporaryExtension>.tmp</temporaryExtension>
    <archiveEnabled>true</archiveEnabled>
  </files>

  <priority>
    <baseUrl></baseUrl>
    <company></company>
    <username></username>
    <password></password>
    <timeoutSeconds>60</timeoutSeconds>
    <retryCount>3</retryCount>
    <retryDelaySeconds>5</retryDelaySeconds>
    <verifyTls>true</verifyTls>
    <endpoints>
      <orders></orders>
      <goodsReceipts></goodsReceipts>
    </endpoints>
  </priority>

  <processing>
    <batchSize>1</batchSize>
    <pollIntervalSeconds>10</pollIntervalSeconds>
    <stopOnError>false</stopOnError>
    <moveInputAfterSuccess>true</moveInputAfterSuccess>
  </processing>

  <mapping>
    <!-- Completeaza regulile de mapare XML -> Priority. -->
  </mapping>
</ediBat>
```

### 4.2 Reguli de configurare

- Credentialele nu se salveaza in Git si nu se pun in fisiere de configuratie distribuite.
- Stabileste daca credentialele vin din variabile de mediu, Windows Credential Manager sau alt mecanism.
- Cai locale si endpoint-urile trebuie sa fie configurabile separat pentru dezvoltare, test si productie.
- Valorile obligatorii si valorile implicite trebuie documentate.
- La pornire, aplicatia trebuie sa valideze configuratia si sa afiseze erori clare.
- Nu se accepta continuarea procesarii daca lipsesc setari critice.

## 5. Maparea campurilor

Completeaza tabelele cu numele exacte ale campurilor.

### 5.1 Priority `PORDERS` -> XML

| Camp Priority | Element XML | Tip | Obligatoriu | Regula / observatii |
|---|---|---|---|---|
|  |  |  |  |  |

### 5.2 XML -> request Priority pentru NIR

| Element XML | Camp request Priority | Tip | Obligatoriu | Regula / observatii |
|---|---|---|---|---|
|  |  |  |  |  |

### 5.3 Reguli pentru linii de comanda

- Identificator linie:
- Articol:
- Cantitate comandata:
- Cantitate receptionata:
- Unitate de masura:
- Pret:
- TVA:
- Lot / serie:
- Data expirarii:
- Depozit / gestiune:
- Diferente intre comandat si receptionat:

## 6. Integrarea API Priority

### 6.1 Autentificare

- Tip autentificare:
- URL autentificare, daca exista:
- Durata tokenului:
- Reinnoire token:
- Header-e obligatorii:
- Tratamentul credentialelor:

### 6.2 Cereri si raspunsuri

Documenteaza pentru fiecare endpoint:

| Operatie | Metoda | Endpoint | Request | Raspuns | Conditie de succes |
|---|---|---|---|---|---|
| Citire comenzi |  |  |  |  |  |
| Creare NIR |  |  |  |  |  |
| Verificare NIR existent |  |  |  |  |  |
| Actualizare status |  |  |  |  |  |

### 6.3 Retry, idempotenta si erori

- Coduri HTTP care permit retry:
- Coduri HTTP care opresc procesarea:
- Numar maxim de incercari:
- Backoff intre incercari:
- Cheie de idempotenta:
- Cum se evita crearea aceluiasi NIR de doua ori?
- Ce se logheaza fara a expune date sensibile?

## 7. Stari si trasabilitate

Defineste starile documentelor si tranzitiile permise:

```text
NOU -> VALIDAT -> EXPORTAT -> PROCESAT -> NIR_CREAT
NOU -> EROARE_VALIDARE
EXPORTAT -> EROARE_TRANSMITERE
PROCESAT -> EROARE_NIR
```

Pentru fiecare fisier si operatie trebuie sa poata fi identificata:

- comanda sursa;
- numele fisierului XML;
- data si ora operatiei;
- rezultatul operatiei;
- identificatorul NIR din Priority;
- mesajul de eroare, daca exista;
- numarul incercarii.

## 8. Jurnalizare si monitorizare

- Niveluri de log: `DEBUG`, `INFO`, `WARNING`, `ERROR`.
- Folder si format log:
- Rotirea fisierelor de log:
- Durata de pastrare:
- Se scrie in consola?
- Se trimit notificari la eroare?
- Ce informatii sunt interzise in log, de exemplu parole si token-uri?

Exemple de evenimente care trebuie jurnalizate:

- pornirea si oprirea aplicatiei;
- incarcarea configuratiei;
- numarul de fisiere gasite;
- validarea unui XML;
- request-ul catre Priority, fara credentiale;
- raspunsul Priority, cu date sensibile mascate;
- mutarea fisierului in `processed` sau `error`;
- rezultatul crearii NIR.

## 9. Interfata de rulare

Alege si documenteaza modurile de rulare:

- [ ] linie de comanda;
- [ ] serviciu Windows;
- [ ] aplicatie GUI;
- [ ] rulare programata;
- [ ] alta varianta: __________.

Parametri propusi:

```text
EDI-BAT.exe --config EDI-BAT.config.xml
EDI-BAT.exe --once
EDI-BAT.exe --dry-run
EDI-BAT.exe --validate-config
EDI-BAT.exe --file <cale-fisier.xml>
```

Completeaza comportamentul fiecarui parametru si codurile de iesire.

## 10. Structura tehnica propusa

Structura poate fi ajustata dupa definirea cerintelor:

```text
EDI-BAT/
  .venv/
  config/
    EDI-BAT.config.example.xml
  src/
    main.py
    config_loader.py
    priority_client.py
    porders_reader.py
    xml_builder.py
    xml_processor.py
    mapping.py
    workflow.py
    models.py
    logging_setup.py
  tests/
    fixtures/
    test_config_loader.py
    test_xml_builder.py
    test_xml_processor.py
    test_priority_client.py
    test_workflow.py
  data/
    input/
    processed/
    error/
    archive/
    output/
  logs/
  requirements.txt
  README.md
  INSTRUCTIUNI_DEZVOLTARE.md
```

## 11. Testare si criterii de acceptanta

### 11.1 Teste functionale

- [ ] Se incarca configuratia XML valida.
- [ ] Se respinge configuratia incompleta cu mesaj clar.
- [ ] Se citesc corect datele din `PORDERS`.
- [ ] Se creeaza XML conform structurii aprobate.
- [ ] XML-ul se salveaza atomic in folderul configurat.
- [ ] Se ignora sau se raporteaza corect un XML invalid.
- [ ] Se face conversia corecta a campurilor.
- [ ] Se creeaza NIR-ul in Priority.
- [ ] Se evita dublarea NIR-ului la retry.
- [ ] Fisierele sunt mutate in folderul corect.
- [ ] Erorile sunt jurnalizate si pot fi diagnosticate.

### 11.2 Cazuri limita

- Folder lipsa sau fara drepturi de scriere.
- XML gol, trunchiat sau cu encoding gresit.
- Camp obligatoriu lipsa.
- Cantitate zero sau negativa.
- Comanda inexistenta in Priority.
- NIR deja creat.
- API indisponibil sau timeout.
- Raspuns API invalid.
- Doua procese care incearca sa prelucreze acelasi fisier.
- Fisier foarte mare sau lot cu multe linii.

## 12. Securitate

- Credentialele si token-urile nu apar in cod, loguri sau exemple reale.
- Comunicatia cu Priority foloseste HTTPS, daca API-ul permite.
- Se valideaza certificatele TLS in productie.
- Se limiteaza drepturile contului folosit de aplicatie.
- Se valideaza caile configurate pentru a evita scrierea in directoare nepermise.
- Se trateaza XML-ul ca date neconforme pana la validare.
- Se documenteaza politica de pastrare si stergere a fisierelor.

## 13. Dependențe si mediu de executie

- Versiune Python:
- Pachete Python necesare:
- Versiune Priority / API:
- Sistem de operare:
- Rulare din `.venv`:
- Comanda de instalare dependente:
- Comanda de rulare locala:
- Comanda de testare:
- Comanda de construire executabil, daca este necesar:

## 14. Date de test

Descrie sau ataseaza date de test anonimizate:

- Exemplu de raspuns / export `PORDERS`:
- Exemplu XML valid:
- Exemplu XML invalid:
- Exemplu payload pentru creare NIR:
- Exemplu raspuns Priority de succes:
- Exemplu raspuns Priority cu eroare:

## 15. Ordinea implementarii

Completeaza prioritatea si observațiile:

1. Definirea structurii XML si a regulilor de mapare.
2. Definirea configuratiei XML si validarea ei.
3. Implementarea citirii si scrierii fisierelor.
4. Implementarea cititorului `PORDERS`.
5. Implementarea conversiei XML.
6. Implementarea clientului API Priority.
7. Implementarea fluxului complet si a starilor.
8. Adaugarea jurnalizarii si a mecanismului de retry.
9. Teste functionale si teste pentru cazuri limita.
10. Documentatie, instalare si construire executabil.

## 16. Decizii deschise

Noteaza aici intrebarile care trebuie clarificate:

- [ ] Care este structura exacta a formei `PORDERS`?
- [ ] Cum se acceseaza datele din Priority: API, export, ODBC sau alta metoda?
- [ ] Care este structura XML finala obligatorie?
- [ ] Care este endpoint-ul si contractul API pentru creare NIR?
- [ ] Cum se identifica unic o comanda si un NIR?
- [ ] Ce directoare sunt folosite in dezvoltare, test si productie?
- [ ] Aplicatia ruleaza o data, continuu sau ca serviciu Windows?
- [ ] Ce se intampla cu documentele procesate cu succes?
- [ ] Este necesar un executabil `.exe`?
- [ ] Care sunt regulile de autentificare si securitate?

## 17. Istoric modificari instructiuni

| Data | Autor | Modificare |
|---|---|---|
| 2026-09-03 |  | Document initial |
