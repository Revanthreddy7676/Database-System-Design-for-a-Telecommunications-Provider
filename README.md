mysql> CREATE TABLE customers (
    ->     customer_id INT PRIMARY KEY AUTO_INCREMENT,
    ->     first_name VARCHAR(50),
    ->     last_name VARCHAR(50),
    ->     email VARCHAR(100),
    ->     phone VARCHAR(20),
    ->     address TEXT,
    ->     city VARCHAR(50),
    ->     state VARCHAR(50),
    ->     country VARCHAR(50),
    ->     customer_type VARCHAR(20),
    ->     registration_date DATE
    -> );
Query OK, 0 rows affected (0.07 sec)

mysql> CREATE TABLE plans (
    ->     plan_id INT PRIMARY KEY AUTO_INCREMENT,
    ->     plan_name VARCHAR(100),
    ->     plan_type VARCHAR(50),
    ->     monthly_fee DECIMAL(10,2),
    ->     data_limit_gb INT,
    ->     voice_limit_minutes INT,
    ->     sms_limit INT,
    ->     validity_days INT,
    ->     status VARCHAR(20)
    -> );
Query OK, 0 rows affected (0.04 sec)

mysql> CREATE TABLE subscriptions (
    ->     subscription_id INT PRIMARY KEY AUTO_INCREMENT,
    ->     customer_id INT,
    ->     plan_id INT,
    ->     start_date DATE,
    ->     end_date DATE,
    ->     status VARCHAR(20),
    ->     FOREIGN KEY (customer_id) REFERENCES customers(customer_id),
    ->     FOREIGN KEY (plan_id) REFERENCES plans(plan_id)
    -> );
Query OK, 0 rows affected (0.10 sec)

mysql> CREATE TABLE usage_details (
    ->     usage_id INT PRIMARY KEY AUTO_INCREMENT,
    ->     subscription_id INT,
    ->     usage_date DATE,
    ->     data_used_gb DECIMAL(5,2),
    ->     voice_minutes_used INT,
    ->     sms_used INT,
    ->     roaming_used_gb DECIMAL(5,2),
    ->     FOREIGN KEY (subscription_id) REFERENCES subscriptions(subscription_id)
    -> );
Query OK, 0 rows affected (0.06 sec)

mysql> CREATE TABLE billing (
    ->     bill_id INT PRIMARY KEY AUTO_INCREMENT,
    ->     subscription_id INT,
    ->     billing_period_start DATE,
    ->     billing_period_end DATE,
    ->     total_charges DECIMAL(10,2),
    ->     billing_date DATE,
    ->     due_date DATE,
    ->     status VARCHAR(20),
    ->     FOREIGN KEY (subscription_id) REFERENCES subscriptions(subscription_id)
    -> );
Query OK, 0 rows affected (0.06 sec)

mysql> CREATE TABLE payments (
    ->     payment_id INT PRIMARY KEY AUTO_INCREMENT,
    ->     bill_id INT,
    ->     payment_date DATE,
    ->     amount_paid DECIMAL(10,2),
    ->     payment_method VARCHAR(50),
    ->     transaction_id VARCHAR(100),
    ->     status VARCHAR(20),
    ->     FOREIGN KEY (bill_id) REFERENCES billing(bill_id)
    -> );
Query OK, 0 rows affected (0.06 sec)

mysql> INSERT INTO customers (customer_id, first_name, last_name, email, phone, address, city, state, country, customer_type, registration_date)
    -> VALUES
    -> (1, 'John', 'Doe', 'john.doe@example.com', '1234567890', '123 Main St', 'New York', 'NY', 'USA', 'Postpaid', '2024-01-15'),
    -> (2, 'Alice', 'Smith', 'alice.smith@example.com', '2345678901', '456 Elm St', 'Los Angeles', 'CA', 'USA', 'Prepaid', '2024-02-10');
Query OK, 2 rows affected (0.03 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> •        Plans table:
    ->
    -> INSERT INTO plans (plan_id, plan_name, plan_type, monthly_fee, data_limit_gb, voice_limit_minutes, sms_limit, validity_days, status)
    -> VALUES
    -> (1, 'Gold Plan', 'Postpaid', 50.00, 50, 1000, 500, 30, 'Active'),
    -> (2, 'Silver Plan', 'Prepaid', 30.00, 20, 500, 200, 30, 'Active');
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '•Plans table:

INSERT INTO plans (plan_id, plan_name, plan_type, monthly_fee,' at line 1
mysql> INSERT INTO subscriptions (subscription_id, customer_id, plan_id, start_date, end_date, status)
    -> VALUES
    -> (101, 1, 1, '2024-03-01', '2024-03-31', 'Active'),
    -> (102, 2, 2, '2024-03-05', '2024-04-04', 'Active');
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`telecom`.`subscriptions`, CONSTRAINT `subscriptions_ibfk_2` FOREIGN KEY (`plan_id`) REFERENCES `plans` (`plan_id`))
mysql> •        Usage_details table (formerly usage)
    ->
    -> INSERT INTO usage_details (usage_id, subscription_id, usage_date, data_used_gb, voice_minutes_used, sms_used, roaming_used_gb)
    -> VALUES
    -> (1001, 101, '2024-03-10', 2.5, 45, 10, 0.5),
    -> (1002, 101, '2024-03-20', 3.0, 60, 15, 1.0),
    -> (1003, 102, '2024-03-15', 1.0, 30, 5, 0.0);
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '•Usage_details table (formerly usage)

INSERT INTO usage_details (usage_id, s' at line 1
mysql> INSERT INTO plans (plan_id, plan_name, plan_type, monthly_fee, data_limit_gb, voice_limit_minutes, sms_limit, validity_days, status)
    -> VALUES
    -> (1, 'Gold Plan', 'Postpaid', 50.00, 50, 1000, 500, 30, 'Active'),
    -> (2, 'Silver Plan', 'Prepaid', 30.00, 20, 500, 200, 30, 'Active');
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> INSERT INTO subscriptions (subscription_id, customer_id, plan_id, start_date, end_date, status)
    -> VALUES
    -> (101, 1, 1, '2024-03-01', '2024-03-31', 'Active'),
    -> (102, 2, 2, '2024-03-05', '2024-04-04', 'Active');
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> INSERT INTO usage_details (usage_id, subscription_id, usage_date, data_used_gb, voice_minutes_used, sms_used, roaming_used_gb)
    -> VALUES
    -> (1001, 101, '2024-03-10', 2.5, 45, 10, 0.5),
    -> (1002, 101, '2024-03-20', 3.0, 60, 15, 1.0),
    -> (1003, 102, '2024-03-15', 1.0, 30, 5, 0.0);
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> INSERT INTO billing (bill_id, subscription_id, billing_period_start, billing_period_end, total_charges, billing_date, due_date, status)
    -> VALUES
    -> (201, 101, '2024-03-01', '2024-03-31', 55.00, '2024-04-01', '2024-04-10', 'Unpaid'),
    -> (202, 102, '2024-03-05', '2024-04-04', 30.00, '2024-04-05', '2024-04-14', 'Paid');
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> INSERT INTO payments (payment_id, bill_id, payment_date, amount_paid, payment_method, transaction_id, status)
    -> VALUES
    -> (301, 202, '2024-04-06', 30.00, 'Credit Card', 'TXN2023', 'Completed');
Query OK, 1 row affected (0.01 sec)

mysql> SELECT
    ->     c.first_name, c.last_name, c.email,
    ->     s.subscription_id, s.start_date, s.end_date,
    ->     p.plan_name, p.monthly_fee
    -> FROM customers c
    -> JOIN subscriptions s ON c.customer_id = s.customer_id
    -> JOIN plans p ON s.plan_id = p.plan_id
    -> WHERE s.status = 'Active';
+------------+-----------+-------------------------+-----------------+------------+------------+-------------+-------------+
| first_name | last_name | email                   | subscription_id | start_date | end_date   | plan_name   | monthly_fee |
+------------+-----------+-------------------------+-----------------+------------+------------+-------------+-------------+
| John       | Doe       | john.doe@example.com    |             101 | 2024-03-01 | 2024-03-31 | Gold Plan   |       50.00 |
| Alice      | Smith     | alice.smith@example.com |             102 | 2024-03-05 | 2024-04-04 | Silver Plan |       30.00 |
+------------+-----------+-------------------------+-----------------+------------+------------+-------------+-------------+
2 rows in set (0.01 sec)
# Database-System-Design-for-a-Telecommunications-Provider
