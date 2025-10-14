# My Event Ticketing SQL Project

<a name="readme-top"></a>

<!-- TABLE OF CONTENTS -->

# 📗 Table of Contents

- [My SQL Project](#about-project)
- [📗 Table of Contents](#-table-of-contents)
- [📖 My SQL Project](#about-project)
  - [🛠 Built With ](#-built-with-)
    - [Tech Stack ](#tech-stack-)
    - [Key Features ](#key-features-)
  - [💻 Getting Started ](#-getting-started-)
    - [Prerequisites](#prerequisites)
    - [Setup](#setup)
    - [Usage](#usage)
  - [👥 Authors ](#-authors-)
  - [🔭 Future Features ](#-future-features-)
  - [🤝 Contributing ](#-contributing-)

<!-- PROJECT DESCRIPTION -->

# 📖 My SQL Project <a name="about-project"></a>

**My SQL Project** is a simple Database that uses SQL, Postgres via Supabase and R to create, query and secure a **Event Ticketing** database.

## 🛠 Built With <a name="built-with"></a>

### Tech Stack <a name="tech-stack"></a>
- SQL
- Postgres DB

<!-- Features -->

### Key Features <a name="key-features"></a>

- [ ] **Tables**
- [ ] **Schema**
- [ ] **Access control**

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->

## 💻 Getting Started <a name="getting-started"></a>

To rebuild this DB, follow these steps.

### Prerequisites

To run this project, you need:
- [A Supabase account](https://supabase.com/)
- [Knowledge on SQL](https://www.w3schools.com/sql/)
- A schema for creating your tables in the DB

<!-- ### Setup -->
### Setup

Copy the contents of this Readme.md to your Project's file

OR

Clone this repository to your desired folder:

```sh
  https://github.com/Evans-dotcom/Data-Tools.git
  cd Event-Ticketing
```

<!-- ### DB Creation -->

### DB Schema
- My Database is Called Event Ticketing.
- The DB is made up of 4 tables. Eaach table has 5 entries.

```sql
-- Drop old tables if they exist
DROP TABLE IF EXISTS events CASCADE;
DROP TABLE IF EXISTS customers CASCADE;
DROP TABLE IF EXISTS tickets CASCADE;
DROP TABLE IF EXISTS Payments CASCADE;

-- CREATE TABLE events (
    event_id SERIAL PRIMARY KEY,
    event_name VARCHAR(100) NOT NULL,
    event_date DATE NOT NULL,
    location VARCHAR(100),
    price NUMERIC(10,2)
);

CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20),
    city VARCHAR(50)
);

CREATE TABLE tickets (
    ticket_id SERIAL PRIMARY KEY,
    event_id INT REFERENCES events(event_id) ON DELETE CASCADE,
    customer_id INT REFERENCES customers(customer_id) ON DELETE CASCADE,
    seat_number VARCHAR(10),
    purchase_date DATE
);
CREATE TABLE payments (
    payment_id SERIAL PRIMARY KEY,
    ticket_id INT REFERENCES tickets(ticket_id) ON DELETE CASCADE,
    payment_date DATE,
    payment_method VARCHAR(50),
    amount NUMERIC(10,2),
    transaction_ref VARCHAR(50)
);

INSERT INTO events (event_name, event_date, location, price) VALUES
('Tech Conference 2025', '2025-11-15', 'Nairobi', 5000.00),
('Music Fest Kenya', '2025-12-20', 'Mombasa', 3500.00),
('Startup Pitch Night', '2025-10-30', 'Kisumu', 2000.00),
('Art & Culture Expo', '2026-01-10', 'Nakuru', 2500.00),
('Comedy Live Show', '2025-11-05', 'Eldoret', 1500.00);

INSERT INTO customers (full_name, email, phone, city) VALUES
('Evans Langat', 'evans@example.com', '0719127100', 'Nairobi'),
('Mary Wambui', 'maryw@example.com', '0721345678', 'Mombasa'),
('Brian Otieno', 'brian.otieno@gmail.com', '0733345678', 'Kisumu'),
('Lucy Njeri', 'lucy.njeri@yahoo.com', '0712456789', 'Nakuru'),
('John Mwangi', 'john.mwangi@outlook.com', '0745678901', 'Eldoret');

INSERT INTO tickets (event_id, customer_id, seat_number, purchase_date) VALUES
(1, 1, 'A12', '2025-11-01'),
(2, 2, 'B05', '2025-12-01'),
(3, 3, 'C08', '2025-10-20'),
(1, 4, 'A15', '2025-11-02'),
(5, 5, 'D02', '2025-11-03');

INSERT INTO payments (ticket_id, payment_date, payment_method, amount, transaction_ref) VALUES
(1, '2025-11-01', 'M-Pesa', 5000.00, 'MP123456'),
(2, '2025-12-01', 'Card', 3500.00, 'CR987654'),
(3, '2025-10-20', 'Cash', 2000.00, 'CS112233'),
(4, '2025-11-02', 'Bank Transfer', 5000.00, 'BT445566'),
(5, '2025-11-03', 'M-Pesa', 1500.00, 'MP778899');

```

- The Tables should look like this in Supabase:
Tickets
<img width="1878" height="923" alt="image" src="https://github.com/user-attachments/assets/1bc9c400-0e1a-4692-b43a-59d2f03f69a8" />

Events:
<img width="1883" height="933" alt="image" src="https://github.com/user-attachments/assets/44493db0-3329-4cb4-a8ad-7c3dbc6588b5" />

customers:
<img width="1906" height="929" alt="image" src="https://github.com/user-attachments/assets/92a79be6-4814-4a5c-8f8c-45c4401619f0" />

Payments:
<img width="1891" height="954" alt="image" src="https://github.com/user-attachments/assets/37aadf83-a6e9-4c56-8e74-9b9b576dff4b" />

- The ERD screenshot from Supabase Event Ticketing is As Shown: 
<img width="1895" height="960" alt="image" src="https://github.com/user-attachments/assets/e6a793d7-fe94-44ea-9e22-2921f43ebc68" />

- To test the table, I used two queries: 

```sql
SELECT t.* 
FROM tickets t
JOIN customers c ON t.customer_id = c.customer_id
WHERE c.full_name = 'Evans Langat';
````

```sql
SELECT * FROM events
WHERE location = 'Nairobi';
````

- Here are the results of the queries:
<img width="1852" height="831" alt="image" src="https://github.com/user-attachments/assets/9ef7dcb2-d3f1-4874-8a83-5ff9ad94e83d" />
<img width="1895" height="879" alt="image" src="https://github.com/user-attachments/assets/dd96ddd6-7f93-4c1a-bdc8-99b7ef820179" />

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- AUTHORS -->

## 👥 Authors <a name="authors"></a>

👤 **Evans Kibet**

- GitHub: [@evans-dotcom](https://github.com/evans-dotcom)
- Twitter: [@Evanzsan](https://x.com/EvanzSan?t=NlAkmnbhX2vcV3m63yCYFA&s=09)
- LinkedIn: [@EvansKibet](https://www.linkedin.com/in/evans-langat-680b05342/))

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- FUTURE FEATURES -->

## 🔭 Future Features <a name="future-features"></a>

- [ ] **Add security**
- [ ] **Link DB to R for visualisation purposes and further analyses**

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTRIBUTING -->

## 🤝 Contributing <a name="contributing"></a>

Contributions, issues, and feature requests are welcome!

Feel free to check the [issues page](../../issues/).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- SUPPORT -->
