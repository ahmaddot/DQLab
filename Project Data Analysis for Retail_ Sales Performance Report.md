![image](https://user-images.githubusercontent.com/68532033/89724335-d5954580-da2b-11ea-8058-bed5e3995cdd.png)
```
Untuk menyelesaikan project, maka kita akan mengetikkan code yang perlu disubmit untuk dicek jawabannya benar atau salah.
Dari data yang sudah diberikan, dari pihak manajemen DQLab store ingin mengetahui:
1A. Overall perofrmance DQLab Store dari tahun 2009 - 2012 untuk jumlah order dan total sales order finished
1B. Overall performance DQLab by subcategory product yang akan dibandingkan antara tahun 2011 dan tahun 2012
2A. Efektifitas dan efisiensi promosi yang dilakukan selama ini, dengan menghitung burn rate dari promosi yang dilakukan overall berdasarkan tahun
2B. Efektifitas dan efisiensi promosi yang dilakukan selama ini, dengan menghitung burn rate dari promosi yang dilakukan overall berdasarkan sub-category

Setelah melihat hasil analisa di Sub Bab 1 dan 2, selanjutnya dilakukan analisa terhadap customer DQLab. Analisa dari sisi customer dengan menggunakan metrics:
3A. Analisa terhadap customer setiap tahunnya
3B. Analisa terhadap jumlah customer baru setiap tahunnya
3C. Cohort untuk mengetahui angka retention customer tahun 2009
```
![image](https://user-images.githubusercontent.com/68532033/89724468-97992100-da2d-11ea-8dd2-5b793b6c868b.png)

<b>1. Overall Performance by Year</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89724480-beefee00-da2d-11ea-9502-f7f9c75a772e.png)

<details>
  <summary>Click to show query sql</summary>
  <p>

```
select left(order_date,4) as years,sum(sales) as sales,count(order_status) as number_of_order from dqlab_sales_store
where order_status = 'Order Finished'
group by left(order_date,4);
```
  </p>
</details>


<b>2. Overall Performance by Product Sub Category</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89724456-49841d80-da2d-11ea-8034-0a76560cd6c2.png)
<details>
  <summary>Click to show query sql</summary>
  <p>

```
select left(order_date,4) as years,product_sub_category,sum(sales) as sales
from dqlab_sales_store
where left(order_date,4) in ('2011','2012') and order_status = 'Order Finished'
group by left(order_date,4),product_sub_category
order by left(order_date,4),sum(sales) desc;
```
  </p>
</details>


<b>3. Promotion Effectiveness and Efficiency by Years</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89724488-f52d6d80-da2d-11ea-8a86-40d80bf4a842.png)
<details>
  <summary>Click to show query sql</summary>
  <p>

```
select left(order_date,4) as years,sum(sales) as sales,sum(discount_value) as promotion_value,
round((sum(discount_value)/sum(sales))*100,2) as burn_rate_percentage
from dqlab_sales_store
where order_status = 'Order Finished'
group by left(order_date,4);
```
  </p>
</details>

<b>4. Promotion Effectiveness and Efficiency by Product Sub Category</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89724506-2017c180-da2e-11ea-86b7-9bb7702bc41d.png)
<details>
  <summary>Click to show query sql</summary>
  <p>

```
select left(order_date,4) as years,product_sub_category,product_category,sum(sales) as sales,sum(discount_value) as promotion_value,round((sum(discount_value)/sum(sales))*100,2) as burn_rate_percentage
from dqlab_sales_store
where left(order_date,4) ='2012' and order_status = 'Order Finished'
group by left(order_date,4),product_sub_category,product_category
order by sum(sales) desc;
```
  </p>
</details>


<b>5. Customers Transactions per Year</b>
<br>
![image](https://user-images.githubusercontent.com/68532033/89724522-4c334280-da2e-11ea-96ca-37a596988cfe.png)
<details>
  <summary>Click to show query sql</summary>
  <p>

```
select left(order_date,4) as years, count(distinct customer) as number_of_customer
from dqlab_sales_store
where left(order_date,4) in ('2009','2010','2011','2012') and order_status = 'Order Finished'
group by left(order_date,4)
order by left(order_date,4);
```
  </p>
</details>
