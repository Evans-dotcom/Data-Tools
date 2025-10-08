# Data Dictionary for Event Ticketing Database 

 This data dictionary describes the structure and purpose of the database tables used in the Event Ticketing project. It defines each table, its columns, data types, and relationships to help developers and analysts clearly understand the data model. 

---
 
## Table: customers
Stores information about users who purchase tickets for events.  
 
<img width="909" height="350" alt="image" src="https://github.com/user-attachments/assets/7bedea0d-758d-493a-bbe7-e560b92506ab" />

---
 
## Table: events
Contains details about upcoming or past events available for ticket purchase. 
 <img width="870" height="380" alt="image" src="https://github.com/user-attachments/assets/38f3cae2-7372-408e-9f7e-82a217fb4799" />
 
---
 
## Table: tickets
Records tickets purchased by customers for specific events. 
 <img width="885" height="367" alt="image" src="https://github.com/user-attachments/assets/5b4d510c-ffe6-49a9-ac97-1de2c344edba" />

---
 
## Table: payments
Stores payment details for tickets purchased by customers  
 <img width="914" height="341" alt="image" src="https://github.com/user-attachments/assets/227813d3-36dd-41e9-9281-821e77855423" />

 
---
 
**Notes:**  
- Primary keys uniquely identify rows in each table.  
- Foreign keys (FK) connect relationships across tables (`songs → artists`, `user_favorites → users/songs`).  
- This schema supports queries like:  
  - "Find all songs liked by a user"  
  - "List all songs by a specific artist"  
  and many more queries based on your criterion. 
