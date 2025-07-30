# Gravity Book Store 📚

Gravity Book Store is a data analysis and visualization project that provides insights into customer behavior, book sales, publisher performance, and order trends in an online book store.

## 📌 Project Overview

This project follows a complete data warehousing and BI pipeline:

1. **Data Modeling**: Designed a Snowflake schema for the book store data warehouse.
2. **ETL Process**: Built and automated data flows using SSIS (SQL Server Integration Services).
3. **Data Visualization**: Created four interactive dashboards using Power BI.

The dashboards help stakeholders monitor:
- Top-performing customers and their locations.
- Sales trends by publisher and publication date.
- Number of books and orders by various dimensions.
- Comparative analytics between countries and publishers.

---
## Tables description:
- **book**: a list of all books available in the store.
- **book_author**: stores the authors for each book, which is a many-to-many relationship.
- **author**: a list of all authors.
- **book_language**: a list of possible languages of books.
- **publisher**: a list of publishers for books.
- **customer**: a list of the customers of the Gravity Bookstore.
- **customer_address**: a list of addresses for customers, as a customer can have more than one address, and an address has more than one customer.
- **address_status**: a list of statuses for an address, because addresses can be current or old.
- **address**: a list of addresses in the system.
- **country**: a list of countries that addresses are in.
- **cust_order**: a list of orders placed by customers.
- **order_line**: a list of books that are a part of each order.
- **shipping_method**: the possible shipping methods for an order.
- **order_history**: the history of an order, such as ordered, cancelled, delivered.
- **order_status**: the possible statuses of an order.
---

## 🧩 Entity Relationship Diagram (ERD) – OLTP Database

The ERD below shows the structure of the operational database (OLTP) used to store day-to-day transactional data for the Gravity Book Store.

<img width="1252" height="717" alt="gravity book_erd" src="https://github.com/user-attachments/assets/9f49c041-e6df-4aa1-bcc2-f85e16a73717" />

### Key Relationships:

- 🧑‍💼 **Customer** entity is linked to their **address** using a bridge table `cust_add` to allow storing multiple addresses per customer.
- 📚 **Books** and **Authors** are connected through the `book_author` table, allowing a many-to-many relationship (e.g. a book with multiple authors, or an author with many books).
- 🛒 **Orders** (or sales) include references to customers and books, capturing who bought what and when.
- 🌐 Additional tables like `shipment_method` and `order_status` provide context to each transaction.

This ERD reflects a **normalized structure**, aiming for data consistency, flexibility, and minimal redundancy.
---
---

## 📦 Data Warehouse Design & Snowflake Model

To support analytical reporting and business intelligence, the OLTP data was transformed into a well-structured **Data Warehouse (DWH)** using a **snowflake schema**.

### 🎯 Design Highlights:

- The DWH is centered around a **fact table**:  
  `sales_fact_table`, which captures sales transactions across customers, books, time, and other dimensions.

- The model follows a **snowflake schema**, where dimension tables are further normalized into sub-dimensions to reduce redundancy and improve consistency.

---

### 📊 Fact Table: `sales_fact_table`

| Measure              | Description                                   |
|----------------------|-----------------------------------------------|
| `quantity` (Line ID) | Represents quantity sold per transaction      |
| `price`              |  book price                                   |
| `order_id`           | Unique identifier for each sale               |

> The fact table contains foreign keys linking to all dimensions.

---

### 🧩 Dimension Tables:

| Dimension Table           | Description                                                   |
|---------------------------|---------------------------------------------------------------|
| `dim_customer`            | Customer details (name, gender, email, phone)                 |
| `dim_book`                | Book info (name, ISBN, language, publisher)                  |
| `dim_author` (via book)   | Author names, connected via `book_author`                    |
| `dim_date`                | Standard calendar table (day, month, year, quarter, week)     |
| `dim_time`                | Detailed time table (hour, minute)                            |
| `dim_shipmentmethod`      | Delivery/shipment methods                                     |
| `dim_status_order`        | Order status (pending, completed, shipped, etc.)              |

Each dimension provides slicing/filtering capabilities for building dashboards in Power BI.

---

### 🛠️ Tools Used:

- **ETL:** SSIS to load and transform data from OLTP to DWH  
- **Storage:** SQL Server DWH  
- **Visualization:** Power BI

---

## 🧠 Benefits of the Snowflake Model:

- Data consistency through normalization  
- Better storage efficiency  
- Easier maintenance of shared dimensions (e.g., author/book)  
- Supports complex queries across time, customer behavior, and product performance

<img width="1008" height="530" alt="dwh_modeling" src="https://github.com/user-attachments/assets/827d76fb-d6af-484f-be45-cd089d703a75" />

---
##  Fact Table:
<img width="771" height="380" alt="fact table2" src="https://github.com/user-attachments/assets/8ca38a27-c189-4e53-af0a-6dd1eea0890c" />
<img width="757" height="378" alt="fact table1" src="https://github.com/user-attachments/assets/507b2d76-b28f-4fea-9619-6bc04ed48f56" />

---
## Dimensions:
<img width="628" height="288" alt="author" src="https://github.com/user-attachments/assets/deeefb4e-02c7-4640-8559-1cfe532183fa" />
<img width="611" height="311" alt="book_author" src="https://github.com/user-attachments/assets/5eacdfac-239e-4e29-ad0b-d78d397d9aa1" />
<img width="638" height="355" alt="dim_book" src="https://github.com/user-attachments/assets/2bbd4dd5-07ba-46e5-9ec6-010296b22b1b" />
<img width="581" height="328" alt="dim_address" src="https://github.com/user-attachments/assets/428f7731-3e0c-4007-8ad4-1d9267ad0c7c" />
<img width="597" height="357" alt="customer_address" src="https://github.com/user-attachments/assets/7183e0b4-3718-4521-890c-7ba85d92a636" />
<img width="578" height="326" alt="dim_customer" src="https://github.com/user-attachments/assets/23ed2a5a-7aa8-4214-a2f2-7097a3434a62" />
<img width="648" height="354" alt="dim_shipmethod" src="https://github.com/user-attachments/assets/1fcab655-ddc5-4827-8174-7d8781e2f5cd" />
<img width="590" height="287" alt="dim_status_order" src="https://github.com/user-attachments/assets/7cc29041-b724-45c0-a8e6-d646d5f66342" />


## 🛠️ Tools & Technologies

- **SQL Server** for data storage and warehouse design.
- **SSIS** for building ETL packages.
- **Power BI** for dashboard development.

---

## 📊 Dashboards

### 👤 Customer Dashboard
- Top 10 customers by order count and sales.
- Number of customers per country.
- Detailed customer breakdown.
<img width="982" height="553" alt="customer dashboard" src="https://github.com/user-attachments/assets/e549cd73-0d68-4489-aa73-fbfe5963f1a8" />



---

### 🌍 Publisher Dashboard
- Top publishers by number of books and sold units.
- Sales performance by publisher.
- Book counts by publication year.
<img width="979" height="554" alt="publisher dashboard" src="https://github.com/user-attachments/assets/ac20d456-e760-49cb-85ba-4c787db70e5d" />

---

### 📚 Book Dashboard
-Top books by sales and number of orders.
-Top authors by number of published books.
-Book overview with language and total sales.

<img width="983" height="555" alt="book dashboard" src="https://github.com/user-attachments/assets/7b4ac9d2-7c8d-4d54-8b72-736f8440d1d7" />
---

### 📦 Order Dashboard
-Orders by country, shipping method, and status.
-Total sales and orders over the years.
-Yearly trend of order count and revenue.

<img width="981" height="553" alt="order dashboard" src="https://github.com/user-attachments/assets/b8492718-4b78-470a-8665-98d4480a45c0" />



---

## 🧑‍💻 Author

This project was fully developed by **Amany Saeed** as a solo effort.

---

## 📂 How to Use

> You can open the Power BI `.pbix` files to explore the dashboards or use the data model in SQL Server to run your own queries.

---

## 📌 Future Work

- Add predictive analytics using Power BI or Python.
- Publish interactive dashboards online via Power BI Service.

---

## 🔗 License

This project is for educational and portfolio purposes.

