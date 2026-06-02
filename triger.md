

[pöhimõisted](README.md) | [andmebaassales](andmebaassales.md) | [kasutaja](kasutaja.md) | [protseduurid](protseduurid.md) | [triger](triger.md) | 
## Trigger - päästik
### SQL triggerid on spetsiaalsed andmebaasi objektid, mis käivituvad automaatselt, kui toimub teatud sündmus (nt INSERT, UPDATE või DELETE)
```sql
--Trigger lisatud kirjeid jälgimiseks tabelis “linnad” – INSERt
--Jälgib andmete sisestamine tabelis linnad ja teeb vastava kirje tabelis logi
	
CREATE TRIGGER linnaLisamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR INSERT
AS
INSERT INTO logi(kuupaev,kasutaja, toiming, andmed)
SELECT
GETDATE(),  --aeg
SYSTEM_USER, --kasutaja mis on logitud servirisse 
'on tehtud INSERT käsk',  --toiming
concat('linn: ', inserted.linnanimi, ', rahvaarv: ', inserted.rahvaarv) --andmed tabelist linnad
FROM inserted;
```
<img width="593" height="436" alt="{ED22FF99-1906-413E-BD88-708AA7552B37}" src="https://github.com/user-attachments/assets/d89b88a3-be54-49ad-8fad-135494c23c74" />

```sql
--DELETE triger,отслеживает удаления
CREATE TRIGGER linnaKustutamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR DELETE
AS
INSERT INTO logi(kuupaev,kasutaja, toiming, andmed)
SELECT
GETDATE(),  --aeg
SYSTEM_USER, --kasutaja mis on logitud servirisse 
'on tehtud DELETE käsk',  --toiming
concat('linn: ', deleted.linnanimi, ', rahvaarv: ', deleted.rahvaarv) --andmed tabelist linnad
FROM deleted;
```
```sql
--drop trigger ...(nimi) 
--вкл выфкл триггер
DISABLE TRIGGER linnaLisamine ON linnad;
ENABLE TRIGGER linnaLisamine ON linnad;
```
<img width="763" height="287" alt="{5FB413F6-AA12-4698-BCCE-ACFB742EADEF}" src="https://github.com/user-attachments/assets/79b5d3ef-37e0-4fa8-bf48-3ccd9d091a49"
/>
```sql
--DELETE trigger
DELETE FROM linnad WHERE linnID=3;
SELECT * FROM logi;
SELECT * FROM linnad;
```
```sql
--Kombineerime INSERT ja DELETE triggerid
DISABLE TRIGGER linnaLisamine ON linnad;
DISABLE TRIGGER linnaKustutamine ON linnad;

CREATE TRIGGER linnaLisaKustuta
ON linnad --tabelinimi, mis on vaja jälgida
FOR INSERT, DELETE
AS
BEGIN
SET NOCOUNT ON;
	INSERT INTO logi(kuupaev,kasutaja, toiming, andmed)

	SELECT
	GETDATE(),  --aeg
	SYSTEM_USER, --kasutaja mis on logitud servirisse 
	'on tehtud INSERT käsk',  --toiming
	concat('linn: ', inserted.linnanimi, ', rahvaarv: ', inserted.rahvaarv) --andmed tabelist linnad
	FROM inserted

	UNION ALL 

	SELECT
	GETDATE(),  --aeg
	SYSTEM_USER, --kasutaja mis on logitud servirisse 
	'on tehtud DELETE käsk',  --toiming
	concat('linn: ', deleted.linnanimi, ', rahvaarv: ', deleted.rahvaarv) --andmed tabelist linnad
	FROM deleted;
END;
```
```sql
--Kontroll
--INSERT triggeri kontroll
insert into linnad(linnanimi, rahvaarv)
values('Narva', 16123);


DELETE FROM linnad WHERE linnID=4;
SELECT * FROM logi;
SELECT * FROM linnad;
```
<img width="855" height="547" alt="{7D11DD6B-F270-40F6-8FB7-2317CB972EB9}" src="https://github.com/user-attachments/assets/1373dff0-58f0-42e0-91ba-68e6ea5bed08" />

```sql
--update triger

CREATE TRIGGER linnaUuendamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR UPDATE
AS
INSERT INTO logi(kuupaev,kasutaja, toiming, andmed)
SELECT
GETDATE(),  --aeg
SYSTEM_USER, --kasutaja mis on logitud servirisse 
'on tehtud UPDATE käsk',  --toiming
concat('vanad andmed - linn: ', deleted.linnanimi, ', rahvaarv: ', deleted.rahvaarv,
'uued andmed -linn: ',inserted.linnanimi, ',rahvaarv -', inserted.rahvaarv) --andmed tabelist linnad
FROM deleted INNER JOIN inserted
ON deleted.linnID=inserted.linnID;
```
```sql
--UPDATE Kontroll
UPDATE linnad SET linnanimi='Tallinn11', rahvaarv=0 WHERE linnanimi='Tallinn';
SELECT * FROM logi;
SELECT * FROM linnad;
```
<img width="979" height="493" alt="{7A684FE9-D26D-4E38-8B13-B0ADE9B4D72B}" src="https://github.com/user-attachments/assets/ea5132ae-ea5c-4436-bc92-65b304bd588f" />

**kasutaja loomine**

```sql
--kasutaja sekretarKarolina, parool 12345
--õigused - sekretarKarolina ei saa luua ehk muuta trigeri,ei näi tabeli logi
--saab ainult näha, lisada, ja kustutada tabelist linnad
--разрешаем
GRANT SELECT, INSERT, DELETE ON linnad TO sekretarKarolina;
--запрещаем
DENY SELECT,DELETE ON logi TO sekretarKarolina;

```

<img width="651" height="263" alt="{99144824-00D3-4F75-A4AB-4BA42C2A55F2}" src="https://github.com/user-attachments/assets/9a4e97d4-73be-4c2e-ad8d-53b4be2522c1" />

<img width="593" height="192" alt="{49332EA8-0896-45F4-A069-517A3C01574C}" src="https://github.com/user-attachments/assets/e28323b4-abe8-4060-8059-3d2c20dfb1e4" />

<img width="1027" height="214" alt="{F1CEDB7E-61DB-46FA-B380-FC6E001F73CA}" src="https://github.com/user-attachments/assets/8152c8fd-6382-44cd-9c34-0fc24714ce8d" />

<img width="691" height="744" alt="{7B7A1213-3ED9-4357-9007-A19EAAF48980}" src="https://github.com/user-attachments/assets/034c3f58-c3e9-4ec7-bb19-01817c9c3ee8" />

<img width="695" height="740" alt="{C27BC7BE-D67B-4A73-BB69-1C95C8473D5D}" src="https://github.com/user-attachments/assets/c9044c14-6431-4b4f-9606-399464bf2e3d" />

<img width="693" height="740" alt="{826EABB6-F12F-462E-A5D5-41C7A7A53C68}" src="https://github.com/user-attachments/assets/57f68440-1d67-45de-b701-0b6e492ac285" />

**sql**

```sql
create database trigerlogitpv24;
use trigerlogitpv24;

--1.tabel kuhu sekretaar lisab andmeid

Create table linnad(
linnID int PRIMARY KEY IDENTITY (1,1),
linnanimi varchar(15) NOT NULL unique,
rahvaarv int);

insert into linnad(linnanimi, rahvaarv)
values('Tartu', 125633)

SELECT * FROM linnad;
--2.table kus automaatselt salvetatakse 1.tabeli muudatused--loogi

Create table logi(
id int PRIMARY KEY IDENTITY (1,1),
kuupaev DATETIME,
kasutaja varchar(30),
toiming  varchar(100),--tegevus
andmed TEXT --tabelist linnad
)

SELECT * FROM logi;

--Trigger lisatud kirjeid jälgimiseks tabelis “linnad” – INSERt
--Jälgib andmete sisestamine tabelis linnad ja teeb vastava kirje tabelis logi
	
CREATE TRIGGER linnaLisamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR INSERT
AS
INSERT INTO logi(kuupaev,kasutaja, toiming, andmed)
SELECT
GETDATE(),  --aeg
SYSTEM_USER, --kasutaja mis on logitud servirisse 
'on tehtud INSERT käsk',  --toiming
concat('linn: ', inserted.linnanimi, ', rahvaarv: ', inserted.rahvaarv) --andmed tabelist linnad
FROM inserted;

--INSERT trigeri kontroll
insert into linnad(linnanimi, rahvaarv)
values('Narva', 16123);

SELECT * FROM logi;
SELECT * FROM linnad;

--DELETE triger,отслеживает удаления
CREATE TRIGGER linnaKustutamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR DELETE
AS
INSERT INTO logi(kuupaev,kasutaja, toiming, andmed)
SELECT
GETDATE(),  --aeg
SYSTEM_USER, --kasutaja mis on logitud servirisse 
'on tehtud DELETE käsk',  --toiming
concat('linn: ', deleted.linnanimi, ', rahvaarv: ', deleted.rahvaarv) --andmed tabelist linnad
FROM deleted;

--drop trigger ...(nimi) 
--вкл выфкл триггер
DISABLE TRIGGER linnaLisamine ON linnad;
ENABLE TRIGGER linnaLisamine ON linnad;

--DELETE trigger
DELETE FROM linnad WHERE linnID=3;
SELECT * FROM logi;
SELECT * FROM linnad;


--Kombineerime INSERT ja DELETE triggerid
DISABLE TRIGGER linnaLisamine ON linnad;
DISABLE TRIGGER linnaKustutamine ON linnad;

CREATE TRIGGER linnaLisaKustuta
ON linnad --tabelinimi, mis on vaja jälgida
FOR INSERT, DELETE
AS
BEGIN
SET NOCOUNT ON;
	INSERT INTO logi(kuupaev,kasutaja, toiming, andmed)

	SELECT
	GETDATE(),  --aeg
	SYSTEM_USER, --kasutaja mis on logitud servirisse 
	'on tehtud INSERT käsk',  --toiming
	concat('linn: ', inserted.linnanimi, ', rahvaarv: ', inserted.rahvaarv) --andmed tabelist linnad
	FROM inserted

	UNION ALL 

	SELECT
	GETDATE(),  --aeg
	SYSTEM_USER, --kasutaja mis on logitud servirisse 
	'on tehtud DELETE käsk',  --toiming
	concat('linn: ', deleted.linnanimi, ', rahvaarv: ', deleted.rahvaarv) --andmed tabelist linnad
	FROM deleted;
END;
--Kontroll
--INSERT triggeri kontroll
insert into linnad(linnanimi, rahvaarv)
values('Narva', 16123);


DELETE FROM linnad WHERE linnID=4;
SELECT * FROM logi;
SELECT * FROM linnad;

--update triger

CREATE TRIGGER linnaUuendamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR UPDATE
AS
INSERT INTO logi(kuupaev,kasutaja, toiming, andmed)
SELECT
GETDATE(),  --aeg
SYSTEM_USER, --kasutaja mis on logitud servirisse 
'on tehtud UPDATE käsk',  --toiming
concat('vanad andmed - linn: ', deleted.linnanimi, ', rahvaarv: ', deleted.rahvaarv,
'uued andmed -linn: ',inserted.linnanimi, ',rahvaarv -', inserted.rahvaarv) --andmed tabelist linnad
FROM deleted INNER JOIN inserted
ON deleted.linnID=inserted.linnID;

--UPDATE Kontroll
UPDATE linnad SET linnanimi='Tallinn11', rahvaarv=0 WHERE linnanimi='Tallinn';
SELECT * FROM logi;
SELECT * FROM linnad;

--kasutaja sekretarKarolina, parool 12345
--õigused - sekretarKarolina ei saa luua ehk muuta trigeri,ei näi tabeli logi
--saab ainult näha, lisada, ja kustutada tabelist linnad
--разрешаем
GRANT SELECT, INSERT, DELETE ON linnad TO sekretarKarolina;
--запрещаем
DENY SELECT,DELETE ON logi TO sekretarKarolina;

EXEC sp_helptext 'LinnaUuendamine'
```



