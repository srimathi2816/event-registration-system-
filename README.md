# event-registration-system-
To store the database and check the user validity 
 import mysql.connector

db = mysql.connector.connect(
    host="localhost",
    user="root",
    password="your_password",
    database="event_registration"
)

cursor = db.cursor()

def add_registration():
    name = input("Enter Name: ").strip()
    email = input("Enter Email: ").strip()
    event = input("Enter Event: ").strip()

    if not name or not email or not event:
        print("All fields are required.")
        return

    query = """
    INSERT INTO registrations (name, email, event)
    VALUES (%s, %s, %s)
    """

    cursor.execute(query, (name, email, event))
    db.commit()
    print("Registration added successfully.")

def view_registrations():
    cursor.execute("SELECT * FROM registrations")
    records = cursor.fetchall()

    for record in records:
        print(record)

def update_registration():
    registration_id = input("Enter Registration ID: ")
    event = input("Enter New Event: ").strip()

    query = """
    UPDATE registrations
    SET event = %s
    WHERE id = %s
    """

    cursor.execute(query, (event, registration_id))
    db.commit()
    print("Registration updated successfully.")

def delete_registration():
    registration_id = input("Enter Registration ID: ")

    cursor.execute(
        "DELETE FROM registrations WHERE id = %s",
        (registration_id,)
    )

    db.commit()
    print("Registration deleted successfully.")

while True:
    print("\nEVENT REGISTRATION SYSTEM")
    print("1. Add Registration")
    print("2. View Registrations")
    print("3. Update Registration")
    print("4. Delete Registration")
    print("5. Exit")

    choice = input("Enter your choice: ")

    if choice == "1":
        add_registration()
    elif choice == "2":
        view_registrations()
    elif choice == "3":
        update_registration()
    elif choice == "4":
        delete_registration()
    elif choice == "5":
        break
    else:
        print("Invalid choice.")

cursor.close()
db.close()CREATE DATABASE event_registration;

USE event_registration;

CREATE TABLE registrations (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
    event VARCHAR(100) NOT NULL
);