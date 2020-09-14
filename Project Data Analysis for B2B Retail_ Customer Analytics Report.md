![image](https://user-images.githubusercontent.com/68532033/89724555-b77d1480-da2e-11ea-995f-22d7ded46ab3.png)
```
Latar Belakang

xyz.com adalah perusahan rintisan B2B yang menjual berbagai produk tidak langsung kepada end user tetapi ke bisnis/perusahaan lainnya. Sebagai data-driven company, maka setiap pengambilan keputusan di xyz.com selalu berdasarkan data. Setiap quarter xyz.com akan mengadakan townhall dimana seluruh atau perwakilan divisi akan berkumpul untuk me-review performance perusahaan selama quarter terakhir.
Tugas dan Langkah

Sebagai seorang data analyst, kamu dimintai untuk menyediakan data dan analisa mengenai kondisi perusahaan bulan terakhir untuk dipresentasikan di townhall tersebut. (Asumsikan tahun yang sedang berjalan adalah tahun 2004).

Adapun hal yang akan direview adalah :

    Bagaimana pertumbuhan penjualan saat ini?
    Apakah jumlah customers xyz.com semakin bertambah ?
    Dan seberapa banyak customers tersebut yang sudah melakukan transaksi?
    Category produk apa saja yang paling banyak dibeli oleh customers?
    Seberapa banyak customers yang tetap aktif bertransaksi?

Langkah yang akan dilakukan :

    Menggunakan klausa “Select … From …” untuk mengambil data di database
    Menggunakan klausa Where dan Operator untuk menfilter data
    Menggunakan “group by”dan fungsi aggregat untuk aggregasi penjualan dan revenue
    Menggunakan “order by” untuk mengurutkan data
    Menggunakan “union” untuk menggabungkan tabel data penjualan
    Menggunakan “date and time function” dan fungsi text untuk data manipulation
    Menggunakan subquery untuk menyimpan hasil sementara untuk digunakan kembali dalam query.

```
![image](https://user-images.githubusercontent.com/68532033/89724573-1cd10580-da2f-11ea-90e0-3e521918c039.png)

<b>1. Memahami table</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89724585-412ce200-da2f-11ea-97f4-2b92109e0763.png)
![image](https://user-images.githubusercontent.com/68532033/89724590-54d84880-da2f-11ea-8fb1-2bb44799112a.png)

<details>
  <summary>Click to show query sql</summary>
  <p>

```
SELECT * FROM orders_1 limit 5;
SELECT * FROM orders_2 limit 5;
SELECT * FROM customer limit 5;
```
  </p>
</details>


<b>2. Total Penjualan dan Revenue pada Quarter-1 (Jan, Feb, Mar) dan Quarter-2 (Apr,Mei,Jun)</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89724664-4dfe0580-da30-11ea-93ae-0d525cffcfe8.png)

<details>
  <summary>Click to show query sql</summary>
  <p>

```
select sum(quantity) as total_penjualan,sum(quantity*priceeach) as revenue from orders_1;
select sum(quantity) as total_penjualan,sum(quantity*priceeach) as revenue from orders_2 where status = 'Shipped';
```
  </p>
</details>


<b>3. Menghitung persentasi keseluruhan penjualan</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89724693-9ae1dc00-da30-11ea-8549-ca3cc02d2020.png)

<details>
  <summary>Click to show query sql</summary>
  <p>

```
select quarter,sum(quantity) as total_penjualan,sum(quantity*priceeach) as revenue from 
(select ordernumber,status,quantity,priceeach, 1 as quarter from orders_1
UNION
select ordernumber,status,quantity,priceeach, 2 as quarter from orders_2)
as tabel_a where status = 'Shipped'
group by quarter;
```
  </p>
</details>

![image](https://user-images.githubusercontent.com/68532033/89724713-cebd0180-da30-11ea-9104-c761c89c7358.png)

<b>4. Apakah jumlah customers xyz.com semakin bertambah?</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89724720-f14f1a80-da30-11ea-82ed-99f330a62143.png)

<details>
  <summary>Click to show query sql</summary>
  <p>

```
select quarter(createdate) as quarter, count(distinct customerid) as total_customers from 
(select customerid,createdate,quarter(createdate) as quarter from customer where createdate between '2004-01-01' and '2004-06-30') as tabel_b
group by quarter(createdate);
```
  </p>
</details>

<b>5. Seberapa banyak customers tersebut yang sudah melakukan transaksi?</b>
<br>

![image](https://user-images.githubusercontent.com/68532033/89724726-26f40380-da31-11ea-8afc-61679c801adc.png)

![image](https://user-images.githubusercontent.com/68532033/89724730-32dfc580-da31-11ea-98a2-391c497c7c7a.png)

<details>
  <summary>Click to show query sql</summary>
  <p>

```
select quarter(createdate) as quarter, count(distinct customerid) as total_customers from 
(select customerid,createdate,quarter(createdate) as quarter from customer where createdate between '2004-01-01' and '2004-06-30' and customerid in 
 (select distinct customerid from orders_1
UNION
select distinct customerid from orders_2)) as tabel_b
group by quarter(createdate);
```
  </p>
</details>

<b>6. Category produk apa saja yang paling banyak di-order oleh customers di Quarter-2?</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89724749-64f12780-da31-11ea-9188-6330b1cde32e.png)
![image](https://user-images.githubusercontent.com/68532033/89724756-79cdbb00-da31-11ea-84f8-553dac4b6fc7.png)
<details>
  <summary>Click to show query sql</summary>
  <p>

```
select left(productcode,3) as categoryid,count(distinct ordernumber) as total_order,sum(quantity) as total_penjualan from 
(select productcode,ordernumber,quantity,status,left(productcode,3) as categoryid from orders_2 where status = "Shipped") tabel_c
group by left(productcode,3)
order by count(distinct ordernumber)desc;
```
  </p>
</details>


<b>7. Seberapa banyak customers yang tetap aktif bertransaksi setelah transaksi pertamanya?</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89724783-a386e200-da31-11ea-8469-dbbc1d023a74.png)
![image](https://user-images.githubusercontent.com/68532033/89724789-b26d9480-da31-11ea-822d-96333c4f367b.png)
<details>
  <summary>Click to show query sql</summary>
  <p>

```
#Menghitung total unik customers yang transaksi di quarter_1
SELECT COUNT(DISTINCT customerID) as total_customers FROM orders_1;
#output = 25
SELECT "1" as quarter, (COUNT(DISTINCT customerID)*100)/25 as q2 FROM orders_1 where customerid in (SELECT DISTINCT customerID FROM orders_2);
```
  </p>
</details>

<details>
  <summary>Kesimpulan</summary>
  <p>
      
```

Berdasarkan data yang telah kita peroleh melalui query SQL, Kita dapat menarik kesimpulan bahwa :
1. Performance xyz.com menurun signifikan di quarter ke-2, terlihat dari nilai penjualan dan revenue yang drop hingga 20% dan 24%,
2. perolehan customer baru juga tidak terlalu baik, dan sedikit menurun dibandingkan quarter sebelumnya.
3. Ketertarikan customer baru untuk berbelanja di xyz.com masih kurang, hanya sekitar 56% saja yang sudah bertransaksi. 
Disarankan tim Produk untuk perlu mempelajari behaviour customer dan melakukan product improvement, sehingga conversion rate 
(register to transaction) dapat meningkat.
4. Produk kategori S18 dan S24 berkontribusi sekitar 50% dari total order dan 60% dari total penjualan, sehingga xyz.com sebaiknya fokus 
untuk pengembangan category S18 dan S24.
5. Retention rate customer xyz.com juga sangat rendah yaitu hanya 24%, artinya banyak customer yang sudah bertransaksi di quarter-1 
tidak kembali melakukan order di quarter ke-2 (no repeat order).
6. com mengalami pertumbuhan negatif di quarter ke-2 dan perlu melakukan banyak improvement baik itu di sisi produk dan bisnis marketing, 
jika ingin mencapai target dan positif growth di quarter ke-3. Rendahnya retention rate dan conversion rate bisa menjadi diagnosa awal bahwa customer tidak tertarik/kurang puas/kecewa berbelanja di xyz.com.

```
  </p>
</details>
