

[pöhimõisted](README.md) | [andmebaassales](andmebaassales.md) | [kasutaja](kasutaja.md) | [protseduurid](protseduurid.md) | [triger](triger.md) | [keys](keys.md)
## SQL SERVER - kasutajate autentimine ja õiguste haödamine

**SQL serveris kasutatakse kahte peamist autentimise tüüpi:**

1. Windows Authentication Selle puhul kasutatkse samu kasutajaandemid,millega logitakse sisse Windows operatsioonisüsteemi
>Kasutajanimi ja parool on seotud windowsiga.Turvalisem lahendus. Paroole haldab Windows. Kasutaja ei pea eraldi SQL Serveri parooli teadma.

2. SQL Server Authentication
>Selle puhul luuakse kasutaja otse SQL Serverisse. Kasutaja ei ole seotud Windowsiga. Määratakse eraldi kasutajanimi ja parool. Sobib veebirakenduste jaoks.

## Näide kasutajast: DirectorKarolina. Parool: director

**Kasutaja loomine SQL Serveris**


1.Serveritaseme kasutaja loomine (Login) Sammud Ava:

Security → Logins Tee paremklikk ja vali:

New Login...

<img width="706" height="671" alt="{6664B458-B1DD-4E14-9E15-40D12923A7D2}" src="https://github.com/user-attachments/assets/36bae72f-0a61-4683-a86e-45b8f62cad11" />
Harjutamiseks võib eemaldada linnukese: User must change password at next login.

Server Roles Menüüst Server Roles saab määrata serveri üldised õigused.

Tavaliselt piisab rollist: public


<img width="709" height="661" alt="{F55C7230-10BE-499A-8639-F2BC657D360D}" src="https://github.com/user-attachments/assets/b84aca50-f747-4900-afb0-90d83588b3ab" />

2.Andmebaasi kasutaja loomine (User) Ava:

Database → Security → Users Tee paremklikk: New User...

<img width="344" height="290" alt="{42B6C9D6-41B9-415E-A477-7AB5C49974B1}" src="https://github.com/user-attachments/assets/93bcaa17-577a-4d9b-85e4-5e266bba9d8b" />


```sql
--GRANT -õiguste määramine
--DENY -õiguste keelamine
--anname kasutajale õigus vaadata tabelit (SELECT)
--lisada andmeid (INSERT)ning uuendada neid (UPDATE)
GRANT SELECT ON opilane TO directorKarolina;
GRANT INSERT ON opilane TO directorKarolina;
GRANT UPDATE ON opilane TO directorKarolina;
```
## Kasutaja õiguste kontroll

1.tuleb sisselogida kasutajana directorIrina. Connect--> Database Engine

<img width="491" height="502" alt="{DD8C7104-5C60-4880-B85A-B6C56404BF17}" src="https://github.com/user-attachments/assets/7007f153-6b0c-46be-964d-554b82e60b62" />

2.saab tabeli sisu näha ja sisestada uus kiri. 

<img width="672" height="577" alt="{848FD195-E7DF-44C9-92F6-7CE94D7F4332}" src="https://github.com/user-attachments/assets/04bf6967-3fc7-45ed-b7e8-8cb10f566796" />

3.kontrollime tegevus, mis ei ole lubatud kasutajale, näiteks tabeli loomine.

<img width="687" height="569" alt="{AB2CA544-7AD0-4066-A274-154971FAF3B7}" src="https://github.com/user-attachments/assets/9e55cdce-21dc-4203-8ec6-0f0248c0fdf4" />


<img width="713" height="554" alt="{476F3D85-96E1-4BE3-8C09-89005887F494}" src="https://github.com/user-attachments/assets/ef5edde9-5a41-428e-9f09-e916166eb81d" />


<img width="811" height="457" alt="{1DB7AFB2-969E-4A7B-9D2F-021AFDB1726F}" src="https://github.com/user-attachments/assets/e97c17e7-7c4f-4d99-b44b-3e3798f92369" />



