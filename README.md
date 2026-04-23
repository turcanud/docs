# Estimarea Volumului de Lucru și a Costului Proiectului — EasySport

---

## 1. Introducere

Prezentul capitol descrie estimarea volumului de lucru și a costului proiectului **EasySport** — o platformă web de tip marketplace sportiv care conectează utilizatorii cu facilitățile sportive disponibile pentru rezervare. Platforma integrează un backend RESTful API dezvoltat în Django 6.0 cu Django REST Framework, un frontend construit în Next.js 15 cu TypeScript, autentificare prin JWT, sistem de roluri (customer / manager / admin), containerizare Docker și suport multi-limbă (română, engleză, rusă).

Estimarea a fost realizată utilizând trei metode complementare:

- **WBS (Work Breakdown Structure)** — descompunerea ierarhică a proiectului în livrabile și activități granulare, măsurabile și asignabile.
- **PERT (Program Evaluation and Review Technique)** — estimarea duratei fiecărei activități prin trei scenarii: optimist, cel mai probabil și pesimist.
- **RACI (Responsible, Accountable, Consulted, Informed)** — matricea de responsabilități care definește rolul fiecărui participant pentru fiecare activitate.

Structura completă a activităților, estimările PERT, calendarul și calculul costurilor sunt prezentate în **Anexa 1 — Tabelul WBS-PERT-Gantt-RAM-Cost**.

---

## 2. Descompunerea Proiectului prin Metoda WBS

### 2.1 Descrierea metodei

**Work Breakdown Structure (WBS)** reprezintă o descompunere ierarhică, orientată pe livrabile, a întregului domeniu de activitate al proiectului [1]. Fiecare nivel al WBS rafinează elementul părinte în componente mai mici și mai ușor de gestionat, până la nivelul de pachete de lucru (*work packages*) — unități executabile, estimabile și asignabile unui singur responsabil.

Utilitatea WBS în contextul proiectului EasySport este multiplă:

- **Vizibilitate completă**: toate activitățile necesare finalizării platformei sunt identificate și documentate explicit, evitând omisiunile.
- **Estimare mai precisă**: pachetele de lucru granulare permit estimări PERT mai realiste decât o estimare globală.
- **Asignarea responsabilităților**: fiecare activitate din WBS primește un responsabil clar în matricea RACI.
- **Urmărirea progresului**: PM-ul poate monitoriza stadiul proiectului la nivel de activitate individuală.

### 2.2 Structura WBS a proiectului EasySport

Proiectul a fost descompus în **7 faze principale** cu un total de **34 de activități**:

```
EasySport
├── 1. Managementul Proiectului
│   ├── 1.1 Definirea cerințelor și planificarea proiectului
│   ├── 1.2 Monitorizarea progresului și comunicarea echipei
│   └── 1.3 Revizuire finală și predarea proiectului
│
├── 2. Proiectare și Arhitectură
│   ├── 2.1 Proiectarea arhitecturii sistemului (three-tier, API design)
│   ├── 2.2 Proiectarea modelului de date (ERD — User, Facility, Field, Booking)
│   └── 2.3 Proiectarea interfeței utilizator (wireframes, design system)
│
├── 3. Configurarea Mediului de Dezvoltare
│   ├── 3.1 Configurare Docker și docker-compose (servicii web/db/redis)
│   ├── 3.2 Configurare proiect Django și structură module (be_active/api, auth)
│   └── 3.3 Configurare proiect Next.js și structură frontend (App Router, i18n)
│
├── 4. Implementare Backend (Django REST API)
│   ├── 4.1 Implementarea modelelor de date Django ORM
│   ├── 4.2 Implementarea sistemului de autentificare JWT (register, login, refresh)
│   ├── 4.3 Implementarea sistemului de permisiuni și roluri (IsManager, IsManagerOwner)
│   ├── 4.4 Implementarea endpoint-urilor publice de catalog (sports, facilities, availability)
│   ├── 4.5 Implementarea logicii de business pentru rezervări (validare, conflicte, prețuri)
│   ├── 4.6 Implementarea endpoint-urilor pentru manageri (CRUD facilități, terenuri, blocări)
│   ├── 4.7 Implementarea endpoint-urilor pentru administratori (users, grant-manager)
│   ├── 4.8 Generarea documentației API (OpenAPI 3.0 / Swagger UI / ReDoc)
│   └── 4.9 Configurarea stratului de cache Redis (django-redis, hiredis)
│
├── 5. Implementare Frontend (Next.js 15)
│   ├── 5.1 Configurarea autentificării frontend (JWT cookies HttpOnly, middleware protecție rute)
│   ├── 5.2 Implementarea paginilor publice (landing, sporturi, facilități, terenuri)
│   ├── 5.3 Implementarea formularelor de autentificare (sign-in, sign-up cu validare Zod)
│   ├── 5.4 Implementarea paginilor utilizator (profil, rezervările mele, anulare)
│   ├── 5.5 Implementarea fluxului de rezervare (calendar disponibilitate, confirmare dialog)
│   ├── 5.6 Implementarea dashboard-ului manager (CRUD facilități, terenuri, gestionare rezervări)
│   ├── 5.7 Implementarea panoului de administrare utilizatori (listare, ștergere, grant-manager)
│   ├── 5.8 Implementarea internaționalizării (ro/en/ru cu next-intl, mesaje JSON)
│   └── 5.9 Implementarea temelor dark/light mode (next-themes, CSS variables OKLCH)
│
├── 6. Testare și Validare
│   ├── 6.1 Testare unitară backend (APITestCase Django — autentificare, roluri, profil)
│   ├── 6.2 Testare de integrare API (endpoint-uri REST — booking, manager, admin)
│   ├── 6.3 Testare funcțională frontend (fluxuri utilizator, formulare, navigare)
│   └── 6.4 Testare de securitate (JWT, permisiuni, CORS, validare parole)
│
└── 7. Documentare
    ├── 7.1 Documentare tehnică backend (arhitectură, modele, API, Docker)
    ├── 7.2 Documentare tehnică frontend (stack, componente, servicii, fluxuri)
    └── 7.3 Ghid de utilizare a aplicației
```

### 2.3 Livrabilele principale ale proiectului

| Livrabil | Conținut | Rol |
|---|---|---|
| **Sistem backend Django** | API REST cu autentificare JWT, modele ORM, permisiuni, logică de business, documentare OpenAPI | Furnizează datele și logica platformei |
| **Aplicație frontend Next.js** | Interfață web cu SSR, componente React, servicii API, i18n, teme | Oferă experiența utilizatorului |
| **Infrastructură Docker** | docker-compose cu servicii web/PostgreSQL/Redis, Dockerfile-uri dev/prod | Asigură mediul reproductibil de rulare |
| **Suite de teste** | Teste unitare backend (APITestCase), teste integrare, teste funcționale frontend | Validează calitatea și corectitudinea sistemului |
| **Documentație tehnică** | DOC_backend.md, DOC_frontend.md, schema OpenAPI (schema.yml) | Facilitează mentenanța și onboarding-ul |

---

## 3. Estimarea Duratei Activităților prin Metoda PERT

### 3.1 Descrierea metodei

**PERT (Program Evaluation and Review Technique)** este o metodă probabilistică de estimare a duratei activităților, concepută pentru proiecte cu incertitudine ridicată [2]. Spre deosebire de estimarea deterministă (o singură valoare), PERT utilizează **trei scenarii**:

| Parametru | Simbol | Semnificație |
|---|---|---|
| Estimare optimistă | **to** | Durata minimă posibilă, în condiții ideale (fără blocaje, cu experiență anterioară) |
| Estimare cel mai probabil | **tm** | Durata realistă, bazată pe experiența similară și complexitatea efectivă |
| Estimare pesimistă | **tp** | Durata maximă posibilă, incluzând riscuri, reluări și probleme tehnice |

### 3.2 Formula de calcul PERT

Durata estimată se calculează ca o medie ponderată cu accent pe scenariul cel mai probabil:

$$T = \frac{t_o + 4 \cdot t_m + t_p}{6}$$

Această formulă acordă o pondere de **4/6 ≈ 67%** estimării cel mai probabile și câte **1/6 ≈ 17%** estimărilor extreme, rezultând o estimare robustă statistical, echivalentă cu media distribuției beta [2].

**Conversia în zile de lucru** se realizează la rata de **4 ore/zi lucrătoare** (conform recomandării din caietul de sarcini):

$$\text{Zile} = \frac{T}{4}$$

### 3.3 Justificarea estimărilor

Valorile **to**, **tm** și **tp** au fost estimate pe baza:

- **Complexității tehnice** a fiecărei activități, derivată din analiza documentațiilor — de exemplu, implementarea logicii de business pentru rezervări (4.5) implică validare multi-etapă, detectare conflicte și calcul automat de preț, justificând o estimare tm = 12 ore.
- **Experienței cu stiva tehnologică** — activitățile de configurare mediu (3.1–3.3) au estimări mai mici (tm = 4 ore) față de activitățile de implementare complexă (5.6 Dashboard Manager, tm = 14 ore).
- **Riscurilor de integrare** — activitățile care implică comunicare între componente (5.1 Auth Frontend, 5.5 Flux rezervare) au tp mai mari pentru a acoperi problemele de CORS, sincronizare JWT sau state management.
- **Criteriul de consistență**: pentru toate activitățile s-a verificat că `to ≤ tm ≤ tp`.

---

## 4. Dependențele dintre Activități

### 4.1 Tipul de dependențe utilizat

Toate dependențele din proiect sunt de tip **Finish-to-Start (FS)**: activitatea succesor nu poate începe decât după finalizarea completă a activității predecesor. Această relație este cea mai comună în proiectele software și asigură că fiecare activitate pornește de la o bază solidă.

### 4.2 Logica dependențelor

**Faza de planificare → Design → Configurare:**

- `2.1` depinde de `1.1` — arhitectura sistemului poate fi proiectată abia după ce cerințele sunt definite.
- `2.2` și `2.3` depind de `2.1` — modelul ERD și wireframe-urile se bazează pe arhitectura aprobată.
- `3.1`, `3.2`, `3.3` depind de `2.1` — mediul Docker și structurile de proiect se configurează conform arhitecturii alese.

**Implementarea backend-ului (lanț secvențial):**

- `4.1` depinde de `3.2` și `2.2` — modelele ORM se implementează după configurarea Django și proiectarea ERD.
- `4.2 → 4.3 → 4.4 → 4.5 → 4.6 → 4.7 → 4.8 → 4.9` — lanț secvențial: autentificarea JWT trebuie finalizată înainte de permisiuni, permisiunile înainte de endpoint-uri protejate, endpoint-urile de catalog înainte de cele de booking, etc.

**Implementarea frontend-ului (dependențe cross-stream):**

- `5.1` (auth frontend) depinde de `4.2` (auth backend JWT) și `3.3` — clientul frontend nu poate implementa autentificarea fără API-ul JWT funcțional.
- `5.2` (pagini publice) depinde de `5.1` și `4.4` (endpoint-uri catalog) — paginile publice consumă API-urile de sporturi și facilități.
- `5.3` (formulare auth) depinde de `5.1` — formularele de sign-in/sign-up necesită serviciile de autentificare deja configurate.
- `5.4` depinde de `5.3` și `4.5` — paginile de profil și rezervări necesită atât autentificarea cât și API-ul de booking.
- `5.6` (dashboard manager) depinde de `4.6` și `5.2` — interfața de management necesită API-ul de manager și componentele publice reutilizabile.
- `5.9` (teme) depinde de `5.2` și `2.3` — sistemul de teme se aplică după finalizarea paginilor principale și conform design system-ului.

**Testare → Documentare (dependențe pe finalizarea implementării):**

- `6.1` depinde de `4.9` — testele backend încep după finalizarea completă a API-ului.
- `6.3` depinde de `5.9` — testele frontend încep după finalizarea tuturor paginilor.
- `7.1`, `7.2` depind de testele corespunzătoare — documentația reflectă sistemul testat și validat.
- `1.3` (revizuire finală) depinde de `7.3` — proiectul se predă după finalizarea tuturor livrabilelor.

---

## 5. Matricea RACI — Distribuirea Responsabilităților

### 5.1 Modelul RACI

**RACI** este un instrument de management al proiectului care clarifică rolurile și responsabilitățile fiecărui participant pentru fiecare activitate [3]:

| Cod | Rol | Semnificație |
|---|---|---|
| **R** | *Responsible* — Responsabil de execuție | Persoana care efectuează efectiv munca; poate fi mai mult de un rol |
| **A** | *Accountable* — Răspunzător de rezultat | Persoana care aprobă și poartă răspunderea finală; exact una per activitate |
| **C** | *Consulted* — Consultat | Furnizează input/expertiză; comunicare bidirecțională |
| **I** | *Informed* — Informat | Este notificat cu privire la progres sau rezultat; comunicare unidirecțională |

### 5.2 Rolurile din proiectul EasySport

| Rol | Abreviere | Responsabilitate principală |
|---|---|---|
| **Project Manager** | PM | Planificare, monitorizare, comunicare, coordonare, predare livrabile |
| **Developer** | Dev | Implementare backend (Django) și frontend (Next.js), configurare mediu |
| **Tester** | Test | Testare unitară, integrare, funcțională și de securitate |
| **Designer** | Design | Proiectare wireframes, design system, UI/UX, teme |

### 5.3 Principii de distribuire a responsabilităților

- **PM** este *Accountable* (A) pentru toate activitățile de implementare și design, asigurând vizibilitatea globală. PM este *Responsible* (R) doar pentru activitățile de management.
- **Dev** este *Responsible* (R) pentru toate activitățile tehnice (configurare, implementare backend, implementare frontend, documentare tehnică).
- **Test** este *Responsible* (R) exclusiv pentru activitățile din faza 6 (Testare).
- **Design** este *Responsible* (R) doar pentru 2.3 (Proiectare UI/UX) și *Consulted* (C) pentru activitățile de frontend care implică stilizare.

### 5.4 Rolul utilizat în calculul costului

**Costul fiecărei activități se calculează folosind tariful rolului marcat cu „R" (Responsible)**, deoarece acesta este rolul care prestează efectiv munca și generează consumul de resurse. Deși PM-ul (A) este răspunzător de toate activitățile, tariful PM-ului se aplică doar activităților unde PM-ul execută efectiv munca (1.1, 1.2, 1.3, 7.3).

---

## 6. Estimarea Costurilor Proiectului

### 6.1 Tarifele utilizate

Tarifele au fost stabilite pe baza datelor de piață din industria IT din Republica Moldova pentru anul 2026, consultând platformele **rabota.md**, **jobs.md** și rapoartele salariale publicate de **ATIC (Asociația Companiilor IT din Moldova)**, precum și sondajele de piață IT Moldova [4]:

| Rol | Tarif (MDL/oră) | Justificare |
|---|---|---|
| **PM** (Project Manager) | 160 MDL/oră | Corespunde unui PM junior-mediu cu experiență în proiecte software. Salariul mediu brut al unui PM în IT în Moldova este de 20.000–28.000 MDL/lună, echivalând cu 125–175 MDL/oră la un program de 160 ore/lună [4]. Tariful de 160 MDL/oră reflectă un PM cu experiență în metodologii agile și gestionare proiecte web. |
| **Dev** (Developer Full-Stack) | 200 MDL/oră | Corespunde unui developer mid-level cu experiență în Python/Django și TypeScript/React. Salariul mediu brut al unui developer mid în Moldova este de 25.000–40.000 MDL/lună, echivalând cu 156–250 MDL/oră [4]. Tariful de 200 MDL/oră reflectă experiența cu stiva tehnologică modernă (Django 6, Next.js 15, Docker, JWT). |
| **Test** (QA Engineer) | 120 MDL/oră | Corespunde unui QA engineer junior-mediu. Salariile în QA în Moldova sunt de 15.000–22.000 MDL/lună brut, echivalând cu 94–138 MDL/oră [4]. Tariful de 120 MDL/oră reflectă experiența cu testare API (Django APITestCase) și testare de securitate. |
| **Design** (UI/UX Designer) | 140 MDL/oră | Corespunde unui designer UI/UX junior-mediu familiar cu sisteme de design (shadcn/ui, Tailwind CSS). Salariile UI/UX în Moldova sunt de 18.000–25.000 MDL/lună, echivalând cu 113–156 MDL/oră [4]. Tariful de 140 MDL/oră reflectă livrabilele concrete: wireframes și design system complet. |

### 6.2 Formula de calcul a costului

$$\text{Cost (MDL)} = \text{Durata estimată (ore)} \times \text{Tarif (MDL/oră)}$$

unde Durata estimată se obține prin formula PERT: $T = (t_o + 4 \cdot t_m + t_p) / 6$.

### 6.3 Costul total al proiectului

| Rol | Ore totale | Tarif (MDL/oră) | Cost total (MDL) |
|---|---|---|---|
| PM | 37,00 | 160 | 5.920,00 |
| Dev | 195,83 | 200 | 39.166,67 |
| Test | 31,83 | 120 | 3.820,00 |
| Design | 16,00 | 140 | 2.240,00 |
| **TOTAL** | **280,67** | — | **51.146,67** |

> **Costul total estimat al proiectului EasySport este de 51.146,67 MDL** (≈ 2.557 EUR la cursul de 20 MDL/EUR).

### 6.4 Activitatea cea mai costisitoare

Activitatea cu cel mai ridicat cost este **5.6 — Implementarea dashboard-ului manager** (2.933,33 MDL), urmată de **5.2** (2.466,67 MDL), **5.5** (2.466,67 MDL) și **4.5** (2.466,67 MDL). Dashboard-ul managerului este cel mai costisitor deoarece integrează funcționalități CRUD complexe (facilități, terenuri, rezervări, blocare/deblocare sloturi) și implică cel mai mare număr de componente frontend și apeluri API coordonate (tm = 14 ore).

### 6.5 Posibilități de optimizare a costurilor

- **Reutilizarea componentelor**: Activitățile 5.2–5.7 partajează componente shadcn/ui (Card, Dialog, Calendar), reducând efortul total față de o abordare fără design system.
- **Reducerea tarifelor prin utilizarea unui singur developer full-stack**: Un developer cu experiență atât în Django cât și Next.js elimină necesitatea unui specialist separat pentru fiecare stivă.
- **Paralelizarea testării**: Dacă Tester-ul începe testele unitare backend (6.1) în paralel cu finalizarea frontend-ului, durata totală a proiectului se reduce cu 1–2 săptămâni.
- **Automatizarea documentației**: Utilizarea `drf-spectacular` pentru generarea automată a schemei OpenAPI (4.8) reduce semnificativ efortul de documentare manuală.

---

## 7. Concluzii

Aplicând metodele WBS, PERT și RACI, proiectul EasySport a fost estimat la **276 ore de muncă** distribuite pe o durată de aproximativ **15 săptămâni** (3,5 luni), cu un cost total de **25.812,50 lei**. Structurarea în 7 faze și 34 de activități permite urmărirea granulară a progresului, detectarea timpurie a riscurilor și o comunicare clară a responsabilităților în echipă.

Tabelul complet WBS-PERT-Gantt-RAM-Cost este prezentat în **Anexa 1** de mai jos.

---

## Bibliografie

1. PMI (Project Management Institute), *A Guide to the Project Management Body of Knowledge (PMBOK® Guide)*, 7th ed., 2021. Disponibil la: https://www.pmi.org/pmbok-guide-standards
2. F. K. Levy, G. L. Thompson, J. D. Wiest, „The ABCs of the Critical Path Method", *Harvard Business Review*, vol. 41, nr. 5, pp. 98–108, 1963. Disponibil la: https://hbr.org/1963/09/the-abcs-of-the-critical-path-method
3. Smith, M., *RACI Matrix: Responsibility Assignment Matrix — A Complete Guide*, 2020. Disponibil la: https://www.projectmanager.com/blog/raci-chart-definition
4. ATIC (Asociația Companiilor IT din Moldova), *Raportul Salarial IT Moldova 2025*, 2025. Disponibil la: https://www.atic.md/; Rabota.md, *Salarii IT Republica Moldova 2026*, 2026. Disponibil la: https://rabota.md/; Jobs.md, *Piața Muncii IT Moldova 2026*, 2026. Disponibil la: https://jobs.md/

---

## Anexa 1 — Tabelul WBS-PERT-Gantt-RAM-Cost

> **Legendă RAM:** R = Responsible | A = Accountable | C = Consulted | I = Informed | — = Neinplicat  
> **Notă:** Durata estimată = (to + 4·tm + tp) / 6 | Zile = Durata / 4 | Cost (MDL) = Durata × Tarif  
> **Zile lucrătoare**: Se exclud sâmbăta, duminica și sărbătorile legale din Republica Moldova 2026 (10 apr. — Vinerea Mare, 13 apr. — Lunea Paștelui, 1 mai — Ziua Muncii).

| Cod WBS | Activitate | to (ore) | tm (ore) | tp (ore) | Durata estimată (ore) | Zile (4h/zi) | Start | Sfârșit | Dependențe | PM | Dev | Test | Design | Rol (R) | Tarif (MDL/oră) | Cost (MDL) |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **1.1** | Definirea cerințelor și planificarea proiectului | 4 | 8 | 16 | 8,67 | 2,17 | 26.01.2026 | 28.01.2026 | — | R | C | I | I | PM | 160 | 1.386,67 |
| **1.2** | Monitorizarea progresului și comunicarea echipei | 8 | 16 | 24 | 16,00 | 4,00 | 29.01.2026 | 23.04.2026 | 1.1 | R | I | I | I | PM | 160 | 2.560,00 |
| **1.3** | Revizuire finală și predarea proiectului | 4 | 8 | 12 | 8,00 | 2,00 | 24.04.2026 | 27.04.2026 | 7.3 | R | C | C | I | PM | 160 | 1.280,00 |
| **2.1** | Proiectarea arhitecturii sistemului (three-tier, API design) | 4 | 8 | 16 | 8,67 | 2,17 | 26.01.2026 | 28.01.2026 | 1.1 | A | R | — | C | Dev | 200 | 1.733,33 |
| **2.2** | Proiectarea modelului de date (ERD — entități și relații) | 2 | 4 | 8 | 4,33 | 1,08 | 29.01.2026 | 30.01.2026 | 2.1 | A | R | — | — | Dev | 200 | 866,67 |
| **2.3** | Proiectarea interfeței utilizator (wireframes, design system) | 8 | 16 | 24 | 16,00 | 4,00 | 29.01.2026 | 03.02.2026 | 2.1 | A | C | — | R | Design | 140 | 2.240,00 |
| **3.1** | Configurare Docker și docker-compose (web / db / redis) | 2 | 4 | 8 | 4,33 | 1,08 | 29.01.2026 | 30.01.2026 | 2.1 | A | R | — | — | Dev | 200 | 866,67 |
| **3.2** | Configurare proiect Django și structură module (be_active) | 2 | 4 | 6 | 4,00 | 1,00 | 02.02.2026 | 02.02.2026 | 3.1 | A | R | I | — | Dev | 200 | 800,00 |
| **3.3** | Configurare proiect Next.js și structură frontend (App Router) | 2 | 4 | 6 | 4,00 | 1,00 | 02.02.2026 | 02.02.2026 | 3.1 | A | R | I | — | Dev | 200 | 800,00 |
| **4.1** | Implementarea modelelor de date Django ORM (User, Facility, Field, Booking) | 4 | 8 | 16 | 8,67 | 2,17 | 03.02.2026 | 05.02.2026 | 3.2, 2.2 | A | R | C | — | Dev | 200 | 1.733,33 |
| **4.2** | Implementarea sistemului de autentificare JWT (register, login, token refresh) | 4 | 8 | 14 | 8,33 | 2,08 | 06.02.2026 | 10.02.2026 | 4.1 | A | R | C | — | Dev | 200 | 1.666,67 |
| **4.3** | Implementarea sistemului de permisiuni și roluri (IsManager, IsManagerOwner) | 3 | 6 | 12 | 6,50 | 1,63 | 11.02.2026 | 12.02.2026 | 4.2 | A | R | C | — | Dev | 200 | 1.300,00 |
| **4.4** | Implementarea endpoint-urilor publice de catalog (sports, facilities, availability) | 4 | 8 | 14 | 8,33 | 2,08 | 13.02.2026 | 16.02.2026 | 4.3 | A | R | C | — | Dev | 200 | 1.666,67 |
| **4.5** | Implementarea logicii de business pentru rezervări (validare, conflicte, calcul preț) | 6 | 12 | 20 | 12,33 | 3,08 | 17.02.2026 | 20.02.2026 | 4.4 | A | R | C | — | Dev | 200 | 2.466,67 |
| **4.6** | Implementarea endpoint-urilor pentru manageri (CRUD facilități, terenuri, blocări) | 6 | 10 | 18 | 10,67 | 2,67 | 23.02.2026 | 25.02.2026 | 4.5 | A | R | C | — | Dev | 200 | 2.133,33 |
| **4.7** | Implementarea endpoint-urilor pentru administratori (users, grant-manager) | 3 | 6 | 10 | 6,17 | 1,54 | 26.02.2026 | 27.02.2026 | 4.6 | A | R | C | — | Dev | 200 | 1.233,33 |
| **4.8** | Generarea documentației API (OpenAPI 3.0 / Swagger UI / ReDoc) | 2 | 4 | 8 | 4,33 | 1,08 | 02.03.2026 | 03.03.2026 | 4.7 | A | R | I | — | Dev | 200 | 866,67 |
| **4.9** | Configurarea stratului de cache Redis (django-redis, hiredis) | 1 | 3 | 6 | 3,17 | 0,79 | 04.03.2026 | 04.03.2026 | 4.8 | A | R | I | — | Dev | 200 | 633,33 |
| **5.1** | Configurarea autentificării frontend (JWT cookies HttpOnly, middleware, protecție rute) | 4 | 8 | 14 | 8,33 | 2,08 | 05.03.2026 | 06.03.2026 | 4.2, 3.3 | A | R | C | I | Dev | 200 | 1.666,67 |
| **5.2** | Implementarea paginilor publice (landing, sporturi, facilități, terenuri) | 6 | 12 | 20 | 12,33 | 3,08 | 09.03.2026 | 11.03.2026 | 5.1, 4.4 | A | R | C | I | Dev | 200 | 2.466,67 |
| **5.3** | Implementarea formularelor de autentificare (sign-in, sign-up cu validare Zod) | 4 | 8 | 14 | 8,33 | 2,08 | 12.03.2026 | 13.03.2026 | 5.1 | A | R | C | I | Dev | 200 | 1.666,67 |
| **5.4** | Implementarea paginilor utilizator (profil, rezervările mele, anulare rezervare) | 4 | 8 | 14 | 8,33 | 2,08 | 16.03.2026 | 17.03.2026 | 5.3, 4.5 | A | R | C | I | Dev | 200 | 1.666,67 |
| **5.5** | Implementarea fluxului de rezervare (calendar disponibilitate, confirmare dialog) | 6 | 12 | 20 | 12,33 | 3,08 | 18.03.2026 | 20.03.2026 | 5.4 | A | R | C | I | Dev | 200 | 2.466,67 |
| **5.6** | Implementarea dashboard-ului manager (CRUD facilități, terenuri, gestionare rezervări) | 8 | 14 | 24 | 14,67 | 3,67 | 23.03.2026 | 26.03.2026 | 4.6, 5.2 | A | R | C | I | Dev | 200 | 2.933,33 |
| **5.7** | Implementarea panoului de administrare utilizatori (listare, ștergere, grant-manager) | 4 | 8 | 14 | 8,33 | 2,08 | 27.03.2026 | 30.03.2026 | 4.7, 5.6 | A | R | C | — | Dev | 200 | 1.666,67 |
| **5.8** | Implementarea internaționalizării (ro/en/ru cu next-intl, fișiere mesaje JSON) | 4 | 8 | 14 | 8,33 | 2,08 | 31.03.2026 | 01.04.2026 | 5.2 | A | R | I | C | Dev | 200 | 1.666,67 |
| **5.9** | Implementarea temelor dark/light mode (next-themes, CSS variables OKLCH) | 2 | 4 | 8 | 4,33 | 1,08 | 02.04.2026 | 02.04.2026 | 5.2, 2.3 | A | R | I | C | Dev | 200 | 866,67 |
| **6.1** | Testare unitară backend (APITestCase Django — autentificare, roluri, profil) | 4 | 8 | 14 | 8,33 | 2,08 | 03.04.2026 | 06.04.2026 | 4.9 | C | I | R | — | Test | 120 | 1.000,00 |
| **6.2** | Testare de integrare API (endpoint-uri REST — booking, manager, admin) | 4 | 8 | 14 | 8,33 | 2,08 | 07.04.2026 | 08.04.2026 | 6.1 | C | I | R | — | Test | 120 | 1.000,00 |
| **6.3** | Testare funcțională frontend (fluxuri utilizator, formulare, navigare) | 4 | 8 | 16 | 8,67 | 2,17 | 09.04.2026 | 14.04.2026 | 5.9, 6.1 | C | I | R | I | Test | 120 | 1.040,00 |
| **6.4** | Testare de securitate (JWT, permisiuni RBAC, CORS, validare parole) | 3 | 6 | 12 | 6,50 | 1,63 | 15.04.2026 | 16.04.2026 | 6.2, 6.3 | C | I | R | — | Test | 120 | 780,00 |
| **7.1** | Documentare tehnică backend (arhitectură, modele, API, Docker, securitate) | 4 | 8 | 14 | 8,33 | 2,08 | 17.04.2026 | 20.04.2026 | 4.9, 6.2 | C | R | I | — | Dev | 200 | 1.666,67 |
| **7.2** | Documentare tehnică frontend (stack, componente, servicii, fluxuri, i18n) | 4 | 8 | 14 | 8,33 | 2,08 | 21.04.2026 | 22.04.2026 | 5.9, 6.3 | C | R | I | I | Dev | 200 | 1.666,67 |
| **7.3** | Ghid de utilizare a aplicației (fluxuri customer, manager, admin) | 2 | 4 | 8 | 4,33 | 1,08 | 23.04.2026 | 23.04.2026 | 7.1, 7.2 | R | C | I | — | PM | 160 | 693,33 |
| | | | | | | | | | | | | | | | **TOTAL** | **51.146,67 MDL** |

---

### Note privind calendarul

| Sărbătoare legală exclusă | Data | Zi | Impact |
|---|---|---|---|
| Ziua Unirii Principatelor | 24.01.2026 | Sâmbătă | Nu afectează zilele lucrătoare |
| Vinerea Mare (Paște Ortodox) | 10.04.2026 | Vineri | Zi nelucrătoare |
| Lunea Paștelui (Paște Ortodox) | 13.04.2026 | Luni | Zi nelucrătoare |
| Ziua Muncii | 01.05.2026 | Vineri | Zi nelucrătoare (după intervalul proiectului) |

> **Observație 1**: Activitatea 6.3 (Testare funcțională frontend) are Start: 09.04.2026 (joi) și Sfârșit: 14.04.2026 (marți), deoarece zilele de 10 aprilie (Vinerea Mare) și 13 aprilie (Lunea Paștelui) sunt sărbători legale — cele 2 zile lucrătoare sunt joi 9 și marți 14 aprilie.  
> **Observație 2**: Activitățile 7.1, 7.2, 7.3 și 1.3 se desfășoară în intervalul 17–27 aprilie 2026, fără interferențe cu sărbători legale în această perioadă.  
> **Observație 3**: Ziua Muncii (01.05.2026) cade după finalizarea proiectului (27.04.2026).

---

### Rezumat Gantt pe faze

```
FAZA                        | IAN  | FEB  | MAR  | APR  |
----------------------------+------+------+------+------+
1. Management Proiect       |  ████|      |      | ████ |
2. Proiectare & Arhitectură |  ████|      |      |      |
3. Configurare Mediu        |  ████|      |      |      |
4. Implementare Backend     |      |█████ |      |      |
5. Implementare Frontend    |      |      |█████ |██    |
6. Testare                  |      |      |      |████  |
7. Documentare              |      |      |      | ████ |
```

**Durată totală proiect**: 26.01.2026 — 27.04.2026 (≈ 13 săptămâni, 62 zile lucrătoare)  
**Rezervă față de termenul limită**: 27.04.2026 → 22.05.2026 = **17 zile lucrătoare buffer**  
**Cost total proiect**: **51.146,67 MDL** (≈ 2.557 EUR la cursul de 20 MDL/EUR)
