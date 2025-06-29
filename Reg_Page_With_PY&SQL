import pymysql
from mysql.connector import Error
import tkinter as tk
from tkinter import messagebox, ttk
import hashlib

class DatabaseConnection:
    def __init__(self):
        self.connection = None
        self.cursor = None
    
    def connect(self):
        try:
            self.connection = pymysql.connect(
                host='Your_Host', 
                port=Your_Port,
                user='Your_User',
                password='Your_Password', 
                database='login_page'
            )
            
            if self.connection:
                self.cursor = self.connection.cursor()
                print("Successfully connected to database")
                return True

                
        except Error as e:
            print(f"Error connecting to database: {e}")
            return False
    
    def disconnect(self):
        if self.connection and self.connection.is_connected():
            self.cursor.close()
            self.connection.close()
            print("Database connection closed")

class RegistrationForm:
    def __init__(self):
        self.db = DatabaseConnection()
        self.root = tk.Tk()
        self.setup_gui()
    
    def setup_gui(self):
        self.root.title("User Registration System")
        self.root.geometry("400x500")
        self.root.configure(bg="#f0f0f0")
        
        # Title
        title_label = tk.Label(
            self.root, 
            text="Registration Form", 
            font=("Arial", 18, "bold"),
            bg="#f0f0f0",
            fg="#333333"
        )
        title_label.pack(pady=20)
        
        form_frame = tk.Frame(self.root, bg="#f0f0f0")
        form_frame.pack(pady=20, padx=40, fill="both", expand=True)
        
        tk.Label(form_frame, text="Username:", font=("Arial", 12), bg="#f0f0f0").pack(anchor="w", pady=(0, 5))
        self.username_entry = tk.Entry(form_frame, font=("Arial", 12), width=30)
        self.username_entry.pack(pady=(0, 15))
        
        tk.Label(form_frame, text="Email:", font=("Arial", 12), bg="#f0f0f0").pack(anchor="w", pady=(0, 5))
        self.email_entry = tk.Entry(form_frame, font=("Arial", 12), width=30)
        self.email_entry.pack(pady=(0, 15))

        tk.Label(form_frame, text="Password:", font=("Arial", 12), bg="#f0f0f0").pack(anchor="w", pady=(0, 5))
        self.password_entry = tk.Entry(form_frame, font=("Arial", 12), width=30, show="*")
        self.password_entry.pack(pady=(0, 15))

        tk.Label(form_frame, text="Confirm Password:", font=("Arial", 12), bg="#f0f0f0").pack(anchor="w", pady=(0, 5))
        self.confirm_password_entry = tk.Entry(form_frame, font=("Arial", 12), width=30, show="*")
        self.confirm_password_entry.pack(pady=(0, 20))

        button_frame = tk.Frame(form_frame, bg="#f0f0f0")
        button_frame.pack(pady=10)
        
        register_btn = tk.Button(
            button_frame,
            text="Register",
            font=("Arial", 12, "bold"),
            bg="#4CAF50",
            fg="white",
            width=15,
            command=self.register_user
        )
        register_btn.pack(side="left", padx=5)
        
        clear_btn = tk.Button(
            button_frame,
            text="Clear",
            font=("Arial", 12),
            bg="#f44336",
            fg="white",
            width=15,
            command=self.clear_fields
        )
        clear_btn.pack(side="left", padx=5)

        show_users_btn = tk.Button(
            form_frame,
            text="Show All Users",
            font=("Arial", 12),
            bg="#2196F3",
            fg="white",
            width=32,
            command=self.show_users
        )
        show_users_btn.pack(pady=10)
    
    def hash_password(self, password):
        """Hash password using SHA-256"""
        return hashlib.sha256(password.encode()).hexdigest()
    
    def validate_input(self):
        """Validate user input"""
        username = self.username_entry.get().strip()
        email = self.email_entry.get().strip()
        password = self.password_entry.get()
        confirm_password = self.confirm_password_entry.get()
        
        if not username or not email or not password:
            messagebox.showerror("Error", "All fields are required!")
            return False
        
        if password != confirm_password:
            messagebox.showerror("Error", "Passwords do not match!")
            return False
        
        if len(password) < 6:
            messagebox.showerror("Error", "Password must be at least 6 characters long!")
            return False
        
        if "@" not in email:
            messagebox.showerror("Error", "Please enter a valid email address!")
            return False
        
        return True
    
    def register_user(self):
        """Register a new user"""
        if not self.validate_input():
            return
        
        if not self.db.connect():
            messagebox.showerror("Error", "Failed to connect to database!")
            return
        
        try:
            username = self.username_entry.get().strip()
            email = self.email_entry.get().strip()
            password = self.hash_password(self.password_entry.get())
            
            # Check if username or email already exists
            check_query = "SELECT username, email FROM users WHERE username = %s OR email = %s"
            self.db.cursor.execute(check_query, (username, email))
            existing_user = self.db.cursor.fetchone()
            
            if existing_user:
                messagebox.showerror("Error", "Username or email already exists!")
                return
            
            # Insert new user
            insert_query = """
                INSERT INTO users (username, email, password) 
                VALUES (%s, %s, %s)
            """
            self.db.cursor.execute(insert_query, (username, email, password))
            self.db.connection.commit()
            
            messagebox.showinfo("Success", f"User '{username}' registered successfully!")
            self.clear_fields()
            
        except Error as e:
            messagebox.showerror("Database Error", f"Error: {e}")
        
        finally:
            self.db.disconnect()
    
    def clear_fields(self):
        """Clear all input fields"""
        self.username_entry.delete(0, tk.END)
        self.email_entry.delete(0, tk.END)
        self.password_entry.delete(0, tk.END)
        self.confirm_password_entry.delete(0, tk.END)
    
    def show_users(self):
        """Display all registered users"""
        if not self.db.connect():
            messagebox.showerror("Error", "Failed to connect to database!")
            return
        
        try:
            users_window = tk.Toplevel(self.root)
            users_window.title("Registered Users")
            users_window.geometry("600x400")

            tree = ttk.Treeview(users_window, columns=("ID", "Username", "Email", "Created"), show="headings")

            tree.heading("ID", text="ID")
            tree.heading("Username", text="Username")
            tree.heading("Email", text="Email")
            tree.heading("Created", text="Created At")

            tree.column("ID", width=50)
            tree.column("Username", width=150)
            tree.column("Email", width=200)
            tree.column("Created", width=150)

            select_query = "SELECT id, username, email, created_at FROM users ORDER BY created_at DESC"
            self.db.cursor.execute(select_query)
            users = self.db.cursor.fetchall()
            
            for user in users:
                tree.insert("", "end", values=user)

            scrollbar = ttk.Scrollbar(users_window, orient="vertical", command=tree.yview)
            tree.configure(yscrollcommand=scrollbar.set)

            tree.pack(side="left", fill="both", expand=True)
            scrollbar.pack(side="right", fill="y")
            
        except Error as e:
            messagebox.showerror("Database Error", f"Error: {e}")
        
        finally:
            self.db.disconnect()
    
    def run(self):
        """Start the application"""
        self.root.mainloop()

if __name__ == "__main__":
    app = RegistrationForm()
    app.run()
