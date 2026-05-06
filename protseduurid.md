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
```
<img width="257" height="366" alt="{F64F09F6-182C-4C15-B672-301839077B72}" src="https://github.com/user-attachments/assets/ffe3dae3-792a-481e-ab59-d9f824956706" />


