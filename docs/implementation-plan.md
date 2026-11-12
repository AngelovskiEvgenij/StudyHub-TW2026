# Plan de implementare – StudyHub

## 1. Justificarea tehnologiilor

| Alegere | Motiv | Alternativă luată în calcul |
|---------|-------|-----------------------------|
| **JavaScript pe frontend și backend** | Un singur limbaj în echipă; ambii membri pot înțelege tot codul și pot explica arhitectura la prezentare | Python/Flask + JS: două limbaje, mai multă documentație de menținut |
| **JavaScript vanilla (fără framework)** | Aplicația are puține ecrane; cerințele de accesibilitate și XSS rămân sub control direct; fără pas de build | React: potrivit, dar adaugă complexitate de configurare |
| **Node.js + Express** | Minimalist, documentație bogată, construcție rapidă de API REST, ecosistem pentru sesiuni, validare și teste | Fastify, NestJS: mai complexe pentru dimensiunea proiectului |
| **PostgreSQL** | Date relaționale legate între ele (materii, întrebări, teste, rezultate); interogări de agregare pentru statistici; constrângeri de integritate | SQLite: mai simplu, dar mai limitat la acces concurent |
| **Sesiuni + cookie HttpOnly** | Simplu și sigur pentru o aplicație cu un singur client web; se pot invalida pe server | JWT: util pentru API-uri multi-client, aici adaugă risc (stocare token) |
| **bcrypt** | Algoritm dedicat parolelor, cu salt și cost configurabil | Argon2id: mai nou, dar nu aduce un beneficiu necesar aici |
| **Chart.js** | Grafice simple (linie, bară) fără configurare complexă | D3: prea complex |
| **Jest + Supertest** | Teste unitare și de integrare pentru API | Mocha/Chai |
| **Mermaid** | Diagrame versionate în repository, randate de GitHub | Imagini externe |

## 2. Strategia de versionare (GitHub)

- **Ramuri:** `main` (stabil, livrabile), `develop` (integrare), `feature/<scurt>` (ex. `feature/csv-import`, `feature/progress-chart`).
- **Flux:** ramură `feature` → Pull Request spre `develop` → revizuit de celălalt membru → merge. La finalul fiecărei etape: merge `develop` → `main` și tag (`v0.2-etapa2`, `v0.3-prototip`, `v1.0`).
- **Commit-uri:** mici, cu mesaje clare, în format `tip: descriere` (`feat:`, `fix:`, `docs:`, `test:`, `refactor:`). Fiecare membru face commit-uri sub propriul cont.
- **Issues și board:** fiecare sarcină este un Issue asignat (etichete: `backend`, `frontend`, `docs`, `test`; milestone-uri pe etape). Commit-urile referă Issue-ul (`#12`).
- **Securitate:** `.env` în `.gitignore`; doar `.env.example` în repository; nicio parolă sau cheie reală.
- **Regula repository-ului:** `main` se modifică doar prin Pull Request.

## 3. Repartizarea responsabilităților

| Zonă | Student 1 (Backend, date, CSV) | Student 2 (Frontend, grafice, teste, administrare) |
|------|-------------------------------|---------------------------------------------------|
| API REST, rute și middleware | ● responsabil | contribuție la contracte |
| Autentificare, sesiuni, roluri | ● responsabil | formulare login/înregistrare |
| Schema PostgreSQL și seed | ● responsabil | — |
| Logica de teste, scor, statistici | ● responsabil | consumă datele în interfață |
| Import/export CSV | ● responsabil (server) | ecranul de încărcare și raportul de erori |
| Interfață responsive și accesibilitate | revizuire | ● responsabil |
| Grafice de progres (Chart.js) | furnizează endpoint-urile | ● responsabil |
| Panou de administrare | endpoint-uri `/admin/*` | ● responsabil (interfață) |
| Teste automate și manuale | teste de API | ● responsabil (plan de testare, teste UI/E2E) |
| Documentație și GitHub | comun | comun |

Regulă: fiecare ecran se dezvoltă după contractul API din [data-model.md](data-model.md); modificările de contract se discută și se actualizează în documentație.

## 4. Planul de implementare până la prototip (Etapa 3)

| Perioadă | Student 1 | Student 2 | Rezultat |
|----------|-----------|-----------|----------|
| **Săpt. 7–8** | Inițializare proiect Node/Express, conexiune PostgreSQL, `schema.sql`, `seed.sql`, înregistrare/login/logout, middleware de roluri | Structura `public/`, CSS de bază (variabile, layout responsive), pagini login/înregistrare, modulul `api.js` | Autentificare funcțională cu roluri |
| **Săpt. 8–9** | CRUD materii, capitole, întrebări; generare test de exersare; trimitere și calcul scor | Pagini bancă de întrebări, formular întrebare, test nou, rezolvare test, rezultat cu feedback | Flux end-to-end: creare întrebare → rezolvare test → scor |
| **Săpt. 9–10** | Import CSV cu raport de erori; istoric și `/me/progress` | Pagina de progres cu Chart.js, ecran import CSV; teste inițiale (Jest + Supertest) | Prototip rulabil, README actualizat, teste inițiale |

**Prioritizare prototip (obligatoriu):** FR-01–03, FR-05–07, FR-11–13, FR-16, FR-21–22.

## 5. Planul până la aplicația finală (Etapa 4)

| Perioadă | Activități |
|----------|-----------|
| **Săpt. 11–12** | Export CSV, teste fixe create de profesor (FR-10), panou de administrare complet, statistici profesor (FR-19), progres pe capitole (FR-18) |
| **Săpt. 12–13** | Securitate: CSRF, rate limiting, `helmet`; verificare accesibilitate și responsive; teste de eroare și cazuri-limită; (opțional) test cu cronometru |
| **Săpt. 13–14** | Documentație finală (README, API, limitări), repetiție demonstrație, date de demonstrație temporare, înghețarea codului |

## 6. Plan de testare

| Tip | Instrument | Ce acoperă |
|-----|-----------|------------|
| Unitar | Jest | calcul scor, validare rând CSV, calcul statistici |
| Integrare API | Jest + Supertest | login, roluri (403), CRUD întrebări, trimitere test, import CSV, erori 400/401/403/404/409 |
| Manual | listă de verificare per scenariu | cele 5 scenarii din [use-cases.md](use-cases.md) |
| Accesibilitate | verificare manuală + Lighthouse | etichete, tastatură, contrast |
| Responsive | browser (360 px, 768 px, desktop) | afișarea ecranelor principale |

## 7. Riscuri și măsuri

| Risc | Probabilitate | Măsură |
|------|---------------|--------|
| Supradimensionarea proiectului | medie | funcționalitățile opționale se fac doar după cele obligatorii; MoSCoW din [requirements.md](requirements.md) |
| Dezechilibru de contribuție | medie | Issues asignate, Pull Request-uri, revizuire reciprocă, istoric Git |
| Integrarea frontend–backend întârzie | medie | contractul API stabilit din timp; date de test (seed) disponibile de la început |
| Vulnerabilități (XSS, CSV, SQL) | medie | validare pe server, interogări parametrizate, teste de eroare |
| Calitatea și drepturile de autor ale întrebărilor | mică | întrebări create de echipă sau cu licență liberă |
| Probleme de mediu local (PostgreSQL) | mică | pași de instalare documentați, opțional Docker Compose |

## 8. Criterii de finalizare pentru Etapa 2

- Documentația completă este în repository și legată din README.
- Wireframe-urile ecranelor principale și fluxul de navigare sunt în `docs/wireframes/`.
- Ambii membri pot explica oral cerințele, scenariile, arhitectura, modelul de date și alegerile tehnologice.
