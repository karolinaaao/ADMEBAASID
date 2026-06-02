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

