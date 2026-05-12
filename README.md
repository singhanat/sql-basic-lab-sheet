# SQL Basic Exam — Northwind Dataset

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

ฝ่ายขายต้องการจัดทำรายงาน **"ลูกค้าที่น่าสนใจ"** ในช่วงโค้งสุดท้ายของปี 1997
โดยมีเงื่อนไขทั้งหมดดังนี้

1. เป็นช่วง **ไตรมาส 4 ของปี 1997** (1 ตุลาคม – 31 ธันวาคม 1997)
2. ลูกค้ามาจาก **ประเทศในกลุ่ม Americas** ได้แก่ USA, Canada, Brazil, Mexico, Argentina, Venezuela
3. ชื่อบริษัท (**company_name**) ต้องมีคำว่า `'the'` หรือขึ้นต้นด้วยตัว `'A'` (เช็กแบบ case-insensitive)
4. แสดงเฉพาะ **customer_id ที่ไม่ซ้ำกัน** พร้อม company_name และ country

**Task:**

แสดง `customer_id`, `company_name` และ `country` ของลูกค้าที่ตรงเงื่อนไขทั้งหมด
เรียงตาม `country` ก่อน แล้วตาม `company_name`

**Sample Data:**

*Table: `customers`*

| customer_id | company_name | country |
| --- | --- | --- |
| ALFKI | Alfreds Futterkiste | Germany |
| ANATR | Ana Trujillo Emparedados | Mexico |
| THEBI | The Big Cheese | USA |
| RANCH | Rancho grande | Argentina |
| OLDWO | Old World Delicatessen | USA |
| GREAL | Great Lakes Food Market | USA |

*Table: `orders`*

| order_id | customer_id | order_date |
| --- | --- | --- |
| 10869 | THEBI | 1997-02-04 |
| 10948 | GREAL | 1997-11-19 |
| 10827 | THEBI | 1997-12-06 |
| 10960 | RANCH | 1997-10-03 |
| 10977 | ANATR | 1997-03-15 |
| 10987 | OLDWO | 1997-12-20 |

**Expected Output:**

| customer_id | company_name | country |
| --- | --- | --- |
| RANCH | Rancho grande | Argentina |
| GREAL | Great Lakes Food Market | USA |
| THEBI | The Big Cheese | USA |

> หมายเหตุ: OLDWO ตกรอบเพราะชื่อไม่ขึ้นต้นด้วย A และไม่มีคำว่า 'the' · ANATR ตกรอบเพราะ order_date ไม่อยู่ใน Q4

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
