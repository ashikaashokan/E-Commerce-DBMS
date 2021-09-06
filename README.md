# E-Commerce-DBMS
As a part of our syllabus ,we made this project for Database Management Systems (DBMS) - ITE1003.
This project contains theoretical as well as implementation in SQL
## Pre-Requiste
MS-SQL Server
## Contents
* Description
* Basic Structure
  * Functional requirements
  * ER Diagram
* Implementation
  * Creating Tables
  * Inserting Data
* Basic Queries

# 1.Description
In this modern era of online shopping no seller wants to be left behind in selling his products.  Hence in this project we have designed a database where small clothing sellers can sell their products online.

# 2. Basic Structure
## 2.1 Functional Requirements
* A new user can register on the website.
* A customer can see details of the product present in the cart
* A customer can view his order history.
* A customer can add or delete a product from the cart.
* A seller can unregister/ stop selling his product.
* A seller/ customer can update his details.
* Admin can view the products purchased on particular date.

## 2.2 ER Diagram
![ER diagram](https://github.com/danyrd/E-Commerce-DBMS/blob/main/ER-Diagram.png)

# 3.Implementation
## 3.1 Creating Tables
```sql
create table cart
( Cart_id int not null, primary key (Cart_id)
);
select*from cart
create table customer
( Customer_id varchar(10) not null,
  Customer_name varchar(30) not null,
  Address varchar(50) not null,
  City varchar(20) not null,
  State varchar(20) not null,
  phone_number numeric(10) not null,
  primary key(Customer_id),
  Cart_id int  not null,
  foreign key (Cart_id) references cart (cart_id)
  );
  select*from customer
 create table seller
 ( Seller_id varchar(10) not null,
   Seller_name varchar(20) not null,
   Email varchar(25) not null,
   primary key (Seller_id)
   );
create table product
(product_id varchar(10) not null,
 type varchar(10) not null,
 brand varchar(10) not null,
 color varchar(10) not null,
 size varchar(2) not null,
 gender varchar(1) not null,
 cost numeric(5) not null,
 quantity numeric(2) null,
 Seller_id varchar(10) not null,
 primary key(product_id),
 foreign key (Seller_id) references seller(Seller_id)
 );
 create table payment
  ( payment_id varchar(10) not null,
     payment_date date not null,
	 payment_type varchar(10)not null,
	 Customer_id varchar(10) not null,
	 Cart_id int not null,
	 primary key (payment_id),
	 foreign key (Customer_id) references customer(Customer_id),
	 foreign key (Cart_id) references cart(Cart_id)
	 );
create table cart_item
( favorite int not null,
  date_added date not null,
  cart_id int not null,
  product_id varchar(10) not null,
  foreign key (Cart_id) references  cart (Cart_id),
  foreign key (product_id) references product(product_id),
  primary key (cart_id,product_id)
  );
  alter table Cart_item add purchased varchar(3) default 'NO';
```
## 3.2 Inserting Data
```sql
insert into cart values (00001);
insert into cart values (00002);
insert into cart values (00003);
insert into cart values (00004);
insert into cart values (00005);

insert into customer values ('CST001','Ashika','4th street rangasamy nagar egmore','chennai','Tamilnadu',9876543210,00001);
insert into customer values ('CST002','Daniel','2nd street Madasamy nagar Anna nagar','chennai','Tamilnadu',9123456780,00002);
insert into customer values ('CST003','Haritha','5th street mahalakshmi nagar Nungabakkam','chennai','Tamilnadu',9887766550,00003);
insert into customer values ('CST004','Chandra','3rd street Chinnaswamy nagar Alwarpet','chennai','Tamilnadu',9871234560,00004);
insert into customer values ('CST005','Siva','1st street rangasamy nagar mogappair','chennai','Tamilnadu',9988765430,00005);



select*from seller

insert into seller values ('SL01','fashion clothings','fcco@gmail.com');
insert into seller values ('SL02','branded collections','brandcollection@gmail.com');
insert into seller values ('SL03','New fashion','NFC3300@gmail.com');

insert into product values('prd100','jumpsuit','H&M','red','S','F',1099,1,'SL03')
insert into product values('prd101','Shirt','levis','white','M','M',899,1,'SL02')
insert into product values('prd102','Top','Pantaloon','red','S','F',899,1,'SL01')
insert into product values('prd103','jean','denim','red','S','F',999,1,'SL02')
insert into product values('prd104','hoodie','H&M','red','S','F',899,1,'SL01')

insert into payment values('PYT001','01-july-2021','COD','CST001',00001);
insert into payment values('PYT002','02-August-2021','online','CST003',00002);
insert into payment values('PYT003','12-june-2021','online','CST002',00004);
insert into payment values('PYT004','31-May-2021','COD','CST004',00003);
insert into payment values('PYT005','02-july-2021','COD','CST005',00005);

insert into cart_item values(3,'01-july-2021',00001,'prd100','yes')
insert into cart_item values(2,'02-August-2021',00002,'prd101','yes')
insert into cart_item values(1,'12-june-2021',00004,'prd102','yes')
insert into cart_item values(2,'31-May-2021',00003,'prd103','yes')
insert into cart_item values(5,'02-july-2021',00005,'prd104','yes')
```
## Basic Queries
### If the customer wants to find an order history
```sql
select product_id,favorite from cart_item where(purchased='yes'
                         and cart_id in (select cart_id from customer where Customer_id='CST004'))
```
### If the customer wants to see details of product present in the cart
```sql
select *from product where product_id in 
          (select product_id from cart_item where (cart_id in (select cart_id from customer where Customer_id='CST002')));
```
### Customer wants to see filtered products on the basis of size,gender,type
```sql
select product_id, color, cost, seller_id from product where (type='jean' and  size='S' and gender='F' and quantity>0);
```
### If admin want to see what are the product purchased on the particular date
```sql
select product_id  from cart_item where(purchased='yes' and date_added='12-june-2021');
```
### How much product sold on the particular date
```sql
select count(product_id)product_sold,date_added from cart_item where purchased='yes' group by (date_added	);
```
### Find total profit of the website from sales
```sql
  select sum(favorite * cost /100) total_profit from product p join cart_item c on p.product_id=c.product_id where purchased='yes';
```
