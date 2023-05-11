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
