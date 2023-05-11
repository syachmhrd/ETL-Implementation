## ETL-Implementation Project
Data Warehouse and ETL Implementation
Case Study

<img width="445" alt="image" src="https://github.com/syachmhrd/ETL-Implementation/assets/71867027/d9cf42ad-50cb-4348-93c2-2eb36ad86e2a">

Pembahasan:

**1. Melakukan Restore Database Staging dengan SQL Server**

<img width="914" alt="image" src="https://github.com/syachmhrd/ETL-Implementation/assets/71867027/1627021a-40eb-4002-932a-9d8fe0ccdf80">

- Klik kanan pada Databases, lalu pilih Restore Database...
- Di windows Restore Database, pilih Device kemudian klik 3 titik di kanan 

<img width="956" alt="image" src="https://github.com/syachmhrd/ETL-Implementation/assets/71867027/1e46780b-1981-4319-9468-e586743a823b">

- Pilih file Staging.bak, kemudian klik OK
- Lalu tunggu hinggal database Staging beserta tabelnya berhasil di restore

**2. Membuat Database dan Tabel di SQL Server**

Untuk membuat database bisa langsung dengan,

<img width="412" alt="image" src="https://github.com/syachmhrd/ETL-Implementation/assets/71867027/59764225-e6f3-44f4-acd2-08f5b012476b">

- Klik kanan pada Databases
- Pilih New Database...
- Masukan nama database (pada case ini DWH_Project)
- Klik OK jika tidak ada penyesuaian lainnya

Selanjutnya berdasarkan perintah, untuk membuat tabel sesuai dengan tabel staging dimana hanya penyesuaian nama tabel dan juga penambahan kolom pada tabel DimCustomer.
Berikut adalah query untuk membuat tabel sesuai perintah

```CREATE TABLE DimCustomer (
    CustomerID int NOT NULL,
    CustomerName varchar(max) NOT NULL,
    FirstName varchar(50) NOT NULL,
	LastName varchar(50) NOT NULL,
	Age int NOT NULL,
	Gender varchar(50) NOT NULL,
	City varchar(50) NOT NULL,
	NoHP varchar(50) NOT NULL
    PRIMARY KEY (CustomerID)
);

CREATE TABLE DimProduct (
    ProductID int NOT NULL,
    ProductName varchar(255) NOT NULL,
    ProductCategory varchar(255) NOT NULL,
	ProductUnitPrice int
	PRIMARY KEY (ProductID)
);

CREATE TABLE DimStatusOrder (
    StatusID int NOT NULL,
    StatusOrder varchar(50) NOT NULL,
    StatusOrderDesc varchar(50) NOT NULL
	PRIMARY KEY (StatusID)
);

CREATE TABLE FactSalesOrder (
    OrderID int NOT NULL,
    CustomerID int NOT NULL,
    ProductID int NOT NULL,
	Quantity int NOT NULL,
	Amount int NOT NULL,
	StatusID int NOT NULL,
	OrderDate date NOT NULL
	PRIMARY KEY (OrderID),
	FOREIGN KEY (CustomerID) REFERENCES DimCustomer(CustomerID),
	FOREIGN KEY (ProductID) REFERENCES DimProduct(ProductID),
	FOREIGN KEY (StatusID) REFERENCES DimStatusOrder(StatusID)
);

```
