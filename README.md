## ETL-Implementation Project

Data Warehouse and ETL Implementation

Project ini merupakan tugas akhir dari ID/X Partners Data Engineer Virtual Internship Program yang diselenggarakan oleh Rakamin Academy.
Melalui program ini saya mempelajari berbagai hal tentang Data Engineer job terutama dalam proses ETL menggunakan Talend dan juga penggunaan 
SQL Server untuk Data Engineer. Pada project ini para peserta di berikan case untuk menjalankan sebuah proses dari mendapatkan data hingga menggunakan data 
dalam bentuk _Store Procedure_ di SQL Server untuk suatu keperluan.


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

```

CREATE TABLE DimCustomer (
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

**3. Membuat Job pada Talend**

a.) _Tahapan pertama dalam membuat job adalah melakukan connection dengan datawarehouse_

![image](https://github.com/syachmhrd/ETL-Implementation/assets/71867027/f45c720e-fb8a-4377-9573-6b82c93968e0)

- Pada Metadata klik kanan pada Db Connection, kemudian pilih Create connection

<img width="716" alt="image" src="https://github.com/syachmhrd/ETL-Implementation/assets/71867027/4f179ec8-0520-4921-8a30-4ee7497fbb2a">

- Masukan nama connection, lalu klik next
- Pilih DB Type (pada case ini menggunakan Microsoft SQL Server)
- Pilih DB Version "Open source JTDS"
- Lalu isi komponen lainnya (login,password,server,port, dan database)
- Test connection untuk memastikan sudah aman
- Klik Finish

b.) _Tahapan Kedua membuat pipeline job_

Berdasarkan perintah, ingin dilakukan proses ETL dari **database Staging** ke **database DWH_Project**
, dimana terdapat transformasi dari tabel customer ke tabel Dim_Customer yaitu penambahan kolom CustomerName
yaitu gabungan Uppercase dari kolom first_name dan last_name. Sehingga Pipeline job yang dibuat adalah sebagai berikut 

![image](https://github.com/syachmhrd/ETL-Implementation/assets/71867027/bc0b8154-0b82-4072-80e1-6937ddf4443b)

Pada pipeline di atas menggunakan 3 komponen yaitu, _tMSSqlInput_, _tMSSqlOutput_, dan _tMap_.
Dari _tMSSqlInput_ diisi _component_ dari tabel yang ada di **database Staging** sedangkan
pada _tMSSqlOutput_ diisi _component_ dari tabel yang ada di **database DWH_Project**. 
Kemudian khusus pada pipeline ke 3 ada tambahan _tMap_ yang merupakan proses transformasi yang digunakan untuk membuat kolom CustomerName.
Pada MapEditor digunakan Expression untuk menggabungkan kolom first_name dan last_name sebagai berikut,

```
StringHandling.UPCASE(row3.FirstName) +" "+ StringHandling.UPCASE(row3.LastName)
```
<img width="959" alt="image" src="https://github.com/syachmhrd/ETL-Implementation/assets/71867027/3adc08c1-2a4e-4972-8c69-75d1e5eaf485">

**4. Membuat Store Procedure untuk menampilkan summary sales order berdasarkan status pengiriman**

```
CREATE PROCEDURE summary_order_status
	@StatusID int
AS
BEGIN
	SET NOCOUNT ON;
	SELECT a.OrderID, b.CustomerName, c.ProductName, a.Quantity, d.StatusOrder
	FROM FactSalesOrder a LEFT JOIN DimCustomer b ON a.CustomerID = b.CustomerID
	LEFT JOIN DimProduct c ON a.ProductID = c.ProductID LEFT JOIN DimStatusOrder d
	ON a.StatusID = d.StatusID WHERE d.StatusID = @StatusID
END
GO
```

```
EXEC [dbo].[summary_order_status] @StatusID = 1
```
