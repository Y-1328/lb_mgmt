Database Design (Entities & Attributes)
Since we are using MySQL, we need to normalize these entities to avoid data redundancy.

```Librarian 
librarian_id: UUID (Primary Key)
user_id: UUID (Foreign Key to User table)
staff_code: VARCHAR(20) (Unique Employee ID)
managed_dept: VARCHAR(50) (Department Name)
inventory_log_id: UUID (Foreign Key to Inventory Log)
approval_queue_count: INT (Pending Requests Count)

