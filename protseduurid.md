## SQL protseduur - 
store procedure - salvestatud protseduurid - sama mis on fumnktsoonid programmeerimises, mingi tegevus mis on salvestatud 
andmebaasi, ja mida saab automaatselt teha (INSERT, UPDATE, SELECT, DELETE).

```sql
--protseduur mis lisab andmeid tabelisse ja kohe kuvab neid.(INSERT, SELECT)
CREATE procedure lisaKategooria 
--parameetrid @...
@uusKategooria varchar(30)
AS
BEGIN
--kirjeldus	
	INSERT INTO categories(category_name)
	VALUES (@uusKategooria);
	SELECT * FROM categories;
END;
--kutse 
EXEC lisaKategooria 'Auto';
```
<img width="257" height="366" alt="{F64F09F6-182C-4C15-B672-301839077B72}" src="https://github.com/user-attachments/assets/ffe3dae3-792a-481e-ab59-d9f824956706" />

```sql

--protseduur, mis kustutab kategooria id järgi 
CREATE procedure kustutaKategooria
@kustutaId int
AS
BEGIN
	SELECT * FROM categories;
	DELETE FROM categories WHERE category_id=@kustutaId;
	SELECT * FROM categories;
END
-- kutse
EXEC kustutaKategooria 1
```
<img width="356" height="291" alt="{CCAAC61C-F432-47E7-A880-3E7ABB90610D}" src="https://github.com/user-attachments/assets/92c6bcf5-5236-4020-a173-97e1db37779c" />

```sql
--protseduur mis kuvab kategooriad sisestatud esimese tähte järgi 
-- процедуры которые показывает все процедуры по первой введенной букве
CREATE PROCEDURE otsing1taht
@taht char(1)
AS
BEGIN
	SELECT * FROM categories 
	WHERE category_name LIKE @taht + '%'; --% - teised sümboolid
END

--kutse 
EXEC otsing1taht 'A';
```
<img width="284" height="100" alt="{1213B838-1BF6-46E4-B436-EA9462B0ADDA}" src="https://github.com/user-attachments/assets/283b24ef-6627-4aa2-aff5-f65ca94295e6" />

```sql
--2.brands
CREATE TABLE brands(
brand_id int PRIMARY KEY identity(1,1),
brand_name varchar(15) UNIQUE);

INSERT INTO brands(brand_name)
VALUES ('Samsung');

SELECT *FROM brands;
```
```sql
--3.products
CREATE TABLE products(
product_id int PRIMARY KEY identity(1,1),
product_name varchar(50) not null,
brand_id int,
FOREIGN KEY (brand_id) references brands(brand_id),
category_id int,
FOREIGN KEY (category_id) references categories(category_id),
model_year int,
list_price money);

SELECT * FROM products;

INSERT INTO products
VALUES ('nokia', 1, 2, 2002, 1700);
```
<img width="453" height="125" alt="{E2BDF75E-D454-4ECE-AC48-6271F4DAB3A3}" src="https://github.com/user-attachments/assets/bf608143-f9b0-49cb-bfc5-4135cae2ddf5" />

## CREATE PROTSEDUURID -

```sql
--protseduur, mis kuvab tooded, kus on hind suurem kui sisestatud hind
CREATE procedure suuremHind
@hind int 
AS
BEGIN
	SELECT * FROM products
	WHERE list_price > @hind;
END
--kutse
EXEC suuremHind 1000;
```
<img width="455" height="177" alt="{A99D10C2-A76A-4AA6-BB0D-169B0CEADF3A}" src="https://github.com/user-attachments/assets/bbc6ad60-5906-48fc-8918-0ab9809c82be" />

```sql
--OUTPUT parameetr

CREATE PROCEDURE minmaxHind
    @minHind MONEY OUTPUT,
    @maxHind MONEY OUTPUT
AS
BEGIN
    SELECT 
        @minHind = MIN(list_price),
        @maxHind = MAX(list_price)
    FROM products;
END;
--kutse

DECLARE @minHind MONEY, @maxHind MONEY;

EXEC minmaxHind @minHind OUTPUT, @maxHind OUTPUT;

PRINT 'Min hind = ' + CONVERT(varchar, @minHind);
PRINT 'Max hind = ' + CONVERT(varchar, @maxHind);
```
<img width="519" height="274" alt="{A14D1407-441B-4754-9ADF-7F522C9047E1}" src="https://github.com/user-attachments/assets/5bf6157d-7ef9-499f-8c44-61794fbe659b" />

```sql
--6.UNIVERSAALNE protseduur, mis töötab üks kõik millise tabeliga 
--muudab struktuuri (veeru lisamine -ADD, veeru kustutamine - DROP)

CREATE PROCEDURE muudatus
    @tegevus varchar(10),
    @tabelinimi varchar(25),
    @veerunimi varchar(25),
    @tyyp varchar(25) = NULL
AS
BEGIN
    DECLARE @sqltegevus varchar(max);

    SET @sqltegevus = CASE 
        WHEN @tegevus = 'add' THEN 
            CONCAT('ALTER TABLE ', @tabelinimi, ' ADD ', @veerunimi, ' ', @tyyp)

        WHEN @tegevus = 'drop' THEN 
            CONCAT('ALTER TABLE ', @tabelinimi, ' DROP COLUMN ', @veerunimi)
    END;

    PRINT @sqltegevus;
    EXEC (@sqltegevus);
END;

--kutse
EXEC muudatus 'add', 'categories', 'testVeerg', int 

SELECT * FROM categories;

EXEC muudatus 'drop', 'categories', 'testVeerg'
```
<img width="603" height="211" alt="{2BE33CAA-5159-4AC0-A1B0-F7E4F0DD9CDC}" src="https://github.com/user-attachments/assets/17ff558a-9c84-484a-a3f9-4c0dc4f66c50" />


<img width="499" height="202" alt="{8B9A8D3C-83A7-4762-9C43-5E078B5FFE0A}" src="https://github.com/user-attachments/assets/0fe1012a-7860-4065-8587-80bff48882fd" />



