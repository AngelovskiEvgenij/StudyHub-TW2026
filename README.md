# StudyHub-TW2026

**Platformă web pentru bănci de întrebări, teste și analiză de progres**

Proiect realizat pentru disciplina *Tehnologii Web*, anul universitar 2026–2027.
Direcția tematică: **P7 – Învățare și evaluare educațională**.

---

## 1. Problema abordată

Studenții și cadrele didactice folosesc adesea materiale de verificare răspândite în fișiere separate (documente Word, PDF-uri, foi de calcul), fără o evidență clară a progresului. Profesorii nu știu care întrebări sunt greșite cel mai des, iar studenții nu știu ce capitole trebuie să recapituleze.

**StudyHub** centralizează aceste materiale într-o aplicație web: profesorii creează și organizează bănci de întrebări pe materii și capitole, iar studenții rezolvă teste și primesc feedback imediat, împreună cu statistici de progres.

## 2. Obiective (verificabile)

| # | Obiectiv | Cum se verifică |
|---|----------|-----------------|
| O1 | Creare și gestionare întrebări pe materii și capitole | Un profesor poate crea, edita, șterge și lista întrebări; acestea apar în banca materiei |
| O2 | Generare și rezolvare de teste cu scor automat | Un student rezolvă un test și vede scorul și explicația fiecărui răspuns |
| O3 | Afișarea progresului individual pe grafice | Dashboardul arată evoluția scorurilor și capitolele slabe |
| O4 | Import/export CSV al întrebărilor | Un fișier CSV valid se importă; rândurile invalide apar într-un raport de erori; exportul produce un CSV reimportabil |

## 3. Utilizatori vizați

- **Student** – exersează pe teste, își urmărește progresul.
- **Profesor** – creează conținut (întrebări, teste), importă/exportă CSV, analizează rezultatele.
- **Administrator** – gestionează conturile și materiile, are acces la panoul de administrare.

## 4. Echipa și responsabilități

| Membru | Contact | Responsabilități |
|--------|---------|------------------|
| **[Nume Prenume – Student 1]** (persoană de contact) | [email] | Backend (API REST), autentificare și roluri, baza de date, logica de business (generare teste, calcul scor, statistici), import/export CSV, securitate pe server |
| **[Nume Prenume – Student 2]** | [email] | Frontend (HTML/CSS/JS responsive, accesibilitate), grafice de progres (Chart.js), panou de administrare, testare (automată și manuală) |

Documentația, arhitectura și evidența GitHub sunt responsabilitate comună; ambii membri cunosc și pot explica arhitectura generală. Detalii în [implementation-plan.md](docs/implementation-plan.md).

## 5. Funcționalități propuse

**Obligatorii**
1. Creare și gestionare întrebări (profesor)
2. Rezolvare test cu scor și feedback per întrebare (student)
3. Progres individual: istoric, grafice, capitole slabe (student)
4. Import/export CSV al întrebărilor, cu raport de erori (profesor)
5. Autentificare cu roluri și panou de administrare (administrator)

**Opționale**
1. Statistici pentru profesor: întrebările cu cea mai mare rată de greșeli
2. Test cu cronometru
3. Mod de exersare pe întrebările greșite
4. Clasament anonim; întrebări cu imagini

**Neincluse:** proctoring, notare oficială a examenelor, integrare cu platforma universității, recomandări generate de AI.

## 6. Tehnologii

| Strat | Tehnologie |
|-------|------------|
| Frontend | HTML5 semantic, CSS responsiv, JavaScript (vanilla), Chart.js |
| Backend | Node.js + Express (API REST), sesiuni, bcrypt |
| Persistență | PostgreSQL |
| Instrumente | GitHub (versionare, Issues), Jest + Supertest (teste), Mermaid (diagrame) |

Justificarea completă: [implementation-plan.md](docs/implementation-plan.md#1-justificarea-tehnologiilor).

## 7. Documentație

| Document | Conținut |
|----------|----------|
| [docs/requirements.md](docs/requirements.md) | Cerințe funcționale și nefuncționale, prioritizate |
| [docs/use-cases.md](docs/use-cases.md) | Scenarii de utilizare complete și verificabile |
| [docs/wireframes/](docs/wireframes/README.md) | Wireframe-uri ale ecranelor principale și fluxuri de navigare |
| [docs/architecture.md](docs/architecture.md) | Diagrama și descrierea arhitecturii |
| [docs/data-model.md](docs/data-model.md) | Model de date, relații, endpoint-uri API |
| [docs/implementation-plan.md](docs/implementation-plan.md) | Tehnologii, versionare, plan, responsabilități, riscuri |

## 8. Rulare (se completează la Etapa 3)

```bash
git clone <url-repository>
cd studyhub
cp .env.example .env     # completați valorile locale; .env nu se urcă în repository
npm install
npm run db:init          # creează schema și datele de demonstrație
npm start
```

## 9. Limitări cunoscute

Se vor completa pe parcursul implementării.
