# 🎟️ Event Ticketing Dataset Documentation (Posit + Supabase)

- This data dictionary documents all tables, columns, and relationships for the Event Ticketing Project, fully aligned with the R analytics workflow we perform in Posit (RStudio) using Supabase as the database.
---

## **Profiles Table**

| id                                   | full_name    | role  | created_at                    |
| ------------------------------------ | ------------ | ----- | ----------------------------- |
| eb08886f-6f46-4cae-a39e-7ad7619f7046 | Evans Langat | admin | 2025-10-19 19:18:59.347057+00 |
| d7544499-b7bf-4be0-82fb-b12ba5500293 | Mary Wambui  | user  | 2025-10-19 19:18:59.347057+00 |
| f3e60dc8-d3b4-49c7-ac7c-210373a8e4cf | Brian Otieno | user  | 2025-10-19 19:18:59.347057+00 |

**Usage in R:**  
* Identify user roles for activity segmentation (Admin vs. User)
* Join with customers via auth_user_id for top-buyer analysis

---

## **Customers Table**

| customer_id | full_name    | email                   | phone      | city    | auth_user_id                         |
| ----------- | ------------ | ----------------------- | ---------- | ------- | ------------------------------------ |
| 3           | Brian Otieno | brian.otieno@gmail.com  | 0733345678 | Kisumu  | yt08886f-6f46-4cae-a39e-7ad7619f8906                                 |
| 4           | Lucy Njeri   | lucy.njeri@yahoo.com    | 0712456789 | Nakuru  | gf98456f-6f46-7cea-a8h4-7ad7619f7098                                 |
| 5           | John Mwangi  | john.mwangi@outlook.com | 0745678901 | Eldoret | qa088539f-6f46-4cae-a39e-7ad7619f7679                                 |
| 1           | Evans Langat | evans@example.com       | 0719127100 | Nairobi | eb08886f-6f46-4cae-a39e-7ad7619f7046 |
| 2           | Mary Wambui  | maryw@example.com       | 0721345678 | Mombasa | d7544499-b7bf-4be0-82fb-b12ba5500293 |

**Usage in R:**  
* Joining with tickets for purchase analysis  
* Counting customers by city or event participation

---

## **Tickets Table**

| ticket_id | event_id | customer_id | seat_number | purchase_date | quantity |
| --------- | -------- | ----------- | ----------- | ------------- | -------- |
| 2         | 2        | 2           | B05         | 2025-12-01    | 1        |
| 3         | 3        | 3           | C08         | 2025-10-20    | 1        |
| 4         | 1        | 4           | A15         | 2025-11-02    | 1        |
| 5         | 5        | 5           | D02         | 2025-11-03    | 1        |
| 7         | 2        | 2           | null        | 2025-12-01    | 3        |
| 8         | 3        | 3           | null        | 2025-10-20    | 1        |
| 9         | 4        | 4           | null        | 2025-11-05    | 2        |
| 10        | 5        | 5           | null        | 2025-11-03    | 4        |

**Usage in R:**  
* Ticket volume per event (event_performance) 
* Top customers (total tickets bought)
---

## **Payments Table**

| payment_id | ticket_id | payment_date | payment_method | amount   | transaction_ref |
| ---------- | --------- | ------------ | -------------- | -------- | --------------- |
| 2          | 2         | 2025-12-01   | Card           | 3500.00  | CR987654        |
| 3          | 3         | 2025-10-20   | Cash           | 2000.00  | CS112233        |
| 4          | 4         | 2025-11-02   | Bank Transfer  | 5000.00  | BT445566        |
| 5          | 5         | 2025-11-03   | M-Pesa         | 1500.00  | MP778899        |
| 7          | 2         | 2025-12-01   | Card           | 10500.00 | CR987654        |
| 8          | 3         | 2025-10-20   | Cash           | 2000.00  | CS112233        |
| 9          | 4         | 2025-11-05   | Bank Transfer  | 5000.00  | BT445566        |
| 10         | 5         | 2025-11-03   | M-Pesa         | 6000.00  | MP778899        |

**Usage in R:**  
* Revenue aggregation per event or payment method 
* Monthly sales performance tracking 

---

## **Relationships**

* **profiles → Customers**: One-to-many  
* **customers → tickets**: One-to-many  
* **tickets → Payments**: One-to-many or Many-to- One 
---

## **Notes for Posit Analysis**

 ✅ Use dplyr for clean transformations (group_by, summarize)
 ✅ Use ggplot2 for professional, minimalistic visualizations
 ✅ Store R scripts as reproducible reports for periodic analytics
 ✅ Combine tables logically using their relationships for deeper insights

---

## **💡 Tip: Why Posit is Great**

Posit (RStudio) makes this workflow smooth because:

* **Seamless DB integration:** Supabase provides a scalable, relational PostgreSQL backend with easy auth integration.  
* **Powerful data wrangling:** `dplyr` allows quick aggregations and transformations.  
* **Visualization-ready:** `ggplot2` enables clean, publication-quality charts with minimal code.  
* **Reproducible workflows:** R scripts can be run repeatedly with updated data, ideal for analytics projects.  

---

# 🔗 Connecting Posit (RStudio) to Supabase

This guide explains how to connect Posit (RStudio) to your Supabase PostgreSQL database for analysis.

---

## **1. Install Required R Packages**

```r
install.packages("DBI")
install.packages("RPostgres")
install.packages("dplyr")
install.packages("ggplot2")
```

---

## **2. Obtain Supabase Database Credentials**

From Supabase → **Settings → Database → Connection info**:

- Host URL
- Port (default: 5432)
- Database name
- Username
- Password
- SSL mode (`require`)

---
## 🔗 Here is a quick visual to learn how to connect supabase database to R Posit
[Supabase Connection Documentation](https://supabase.com/docs/guides/database/connecting-to-postgres) **Learn more from here..**

<img width="1366" height="670" alt="image" src="https://github.com/user-attachments/assets/88c68653-aae2-4e18-8c6c-4006c2ba4da3" />


## **3. Connect from Posit**

```r
library(DBI)
library(RPostgres)

connect_db <- function() {
  con <- dbConnect(
    RPostgres::Postgres(),
    dbname = "your_database_name",
    host = "your_host_url",
    port = 5432,
    user = "your_username",
    password = "your_password",
    sslmode = "require"
  )
  return(con)
}
```

---

## **4. Test the Connection**

```r
source("connect_db.R")
con <- connect_db()
dbListTables(con)
users <- dbGetQuery(con, "SELECT * FROM users LIMIT 5;")
print(users)
```

---

## **5. Use in Analysis**

* Aggregate with `dplyr` (`count`, `group_by`, `summarize`)  
* Visualize with `ggplot2` (popular events, active tickets, Payment methods)  
* Query directly via `dbGetQuery()`

Example:
1️⃣ Top Ticket Buyers
```r
top_buyers <- dbGetQuery(con, "
  SELECT c.full_name, COUNT(t.ticket_id) AS tickets_bought
  FROM customers c
  JOIN tickets t ON c.customer_id = t.customer_id
  GROUP BY c.full_name
  ORDER BY tickets_bought DESC;
")
ggplot(top_buyers, aes(x = reorder(full_name, tickets_bought), y = tickets_bought, fill = full_name)) +
  geom_col(show.legend = FALSE) +
  coord_flip() +
  labs(title = 'Top Ticket Buyers', x = 'Customer', y = 'Tickets Bought') +
  theme_minimal()
```

---

## **💡 Tip**

Connecting Posit to Supabase allows real-time queries, reproducible analysis, and clean integration with `ggplot2` for professional charts.
---
