# Baze-de-date
#Construirea bazei de date – tabele, legături între tabele şi restricţii de integritate. Exemplificarea operaţiile LDD (CREATE, ALTER, DROP) asupra tabelelor (min 7).
CREATE TABLE client (
    id_client NUMBER(6) PRIMARY KEY,
    nume_client VARCHAR2(20),
    prenume_client VARCHAR2(20),
    adresa_client VARCHAR2(20),
    telefon_client NUMBER(10),
    email_client VARCHAR2(30)
);


CREATE TABLE bucatari_sef (
    id_bucatar_sef NUMBER(6) PRIMARY KEY,
    nume_bucatar_sef VARCHAR2(50),
    id_bucatar_subordonat NUMBER(6),
    FOREIGN KEY (id_bucatar_subordonat) REFERENCES bucatari(id_bucatar)
);

INSERT INTO bucatari_sef (id_bucatar_sef, nume_bucatar_sef, id_bucatar_subordonat)
VALUES (1, 'Ion Ionescu', 2);

ALTER TABLE client
RENAME TO clientii;

ALTER TABLE clientii
ADD CONSTRAINT ck_tlf check(telefon_client >0);

CREATE TABLE Plati(
    id_plati NUMBER(2) PRIMARY KEY,
    id_comanda NUMBER(4),
    id_client NUMBER(6)
);

ALTER TABLE Plati
ADD CONSTRAINT fk_plati_comanda FOREIGN KEY (id_comanda) REFERENCES Comanda(id_comanda);

ALTER TABLE Plati
ADD CONSTRAINT fk_plati_client FOREIGN KEY (id_client) REFERENCES clientii(id_client);

CREATE TABLE Comanda(
    id_comanda NUMBER(4) PRIMARY KEY,
    id_client NUMBER(6),
    data_comanda DATE,
    status_comanda VARCHAR2(20)
);

ALTER TABLE Comanda
ADD CONSTRAINT fk_comanda_client FOREIGN KEY (id_client) REFERENCES clientii(id_client);

CREATE TABLE design (
    id_design NUMBER(4) PRIMARY KEY,
    id_produs NUMBER(4),
    culoare VARCHAR2(20),
    forma VARCHAR2(20),
    aroma VARCHAR2(20),
    glazura VARCHAR2(20),
    alte_informatii VARCHAR2(30)
);


ALTER TABLE comanda_design
ADD CONSTRAINT fk_design_comanda FOREIGN KEY (id_comanda) REFERENCES Comanda(id_comanda);

CREATE TABLE produse_comanda(
    id_comanda NUMBER(4) PRIMARY KEY,
    id_produse NUMBER(12),
    ingrediente  VARCHAR2(20)
);

CREATE TABLE Produs(
    id_produs NUMBER(12) PRIMARY KEY,
    nume_produs VARCHAR2(20),
    pret_produs NUMBER(4),
    cantitate_produs NUMBER(6),
    descriere_produs VARCHAR2(30)
);

ALTER TABLE Produs
ADD CONSTRAINT ck_pret check(pret_produs>0);

ALTER TABLE Produs
ADD CONSTRAINT ch_cantitate check(cantitate_produs>0); 

CREATE TABLE comanda_design(
    id_design NUMBER(4),
    id_comanda NUMBER(4),
    pret_ornament NUMBER(3),
    descriere_design VARCHAR2(30)
);

ALTER TABLE comanda_design
ADD CONSTRAINT ck_int PRIMARY KEY(id_comanda);

ALTER TABLE comanda_design
ADD CONSTRAINT pret_ck check(pret_ornament>0);

CREATE TABLE bucatari(
    id_bucatar NUMBER(6) PRIMARY KEY,
    nume_bucatar VARCHAR2(50),
    specialitate VARCHAR2(50),
    experienta NUMBER(3),
    id_produs NUMBER(12)
);

ALTER TABLE bucatari
ADD CONSTRAINT fk_bucatari_prod FOREIGN KEY (id_produs) REFERENCES Produs(id_produs);

 

 


 

 

 


 
 
 

Exemple cu operaţii de actualizare a datelor: INSERT, UPDATE, DELETE, MERGE(opţional) (min 10). Obligatoriu, într-o tabelă trebuie să existe o înregistrare (rând) cu numele studentului, se va prezenta un printscreen după interogarea care demonstrează acest lucru. În caz contrar, proiectul va fi notat cu 1p. 
INSERT INTO comanda VALUES (1,2,TO_DATE('2023-12-31', 'YYYY-MM-DD'),'Platita');

DELETE FROM comanda
WHERE id_comanda = 1;

INSERT INTO clientii VALUES (1,'pop','ion','bucuresti',0776252112,'ionpop@gmail.com');
 INSERT INTO clientii VALUES (2,'Hotescu','Iulia-Alexanda','Buzau',077514004,'hotescuiulia22@gmail.com');
 INSERT INTO design_produs VALUES (12,'BLUE','LICHID','AFINE','NU','SIROP DE AFINE');
 INSERT INTO Produs VALUES (2,'ECLAIR','12','1','ECLERE CU CIOCOLATA');
 INSERT INTO Produs VALUES (3,'Tiramisu','12','12','Tiramisu');
INSERT INTO Produs VALUES (4,'','12','12','Tiramisu');

UPDATE Produs 
set cantitate_produs=cantitate_produs+12
where pret_produs=21;

UPDATE clientii
set nume_client='Ionescu'
where id_client=1;

DELETE FROM clientii
WHERE id_client=1;

DELETE FROM comanda
WHERE data_comanda<TO_DATE('01-04-2022','DD-MM-YYYY');
 
 
 


 
   

--Exemple de interogări cât mai variate şi relevante pentru tema aleasă (min 20) care să combine
--toate elementele de mai jos:

SELECT id_produs,nume_produs
FROM Produs
WHERE pret_produs > 10;

SELECT id_client,nume_client,adresa_client,telefon_client 
FROM clientii 
WHERE adresa_client LIKE '%Buzau%';

SELECT * FROM Produs 
WHERE descriere_produs IS not NULL;

SELECT id_produs,nume_produs,cantitate_produs FROM Produs 
WHERE cantitate_produs BETWEEN 1 AND 10;


SELECT id_comanda,status_comanda FROM Comanda 
JOIN clientii
ON Comanda.id_client = clientii.id_client
ORDER BY id_comanda;


SELECT TO_CHAR(data_comanda, 'DD-Mon-YYYY') data_comanda
FROM Comanda;

SELECT EXTRACT(YEAR FROM data_comanda) an_comanda 
FROM Comanda;

SELECT SUBSTR(nume_produs, 1, 3)  nume_scurt
FROM Produs;


SELECT c.id_client, c.nume_client, c.prenume_client
FROM clientii c
WHERE EXISTS (
    SELECT 1 
    FROM Comanda co 
    WHERE co.id_client = c.id_client 
    AND co.status_comanda like '%Platita%'
);


SELECT * FROM Produs 
WHERE cantitate_produs > (SELECT AVG(cantitate_produs) FROM Produs);

SELECT id_produs, nume_produs, pret_produs FROM Produs WHERE pret_produs <=15
UNION
SELECT id_produs, nume_produs, pret_produs FROM Produs WHERE cantitate_produs > 5;

SELECT id_client, COUNT(id_comanda)
FROM Comanda
GROUP BY id_client
HAVING COUNT(id_comanda) >=0;


CREATE VIEW V AS
SELECT id_client, nume_client
FROM clientii
WHERE adresa_client = 'Bucuresti';

SELECT NVL(nume_produs, 'Altă_valoare') FROM Produs;

SELECT * 
FROM Produs 
WHERE pret_produs != 12;


INSERT INTO clientii VALUES (2, 'Iulia', 'Hotescu', 'Buzau', 0771234567, 'iuliah@example.com');
DELETE FROM clientii WHERE id_client = 2;

UPDATE clientii
SET nume_client = 'Alexandra'
WHERE adresa_client='Buzau';

INSERT INTO bucatari (id_bucatar, nume_bucatar, specialitate, experienta, id_produs)
VALUES (1, 'Diana Lazar', 'Patiserie', 10, 2);

INSERT INTO bucatari (id_bucatar, nume_bucatar, specialitate, experienta, id_produs)
VALUES (2, 'Raluca Ion', 'Deserturi', 8, 3);

Select id_client from clientii
MINUS
SELECT id_client from comanda;




SELECT bs.id_bucatar_sef, bs.nume_bucatar_sef, bs.id_bucatar_subordonat, b.nume_bucatar
FROM bucatari_sef bs
JOIN bucatari b ON bs.id_bucatar_subordonat = b.id_bucatar
START WITH bs.id_bucatar_subordonat = 2
CONNECT BY PRIOR bs.id_bucatar_sef = bs.id_bucatar_subordonat;


SELECT id_produs FROM Produs
INTERSECT
SELECT id_produs FROM design;

Update comanda
SET status_comanda='neplatita'
WHERE data_comanda<TO_DATE('01-12-2023','DD-MM-YYYY');  
