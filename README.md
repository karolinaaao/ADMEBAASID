## KAROLINA ANDMEBAASIDE KONSPEKT

[pöhimõisted](README.md) | [andmebaassales](andmebaassales.md) | [kasutaja](kasutaja.md) | [protseduurid](protseduurid.md) | [triger](triger.md) | 


**SQL - structured Query Language - struktureeritud päringukeel**

**DDL - Data Definition Language -andmebaasi struktuuri loomiseks - CREATE, ALTER**

**DML - Data Manipulation Language -andmete lisamine ja uuendamine tabelis - INSERT, UPDATE, DELETE**

**INT**

Terve arv.
Kasutatakse ID‑de, koguste, vanuse jaoks.
Näide: 5, 120, 9999.

**SMALLINT**

Väiksema ulatusega terve arv.
Kasutatakse väikeste väärtuste jaoks (nt tubade arv, inimeste arv).
Näide: 1, 25, 300.

**DECIMAL**

Täpsete komakohtadega arv.
Kasutatakse raha, hindade ja täpsete mõõtmiste jaoks.
Näide: 15.90, 1234.56.

**FLOAT**

Ujuvkomaarv (mitte väga täpne).
Kasutatakse ligikaudsete väärtuste jaoks: temperatuur, kaugus, protsendid.
Näide: 3.14159, 0.0001.

**CHAR**

Fikseeritud pikkusega tekst.
Kui on CHAR(10), siis tekst on alati 10 märki pikk.
Kasutatakse koodide jaoks, kus pikkus ei muutu.
Näide: 'EE12345678'.

**VARCHAR**

Muutuva pikkusega tekst.
Kõige sagedamini kasutatav tekstiväli.
Näide: 'Tallinn', 'Harry Potter'.

**TEXT**

Pikk tekst.
Kasutatakse kirjelduste, kommentaaride, logide jaoks.
Näide: 'See on pikk kirjeldus...'.

**DATE**

Ainult kuupäev.
Näide: 2026‑06‑02.

**TIME**

Ainult kellaaeg.
Näide: 14:30:00.
DATETIME

Kuupäev + kellaaeg koos.
Kasutatakse logides, tellimustes, sündmustes.
Näide: 2026‑06‑02 14:30:00.

**BOOLEAN**

Tõeväärtus: TRUE / FALSE.
SQL Serveris kasutatakse BIT (0 = false, 1 = true).
Näide: 1 või 0.

