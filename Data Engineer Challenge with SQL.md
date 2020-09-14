![image](https://user-images.githubusercontent.com/68532033/89723837-a54aa880-da25-11ea-928f-560b73304c97.png)
```
Project SQL pertama di DQLab yaitu "Data Engineer Challenge with SQL". Dimana di project ini kita menggunakan 
syntax left join,right join,inner join, dan union. Kita menggunakan table yang sudah di sediakan DQLab yaitu 
ms_pelanggan,ms_produk,tr_penjualan dan tr_penjualan_detail.Semua table sudah tersedia, kita tinggal menulis query dalam Code Editor.
```
<b>Table yang digunakan</b>
![image](https://user-images.githubusercontent.com/68532033/89723872-2f930c80-da26-11ea-80f8-4a637b84ef0d.png)

<b>1. Product DQLab Mart</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89723332-fa36f080-da1e-11ea-850e-416586f3e187.png)

<details>
  <summary>Click to show query sql</summary>
  <p>

```
select no_urut,kode_produk,nama_produk,harga from ms_produk where harga >= 50000 and harga <= 150000;
```
  </p>
</details>

<b>2. Thumb Drive di DQLab Mart</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89723362-569a1000-da1f-11ea-866b-9d83fcc59145.png)
<details>
  <summary>Click to show query sql</summary>

```
select no_urut,kode_produk,nama_produk,harga from ms_produk where nama_produk like '%Flashdisk%';
```
</details>

<b>3. Pelanggan Bergelar</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89723388-b85a7a00-da1f-11ea-8800-df507a425f2e.png)
<details>
  <summary>Click to show query sql</summary>

```
select no_urut,kode_pelanggan,nama_pelanggan,alamat from ms_pelanggan where nama_pelanggan like '%S.H%' or nama_pelanggan like '%Ir.%' or nama_pelanggan like '%Drs.%';
```
</details>

<b>4. Mengurutkan Nama Pelanggan</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89723559-56e7da80-da22-11ea-8dd9-bb881b50a5b7.png)
<details>
  <summary>Click to show query sql</summary>

```
select nama_pelanggan from ms_pelanggan order by nama_pelanggan;
```
</details>

<b>5. Mengurutkan Nama Pelanggan</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89723575-96aec200-da22-11ea-9225-7f80abf12b51.png)
<details>
  <summary>Click to show query sql</summary>

```
select nama_pelanggan from ms_pelanggan 
order by REPLACE(nama_pelanggan,'Ir. ','')asc;
```
</details>

<b>6. Nama Pelanggan yang Paling Panjang</b>
![image](https://user-images.githubusercontent.com/68532033/89723599-e1303e80-da22-11ea-92ab-c1c1745786ad.png)
<details>
  <summary>Click to show query sql</summary>

```
select nama_pelanggan from ms_pelanggan where length(nama_pelanggan) = 19 order by length(nama_pelanggan) desc;
```
</details>

<b>7. Nama Pelanggan yang Paling Panjang</b>
![image](https://user-images.githubusercontent.com/68532033/89723628-1dfc3580-da23-11ea-8a51-b5b5f664b66a.png)
<details>
  <summary>Click to show query sql</summary>

```
select nama_pelanggan from ms_pelanggan where length(nama_pelanggan) = 19 or length(nama_pelanggan) = 9 order by length(nama_pelanggan) desc; 
```
</details>

<b>8. Kuantitas Produk yang Banyak Terjual</b>
![image](https://user-images.githubusercontent.com/68532033/89723722-2b65ef80-da24-11ea-9978-73a10a164c0c.png)
<details>
  <summary>Click to show query sql</summary>

```
select ms_produk.kode_produk,ms_produk.nama_produk,
sum(tr_penjualan_detail.qty) as total_qty
from ms_produk
JOIN
tr_penjualan_detail 
ON 
ms_produk.kode_produk = tr_penjualan_detail.kode_produk
group by ms_produk.kode_produk,ms_produk.nama_produk
order by  sum(tr_penjualan_detail.qty) desc limit 2;

```
</details>

<b>9. Pelanggan Paling Tinggi Nilai Belanjanya</b>
![image](https://user-images.githubusercontent.com/68532033/89723735-5a7c6100-da24-11ea-89d2-b7c47b218f6d.png)
<details>
  <summary>Click to show query sql</summary>

```
select tr_penjualan.kode_pelanggan,ms_pelanggan.nama_pelanggan,
sum(tr_penjualan_detail.qty*tr_penjualan_detail.harga_satuan) as total_harga
from tr_penjualan
JOIN ms_pelanggan
ON tr_penjualan.kode_pelanggan = ms_pelanggan.kode_pelanggan
JOIN tr_penjualan_detail
ON tr_penjualan.kode_transaksi = tr_penjualan_detail.kode_transaksi
group by tr_penjualan.kode_pelanggan,ms_pelanggan.nama_pelanggan
order by sum(tr_penjualan_detail.qty*tr_penjualan_detail.harga_satuan) desc limit 1;
```
</details>

<b>10. Pelanggan yang Belum Pernah Berbelanja</b>
![image](https://user-images.githubusercontent.com/68532033/89723762-90214a00-da24-11ea-8295-7deb9108994b.png)
<details>
  <summary>Click to show query sql</summary>

```
select ms_pelanggan.kode_pelanggan,ms_pelanggan.nama_pelanggan,ms_pelanggan.alamat
from ms_pelanggan
LEFT JOIN
tr_penjualan ON
ms_pelanggan.kode_pelanggan = tr_penjualan.kode_pelanggan
where tr_penjualan.kode_pelanggan is null;
```
</details>

<b>11. Transaksi Belanja dengan Daftar Belanja lebih dari 1</b>
![image](https://user-images.githubusercontent.com/68532033/89723285-5e0ce980-da1e-11ea-97d9-9a0e8ec4cbc8.png)
<details>
  <summary>Click to show query sql</summary>

```
select tr_penjualan_detail.kode_transaksi,ms_pelanggan.kode_pelanggan,ms_pelanggan.nama_pelanggan,tr_penjualan.tanggal_transaksi,count(tr_penjualan_detail.kode_produk) as jumlah_detail
from ms_pelanggan
LEFT JOIN tr_penjualan ON
tr_penjualan.kode_pelanggan = ms_pelanggan.kode_pelanggan
LEFT JOIN tr_penjualan_detail ON
tr_penjualan.kode_transaksi = tr_penjualan_detail.kode_transaksi
group by tr_penjualan_detail.kode_transaksi,ms_pelanggan.kode_pelanggan,ms_pelanggan.nama_pelanggan,tr_penjualan.tanggal_transaksi
having count(tr_penjualan_detail.kode_produk)>1;
```
</details>






