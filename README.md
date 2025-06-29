# Registration_Page_with_PY-SQL-
Registration Page With Python Connect To SQL Database.
📝 User Registration System
A desktop-based user registration system built with Python, Tkinter, and MySQL, providing a graphical interface to register users, validate input, hash passwords securely, and manage user data.

🔧 Features
🧑 Register new users with username, email, and password

🔒 Passwords are securely hashed using SHA-256

✅ Validates form input: checks for required fields, email format, password length, and matching passwords

📬 Prevents duplicate usernames or emails

👀 View all registered users in a scrollable table

📦 Modular code structure using DatabaseConnection and RegistrationForm classes

💾 Requirements
Python 3.x

MySQL server (with a login_page database and users table)

Python packages:

pymysql

mysql-connector-python

tkinter (usually included with Python)

Install dependencies:
pip install pymysql mysql-connector-python

🗄️ Database Setup
Create a MySQL database and table:

CREATE DATABASE login_page;

USE login_page;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(100) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
To Run Code:-
SELECT * FROM Your_Database.Your_Table_Name;
