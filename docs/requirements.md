# Cerințe – StudyHub

Prioritizare (MoSCoW): **M** = obligatoriu (Must), **S** = important (Should), **C** = opțional (Could), **W** = exclus (Won't).

## 1. Domeniul proiectului

**Inclus:** conturi cu roluri; bănci de întrebări pe materii și capitole; teste; scor și feedback; statistici de progres; import/export CSV; administrare.

**Exclus:** proctoring, notare oficială, integrare cu platforma universității, recomandări generate de AI.

## 2. Cerințe funcționale

### 2.1 Autentificare și conturi
| ID | Cerință | Prio | Verificare |
|----|---------|------|------------|
| FR-01 | Utilizatorul se poate înregistra (rol implicit: student) cu email și parolă | M | Cont nou apare în baza de date; parola nu este stocată în clar |
| FR-02 | Utilizatorul se autentifică și se deconectează (sesiune) | M | Login valid creează sesiune; logout o distruge |
| FR-03 | Accesul la funcționalități depinde de rol (student / profesor / admin) | M | Un student primește 403 pe rute de profesor |
| FR-04 | Utilizatorul își poate schimba parola | S | Login cu parola veche eșuează după schimbare |

### 2.2 Materii și capitole
| ID | Cerință | Prio | Verificare |
|----|---------|------|------------|
| FR-05 | Adminul creează, editează, șterge/dezactivează materii și asociază profesori | M | Materia apare/dispare în liste |
| FR-06 | Profesorul gestionează capitolele materiilor la care este asociat | M | Capitolul apare la crearea întrebărilor |

### 2.3 Bancă de întrebări
| ID | Cerință | Prio | Verificare |
|----|---------|------|------------|
| FR-07 | Profesorul creează o întrebare: text, 2–6 variante, o variantă corectă, dificultate (ușor/mediu/greu), capitol, explicație | M | Întrebarea apare în banca materiei |
| FR-08 | Profesorul editează și șterge întrebări proprii (adminul pe toate) | M | Modificările se văd imediat; ștergerea nu afectează rezultatele istorice |
| FR-09 | Listare cu filtrare (capitol, dificultate) și paginare | M | Filtrele returnează doar întrebările potrivite |

### 2.4 Teste
| ID | Cerință | Prio | Verificare |
|----|---------|------|------------|
| FR-10 | Profesorul creează, editează, șterge teste (set fix de întrebări, titlu, materie) | M | Testul apare în lista studenților |
| FR-11 | Studentul generează un test de exersare: materie, capitole, număr de întrebări, selecție aleatorie | M | Testul conține exact N întrebări distincte |
| FR-12 | Studentul rezolvă testul și îl trimite; serverul calculează scorul | M | Scorul corespunde răspunsurilor corecte |
| FR-13 | După trimitere se afișează feedback per întrebare: răspuns ales, răspuns corect, explicație | M | Fiecare întrebare are verdict și explicație |
| FR-14 | Test cu cronometru; la expirare, testul se trimite automat | C | Serverul respinge răspunsuri după termen |
| FR-15 | Mod de exersare doar pe întrebările greșite anterior | C | Testul conține doar întrebări greșite |

### 2.5 Progres și statistici
| ID | Cerință | Prio | Verificare |
|----|---------|------|------------|
| FR-16 | Istoricul testelor studentului (dată, materie, scor) | M | Lista conține toate rezolvările |
| FR-17 | Grafic al evoluției scorurilor în timp, pe materie | M | Graficul corespunde datelor din istoric |
| FR-18 | Procent de reușită pe capitol; capitolele slabe sunt evidențiate (sub 60%) | M | Valorile corespund calculului manual |
| FR-19 | Profesorul vede întrebările cu cea mai mare rată de greșeli | S | Lista este ordonată descrescător după rata de greșeli |
| FR-20 | Clasament anonim | C | Fără date personale afișate |

### 2.6 Import/export CSV
| ID | Cerință | Prio | Verificare |
|----|---------|------|------------|
| FR-21 | Profesorul importă întrebări dintr-un CSV (UTF-8, antet obligatoriu, câmpuri între ghilimele) | M | Rândurile valide sunt salvate |
| FR-22 | Rândurile invalide nu opresc importul; raportul afișează numărul rândului și motivul | M | Raport cu rânduri respinse |
| FR-23 | Importul este atomic per rând (rândurile valide se salvează, cele invalide nu) și evită duplicatele | S | Reimportul aceluiași fișier nu dublează întrebările |
| FR-24 | Profesorul exportă întrebările unei materii în CSV, reimportabil | M | Exportul se reimportă fără erori |
| FR-25 | Adminul poate exporta rezultatele unei materii (CSV) | C | Fișier cu date agregate |

### 2.7 Administrare
| ID | Cerință | Prio | Verificare |
|----|---------|------|------------|
| FR-26 | Adminul listează, creează, dezactivează utilizatori și schimbă rolurile | M | Utilizatorul dezactivat nu se poate autentifica |
| FR-27 | Adminul gestionează materii și asocierile profesor–materie | M | Profesorul vede doar materiile lui |
| FR-28 | Jurnal al importurilor (cine, când, câte rânduri) | S | Intrări vizibile în panou |

## 3. Cerințe nefuncționale

| ID | Categorie | Cerință | Prio |
|----|-----------|---------|------|
| NFR-01 | Securitate | Parole hash-uite cu bcrypt (cost ≥ 12); niciodată în clar sau criptate reversibil | M |
| NFR-02 | Securitate | Validare obligatorie pe server pentru toate intrările; validare și pe client unde ajută | M |
| NFR-03 | Securitate | Interogări parametrizate (fără SQL concatenat); conținut afișat escapat (XSS) | M |
| NFR-04 | Securitate | Cookie de sesiune `HttpOnly`, `SameSite`; protecție CSRF pe cereri care modifică date | M |
| NFR-05 | Securitate | Secretele în `.env`, exclus din repository; `.env.example` fără valori reale | M |
| NFR-06 | Securitate | Limitarea încercărilor de autentificare (rate limiting) | S |
| NFR-07 | Accesibilitate | Etichete pentru formulare, navigare cu tastatura, contrast lizibil, mesaje de eroare clare | M |
| NFR-08 | Responsive | Interfață utilizabilă de la 360 px (telefon) la desktop | M |
| NFR-09 | Performanță | Răspuns API sub 500 ms pentru operații uzuale, la ~10.000 de întrebări | S |
| NFR-10 | Fiabilitate | Erori tratate unitar: cod HTTP corect și mesaj JSON `{error, details}` | M |
| NFR-11 | Testabilitate | Teste automate pentru fluxurile esențiale și pentru cazurile de eroare | M |
| NFR-12 | Reproductibilitate | Instalare și rulare cu pași documentați; date de demonstrație temporare | M |
| NFR-13 | Confidențialitate | Se stochează doar datele necesare (email, nume); fără date personale reale în demonstrații | M |
| NFR-14 | Întreținere | Cod structurat pe straturi (rute – servicii – acces la date) | S |

## 4. Prioritizare și limite

- **Pentru prototip (Etapa 3):** FR-01–03, FR-05–07, FR-11–13, FR-16, FR-21–22.
- **Pentru aplicația finală (Etapa 4):** toate cerințele **M** și **S**.
- **Doar dacă rămâne timp:** cerințele **C**.
