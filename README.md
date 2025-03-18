# Database-Project-for-ShoppingCart
The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

Application under test: ShoppingCart

Tools used: MySQL Workbench

This database is designed to manage relationships between customers and the products they purchase. The primary goal is to track customer information, available products, and placed orders, facilitating sales management and data analysis.



# Database Schema

The following database schema represents the structure of the tables and their relationships:

-clienti is connected with client_produse through a one-to-many relationship. The connection is implemented using clienti.id_clienti as the primary key and client_produse.id_clienti as a foreign key.
-produs is connected with client_produse through a one-to-many relationship. The connection is implemented using produs.id_produs as the primary key and client_produse.id_produs as a foreign key.
This schema enables tracking customer purchases and analyzing product sales.



# Database Queries
DDL (Data Definition Language)

The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS):
CREATE TABLE clienti (
    id_clienti INT PRIMARY KEY,
    nume VARCHAR(50),
    email VARCHAR(150) 
);

CREATE TABLE produs (
    id_produs INT PRIMARY KEY,
    nume VARCHAR(30) NOT NULL,
    cod CHAR(5)
);
CREATE TABLE client_produse (
    id_clienti INT,
    id_produs INT,
    PRIMARY KEY(id_clienti, id_produs),
    FOREIGN KEY (id_clienti) REFERENCES clienti(id_clienti),
    FOREIGN KEY (id_produs) REFERENCES produs(id_produs)
);



After the database and the tables have been created, a few ALTER instructions were written in order to update the structure of the database, as described below:
ALTER TABLE clienti
ADD COLUMN data_nasterii DATE;


ALTER TABLE clienti 
MODIFY email VARCHAR(150);

DROP TABLE client_produse;

TRUNCATE TABLE clienti;


# DML (Data Manipulation Language)

In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase.

Below you can find all the insert instructions that were created in the scope of this project:
INSERT INTO clienti (id_clienti, nume, email)
VALUES (1, 'Ion Popescu', 'ion.popescu@email.com');

INSERT INTO clienti
VALUES (2, 'Maria Ionescu', 'maria.ionescu@email.com');


After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:
UPDATE clienti
SET email = 'ion.popescu2024@email.com'
WHERE id_clienti = 1;

UPDATE clienti
SET email = 'default@email.com';

After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean:
DELETE FROM clienti
WHERE id_clienti = 2;

DELETE FROM clienti;

# DQL (Data Query Language)

In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:
SELECT * FROM clienti;

SELECT nume, email FROM clienti;

SELECT * FROM clienti WHERE id_clienti = 1;

SELECT * FROM clienti WHERE nume LIKE 'Ion%';

SELECT * FROM clienti
WHERE nume LIKE 'I%' AND email LIKE '%@gmail.com';

SELECT * FROM clienti
WHERE nume LIKE 'I%' OR email LIKE '%@yahoo.com';

SELECT COUNT(*) AS numar_clienti FROM clienti;

SELECT nume, COUNT(*) AS numar_produs 
FROM produs 
GROUP BY nume
HAVING COUNT(*) > 10;

SELECT clienti.nume, produs.nume
FROM clienti
INNER JOIN client_produse ON clienti.id_clienti = client_produse.id_clienti
INNER JOIN produs ON client_produse.id_produs = produs.id_produs;

SELECT clienti.nume, produs.nume
FROM clienti
LEFT JOIN client_produse ON clienti.id_clienti = client_produse.id_clienti
LEFT JOIN produs ON client_produse.id_produs = produs.id_produs;

SELECT clienti.nume, produs.nume
FROM clienti
RIGHT JOIN client_produse ON clienti.id_clienti = client_produse.id_clienti
RIGHT JOIN produs ON client_produse.id_produs = produs.id_produs;

SELECT clienti.nume, produs.nume
FROM clienti
CROSS JOIN produs;

SELECT * FROM clienti ORDER BY nume ASC;

SELECT * FROM clienti ORDER BY nume ASC LIMIT 5;

SELECT nume FROM clienti 
WHERE id_clienti IN (SELECT id_clienti FROM comenzi);

SELECT nume, (SELECT COUNT(*) FROM comenzi WHERE comenzi.id_clienti = clienti.id_clienti) AS numar_produse
FROM clienti;

SELECT nume_produs, pret FROM produs 
WHERE pret > (SELECT AVG(pret) FROM produs);

SELECT id_produs, COUNT(*) AS total_vanzari
FROM vanzari
GROUP BY id_produs
HAVING COUNT(*) > (SELECT AVG(total_vanzari) FROM (SELECT COUNT(*) AS total_vanzari FROM vanzari GROUP BY id_produs) AS subquery);

# Conclusions

Throughout this project, we have learned and applied fundamental and advanced concepts related to database design and management. By working with DDL (Data Definition Language) statements such as CREATE, ALTER, and DROP, we gained a deeper understanding of how to structure a relational database efficiently.

Final Thoughts:
This project provided valuable hands-on experience in designing, creating, and modifying databases. By implementing different SQL commands and best practices, we reinforced our understanding of database management and how it plays a crucial role in application development. Moving forward, this knowledge will be beneficial for optimizing and scaling databases for real-world applications.




