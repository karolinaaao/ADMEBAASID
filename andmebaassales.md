```sql
--1.categories
create table categories(
category_id int PRIMARY KEY identity (1,1),
category_name varchar(25) UNIQUE);

INSERT INTO categories(category_name)
VALUES ('Arvuti');

SELECT * FROM categories;
```
<img width="197" height="78" alt="{637849F6-2A1C-429C-8773-3F6037341705}" src="https://github.com/user-attachments/assets/2bff3047-cba5-4885-a05d-c5a18fbf6f23" />



```sql
--2.brands
CREATE TABLE brands(
brand_id int PRIMARY KEY identity(1,1),
brand_name varchar(15) UNIQUE);

INSERT INTO brands(brand_name)
VALUES ('Samsung');

SELECT *FROM brands;
```
<img width="166" height="77" alt="{6BABC0A3-2169-4B1B-92B9-900416BD2E4D}" src="https://github.com/user-attachments/assets/40179068-f3b9-4b50-af79-e4e2c8c3bec5" />

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
VALUES ('nutitelefon´X10', 1, 1, 2025, 600);
```
<img width="445" height="82" alt="{A61720A9-D3A5-4AA6-992F-616880EAE07C}" src="https://github.com/user-attachments/assets/179f4678-4336-45bd-9761-8752f052654a" />

```sql
--4.stores
CREATE TABLE stores(
store_id int PRIMARY KEY identity (1,1),
store_name varchar(20) not null,
phone varchar(13),
email varchar(20),
street varchar(21),
city varchar(15),
state varchar(10),
zip_code char(5)
)
SELECT * FROM stores;

INSERT INTO stores
VALUES ('T1', '58444433', 'T1@gmail.com', 'T1 tee', 'Tallinn', 'Eesti', '46376');
```
<img width="554" height="81" alt="{690EA2DF-D07C-4D3C-ADF5-7660E514CA13}" src="https://github.com/user-attachments/assets/a174a52a-4d83-49e1-ad08-5da233b9cd63" />


```sql
--5.stocks
CREATE TABLE stocks( 
store_id int,
product_id int, 
PRIMARY KEY (store_id, product_id),
FOREIGN KEY (store_id) references stores(store_id),
FOREIGN KEY (product_id) references products(product_id),
quantity int 
)
SELECT * FROM stocks;

INSERT INTO stocks
VALUES (2, 1, 4);
```
<img width="217" height="84" alt="{F07AA490-5DDA-4F86-A079-05BB81840B85}" src="https://github.com/user-attachments/assets/c5d85997-429d-454e-b082-403aed050312" />


```sql
--6.customers
CREATE TABLE customers(
customer_id int PRIMARY KEY identity(1,1),
first_name varchar(15) not null,
last_name varchar(15) not null,
phone varchar(13),
email varchar(15),
city varchar(15) check (city='Tallinn' or city='Narva'),
state varchar(15),
zip_code char(5)
); 

SELECT * FROM customers

INSERT INTO customers
VALUES('Marina','Uuema','58593940','Mari@gmail.com', 'Tallinn', 'Harjumaa','12913');
```
<img width="584" height="75" alt="{E3D18B49-5067-4A0E-8C63-4623040CB4A2}" src="https://github.com/user-attachments/assets/c7625553-7dd1-4018-bd21-7ed01e3d5e49" />

```sql
--7.staffs
CREATE TABLE staffs(
staff_id int PRIMARY KEY identity(1,1),
first_name varchar(15) not null,
last_name varchar(15) not null,
phone varchar(13),
active bit,
store_id int,
FOREIGN KEY (store_id) references stores(store_id),
manager bit
)

INSERT INTO staffs
VALUES('Marina', 'Rahva', '5948948', 1, 2, 0);

SELECT * FROM staffs
```
<img width="429" height="77" alt="{188068F7-CEDE-4E57-A048-F971A0CE6832}" src="https://github.com/user-attachments/assets/bd7061d7-f04d-49e3-9fb7-7414a0bb0725" />


```sql
--8.orders
CREATE TABLE orders(
order_id int PRIMARY KEY identity(1,1),
customer_id int,
order_status varchar(15) check(order_status='complete' or order_status='incomplete'),
order_date Date,
required_date Date,
shipped_date Date,
store_id int,
FOREIGN KEY(store_id) references stores(store_id),
staff_id int,
FOREIGN KEY(staff_id) references staffs(staff_id));

 SELECT * FROM orders;

INSERT INTO orders 
VALUES (5,'incomplete','2026-05-18','2026-05-20','2026-05-22', 1,1
);
```
<img width="581" height="84" alt="{F1466196-4D87-4CEE-8DD0-B046EF95C69D}" src="https://github.com/user-attachments/assets/bb0ced7d-1d1b-45ea-9d05-037c61b4ccd2" />


```sql
--9.order_items
CREATE TABLE order_items(
order_id int,
product_id int,
item_id int,
quatity int,
list_price money,
discount int,
FOREiGN KEY (order_id) references orders(order_id),
FOREIGN KEY(product_id) references products(product_id)
)
SELECT * FROM order_items
SELECT * FROM orders
SELECT * FROM products
INSERT INTO order_items
VALUES(1,1,1,10,600.00,100)
```
<img width="1267" height="694" alt="{2D9872A6-2DCE-4001-A9B3-6861E332EA2D}" src="https://github.com/user-attachments/assets/71a3a99d-84c5-4f7c-bd58-d8352cdfa4a1" />

