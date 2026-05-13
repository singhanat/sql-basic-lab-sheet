# SQL Basic Exam — Northwind Dataset

## PG4ADMIN

URL: http://pgadmin4.datacaster.org/

Username
```
user_XXX@datacaster.org
```
XXX แทนด้วยลำดับของท่านที่ได้รับจาก TA

Password
```
strange_stoic_shannon
```

## การเพิ่ม PostgreSQL Database 

Username
```
dbuser
```

Password
```
tenacious_trusting_torvalds
```

![setup_1](img/setup_1.png)

![setup_1](img/setup_2.png)

![setup_1](img/setup_3.png)

## Data set
![ER](img/ER.png)


---

### Q01 : สินค้าที่หมดสต็อก
**Topic:** `Basic SQL Queries — SELECT / WHERE`

**Scenario:**
ทีม warehouse ต้องการรายชื่อสินค้าที่หมดสต็อก (units_in_stock = 0) เพื่อสั่งซื้อเพิ่ม

**Task:**
แสดง `product_name` และ `units_in_stock` ของสินค้าที่มี `units_in_stock = 0`

**Sample Data:**

*Table: `products`*

| product_id | product_name | units_in_stock |
| --- | --- | --- |
| 1 | Chai | 39 |
| 5 | Chef Anton's Gumbo Mix | 0 |
| 17 | Alice Mutton | 0 |
| 21 | Sir Rodney's Scones | 3 |

**Expected Output:**

| product_name | units_in_stock |
| --- | --- |
| Chef Anton's Gumbo Mix | 0 |
| Alice Mutton | 0 |

<details>
<summary>Solution</summary>

```sql
select
    product_name,
    units_in_stock
from products
where units_in_stock = 0
order by product_name;
```

</details>

---

### Q02 : ประเทศของลูกค้า (ไม่ซ้ำ)
**Topic:** `Basic SQL Queries — DISTINCT`

**Scenario:**
ฝ่าย marketing ต้องการทราบว่า Northwind มีลูกค้าอยู่ในประเทศใดบ้าง

**Task:**
แสดงรายการ `country` ที่ไม่ซ้ำกันของลูกค้าทั้งหมด เรียงตามตัวอักษร

**Sample Data:**

*Table: `customers`*

| customer_id | company_name | country |
| --- | --- | --- |
| ALFKI | Alfreds Futterkiste | Germany |
| ANATR | Ana Trujillo | Mexico |
| ANTON | Antonio Moreno | Mexico |
| AROUT | Around the Horn | UK |

**Expected Output:**

| country |
| --- |
| Argentina |
| Austria |
| Belgium |
| ... |

<details>
<summary>Solution</summary>

```sql
select distinct
    country
from customers
order by country;
```

</details>

---

### Q03 : สินค้าที่ชื่อขึ้นต้นด้วย 'C'
**Topic:** `Basic SQL Queries — LIKE`

**Scenario:**
ผู้ใช้ระบบค้นหาสินค้าโดยพิมพ์ตัวอักษร 'C' นำหน้า

**Task:**
แสดง `product_id` และ `product_name` ของสินค้าที่ชื่อขึ้นต้นด้วยตัวอักษร 'C'

**Sample Data:**

*Table: `products`*

| product_id | product_name |
| --- | --- |
| 1 | Chai |
| 2 | Chang |
| 3 | Aniseed Syrup |
| 4 | Chef Anton's Cajun Seasoning |

**Expected Output:**

| product_id | product_name |
| --- | --- |
| 1 | Chai |
| 2 | Chang |
| 4 | Chef Anton's Cajun Seasoning |

<details>
<summary>Solution</summary>

```sql
select
    product_id,
    product_name
from products
where product_name like 'C%'
order by product_name;
```

</details>

---

### Q04 : Order ในช่วงปี 1997
**Topic:** `Basic SQL Queries — BETWEEN / AND`

**Scenario:**
ฝ่ายบัญชีต้องการดึง order ทั้งหมดในปี 1997 เพื่อทำรายงานประจำปี

**Task:**
แสดง `order_id` และ `order_date` ของ order ที่เกิดขึ้นระหว่าง 1997-01-01 ถึง 1997-12-31

**Sample Data:**

*Table: `orders`*

| order_id | order_date |
| --- | --- |
| 10248 | 1996-07-04 |
| 10400 | 1997-01-01 |
| 10835 | 1997-12-15 |
| 10836 | 1998-01-05 |

**Expected Output:**

| order_id | order_date |
| --- | --- |
| 10400 | 1997-01-01 |
| ... | ... |
| 10835 | 1997-12-15 |

<details>
<summary>Solution</summary>

```sql
select
    order_id,
    order_date
from orders
where order_date between '1997-01-01' and '1997-12-31'
order by order_date;
```

</details>

---

### Challenge A : ลูกค้า VIP ที่สั่งของในช่วงปลายปี 1997

**Topic:** `WHERE · DISTINCT · LIKE · BETWEEN`

**Scenario:**

ฝ่ายขายต้องการรายชื่อ **customer_id ที่ไม่ซ้ำกัน** ของลูกค้าที่ตรงเงื่อนไขทั้งหมด

1. `customer_id` ขึ้นต้นด้วยตัวอักษร **'A'** หรือ **'E'**
2. `country` อยู่ในกลุ่ม Americas (**USA, Canada, Brazil, Mexico, Argentina, Venezuela**)
3. `customer_id` อยู่ในช่วงตัวอักษร **'AA' ถึง 'QZ'** (BETWEEN บน string)

**Task:**

แสดง `customer_id`, `company_name` และ `country`
ของลูกค้าที่ตรงเงื่อนไขทั้งหมด เรียงตาม `country` แล้วตาม `customer_id`

**Sample Data:**

*Table: `customers`*

| customer_id | company_name | country |
| --- | --- | --- |
| ANATR | Ana Trujillo Emparedados | Mexico |
| ANTON | Antonio Moreno Taquería | Mexico |
| EASTC | Eastern Connection | UK |
| ERNSH | Ernst Handel | Austria |
| AROUT | Around the Horn | UK |
| QUEEN | Queen Cozinha | Brazil |

**Expected Output:**

| customer_id | company_name | country |
| --- | --- | --- |
| QUEEN | Queen Cozinha | Brazil |
| ANATR | Ana Trujillo Emparedados | Mexico |
| ANTON | Antonio Moreno Taquería | Mexico |

> EASTC ตกรอบ — country = UK ไม่อยู่ใน Americas
> ERNSH ตกรอบ — ขึ้นต้นด้วย E แต่ country = Austria
> AROUT ตกรอบ — ขึ้นต้นด้วย A แต่ country = UK

---

### Q05 : ชื่อพนักงานตัวพิมพ์ใหญ่
**Topic:** `Functions — String`

**Scenario:**
ระบบ report ต้องการแสดงชื่อพนักงานในรูปแบบตัวพิมพ์ใหญ่ทั้งหมด

**Task:**
แสดง `employee_id` และชื่อเต็มของพนักงานโดยรวม `first_name` และ `last_name` เป็นตัวพิมพ์ใหญ่ทั้งหมด (`full_name`)

**Sample Data:**

*Table: `employees`*

| employee_id | first_name | last_name |
| --- | --- | --- |
| 1 | Nancy | Davolio |
| 2 | Andrew | Fuller |
| 3 | Janet | Leverling |

**Expected Output:**

| employee_id | full_name |
| --- | --- |
| 1 | NANCY DAVOLIO |
| 2 | ANDREW FULLER |
| 3 | JANET LEVERLING |

<details>
<summary>Solution</summary>

```sql
select
    employee_id,
    upper(first_name || ' ' || last_name) as full_name
from employees
order by employee_id;
```

</details>

---

### Q6 : จัดระดับ freight
**Topic:** `Functions — CASE WHEN`

**Scenario:**
ต้องการแบ่งกลุ่ม order ตามค่า freight เพื่อวิเคราะห์ต้นทุนการขนส่ง

**Task:**
แสดง `order_id`, `freight` และคอลัมน์ `freight_level` โดยใช้ CASE: freight < 50 = 'Low', 50–200 = 'Medium', > 200 = 'High'

**Sample Data:**

*Table: `orders`*

| order_id | freight |
| --- | --- |
| 10248 | 32.38 |
| 10249 | 11.61 |
| 10251 | 41.34 |
| 10252 | 51.30 |
| 10273 | 76.07 |
| 10372 | 890.78 |

**Expected Output:**

| order_id | freight | freight_level |
| --- | --- | --- |
| 10248 | 32.38 | Low |
| 10249 | 11.61 | Low |
| 10251 | 41.34 | Low |
| 10252 | 51.30 | Medium |
| 10273 | 76.07 | Medium |
| 10372 | 890.78 | High |

<details>
<summary>Solution</summary>

```sql
select
    order_id,
    freight,
    case
        when freight < 50            then 'Low'
        when freight between 50 and 200 then 'Medium'
        else 'High'
    end as freight_level
from orders
order by order_id;
```

</details>

---

### Q7 : แสดง region แทน NULL
**Topic:** `Functions — NULL / COALESCE`

**Scenario:**
ตาราง customers มี column `region` ที่บางแถวเป็น NULL สำหรับลูกค้าในยุโรป ต้องการแทนด้วย 'N/A'

**Task:**
แสดง `customer_id`, `company_name` และ `region` โดยถ้า `region` เป็น NULL ให้แสดงว่า `'N/A'` แทน

**Sample Data:**

*Table: `customers`*

| customer_id | company_name | region |
| --- | --- | --- |
| ALFKI | Alfreds Futterkiste | NULL |
| BLONP | Blondesddsl père | NULL |
| BOLID | Bólido Comidas | NULL |
| EASTC | Eastern Connection | NULL |
| GREAL | Great Lakes Food | OR |
| HUNGC | Hungry Coyote | OR |

**Expected Output:**

| customer_id | company_name | region |
| --- | --- | --- |
| ALFKI | Alfreds Futterkiste | N/A |
| BLONP | Blondesddsl père | N/A |
| GREAL | Great Lakes Food | OR |
| HUNGC | Hungry Coyote | OR |

<details>
<summary>Solution</summary>

```sql
select
    customer_id,
    company_name,
    coalesce(region, 'N/A') as region
from customers
order by customer_id;
```

</details>

---

### Challenge B : สรุประดับ Freight ต่อพนักงาน

**Topic:** `UPPER · CASE WHEN · COALESCE`

**Scenario:**

ฝ่าย logistics ต้องการรายงาน order แต่ละใบพร้อม metadata ดังนี้

1. **ชื่อพนักงาน** ในรูปแบบ `"LASTNAME, Firstname"` (นามสกุลตัวพิมพ์ใหญ่ + ลูกน้ำ + ชื่อจริง)
2. **ระดับ freight** แบ่งเป็น Low / Medium / High (< 50 / 50–200 / > 200)
3. **ship_region** — ถ้าเป็น NULL ให้แสดง `'Unspecified'`

**Task:**

แสดง `employee_label`, `order_id`, `freight`, `freight_level` และ `ship_region`
เรียงตาม `employee_label` แล้วตาม `freight` จากมากไปน้อย

**Sample Data:**

*Table: `employees`*

| employee_id | first_name | last_name |
| --- | --- | --- |
| 1 | Nancy | Davolio |
| 2 | Andrew | Fuller |

*Table: `orders`*

| order_id | employee_id | freight | ship_region |
| --- | --- | --- | --- |
| 10258 | 1 | 140.51 | NULL |
| 10270 | 1 | 136.54 | NULL |
| 10295 | 2 | 1.15 | OR |
| 10372 | 1 | 890.78 | NULL |
| 10515 | 2 | 204.47 | WA |

**Expected Output:**

| employee_label | order_id | freight | freight_level | ship_region |
| --- | --- | --- | --- | --- |
| DAVOLIO, Nancy | 10372 | 890.78 | High | Unspecified |
| DAVOLIO, Nancy | 10258 | 140.51 | Medium | Unspecified |
| DAVOLIO, Nancy | 10270 | 136.54 | Medium | Unspecified |
| FULLER, Andrew | 10515 | 204.47 | High | WA |
| FULLER, Andrew | 10295 | 1.15 | Low | OR |

---

### Q8 : Top 5 สินค้าราคาแพงสุด
**Topic:** `Sorting & Limiting — ORDER BY / LIMIT`

**Scenario:**
ฝ่าย marketing ต้องการรายชื่อสินค้า 5 อันดับแรกที่มีราคาแพงที่สุด

**Task:**
แสดง `product_name` และ `unit_price` ของสินค้า 5 อันดับที่มีราคาสูงสุด เรียงจากมากไปน้อย

**Sample Data:**

*Table: `products`*

| product_id | product_name | unit_price |
| --- | --- | --- |
| 38 | Côte de Blaye | 263.50 |
| 29 | Thüringer Rostbratwurst | 123.79 |
| 9 | Mishi Kobe Niku | 97.00 |
| 20 | Sir Rodney's Marmalade | 81.00 |
| 18 | Carnarvon Tigers | 62.50 |
| ... | ... | ... |

**Expected Output:**

| product_name | unit_price |
| --- | --- |
| Côte de Blaye | 263.50 |
| Thüringer Rostbratwurst | 123.79 |
| Mishi Kobe Niku | 97.00 |
| Sir Rodney's Marmalade | 81.00 |
| Carnarvon Tigers | 62.50 |

<details>
<summary>Solution</summary>

```sql
select
    product_name,
    unit_price
from products
order by unit_price desc
limit 5;
```

</details>

---

### Q9 : หน้าที่ 2 ของรายการ Order
**Topic:** `Sorting & Limiting — OFFSET (Pagination)`

**Scenario:**
ระบบ web ต้องการแสดงรายการ order แบบ pagination โดยแต่ละหน้าแสดง 10 รายการ

**Task:**
แสดง order หน้าที่ 2 (records ที่ 11–20) เรียงตาม `order_id` จากน้อยไปมาก

**Sample Data:**

*Table: `orders`*

| order_id | customer_id | order_date |
| --- | --- | --- |
| 10248 | VINET | 1996-07-04 |
| 10249 | TOMSP | 1996-07-05 |
| ... | ... | ... |
| 10257 | HILAA | 1996-07-22 |
| 10258 | ERNSH | 1996-07-23 |
| ... | ... | ... |

**Expected Output:**

| order_id | customer_id | order_date |
| --- | --- | --- |
| 10258 | ERNSH | 1996-07-23 |
| 10259 | CENTC | 1996-07-24 |
| ... | ... | ... |
| 10267 | FRANK | 1996-08-01 |

<details>
<summary>Solution</summary>

```sql
select
    order_id,
    customer_id,
    order_date
from orders
order by order_id
limit 10
offset 10;
```

</details>

---

### Challenge C : ระบบค้นหาสินค้าราคาแพง — หน้าที่ 2

**Topic:** `ORDER BY · LIMIT · OFFSET`

**Scenario:**

ทีม dev กำลังสร้างหน้า **"สินค้าราคาสูง"** บนเว็บไซต์
โดยระบบจะ **เรียงสินค้าจากราคาแพงที่สุด** และแสดงแบบ pagination **หน้าละ 5 รายการ**

ตอนนี้ผู้ใช้กดไปที่ **หน้าที่ 2** แล้ว

**Task:**

แสดง `product_name` และ `unit_price` ของสินค้า
**อันดับที่ 6–10** เมื่อเรียงตาม `unit_price` จากมากไปน้อย
(หน้า 2 ของ pagination ที่หน้าละ 5 รายการ)

**Sample Data:**

*Table: `products`*

| product_id | product_name | unit_price |
| --- | --- | --- |
| 38 | Côte de Blaye | 263.50 |
| 29 | Thüringer Rostbratwurst | 123.79 |
| 9 | Mishi Kobe Niku | 97.00 |
| 20 | Sir Rodney's Marmalade | 81.00 |
| 18 | Carnarvon Tigers | 62.50 |
| 59 | Raclette Courdavault | 55.00 |
| 51 | Manjimup Dried Apples | 53.00 |
| 62 | Tarte au sucre | 49.30 |
| 43 | Ipoh Coffee | 46.00 |
| 28 | Rössle Sauerkraut | 45.60 |
| ... | ... | ... |

**Expected Output:**

| product_name | unit_price |
| --- | --- |
| Raclette Courdavault | 55.00 |
| Manjimup Dried Apples | 53.00 |
| Tarte au sucre | 49.30 |
| Ipoh Coffee | 46.00 |
| Rössle Sauerkraut | 45.60 |

---

### Q10 : รายการสินค้าพร้อมชื่อ Category
**Topic:** `JOIN — INNER JOIN`

**Scenario:**
ผู้ใช้ต้องการเห็นชื่อ category ของสินค้าแทน category_id

**Task:**
แสดง `product_name`, `unit_price` และ `category_name` ของสินค้าทุกรายการ เรียงตาม `category_name` แล้วตาม `product_name`

**Sample Data:**

*Table: `products`*

| product_id | product_name | category_id | unit_price |
| --- | --- | --- | --- |
| 1 | Chai | 1 | 18.00 |
| 2 | Chang | 1 | 19.00 |
| 3 | Aniseed Syrup | 2 | 10.00 |

*Table: `categories`*

| category_id | category_name |
| --- | --- |
| 1 | Beverages |
| 2 | Condiments |

**Expected Output:**

| product_name | unit_price | category_name |
| --- | --- | --- |
| Chai | 18.00 | Beverages |
| Chang | 19.00 | Beverages |
| Aniseed Syrup | 10.00 | Condiments |
| ... | ... | ... |

<details>
<summary>Solution</summary>

```sql
select
    p.product_name,
    p.unit_price,
    c.category_name
from products as p
join categories as c
    on p.category_id = c.category_id
order by c.category_name, p.product_name;
```

</details>

---

### Q11 : ลูกค้าที่ยังไม่เคยสั่งซื้อ
**Topic:** `JOIN — LEFT JOIN`

**Scenario:**
ฝ่ายขายต้องการรายชื่อลูกค้าที่ยังไม่เคยมี order เพื่อติดตามกลับ

**Task:**
แสดง `customer_id` และ `company_name` ของลูกค้าที่ยังไม่มี order ใด ๆ ในระบบ

**Sample Data:**

*Table: `customers`*

| customer_id | company_name |
| --- | --- |
| ALFKI | Alfreds Futterkiste |
| PARIS | Paris spécialités |
| FISSA | FISSA Fabrica |

*Table: `orders`*

| order_id | customer_id |
| --- | --- |
| 10248 | VINET |
| 10249 | TOMSP |
| — (PARIS ไม่มี) | — |
| — (FISSA ไม่มี) | — |

**Expected Output:**

| customer_id | company_name |
| --- | --- |
| FISSA | FISSA Fabrica Inter |
| PARIS | Paris spécialités |

<details>
<summary>Solution</summary>

```sql
select
    c.customer_id,
    c.company_name
from customers as c
left join orders as o
    on c.customer_id = o.customer_id
where o.order_id is null
order by c.customer_id;
```

</details>

---

### Q12 : Order พร้อมชื่อลูกค้าและพนักงาน
**Topic:** `JOIN — Multiple JOINs`

**Scenario:**
ผู้จัดการต้องการรายงาน order พร้อมชื่อลูกค้าและชื่อพนักงานที่รับออเดอร์

**Task:**
แสดง `order_id`, `order_date`, `company_name` (ลูกค้า) และ `full_name` (ชื่อ+นามสกุลพนักงาน) เรียงตาม `order_date` desc

**Sample Data:**

*Table: `orders`*

| order_id | customer_id | employee_id | order_date |
| --- | --- | --- | --- |
| 10248 | VINET | 5 | 1996-07-04 |
| 10249 | TOMSP | 6 | 1996-07-05 |

*Table: `customers`*

| customer_id | company_name |
| --- | --- |
| VINET | Vins et alcools Chevalier |
| TOMSP | Toms Spezialitäten |

*Table: `employees`*

| employee_id | first_name | last_name |
| --- | --- | --- |
| 5 | Steven | Buchanan |
| 6 | Michael | Suyama |

**Expected Output:**

| order_id | order_date | company_name | full_name |
| --- | --- | --- | --- |
| 10249 | 1996-07-05 | Toms Spezialitäten | Michael Suyama |
| 10248 | 1996-07-04 | Vins et alcools Chevalier | Steven Buchanan |
| ... | ... | ... | ... |

<details>
<summary>Solution</summary>

```sql
select
    o.order_id,
    o.order_date,
    c.company_name,
    e.first_name || ' ' || e.last_name as full_name
from orders as o
join customers as c
    on o.customer_id = c.customer_id
join employees as e
    on o.employee_id = e.employee_id
order by o.order_date desc;
```

</details>

---

### Challenge D : รายการ Order ที่ยังไม่มีข้อมูล Shipper ครบ

**Topic:** `INNER JOIN · LEFT JOIN · Multiple JOINs`

**Scenario:**

ฝ่าย logistics ต้องการตรวจสอบ order ทุกใบในปี 1997
โดยต้องการเห็น **ชื่อลูกค้า**, **ชื่อพนักงาน** และ **ชื่อบริษัทขนส่ง**
แต่บาง order **ยังไม่ได้กำหนดบริษัทขนส่ง** (ship_via เป็น NULL) ให้แสดง `'Unassigned'` แทน

**Task:**

แสดง `order_id`, `order_date`, `company_name` (ลูกค้า),
`employee_name` (first + last ของพนักงาน) และ `shipper_name` (ชื่อบริษัทขนส่ง หรือ 'Unassigned')

กรองเฉพาะ order ในปี 1997 เรียงตาม `order_date`, `order_id`

**Sample Data:**

*Table: `orders`*

| order_id | customer_id | employee_id | ship_via | order_date |
| --- | --- | --- | --- | --- |
| 10400 | ERNSH | 1 | 3 | 1997-01-01 |
| 10401 | HANAR | 1 | 1 | 1997-01-01 |
| 10410 | BOTM | 3 | NULL | 1997-01-14 |

*Table: `customers`*

| customer_id | company_name |
| --- | --- |
| ERNSH | Ernst Handel |
| HANAR | Hanari Carnes |
| BOTM | Bottom-Dollar Markets |

*Table: `employees`*

| employee_id | first_name | last_name |
| --- | --- | --- |
| 1 | Nancy | Davolio |
| 3 | Janet | Leverling |

*Table: `shippers`*

| shipper_id | company_name |
| --- | --- |
| 1 | Speedy Express |
| 3 | Federal Shipping |

**Expected Output:**

| order_id | order_date | company_name | employee_name | shipper_name |
| --- | --- | --- | --- | --- |
| 10400 | 1997-01-01 | Ernst Handel | Nancy Davolio | Federal Shipping |
| 10401 | 1997-01-01 | Hanari Carnes | Nancy Davolio | Speedy Express |
| 10410 | 1997-01-14 | Bottom-Dollar Markets | Janet Leverling | Unassigned |

---

### Q13 : สินค้าที่เคยถูกสั่งซื้อใน order ของลูกค้า VINET
**Topic:** `Subquery — WHERE IN`

**Scenario:**
ต้องการรู้ว่า VINET เคยสั่งสินค้าอะไรบ้าง

**Task:**
แสดง `product_name` ของสินค้าทั้งหมดที่เคยปรากฏใน order ของลูกค้า `VINET`

**Sample Data:**

*Table: `orders`*

| order_id | customer_id |
| --- | --- |
| 10248 | VINET |
| 10249 | TOMSP |

*Table: `order_details`*

| order_id | product_id |
| --- | --- |
| 10248 | 11 |
| 10248 | 42 |
| 10248 | 72 |
| 10249 | 14 |

*Table: `products`*

| product_id | product_name |
| --- | --- |
| 11 | Queso Cabrales |
| 42 | Singaporean Hokkien Fried Mee |
| 72 | Mozzarella di Giovanni |
| 14 | Tofu |

**Expected Output:**

| product_name |
| --- |
| Mozzarella di Giovanni |
| Queso Cabrales |
| Singaporean Hokkien Fried Mee |

<details>
<summary>Solution</summary>

```sql
select
    product_name
from products
where product_id in (
    select od.product_id
    from order_details as od
    join orders as o
        on od.order_id = o.order_id
    where o.customer_id = 'VINET'
)
order by product_name;
```

</details>

---

### Q14 : Order ที่มี freight สูงกว่าค่าเฉลี่ย
**Topic:** `Subquery — Scalar`

**Scenario:**
ต้องการหา order ที่มีค่าขนส่งสูงกว่าค่าเฉลี่ยของทั้งระบบ

**Task:**
แสดง `order_id`, `customer_id` และ `freight` ของ order ที่มี `freight` สูงกว่าค่าเฉลี่ย freight ทั้งหมด เรียงจากมากไปน้อย

**Sample Data:**

*Table: `orders`*

| order_id | customer_id | freight |
| --- | --- | --- |
| 10248 | VINET | 32.38 |
| 10249 | TOMSP | 11.61 |
| 10372 | QUEEN | 890.78 |
| 10515 | QUICK | 204.47 |

**Expected Output:**

| order_id | customer_id | freight |
| --- | --- | --- |
| 10372 | QUEEN | 890.78 |
| 10515 | QUICK | 204.47 |
| ... | ... | ... |

<details>
<summary>Solution</summary>

```sql
select
    order_id,
    customer_id,
    freight
from orders
where freight > (
    select avg(freight)
    from orders
)
order by freight desc;
```

</details>

---

### Q15 : Category ที่มีสินค้ามากกว่า 10 รายการ
**Topic:** `Subquery — FROM clause`

**Scenario:**
ต้องการหา category ที่มีความหลากหลายของสินค้ามาก

**Task:**
แสดง `category_name` และ `product_count` ของ category ที่มีสินค้ามากกว่า 10 รายการ เรียงจากมากไปน้อย

**Sample Data:**

*Table: `categories`*

| category_id | category_name |
| --- | --- |
| 1 | Beverages |
| 2 | Condiments |
| 3 | Confections |

*Table: `products`*

| product_id | category_id |
| --- | --- |
| ... | 1 (12 items) |
| ... | 2 (12 items) |
| ... | 3 (13 items) |

**Expected Output:**

| category_name | product_count |
| --- | --- |
| Confections | 13 |
| Beverages | 12 |
| Condiments | 12 |
| ... | ... |

<details>
<summary>Solution</summary>

```sql
select
    c.category_name,
    pc.product_count
from categories as c
join (
    select
        category_id,
        count(*) as product_count
    from products
    group by category_id
) as pc
    on c.category_id = pc.category_id
where pc.product_count > 10
order by pc.product_count desc;
```

</details>

---

### Challenge E : สินค้าขายดี เฉพาะ Category ที่ Active จริง ๆ

**Topic:** `WHERE IN · Scalar Subquery · FROM clause`

**Scenario:**

ฝ่าย product ต้องการรายงาน **"สินค้าที่น่าลงทุน"** โดยมีเงื่อนไขทั้งหมดนี้

1. สินค้าต้องอยู่ใน **category ที่ "active"** — คือ category ที่มีจำนวนสินค้า **มากกว่าค่าเฉลี่ย** ของทุก category
2. สินค้านั้นต้องเคย **ถูกสั่งซื้อจริง** (ปรากฏใน `order_details` อย่างน้อย 1 ครั้ง)
3. แสดง `product_name`, `category_name`, `unit_price` และ `times_ordered` (จำนวนครั้งที่ถูกสั่ง)

เรียงตาม `times_ordered` จากมากไปน้อย

**Task:**

แสดง `product_name`, `category_name`, `unit_price` และ `times_ordered`
ของสินค้าที่ตรงเงื่อนไขทั้งหมด เรียงตาม `times_ordered` desc

**Sample Data:**

*Table: `categories`*

| category_id | category_name |
| --- | --- |
| 1 | Beverages |
| 2 | Condiments |
| 3 | Confections |
| 4 | Dairy Products |

*Table: `products`*

| product_id | product_name | category_id | unit_price |
| --- | --- | --- | --- |
| 1 | Chai | 1 | 18.00 |
| 2 | Chang | 1 | 19.00 |
| 3 | Aniseed Syrup | 2 | 10.00 |
| 11 | Queso Cabrales | 4 | 21.00 |
| 38 | Côte de Blaye | 1 | 263.50 |

*Table: `order_details` (ตัวอย่างบางส่วน)*

| order_id | product_id | quantity |
| --- | --- | --- |
| 10248 | 11 | 12 |
| 10248 | 42 | 10 |
| 10249 | 14 | 9 |
| 10250 | 41 | 10 |
| 10250 | 51 | 35 |

*จำนวนสินค้าต่อ category (ข้อมูลจริงใน Northwind):*

| category_name | product_count | เทียบกับค่าเฉลี่ย (~9.6) |
| --- | --- | --- |
| Confections | 13 |  มากกว่า |
| Beverages | 12 |  มากกว่า |
| Condiments | 12 |  มากกว่า |
| Seafood | 12 |  มากกว่า |
| Dairy Products | 10 |  มากกว่า |
| Meat/Poultry | 6 |  น้อยกว่า |
| Produce | 5 |  น้อยกว่า |
| Grains/Cereals | 7 |  น้อยกว่า |

**Expected Output** (บางส่วน):

| product_name | category_name | unit_price | times_ordered |
| --- | --- | --- | --- |
| Raclette Courdavault | Dairy Products | 55.00 | 54 |
| Camembert Pierrot | Dairy Products | 34.00 | 51 |
| Gorgonzola Telino | Dairy Products | 12.50 | 51 |
| Chang | Beverages | 19.00 | 44 |
| ... | ... | ... | ... |

> หมายเหตุ: ตัวเลขในตัวอย่างเป็นค่าสมมติเพื่ออธิบาย shape — ค่าจริงขึ้นกับ database

---

### Q16 : จำนวน Order ต่อลูกค้า
**Topic:** `GROUP BY — COUNT`

**Scenario:**
ฝ่ายขายต้องการรู้ว่าลูกค้าแต่ละรายมี order กี่ใบ

**Task:**
แสดง `customer_id` และ `order_count` (จำนวน order) เรียงจากมากไปน้อย แสดงเฉพาะลูกค้าที่มี order มากกว่า 5 ใบ

**Sample Data:**

*Table: `orders`*

| order_id | customer_id |
| --- | --- |
| 10248 | VINET |
| 10274 | VINET |
| 10295 | VINET |
| ... | VINET (7 total) |
| 10249 | TOMSP |
| 10270 | HANAR |
| ... | ... |

**Expected Output:**

| customer_id | order_count |
| --- | --- |
| SAVEA | 31 |
| ERNSH | 30 |
| QUICK | 28 |
| ... | ... |

<details>
<summary>Solution</summary>

```sql
select
    customer_id,
    count(*) as order_count
from orders
group by customer_id
having count(*) > 5
order by order_count desc;
```

</details>

---

### Q17 : ยอดขายรวมต่อ Category
**Topic:** `GROUP BY — SUM / Revenue`

**Scenario:**
ต้องการรายงานยอดขาย (revenue) รวมของสินค้าแต่ละ category

**Task:**
แสดง `category_name` และ `total_revenue` (= SUM ของ quantity × unit_price × (1 - discount)) เรียงจากมากไปน้อย

> Revenue Formula: `quantity × unit_price × (1 - discount)`

**Sample Data:**

*Table: `categories`*

| category_id | category_name |
| --- | --- |
| 1 | Beverages |
| 2 | Condiments |

*Table: `products`*

| product_id | category_id | unit_price |
| --- | --- | --- |
| 1 | 1 | 18.00 |

*Table: `order_details`*

| order_id | product_id | quantity | unit_price | discount |
| --- | --- | --- | --- | --- |
| 10248 | 11 | 12 | 14.00 | 0 |
| ... | ... | ... | ... | ... |

**Expected Output:**

| category_name | total_revenue |
| --- | --- |
| Beverages | 267868.19 |
| Dairy Products | 234507.29 |
| Confections | 167357.23 |
| ... | ... |

<details>
<summary>Solution</summary>

```sql
select
    c.category_name,
    round(
        sum(od.quantity * od.unit_price * (1 - od.discount))::numeric,
        2
    ) as total_revenue
from order_details as od
join products as p
    on od.product_id = p.product_id
join categories as c
    on p.category_id = c.category_id
group by c.category_name
order by total_revenue desc;
```

</details>

---

### Q18 : พนักงานที่รับ Order ค่าเฉลี่ย freight สูง
**Topic:** `GROUP BY — AVG / HAVING`

**Scenario:**
ต้องการหาพนักงานที่มีค่าเฉลี่ย freight ต่อ order สูงกว่า 80

**Task:**
แสดง `employee_id` และ `avg_freight` (ค่าเฉลี่ย freight ของ order ที่พนักงานรับ) เฉพาะพนักงานที่มี avg_freight > 80 เรียงจากมากไปน้อย

**Sample Data:**

*Table: `orders`*

| order_id | employee_id | freight |
| --- | --- | --- |
| 10248 | 5 | 32.38 |
| 10372 | 1 | 890.78 |
| 10515 | 3 | 204.47 |
| ... | ... | ... |

**Expected Output:**

| employee_id | avg_freight |
| --- | --- |
| 4 | 105.51 |
| 3 | 98.22 |
| ... | ... |

<details>
<summary>Solution</summary>

```sql
select
    employee_id,
    round(avg(freight)::numeric, 2) as avg_freight
from orders
group by employee_id
having avg(freight) > 80
order by avg_freight desc;
```

</details>

---

### Q19 : ยอดขายรายเดือนต่อพนักงาน ปี 1997
**Topic:** `GROUP BY — Multi-column + JOIN`

**Scenario:**
ผู้จัดการต้องการเห็น performance ของพนักงานแต่ละคนในแต่ละเดือนของปี 1997

**Task:**
แสดง `full_name` (ชื่อ+นามสกุลพนักงาน), `month` (เดือน 1-12) และ `monthly_revenue` (SUM revenue) เฉพาะปี 1997 เรียงตาม `full_name`, `month`

**Sample Data:**

*Table: `employees`*

| employee_id | first_name | last_name |
| --- | --- | --- |
| 1 | Nancy | Davolio |
| ... | ... | ... |

*Table: `orders`*

| order_id | employee_id | order_date |
| --- | --- | --- |
| 10400 | 1 | 1997-01-01 |
| ... | ... | ... |

*Table: `order_details`*

| order_id | quantity | unit_price | discount |
| --- | --- | --- | --- |
| 10400 | ... | ... | ... |

**Expected Output:**

| full_name | month | monthly_revenue |
| --- | --- | --- |
| Andrew Fuller | 1 | 3500.25 |
| Andrew Fuller | 2 | 4200.10 |
| ... | ... | ... |

<details>
<summary>Solution</summary>

```sql
select
    e.first_name || ' ' || e.last_name as full_name,
    extract(month from o.order_date)::integer as month,
    round(
        sum(od.quantity * od.unit_price * (1 - od.discount))::numeric,
        2
    ) as monthly_revenue
from employees as e
join orders as o
    on e.employee_id = o.employee_id
join order_details as od
    on o.order_id = od.order_id
where extract(year from o.order_date) = 1997
group by e.first_name, e.last_name, extract(month from o.order_date)
order by full_name, month;
```

</details>

---

### Challenge F : สรุปยอดขายต่อพนักงาน ต่อ Category ปี 1997

**Topic:** `COUNT · SUM · AVG · HAVING · Multi-column GROUP BY`

**Scenario:**

ผู้บริหารต้องการรายงานว่า **พนักงานแต่ละคน** ขายสินค้าใน **category ไหนได้เท่าไหร่** ในปี 1997
โดยต้องการเห็นเฉพาะ **คู่ employee–category ที่มียอดขายสูงกว่าค่าเฉลี่ยของทุกคู่**

**Task:**

แสดง `employee_name` (first + last), `category_name`,
`order_count` (จำนวน order ใบที่มีสินค้า category นั้น),
`total_revenue` (SUM revenue) และ `avg_revenue` (AVG revenue ต่อ order)

กรองเฉพาะ **คู่ที่มี total_revenue สูงกว่าค่าเฉลี่ย total_revenue ของทุกคู่**

เรียงตาม `total_revenue` จากมากไปน้อย

**Sample Data:**

*Table: `employees`*

| employee_id | first_name | last_name |
| --- | --- | --- |
| 1 | Nancy | Davolio |
| 2 | Andrew | Fuller |

*Table: `orders`*

| order_id | employee_id | order_date |
| --- | --- | --- |
| 10400 | 1 | 1997-01-01 |
| 10401 | 2 | 1997-01-01 |

*Table: `order_details`*

| order_id | product_id | quantity | unit_price | discount |
| --- | --- | --- | --- | --- |
| 10400 | 1 | 12 | 18.00 | 0.0 |
| 10401 | 11 | 20 | 21.00 | 0.0 |

*Table: `products`*

| product_id | category_id |
| --- | --- |
| 1 | 1 |
| 11 | 4 |

*Table: `categories`*

| category_id | category_name |
| --- | --- |
| 1 | Beverages |
| 4 | Dairy Products |

**Expected Output** (ค่าสมมติ):

| employee_name | category_name | order_count | total_revenue | avg_revenue |
| --- | --- | --- | --- | --- |
| Margaret Peacock | Dairy Products | 38 | 45230.50 | 1190.28 |
| Janet Leverling | Beverages | 29 | 38120.75 | 1314.51 |
| Nancy Davolio | Confections | 27 | 31450.20 | 1164.82 |
| ... | ... | ... | ... | ... |

> หมายเหตุ: ตัวเลขเป็นค่าสมมติ — ค่าจริงขึ้นกับ database

---

### Challenge G : ตาราง Freight Summary รายไตรมาส ต่อพนักงาน

**Topic:** `UPPER · CASE WHEN · COALESCE · GROUP BY · EXTRACT`

**Scenario:**

ผู้จัดการต้องการ **cross table** สรุปค่า freight รายไตรมาส ของแต่ละพนักงาน
โดยมีรูปแบบตาราง ดังนี้

- **แกนตั้ง (rows):** ปี + ชื่อพนักงาน (ตัวพิมพ์ใหญ่)
- **แกนนอน (columns):** Q1 · Q2 · Q3 · Q4 (ค่า freight รวมต่อไตรมาส)
- ถ้าพนักงานไม่มี order ในไตรมาสนั้น ให้แสดง `0.00` แทน NULL

> Q1 = Jan–Mar · Q2 = Apr–Jun · Q3 = Jul–Sep · Q4 = Oct–Dec

**Task:**

แสดง `order_year`, `employee_name` (UPPER ของ first_name + ' ' + last_name),
`q1_freight`, `q2_freight`, `q3_freight`, `q4_freight`

โดยแต่ละ column คือ **SUM ของ freight** ของ order ในไตรมาสนั้น
(ROUND 2 ตำแหน่ง, แสดง 0.00 เมื่อไม่มี order)

เรียงตาม `order_year` แล้วตาม `employee_name`

**Sample Data:**

*Table: `employees`*

| employee_id | first_name | last_name |
| --- | --- | --- |
| 1 | Nancy | Davolio |
| 2 | Andrew | Fuller |
| 3 | Janet | Leverling |

*Table: `orders` (ตัวอย่างบางส่วน)*

| order_id | employee_id | order_date | freight |
| --- | --- | --- | --- |
| 10258 | 1 | 1996-07-17 | 140.51 |
| 10270 | 1 | 1996-08-01 | 136.54 |
| 10295 | 2 | 1996-09-02 | 1.15 |
| 10400 | 1 | 1997-01-01 | 83.93 |
| 10410 | 3 | 1997-01-14 | 2.40 |
| 10500 | 1 | 1997-04-12 | 42.68 |
| 10512 | 3 | 1997-10-21 | 3.53 |

**Expected Output** (บางส่วน — ปี 1997):

| order_year | employee_name | q1_freight | q2_freight | q3_freight | q4_freight |
| --- | --- | --- | --- | --- | --- |
| 1997 | ANDREW FULLER | 41.72 | 292.43 | 382.11 | 130.22 |
| 1997 | JANET LEVERLING | 2.40 | 418.57 | 124.33 | 240.95 |
| 1997 | NANCY DAVOLIO | 187.83 | 150.49 | 230.18 | 101.27 |
| ... | ... | ... | ... | ... | ... |

> หมายเหตุ: ตัวเลขในตัวอย่างเป็นค่าสมมติ — ค่าจริงขึ้นกับ database
