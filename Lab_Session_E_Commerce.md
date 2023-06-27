```python
#load libraries
import mysql.connector
import pandas as pd
```


```python
#connect to database
connection=mysql.connector.connect(host="localhost",
                                  user="root",
                                  passwd="K@rt1905")
#creating cursor object
cursorObject = connection.cursor()
```


```python
# Lets make a connection to Mysql server and create a database named 'e_commerce'
connection = mysql.connector.connect(host ="localhost",
                                     user ="root",
                                     passwd ="K@rt1905")
 
## creating a cursor object
cursorObject = connection.cursor()
 
## creating database
cursorObject.execute("create database e_commerce")

## closing the connection after creating a database('e_commerce')
connection.close()
```


```python
##connect to the Mysql server and while connecting Choose 'e_commerce' database

connection = mysql.connector.connect(host ="localhost",
                                     user ="root",
                                     passwd ="K@rt1905",
                                     database = 'e_commerce')
 
## creating a cursor object
cursorObject = connection.cursor()
```


```python
##table creation query
table_creation_query = """create table supplier( SUPP_ID int primary key , SUPP_NAME varchar(50),SUPP_CITY varchar(50), 
                           SUPP_Phone_NO varchar(10));

                           create table customer(CUS_ID INT NOT NULL,CUS_NAME VARCHAR (20) NULL DEFAULT NULL,
                           CUS_PHONE VARCHAR(10),CUS_CITY VARCHAR(30),CUS_GENDER CHAR,PRIMARY KEY(CUS_ID));
                           
                          create table category(CAT_Id INT NOT NULL,CAT_NAME VARCHAR(20) NULL DEFAULT NULL,
                          PRIMARY KEY(CAT_ID));

                          create table product(PRO_ID INT NOT NULL,PRO_NAME VARCHAR(20) NULL DEFAULT NULL,PRO_DESC VARCHAR(60)
                          NULL DEFAULT NULL, CAT_ID NOT NULL,PRIMARY KEY(PRO_ID) ,FOREIGN KEY(CAT_ID) REFERENCES CATEGORY(CAT_ID)); 
                          

                          create table product_details(PROD_ID INT NOT NULL, PRO_ID  INT NOT NULL,SUPP_ID NOT NULL,
                          PROD_PRICE INT NOT NULL,PRIMARY KEY(PROD_ID),FOREIGN KEY(PRO_ID) REFERENCES PRODUCT(PRO_ID),
                          FOREIGN KEY(SUPP_ID) REFERENCES SUPPLIER(SUPP_ID));
                          

                          create table orders(ORD_ID INT NOT NULL,ORD_AMOUNT INT NOT NULL,SUPP_ID INT NOT NULL,PROD_PRICE
                          INT NOT NULL,PRIMARY KEY(ORD_ID),FOREIGN KEY(CUS_ID) REFERENCES CUSTOMER(CUS_ID),FOREIGN KEY(PROD_ID)
                          REFERENCES PRODUCT_DETAILS(PROD_ID));
                          
                          create table rating(RAT_ID INT NOT NULL,CUS_ID INT NOT NULL,SUPP_ID INT NOT NULL,RAT_RATSTARS INT NOT
                          NOT NULL,PRIMARY KEY(RAT_ID),FOREIGN KEY(SUPP_ID) REFERENCES SUPPLIER(SUPP_ID),FOREIGN KEY(CUS_ID)
                          REFERENCES CUSTOMER(CUS_ID));"""
                  
##Executing the query
cursorObject.execute(table_creation_query)
```


```python
## After creating tables close the connection and reconnect to the server for inserting the data 

## closing the connection 
connection.close()
## Lets make a connection to Mysql server and choose database 'e_commerce'
connection = mysql.connector.connect(host ="localhost",
                                     user ="root",
                                     passwd ="K@rt1905",
                                     database='e_commerce')
## creating a cursor object
cursorObject = connection.cursor()
```
## Inserting data into "supplier"

insert_supplier_details = """INSERT INTO supplier (SUPP_ID, SUPP_NAME, SUPP_CITY, SUPP_PHONE) VALUES (%s, %s, %s, %s)"""

val = [(1,"Rajesh Retails","Delhi",'1234567890'),
       (2,"Appario Ltd.","Mumbai",'2589631470'),
       (3,"Knome products","Banglore",'9785462315'),
       (4,"Bansal Retails","Kochi",'8975463285'),
       (5,"Mittal Ltd.","Lucknow",'7898456532')]
cursorObject.executemany(insert_supp_details, val)
connection.commit()

```python
## Inserting data into "supplier"
insert_supplier_details = """INSERT INTO supplier (SUPP_ID, SUPP_NAME, SUPP_CITY, SUPP_PHONE_No)
                             VALUES (%s, %s, %s, %s)"""
val = [(1,"Rajesh Retails","Delhi",'1234567890'),
       (2,"Appario Ltd.","Mumbai",'2589631470'),
       (3,"Knome products","Banglore",'9785462315'),
       (4,"Bansal Retails","Kochi",'8975463285'),
       (5,"Mittal Ltd.","Lucknow",'7898456532')]
cursorObject.executemany(insert_supplier_details, val)
connection.commit()

```


```python
## Inserting data into "customer"
insert_customer="""INSERT INTO CUSTOMER(CUS_ID,CUS_NAME,CUS_PHONE,CUS_CITY,CUS_GENDER)
                   VALUES (%s,%s, %s, %s, %s)"""
val=[(1,"Aakash",9999999999,"Delhi","M"),
    (2,"Aman",9785463215,"Noida","M"),
    (3,"Neha",9999999998,"Mumbai","F"),
    (4,"Megha",9994562399,"Kolkatta","F"),
    (5,"Pulkit",7895999999,"Lucknow","M")]
cursorObject.executemany(insert_customer, val)
connection.commit()
```


```python
## Inserting data into "category"
insert_into_category="""INSERT INTO CATEGORY(CAT_Id,CAT_NAME)
                        VALUES(%s,%s)"""
val=[(1,"Books"),
    (2,"Games"),
    (3,"Groceries"),
    (4,"Electronics"),
    (5,"Clothes")]
cursorObject.executemany(insert_into_category, val)
connection.commit()
```


```python
connection.close()
```


```python
##Display the number of customer group by their genders who have placed any order of amount greater than or equal to 3000

question_three="""Select C.cus_id,C.Cus_gender,C.cus_name,Count(*) as num_customers from customer as C
                inner join
                orders as O on C.cus_id=O.cus_id
                where O.ord_amount>=3000 
                group by C.cus_gender"""
                
cursorObject.execute(question_three)
 
output = cursorObject.fetchall()
   
## Lets put the output of this query in pandas DataFrame 
output_df = pd.DataFrame(output, columns=['cus_id','cus_gender','cus_name','num_customers'])
output_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cus_id</th>
      <th>cus_gender</th>
      <th>cus_name</th>
      <th>num_customers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>M</td>
      <td>Pulkit</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>F</td>
      <td>Megha</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
##Display all order along with prod name ordered by customer having customer_id=2
question_four="""select O.*,B.PRO_NAME from Orders as O inner join
                (SELECT product.PRO_ID, product.PRO_NAME,PD.PROD_ID
                 FROM product
                 INNER JOIN product_details as PD on product.PRO_ID=PD.PRO_ID) as B
                 on O.PROD_ID=B.PROD_ID
                 WHERE O.CUS_ID=2"""
cursorObject.execute(question_four)
 
output = cursorObject.fetchall()
   
## Lets put the output of this query in pandas DataFrame 
output_df = pd.DataFrame(output, columns=['ORD_ID','ORD_AMOUNT','ORD_DATE','CUS_ID','PROD_ID','PRO_NAME'])
output_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ORD_ID</th>
      <th>ORD_AMOUNT</th>
      <th>ORD_DATE</th>
      <th>CUS_ID</th>
      <th>PROD_ID</th>
      <th>PRO_NAME</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>50</td>
      <td>2000</td>
      <td>2021-10-06</td>
      <td>2</td>
      <td>1</td>
      <td>GTA V</td>
    </tr>
  </tbody>
</table>
</div>




```python
##Display supplier details who can supply more than one product
question_five="""select * from supplier as S inner join
                 (select PD.SUPP_ID from product_details as PD
                 group by PD.SUPP_ID having count(distinct PD.PRO_ID)>1)
                 as B on S.SUPP_ID=B.SUPP_ID"""
cursorObject.execute(question_five)
 
output = cursorObject.fetchall()
   
## Lets put the output of this query in pandas DataFrame 
output_df = pd.DataFrame(output, columns=['SUPP_ID','SUPP_NAME','SUPP_CITY','SUPP_PHONE_NO','PRO_ID'])
output_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SUPP_ID</th>
      <th>SUPP_NAME</th>
      <th>SUPP_CITY</th>
      <th>SUPP_PHONE_NO</th>
      <th>PRO_ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Rajesh Retails</td>
      <td>Delhi</td>
      <td>1234567890</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
## display category of product whose order amount is min
question_six="""select K.*,MIN_ORDER,P.PRO_ID from category as K inner join
               (select P.CAT_ID ,Q.PRO_ID,MIN_ORDER from product as P inner join
               (select PD.PRO_ID,MIN_ORDER from product_details as PD inner join
               (select O.PROD_ID,min(O.ORD_AMOUNT) as MIN_ORDER FROM orders as O )as B
                on PD.PROD_ID=B.PROD_ID)as Q
                on P.PRO_ID=Q.PRO_ID) as P
                on K.CAT_ID=P.CAT_ID"""
cursorObject.execute(question_six)
 
output = cursorObject.fetchall()
   
## Lets put the output of this query in pandas DataFrame 
output_df = pd.DataFrame(output, columns=['CAT_ID','CAT_NAME','MIN_ORDER','PRO_ID'])
output_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CAT_ID</th>
      <th>CAT_NAME</th>
      <th>MIN_ORDER</th>
      <th>PRO_ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>Groceries</td>
      <td>1500</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
##display Id & name of prod ordered after 2021-10-05
question_seven="""select P.PRO_ID,P.PRO_NAME,Q.ORD_DATE from product as P inner join
                 (select PD.PRO_ID,B.ORD_DATE FROM product_details as PD inner join
                 (select O.ORD_DATE,O.PROD_ID from orders as O where
                 O.ORD_DATE>'2021-10-05') as B
                 where PD.PROD_ID=B.PROD_ID) as Q
                 ON P.PRO_ID=Q.PRO_ID"""

cursorObject.execute(question_seven)
 
output = cursorObject.fetchall()
   
## Lets put the output of this query in pandas DataFrame 
output_df = pd.DataFrame(output, columns=['PRO_ID','PRO_NAME','ORD_DATE'])
output_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PRO_ID</th>
      <th>PRO_NAME</th>
      <th>ORD_DATE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>OATS</td>
      <td>2021-10-12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>GTA V</td>
      <td>2021-10-06</td>
    </tr>
  </tbody>
</table>
</div>




```python
##display top 3 supplier name & id &rating on basis of their rating along with customer name who has given the rating
question_eight="""select C.CUS_NAME,C.CUS_ID,P.SUPP_NAME,P.SUPP_ID,P.RAT_RATSTARS FROM customer as C inner join
                 (select S.SUPP_ID,S.SUPP_NAME,Q.CUS_ID,Q.RAT_RATSTARS FROM supplier as S inner join
                 (select * from rating as R order by R.RAT_RATSTARS desc limit 3)as Q
                  on S.SUPP_ID=Q.SUPP_ID) as P
                  on C.CUS_ID=P.CUS_ID"""
cursorObject.execute(question_eight)
 
output = cursorObject.fetchall()
   
## Lets put the output of this query in pandas DataFrame 
output_df = pd.DataFrame(output, columns=['CUS_ID','CUS_NAME','SUPP_NAME','SUPP_ID','RAT_RATSTARS'])
output_df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUS_ID</th>
      <th>CUS_NAME</th>
      <th>SUPP_NAME</th>
      <th>SUPP_ID</th>
      <th>RAT_RATSTARS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Pulkit</td>
      <td>5</td>
      <td>Rajesh Retails</td>
      <td>1</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aman</td>
      <td>2</td>
      <td>Appario Ltd.</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Megha</td>
      <td>4</td>
      <td>Mittal Ltd.</td>
      <td>5</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
## display customer name and gender whose name start or end with character 'A'
question_nine="""select CUS_NAME,CUS_GENDER FROM customer where CUS_NAME LIKE 'A%' OR CUS_NAME LIKE '%A'"""

cursorObject.execute(question_nine)
 
output = cursorObject.fetchall()
   
## Lets put the output of this query in pandas DataFrame 
output_df = pd.DataFrame(output, columns=['CUS_NAME','CUS_GENDER'])
output_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUS_NAME</th>
      <th>CUS_GENDER</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aakash</td>
      <td>M</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aman</td>
      <td>M</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Neha</td>
      <td>F</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Megha</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>




```python
##display total order amount of male customer
question_ten="""select sum(Q.ORD_AMOUNT) AS Total_Order_Amount,C.CUS_GENDER from customer as C inner join
                (select O.CUS_ID,O.ORD_AMOUNT from Orders as O)as Q
                 on C.CUS_ID=Q.CUS_ID
                 WHERE C.CUS_GENDER='M'"""
cursorObject.execute(question_ten)
 
output = cursorObject.fetchall()
   
## Lets put the output of this query in pandas DataFrame 
output_df = pd.DataFrame(output, columns=['Total_Order_Amount','CUS_GENDER'])
output_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total_Order_Amount</th>
      <th>CUS_GENDER</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>34500</td>
      <td>M</td>
    </tr>
  </tbody>
</table>
</div>




```python
##display all customer left outer join with orders
question_eleven="""select C.CUS_NAME,C.CUS_CITY,C.CUS_GENDER,C.CUS_PHONE,O.CUS_ID from 
                  customer as C left join  orders as O on O.CUS_ID=C.CUS_ID"""
cursorObject.execute(question_eleven)
 
output = cursorObject.fetchall()
   
## Lets put the output of this query in pandas DataFrame 
output_df = pd.DataFrame(output, columns=['CUS_NAME','CUS_CITY','CUS_GENDER','CUS_PHONE','CUS_ID'])
output_df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUS_NAME</th>
      <th>CUS_CITY</th>
      <th>CUS_GENDER</th>
      <th>CUS_PHONE</th>
      <th>CUS_ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aakash</td>
      <td>Delhi</td>
      <td>M</td>
      <td>9999999999</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aman</td>
      <td>Noida</td>
      <td>M</td>
      <td>9785463215</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Neha</td>
      <td>Mumbai</td>
      <td>F</td>
      <td>9999999998</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Megha</td>
      <td>Kolkatta</td>
      <td>F</td>
      <td>9994562399</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pulkit</td>
      <td>Lucknow</td>
      <td>M</td>
      <td>7895999999</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
##closing the connection
connection.close()
```


```python

```
