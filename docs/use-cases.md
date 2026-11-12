# Scenarii de utilizare – StudyHub

Fiecare scenariu are actor, precondiții, pași, rezultat și **criterii de verificare** (cum demonstrăm că funcționează). Scenariile alternative acoperă situațiile de eroare.

---

## UC-01 – Profesorul creează o întrebare

- **Actor:** Profesor
- **Cerințe acoperite:** FR-03, FR-06, FR-07, FR-09
- **Precondiții:** profesorul este autentificat și asociat materiei „Baze de date", care are capitolul „SQL – JOIN-uri".

**Flux principal**
1. Profesorul deschide *Banca de întrebări* → materia „Baze de date" → **Întrebare nouă**.
2. Completează textul, adaugă 4 variante, marchează una corectă, alege capitolul, dificultatea „mediu" și scrie explicația.
3. Apasă **Salvează**.
4. Sistemul validează datele pe server, salvează întrebarea și variantele și afișează mesajul „Întrebare salvată".
5. Întrebarea apare în lista materiei, la capitolul ales.

**Fluxuri alternative**
- *3a. Nicio variantă corectă sau textul gol:* serverul răspunde 400 cu detalii pe câmp; formularul afișează erorile lângă câmpuri; nu se salvează nimic.
- *3b. Text cu cod HTML (`<script>`):* se salvează ca text și se afișează escapat, fără executare.
- *1a. Utilizator cu rol student:* accesul la pagină este refuzat (403).

**Postcondiție:** întrebarea există în baza de date, legată de materie și capitol.

**Verificare:** (1) întrebarea apare în listă și în baza de date; (2) cerere fără variantă corectă → 400; (3) cerere cu cont de student → 403.

---

## UC-02 – Studentul rezolvă un test de exersare

- **Actor:** Student
- **Cerințe acoperite:** FR-11, FR-12, FR-13, FR-16
- **Precondiții:** studentul este autentificat; materia are cel puțin 10 întrebări active.

**Flux principal**
1. Studentul alege **Test nou** → materia „Baze de date", capitolele dorite și 10 întrebări.
2. Sistemul alege aleatoriu 10 întrebări distincte și afișează testul (o întrebare pe ecran sau toate pe pagină).
3. Studentul selectează câte un răspuns pentru fiecare întrebare (poate reveni la cele anterioare).
4. Apasă **Trimite testul**; dacă există întrebări fără răspuns, primește un avertisment și poate confirma.
5. Serverul compară răspunsurile cu variantele corecte, calculează scorul (ex. 7/10 = 70%) și salvează rezolvarea.
6. Ecranul de rezultat afișează scorul și, pentru fiecare întrebare, răspunsul ales, cel corect și explicația.

**Fluxuri alternative**
- *1a. Mai puține întrebări disponibile decât cele cerute:* mesaj „Sunt disponibile doar N întrebări"; studentul alege un număr mai mic.
- *5a. Trimiterea repetată a aceluiași test:* serverul respinge a doua trimitere (409); scorul inițial rămâne.
- *5b. Încercare de a trimite răspunsuri pentru o opțiune care nu aparține întrebării:* 400, nimic salvat.

**Postcondiție:** rezolvarea și răspunsurile sunt în istoric.

**Verificare:** (1) testul conține exact 10 întrebări distincte; (2) scorul afișat = răspunsuri corecte calculate manual; (3) a doua trimitere → 409; (4) rezolvarea apare în istoric.

---

## UC-03 – Studentul își urmărește progresul

- **Actor:** Student
- **Cerințe acoperite:** FR-16, FR-17, FR-18
- **Precondiții:** studentul are cel puțin 3 teste rezolvate la aceeași materie.

**Flux principal**
1. Studentul deschide **Progresul meu** și alege materia.
2. Sistemul afișează graficul evoluției scorurilor (axa X: dată, axa Y: scor %).
3. Sistemul afișează procentul de reușită pe capitole; capitolele sub 60% sunt marcate „de recapitulat".
4. Studentul deschide un test din istoric și vede din nou feedbackul detaliat.

**Fluxuri alternative**
- *1a. Niciun test rezolvat:* mesaj „Nu ai încă teste rezolvate" și buton către *Test nou*.
- *2a. Eroare la încărcarea datelor:* mesaj clar și opțiune de reîncărcare.

**Postcondiție:** nu se modifică date (scenariu de consultare).

**Verificare:** (1) valorile graficului coincid cu istoricul; (2) procentele pe capitol coincid cu calculul manual pe un set de date de test; (3) un student nu vede datele altui student (acces direct la ID străin → 403/404).

---

## UC-04 – Profesorul importă întrebări din CSV

- **Actor:** Profesor
- **Cerințe acoperite:** FR-21, FR-22, FR-23, FR-24
- **Precondiții:** profesorul este autentificat; are un fișier CSV cu antetul `chapter,text,difficulty,option_1,option_2,option_3,option_4,correct,explanation`.

**Flux principal**
1. Profesorul alege materia → **Import CSV** și încarcă fișierul.
2. Serverul verifică tipul și dimensiunea fișierului (max. 2 MB), apoi antetul.
3. Pentru fiecare rând validează: text nenul, dificultate validă, minimum 2 variante, `correct` între 1 și numărul variantelor, capitol existent (sau creat, conform setării).
4. Rândurile valide sunt salvate; rândurile duplicate sunt omise.
5. Sistemul afișează raportul: „18 importate, 1 omis (duplicat), 2 respinse" și lista erorilor cu **numărul rândului** și motivul.
6. Profesorul exportă ulterior banca materiei într-un CSV, pe care îl poate reimporta.

**Fluxuri alternative**
- *2a. Fișier cu alt tip sau prea mare:* 400/413 cu mesaj.
- *2b. Antet lipsă sau incorect:* import refuzat, cu lista coloanelor așteptate.
- *3a. Rând cu `correct = 9` la 4 variante:* rândul este respins cu mesajul „Variantă corectă invalidă"; restul fișierului continuă.
- *3b. Câmp cu virgulă sau ghilimele* (`"Ce face SELECT, FROM?"`): se interpretează corect conform RFC 4180.

**Postcondiție:** întrebările valide există în bancă; importul este înregistrat în jurnal.

**Verificare:** (1) un CSV de test cu 20 de rânduri (2 invalide) produce 18 întrebări și 2 erori cu rândurile corecte; (2) reimportul aceluiași fișier nu produce dubluri; (3) exportul produce un fișier care se reimportă fără erori.

---

## UC-05 – Administratorul gestionează utilizatorii

- **Actor:** Administrator
- **Cerințe acoperite:** FR-03, FR-26, FR-27
- **Precondiții:** administratorul este autentificat.

**Flux principal**
1. Administratorul deschide **Administrare → Utilizatori**.
2. Caută un cont după email și îi schimbă rolul din „student" în „profesor".
3. Deschide **Materii** și asociază profesorul materiei „Baze de date".
4. Dezactivează un cont care nu mai trebuie să aibă acces.

**Fluxuri alternative**
- *2a. Adminul încearcă să-și retragă singur rolul sau să-și dezactiveze contul:* operația este blocată (cel puțin un admin activ trebuie să existe).
- *4a. Utilizatorul dezactivat încearcă să se autentifice:* primește mesajul „Cont dezactivat".

**Verificare:** (1) după schimbarea rolului, contul poate accesa funcțiile de profesor; (2) contul dezactivat nu se mai poate autentifica; (3) un cont non-admin primește 403 pe rutele `/api/admin/*`.
