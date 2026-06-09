
 
 [pöhimõisted](README.md) | [andmebaassales](andmebaassales.md) | [kasutaja](kasutaja.md) | [protseduurid](protseduurid.md) | [triger](triger.md) | [keys](keys.md)
 # Primary Key 
Definitsioon: Veerg või veergude kombinatsioon, mis identifitseerib iga rea üheselt.
Milleks kasutatakse: Tagab, et tabelis pole duplikaate.
Erinevus: Primary Key ei tohi olla NULL ja võib olla ainult üks tabelis.

```sql
CREATE TABLE Opilane (
    id INT PRIMARY KEY,
    nimi VARCHAR(50),
    vanus INT
);
```
<img width="370" height="234" alt="{D5D4EC54-E954-41DD-9142-8198B0CE6FBD}" src="https://github.com/user-attachments/assets/23bc1e6e-bc35-480f-a08a-7e15368b9241" />

# Foreign Key
Definitsioon: Veerg, mis viitab teise tabeli Primary Key-le.
Milleks kasutatakse: Seob tabelid omavahel.
Erinevus: Foreign Key võib korduda, kuid väärtus peab eksisteerima viidatavas tabelis.

```sql
CREATE TABLE Tellimus (
    id INT PRIMARY KEY,
    opilane_id INT,
    FOREIGN KEY (opilane_id) REFERENCES Opilane(id)
);
```
<img width="420" height="258" alt="{3C35592B-1D22-4ACF-9473-16F89E88C432}" src="https://github.com/user-attachments/assets/4ebc177d-2437-4ec8-b9db-cbe339fa2ec8" />

# Unique Key
Definitsioon: Tagab, et veeru väärtused ei korduks.
Milleks kasutatakse: Väärtuste unikaalsuse tagamiseks.
Erinevus: Võib olla mitu Unique Key-d, erinevalt Primary Key-st.

```sql
CREATE TABLE Kasutaja (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE
);
```
<img width="402" height="273" alt="{93C4F479-63DB-4148-A8BE-535FF1801EC5}" src="https://github.com/user-attachments/assets/cb50cd00-9321-4807-b989-6f515b8d1bcd" />


# Simple Key
Definitsioon: Võti, mis koosneb ainult ühest veerust.
Milleks kasutatakse: Ühe veeru põhjal identifitseerimiseks.
Erinevus: Ei ole mitmest veerust nagu Composite Key.

```sql
CREATE TABLE Auto (
    regnr VARCHAR(20) PRIMARY KEY
);
```
<img width="374" height="201" alt="{6DB9AB75-652D-488A-9375-218BF422C42D}" src="https://github.com/user-attachments/assets/88d4a917-f2fd-4776-9ca9-3934a0ee66e0" />

# Composite Key
Definitsioon: Võti, mis koosneb mitmest veerust.
Milleks kasutatakse: Kui üks veerg ei ole piisav unikaalsuseks.
Erinevus: Koosneb vähemalt kahest veerust.
```sql
CREATE TABLE Osalemine (
    opilane_id INT,
    kursus_id INT,
    PRIMARY KEY (opilane_id, kursus_id)
);
```
<img width="390" height="221" alt="{F21E418E-68F1-4974-BBDE-445A76C9BF7B}" src="https://github.com/user-attachments/assets/703826d9-d955-4111-9593-b68a1a85841e" />

# Compound Key
Definitsioon: Sisuliselt sama mis Composite Key – mitmest veerust koosnev võti.
Milleks kasutatakse: Mitme veeru kombinatsiooni unikaalsuse tagamiseks.
Erinevus: Mõnes kirjanduses kasutatakse Composite/Compound sünonüümidena.
```sql
CREATE TABLE Hinne (
    opilane_id INT,
    aine_id INT,
    kuupaev DATE,
    PRIMARY KEY (opilane_id, aine_id)
);
```
<img width="379" height="233" alt="{726906FE-B625-4634-B182-79677814CDFA}" src="https://github.com/user-attachments/assets/8d99b568-697b-4026-b7ab-60c5877cea04" />

# Superkey
Definitsioon: Iga veergude kombinatsioon, mis suudab rea üheselt identifitseerida.
Milleks kasutatakse: Loogilise mudeli analüüsiks.
Erinevus: Võib sisaldada üleliigseid veerge.

```sql
CREATE TABLE Toode (
    id INT,
    kood VARCHAR(20),
    nimetus VARCHAR(50),
    UNIQUE(id, kood) 
);
```
<img width="442" height="252" alt="{30814157-2902-4968-A782-FBFDD30C1088}" src="https://github.com/user-attachments/assets/9f0f2db9-fa5e-4535-9d38-e7e093beee67" />

# Candidate Key
Definitsioon: Kõik võimalikud võtmed, mis võiksid olla Primary Key.
Milleks kasutatakse: Valitakse nende seast Primary Key.
Erinevus: Candidate Key ei sisalda üleliigseid veerge (erinevalt Superkey-st).
```sql
CREATE TABLE Raamat (
    id INT,
    isbn VARCHAR(20),
    UNIQUE(id),
    UNIQUE(isbn)
);
```
<img width="452" height="236" alt="{1A517865-6919-4C08-BFED-1B7717E88ED9}" src="https://github.com/user-attachments/assets/7c26052d-0294-47b9-9bbd-98330f6385c0" />

# Alternate Key
Definitsioon: Candidate Key, mida EI valitud Primary Key-ks.
Milleks kasutatakse: Alternatiivne unikaalne identifikaator.
Erinevus: Primary Key on üks, Alternate Key-sid võib olla mitu.

```sql
CREATE TABLE Inimene (
    id INT PRIMARY KEY,
    isikukood VARCHAR(20) UNIQUE -- alternate key
);
```

<img width="489" height="270" alt="{94943AB2-4BD4-4459-BC67-240BB77E189C}" src="https://github.com/user-attachments/assets/58c79b6c-bffa-447a-99fa-f69ed17d8947" />


