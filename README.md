





























//////////////////////
main.py
//////////////////////

from tkinter import *
from tkinter import messagebox 
import mysql.connector
from mysql.connector import Error  

from parcel_calculator import calculate_price
from parcel_saver import save_parcel_details

# Function to create the database and the parcel_item table if they don't exist
def create_database_and_table_if_not_exists():
    try:
        conn = mysql.connector.connect(host="localhost", user="root", password="")
        cursor = conn.cursor()
        cursor.execute("CREATE DATABASE IF NOT EXISTS dbparcel")
        cursor.execute("USE dbparcel")
        cursor.execute("""
            CREATE TABLE IF NOT EXISTS parcel_item (
                item_id INT AUTO_INCREMENT PRIMARY KEY,
                item_height VARCHAR(10),
                item_width VARCHAR(5),
                item_length VARCHAR(5),
                item_volume VARCHAR(5),
                item_price VARCHAR(10)
            )
        """)
        conn.commit()
        cursor.close()
        conn.close()
    except Error as e:
        print(f"Error: {e}")

# Function to calculate the parcel price and save data to the parcel_item table
def calculate_and_save():
    try:
        conn = mysql.connector.connect(host="localhost", user="root", password="", database="dbparcel")
        cursor = conn.cursor()
        length = float(entry_length.get())
        width = float(entry_width.get())
        height = float(entry_height.get())
        weight = float(entry_weight.get())

        # Calculate parcel price using imported function
        price = calculate_price(length, width, height, weight)

        # Save parcel details to database
        cursor.execute("""
            INSERT INTO parcel_item (item_height, item_width, item_length, item_volume, item_price)
            VALUES (%s, %s, %s, %s, %s)
        """, (height, width, length, length*width*height, price))
        conn.commit()

        cursor.close()
        conn.close()

        save_parcel_details(length, width, height, weight, price)
        label_price.config(text=f"The price of your parcel is: ${price}")

        
        messagebox.showinfo("Success", "Data added successfully!")
    except ValueError:
        messagebox.showerror("Error", "Please enter valid numerical values.")
    except Error as e:
        print(f"Error: {e}")
        messagebox.showerror("Error", "Failed to add data to the database.")

# Create the database and table if they don't exist
create_database_and_table_if_not_exists()

# Create Window
page = Tk()
page.title("Parcel Price Calculator")
page.geometry("400x400")
page.configure(bg='white')
page.resizable(False, False)

# Create input fields and labels for parcel dimensions and price
lblLength = Label(page, text="Enter parcel length (cm):", bg='white')
lblLength.place(x=20, y=20)

entry_length = Entry(page, width=20)
entry_length.place(x=20, y=50)

lblWidth = Label(page, text="Enter parcel width (cm):", bg='white')
lblWidth.place(x=20, y=80)

entry_width = Entry(page, width=20)
entry_width.place(x=20, y=110)

lblHeight = Label(page, text="Enter parcel height (cm):", bg='white')
lblHeight.place(x=20, y=140)

entry_height = Entry(page, width=20)
entry_height.place(x=20, y=170)

lblWeight = Label(page, text="Enter parcel weight (kg):", bg='white')
lblWeight.place(x=20, y=200)

entry_weight = Entry(page, width=20)
entry_weight.place(x=20, y=230)

# Button to calculate price and save details
btn_calculate = Button(page, text="Add to Database", command=calculate_and_save)
btn_calculate.place(x=20, y=270)

label_price = Label(page, text="", bg='white')
label_price.place(x=200, y=200)

# Run the Tkinter event loop
page.mainloop()
























///////////////////////////////////////////////
parcel_calculator.py
//////////////////////////////////////////////

def calculate_price(length, width, height, weight):
    volume = length * width * height
    if weight <= 1:
        if volume <= 5000:
            return 3
        elif volume <= 10000:
            return 5
        else:
            return 7
    elif weight <= 5:
        if volume <= 5000:
            return 5
        elif volume <= 10000:
            return 7
        else:
            return 9
    else:
        if volume <= 5000:
            return 7
        elif volume <= 10000:
            return 9
        else:
            return 11

















//////////////////////////////////
parcel_saver.py
///////////////////////////////////

import os

def save_parcel_details(length, width, height, weight, price, filename="parcel_details.txt"):
    current_directory = os.path.dirname(os.path.abspath(__file__))
    filepath = os.path.join(current_directory, filename)
    
    try:
        with open(filepath, 'a') as file:  # Open file in append mode
            file.write(f"Length: {length} cm\n")
            file.write(f"Width: {width} cm\n")
            file.write(f"Height: {height} cm\n")
            file.write(f"Weight: {weight} kg\n")
            file.write(f"Price: ${int(price)} \n \n")
        print(f"Parcel details appended to {filepath}")
    except IOError as e:
        print(f"An error occurred while saving the file: {e}")









a1650-c17dd
6b0d6-35952
e7720-e188d
23785-21c82
f8faf-f51b6
2127a-e1cd5
1a24d-6d126
40ada-aeae3
56686-5e4ef
45ea5-f11e6
31729-8ac90
a9d7a-82625
441ed-f5303
fbd7b-02a47
f665d-19557
e1815-41935







