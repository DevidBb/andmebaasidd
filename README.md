# andmebaasidd
my
--trigerid
--sql triger - protsess
--mille abil tema sisse 
--kirjutad tegevused automaatselt käivitatakse

--insert; update; delete trigerid
--mis jälgivad antud tegevused tabelites
--ja kirjutavad logi tabeli mida nad jälgisid

CREATE DATABASE trigerLOGITPV;
USE trigerLOGITPV

CREATE TABLE toode(
toodeID int PRIMARY KEY identity(1,1),
toodeNimetus varchar(25),
toodeHind decimal (5,2))
CREATE TABLE logi(
id int PRIMARY KEY identity(1,1),
tegevus varchar(25),
kuupaev datetime,
andmed TEXT)

CREATE TRIGGER toodeLisamine 
ON toode
FOR INSERT 
AS 
INSERT INTO logi(tegevus, kuupaev, andmed)
SELECT 'toode on lisatud',
GETDATE(),
inserted.toodeNimetus,
FROM inserted;

DROP TRIGGER toodeLisamine;

--kontroll
INSERT  INTO toode(toodeNimetus, toodeHind)
VALUES ('cola', 2.20);

SELECT * FROM toode;
SELECT * FROM logi;

--DELETE TRIGER, mis jälgib toode kustutamine tableis
CREATE TRIGGER toodeKustutamine
ON toode
FOR DELETE
AS
INSERT INTO logi(tegevus, kuupaev, andmed)
SELECT 'toode on kustutatud',
GETDATE(),
CONCAT(deleted.ToodeNimetus,
'| tegi kasutaja ' , USER)
FROM deleted;


--kotnroll
DELETE FROM toode
WHERE toodeID=3;
SELECT * FROM toode;
SELECT * FROM logi;

--update triger, mis jälgib toode uuendamine tabelis
CREATE TRIGGER toodeUuendamine
ON toode
FOR UPDATE
AS 
INSERT INTO logi(tegevus, kuupaev, andmed)
SELECT 'toode on lisatud',
GETDATE(),
CONCAT('Vanad andmed - ', deleted.toodeNimetus,', ', deleted.toodeHind,
'\nUues andmed -', inserted.toodenimetus,', ',inserted.toodehind,
'| tegi kasutaja ', SYSTEM_USER)
FROM deleted INNER JOIN inserted
on deleted.toodeIT=inserted.toodeIT;

--kontroll
UPDATE toode SET toodeHind=4.00
WHERE toodeNimetus='Fanta'
SELECT * FROM toode;
SELECT * FROM logi;

create table production(
productID int not null primary key identity(1,1),
productName varchar (252) not null,
brandID int not null,
categoryID int not null,
modelYear int not null,
listPrice DEC(10,2) not null,)
