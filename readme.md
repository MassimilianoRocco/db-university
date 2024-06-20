Selezionare tutti gli studenti nati nel 1990 (160)  =>  SELECT * FROM `students` WHERE YEAR(date_of_birth) = 1990;
Selezionare tutti i corsi che valgono più di 10 crediti (479) => SELECT * FROM `courses` WHERE cfu > 10;
Selezionare tutti gli studenti che ad OGGI hanno almeno 30 anni compiuti (3725) => SELECT * FROM `students` WHERE YEAR(CURDATE()) - YEAR(date_of_birth) >= 30;
Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286) => SELECT * FROM `courses` WHERE year = 1 AND period = 'I semestre';
Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21) => SELECT * FROM `exams` WHERE hour > '14:00:00' AND date = '2020-06-20';
Selezionare tutti i corsi di laurea magistrale (38) => SELECT * FROM `degrees` WHERE level = 'magistrale';
Da quanti dipartimenti è composta l'università? (12) => SELECT * FROM `departments`;
Quanti sono gli insegnanti che non hanno un numero di telefono? (50) => SELECT * FROM `teachers` WHERE phone IS NULL;

Inserire nella tabella degli studenti un nuovo record con i propri dati (per il campo degree_id, inserire un valore casuale) => 
INSERT INTO `students`(`degree_id`, `name`, `surname`, `date_of_birth`, `fiscal_code`, `enrolment_date`, `registration_number`, `email`) 
VALUES ('16','Massimiliano','Rocco','1993-07-06','RCCMSM93L06H501K','2024-06-19','620675','massi1993@hotmail.it');

Cambiare il numero dell’ufficio del professor Pietro Rizzo in 126 => 
UPDATE teachers
SET office_number = 126
WHERE name='Pietro' AND surname= 'Rizzo';

Eliminare dalla tabella studenti il record creato precedentemente al punto 9 => 
DELETE FROM students WHERE name="Massimiliano" AND surname="Rocco" AND email='massi1993@hotmail.it';

// GROUP BY //
1. Contare quanti iscritti ci sono stati ogni anno => SELECT COUNT(*) FROM students GROUP BY YEAR(enrolment_date);
2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio => 
SELECT COUNT(*), office_address
FROM teachers
GROUP BY office_address;
3. Calcolare la media dei voti di ogni appello d'esame => 
SELECT AVG(vote), exam_id
FROM exam_student
GROUP BY exam_id;
4. Contare quanti corsi di laurea ci sono per ogni dipartimento => 
SELECT COUNT(*), department_id
FROM degrees
GROUP BY department_id;

// JOIN //
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia => 
SELECT degrees.id, degrees.name, students.name
FROM degrees
INNER JOIN students
ON degree_id = degrees.id
WHERE degrees.name = "Corso di Laurea in Economia";
2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze  => 
SELECT degrees.name, departments.name
FROM degrees
INNER JOIN departments
ON degrees.department_id = department_id
WHERE degrees.level = "magistrale" AND departments.name = "Dipartimento di Neuroscienze";
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44) => 
SELECT courses.id AS "id_corso", courses.name AS "nome_corso" , teachers.name AS "nome_insegnante"
FROM courses
INNER JOIN course_teacher
ON course_id = courses.id
INNER JOIN teachers
ON teacher_id = teachers.id
WHERE teachers.id = 44;
4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
SELECT courses.name AS "nome_materia", degrees.name AS "nome_corso", teachers.name AS "nome_insegnante"
FROM courses 
INNER JOIN degrees 
ON courses.degree_id = degrees.id
INNER JOIN course_teacher 
ON course_id = courses.id
INNER JOIN teachers
ON teachers.id = teacher_id; 
(non so se corretto)

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54) => 
SELECT teachers.name AS "Nome_insegnante", departments.name AS "Nome_dipartimento"
FROM departments
INNER JOIN degrees 
ON degrees.department_id = departments.id
INNER JOIN courses 
ON courses.degree_id = degrees.id
INNER JOIN course_teacher
ON course_teacher.course_id = courses.id
INNER JOIN teachers
ON teachers.id = course_teacher.teacher_id
WHERE departments.name = "Dipartimento di Biologia";
(no so se corretto)

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18.
