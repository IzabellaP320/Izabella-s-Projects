#!/usr/bin/env python
# coding: utf-8

# In[1]:


import csv
import random
import getpass
import os 
import sys

base_dir = '/Users/izzy/Downloads/python_assessment'
User_File = os.path.join(base_dir, 'users.csv')


class UserAuth:
    def register_user(self, username, password):
        # Store username and password
        with open(User_File, 'a', newline='') as file:
            writer = csv.writer(file)
            writer.writerow([username, password])  # Store plain password
        
        print(f"User {username} registered successfully.")

    def login_user(self, username, password):
        with open(User_File, 'r') as file:
            reader = csv.reader(file)
            for row in reader:
                if row[0] == username and row[1] == password:
                    print(f"User {username} logged in successfully.")
                    return True  # Login successful
        print("Login failed. Invalid credentials.")
        return False  # Login failed

user_info = UserAuth()

class Stock:
    def __init__(self, name, price, quantity):
        self.name = name
        self.price = price
        self.quantity = quantity

    def stock_info(self):
        return "Name :" +str(self.name)+ " Stock Price : " +str(self.price)+ " Stock Quantity : "+ str(self.quantity)


class BuyStock(Stock):
    def __init__(self, order_id, name, price, quantity):
        super().__init__(name, price, quantity)
        self.order_id = round(random.randint(0, 500000), 0)
        self.action = ' buy '
        self.status = ' pending '
    
    def buy_info(self):
        return " Order ID: " +str(self.order_id)+" Status: " + self.status + " Action: " +str(self.action) + super().stock_info() 
    
class SellStock(Stock):
    def __init__(self, order_id, name, price, quantity):
        super().__init__(name, price, quantity)
        self.order_id = round(random.randint(0, 500000), 0)
        self.action = ' sell '
        self.status = ' pending '
        
    def sell_info(self):
        return " Order ID: " +str(self.order_id)+ " Status: " + self.status + " Action: " +str(self.action) + super().stock_info()

class OrderManager:
    def add_order(self, order_type, stock_name, price, quantity):
        if order_type == 'buy':
            order_id = round(random.randint(0, 500000), 0) # Unique ID for the new order
            new_order = BuyStock(order_id, stock_name, price, quantity)
            print(f"Added Buy Order: {new_order.stock_info()}")
            self.save_buy_order(new_order)  # Save to CSV
        elif order_type == 'sell':
            order_id = round(random.randint(0, 500000), 0)  # Unique ID for the new order
            new_order = SellStock(order_id, stock_name, price, quantity)
            print(f"Added Sell Order: {new_order.sell_info()}")
            self.save_sell_order(new_order)  # Save to CSV
    
    def save_buy_order(self, order):
    
        with open('buy_orders.csv', 'a', newline='') as csvfile:  # Append mode
            writer = csv.writer(csvfile)
            writer.writerow([order.order_id, order.status, order.action.strip(), order.name, order.price, order.quantity])  # Append new order
            
        with open('trade_history.csv', 'a', newline='') as csvfile:  # Append mode
            writer = csv.writer(csvfile)
            writer.writerow([order.order_id, order.status, order.action.strip(), order.name, order.price, order.quantity])  # Append new order
    
    def save_sell_order(self, order):
        with open('sell_orders.csv', 'a', newline='') as csvfile:  # Append mode
            writer = csv.writer(csvfile)
            writer.writerow([order.order_id, order.status, order.action.strip(), order.name, order.price, order.quantity])  # Append new order
        
        with open('trade_history.csv', 'a', newline='') as csvfile:  # Append mode
            writer = csv.writer(csvfile)
            writer.writerow([order.order_id, order.status, order.action.strip(), order.name, order.price, order.quantity])  # Append new order
    
    def cancel_order(self, order_id, order_type, stock_name, price, quantity):
        if order_type == 'buy':
            self.cancel_buy_order(order_id, stock_name, price, quantity)
        elif order_type == 'sell':
            self.cancel_sell_order(order_id, stock_name, price, quantity)
        else:
            print("Error: Please select either 'buy' or 'sell'.")

    def cancel_buy_order(self, order_id, stock_name, price, quantity):
        buy_cancel_file = []
        order_found = False

        with open('buy_orders.csv', 'r') as file:
            reader = csv.reader(file)
            header = next(reader)  # Read the header
            buy_cancel_file.append(header)  # Keep the header for rewriting later

            for row in reader:
                if row[0] == str(order_id) and row[3] == stock_name and row[4] == str(price) and row[5] == str(quantity):
                    row[1] = 'Cancelled'
                    print(f"Buy Order {order_id} for stock {stock_name} has been cancelled.")
                    order_found = True  # Mark that we found and updated the order

                buy_cancel_file.append(row)  # Add the updated row (or original if not updated)

        if order_found:
            # Write back to the file
            with open('buy_orders.csv', 'w', newline='') as csvfile:  # Overwrite mode
                writer = csv.writer(csvfile)
                writer.writerows(buy_cancel_file)  # Write all rows back to the file

                # Write to trade history as well
            with open('trade_history.csv', 'a', newline='') as csvfile:  # Append mode
                writer = csv.writer(csvfile)
                writer.writerow([order_id, 'Cancelled', 'buy', stock_name])  # Append new order
        else: 
            print("Error: 'buy_orders.csv' order in file not found.")

    def cancel_sell_order(self, order_id, stock_name, price, quantity):
        sell_cancel_file = []
        order_found = False
        
        with open('sell_orders.csv', 'r') as file:
            reader = csv.reader(file)
            header = next(reader)  # Read the header
            sell_cancel_file.append(header)  # Keep the header for rewriting later

            for row in reader:
                if row[0] == str(order_id) and row[3] == stock_name and row[4] == str(price) and row[5] == str(quantity):
                    row[1] = 'Cancelled'
                    print(f"Sell Order {order_id} for stock {stock_name} has been cancelled.")
                    order_found = True  # Mark that we found and updated the order

                sell_cancel_file.append(row)  # Add the updated row (or original if not updated)

        if order_found:
        # Write back to the file
            with open('sell_orders.csv', 'w', newline='') as csvfile:  # Overwrite mode
                writer = csv.writer(csvfile)
                writer.writerows(sell_cancel_file)  # Write all rows back to the file

                # Write to trade history as well
            with open('trade_history.csv', 'a', newline='') as csvfile:  # Append mode
                writer = csv.writer(csvfile)
                writer.writerow([order_id, 'Cancelled', 'sell', stock_name])  # Append new order
        else:
            print("Error: 'sell_orders.csv' order in file not found.")

order_manager = OrderManager()   

class ReplaceOrder:
    def replace_order(self, order_id, stock_name, order_type, new_price, new_quantity):
        if order_type == 'buy':
            self._replace_buy_order(order_id, stock_name, new_price, new_quantity)
        elif order_type == 'sell':
            self._replace_sell_order(order_id, stock_name, new_price, new_quantity)
        else:
            print("Error, please select either buy or sell.")

    def _replace_buy_order(self, order_id, stock_name, new_price, new_quantity):
        buy_file = []
        order_found = False
        
        with open('buy_orders.csv', 'r') as file:
            reader = csv.reader(file)
            header = next(reader)  # Read the header
            buy_file.append(header)  # Keep the header for rewriting later

            for row in reader:
                if row[0] == str(order_id) and row[3] == stock_name:
                    row[4] = str(new_price)  # Update the price
                    row[5] = str(new_quantity)  # Update the quantity 
                    row[1] = 'Pending (Replaced Order)'
                    print(f"Buy Order {order_id} for stock {stock_name} has been replaced with new price: {new_price}, quantity: {new_quantity}, and status: {row[2]}.")
                    order_found = True  # Mark that we found and updated the order
                    
                buy_file.append(row)  # Add the updated row (or original if not updated)
        
        if order_found:
            # Write back to the file
            with open('buy_orders.csv', 'w', newline='') as csvfile:  # Overwrite mode
                writer = csv.writer(csvfile)
                writer.writerows(buy_file)  # Write all rows back to the file

            # Write to trade history as well
            with open('trade_history.csv', 'a', newline='') as csvfile:  # This adds a new row so all the hsitory of users is stored
                writer = csv.writer(csvfile)
                writer.writerow([order_id, 'Pending (Replaced Order)', 'buy', stock_name, new_price, new_quantity])  # Append new order
        
        else:
            print(f"Buy Order {order_id} not found for stock {stock_name}.")

    def _replace_sell_order(self, order_id, stock_name, new_price, new_quantity):
        sell_file = []
        order_found = False
        
        with open('sell_orders.csv', 'r') as file:
            reader = csv.reader(file)
            header = next(reader)  # Read the header
            sell_file.append(header)  # Keep the header for rewriting later

            for row in reader:
                if row[0] == str(order_id) and row[3] == stock_name:
                    row[4] = str(new_price)  # Update the price
                    row[5] = str(new_quantity)  # Update the quantity 
                    row[1] = 'Pending (Replaced Order)'
                    print(f"Sell Order {order_id} for stock {stock_name} has been replaced with new price: {new_price}, quantity: {new_quantity}, and status: {row[2]}.")
                    order_found = True  # Mark that we found and updated the order
                    
                sell_file.append(row)  # Add the updated row (or original if not updated)
        
        if order_found:
            # Write back to the file
            with open('sell_orders.csv', 'w', newline='') as csvfile:  # Overwrite mode
                writer = csv.writer(csvfile)
                writer.writerows(sell_file)  # Write all rows back to the file

            # Write to trade history as well
            with open('trade_history.csv', 'a', newline='') as csvfile:  # Append mode
                writer = csv.writer(csvfile)
                writer.writerow([order_id, 'Pending (Replaced Order)', 'sell', stock_name, new_price, new_quantity])  # Append new order
        
        else:
            print(f"Sell Order {order_id} not found for stock {stock_name}.")
            

replace_order = ReplaceOrder() 

class MatchOrders:
    def match_order(self):
        buy_list = []
        sell_list = []

        with open('buy_orders.csv', 'r') as file:
            reader = csv.reader(file)
            header = next(reader)
            for row in reader:
                if row[1].strip() == 'pending' or row[1].strip() == 'Pending (Replaced Order)':
                    buy_list.append(row)  # Collect all pending buy orders

        # Read pending sell orders
        with open('sell_orders.csv', 'r') as file:
            reader = csv.reader(file)
            header = next(reader)
            for row in reader:
                if row[1].strip() == 'pending' or row[1].strip() == 'Pending (Replaced Order)':
                    sell_list.append(row)  # Collect all pending sell orders
        
        for buylist in buy_list:
            for selllist in sell_list:
                if buylist[3] == selllist[3] and float(buylist[4]) >= float(selllist[4]):
                    min_quantity = min(float(buylist[5]),float(selllist[5]))
                    
                    buylist[5] = str(float(buylist[5]) - min_quantity)
                    selllist[5] = str(float(selllist[5]) - min_quantity)

                    # Update statuses
                    if float(buylist[5]) == 0:
                        buylist[1] = "Fully filled order"
                    if float(selllist[5]) == 0:
                        selllist[1] = "Fully filled order"
                        
                    with open('buy_orders.csv', 'w', newline='') as csvfile:  # Overwrite mode
                        writer = csv.writer(csvfile)
                        writer.writerows(buy_list)  # Write all rows back to the file
        
                    with open('sell_orders.csv', 'w', newline='') as csvfile:  # Overwrite mode
                       writer = csv.writer(csvfile)
                       writer.writerows(sell_list)
                        
                    #Write to trade history as well
                    with open('trade_history.csv', 'a', newline='') as csvfile:
                        writer = csv.writer(csvfile)
                        writer.writerow([buylist[0], buylist[1], buylist[2], buylist[3], buylist[4], buylist[5], selllist[0], selllist[1], selllist[2], selllist[3], selllist[4], selllist[5]])

matcher = MatchOrders()

#Predefined list of stocks
stock_names = ['AAPL', 'GOOG', 'AMZN', 'MSFT', 'TSLA', 'NFLX', 'FB', 'NVDA', 'BABA', 'DIS', 'JPM', 'V', 'PG', 'BMW', 'MERC', 'PEP', 'COST', 'STBX', 'HD', 'JNJ', 'UNI', 'KFC', 'ESYJ', 'BA', 'INTC', 'CSCO', 'ADBE', 'PYPL', 'MCD', 'ORCL']

stock_list = []

for i in range(30):
    name = stock_names[i]
    price = round(random.uniform(0, 500), 2)  # Random price between 0 and 500
    quantity = round(random.uniform(0, 50), 0)  # Random quantity between 0 and 50
    stock_list.append(Stock(name, price, quantity))  # Append stock to the list

# Display Stock Info
#print("Stock List:")
#for stock in stock_list:
    #print(stock.stock_info())    

# Display Buy and Sell Info
#print("Buy List:")
#for buy_stock in buy_list:
    #print(buy_stock.stock_info())

#print("Sell List:")
#for sell_stock in sell_list:
    #print(sell_stock.sell_info())
    

file1 = os.path.join(base_dir, 'buy_orders.csv')
file2 = os.path.join(base_dir, 'sell_orders.csv')
merged_file = os.path.join(base_dir, 'trade_history.csv')
buy_list = []
sell_list = []

# Check if the CSV file buy_already exists
if os.path.exists(file1):
    print("CSV file already exists. Skipping data input...")
else:
    print("Generating new stock data and writing to CSV...")

    for stock in stock_list:
        for i in range(10):  # Generate 10 buy and 10 sell orders for each stock
            buy_price = round(random.uniform(0, 500), 2)
            buy_quantity = round(random.uniform(0, 100), 0)
            buy_list.append(BuyStock(i+1, stock.name, buy_price, buy_quantity))  # Add buy order with unique ID
    with open('buy_orders.csv', 'w', newline='') as csvfile:
        write = csv.writer(csvfile)
        write.writerow(['Order ID','Status', 'Action', 'Stock Name', 'Buy Price', 'Buy Quantity'])  # Wrote a header for each column
        for buy_stock in buy_list:
            write.writerow([buy_stock.order_id,buy_stock.status, buy_stock.action, buy_stock.name, buy_stock.price, buy_stock.quantity])
    
# Store sell_list in a CSV file
if os.path.exists(file2):
    print("CSV file already exists. Skipping data input...")
else:
    print("Generating new stock data and writing to CSV...")
    for stock in stock_list:
        for i in range(10):
            sell_price = round(random.uniform(0, 500), 2)
            sell_quantity = round(random.uniform(0, 100), 0)
            sell_list.append(SellStock(i+1, stock.name, sell_price, sell_quantity))  # Add sell order with unique ID
    with open('sell_orders.csv', 'w', newline='') as csvfile:
        write = csv.writer(csvfile)
        write.writerow(['Order ID', 'Status', 'Action', 'Stock Name', 'Sell Price', 'Sell Quantity'])
        for sell_stock in sell_list:
            write.writerow([sell_stock.order_id, sell_stock.status, sell_stock.action, sell_stock.name, sell_stock.price, sell_stock.quantity])

#creating a trade_history file
if os.path.exists(merged_file):
    print("CSV file already exists. Skipping data input...")
else:
    print("Generating new stock data and writing to CSV...")
            
    with open(file1, 'r') as f1:
        reader1 = csv.reader(f1)
        header1 = next(reader1)
        data1 = list(reader1)

    # Read data from second file
    with open(file2, 'r') as f2:
        reader2 = csv.reader(f2)
        header2 = next(reader2)
        data2 = list(reader2)
    # List of CSV file names to merge
    file_names = ['buy_orders.csv', 'sell_orders.csv']

    # Output file name
    trade_history = 'trade_history.csv'

    with open(trade_history, 'w', newline='') as f_out:
        writer = csv.writer(f_out)
        writer.writerow(['Order ID', 'Status', 'Action', 'Stock Name', 'Buy Price', 'Buy Quantity'] + ['Order ID', 'Status', 'Action', 'Stock Name', 'Buy Price', 'Buy Quantity'])  # Merge headers
        for row1, row2 in zip(data1, data2):
            writer.writerow(row1 + row2)  # Merge rows
            
print("Buy and Sell lists have been written to trade_history.csv.")

def menu():
    print("Izzy's Electronic Trading platform:")
    print("1) Register ")
    print("2) Login")
    print("3) View buy and sell orders for all stocks")
    print("4) Add a new order (buy/sell)")
    print("5) Cancel an order")
    print("6) Replace an order")
    print("7) View the status of all orders/ View your trade history")
    print("8) Logout")

logged_in = False

while True:
    menu()
    option = int(input("Please enter your choice from the menu: "))
    
    if option == 8:
        print("Logged out. Thank you for using Izzy's Electronic Trading platform.")
        logged_in = False
        break

    elif option == 1:
        print(" Please create a username and password ")
        
        username = input(" Please create a username : ")
        
        password = getpass.getpass("Please create a password: ")
        
        user_info.register_user(username, password)
        
        print(f"User {username} registered successfully.")
    
        print("( Username and password created please login. )")
        
    elif option == 2:
        print(" Please enter your username and password ")
        username = input("Please enter your username: ")
        password = getpass.getpass("Please enter your password: ")
        
        success = user_info.login_user(username, password)  # Only call it once
    
        if success:
            logged_in = True  # Set login state to True
            print(f"Welcome back, {username}!")
        else:
            print("Login attempt failed.")
            
    elif logged_in:
        if option == 3:
            print("Choose to view either buy or sell orders for all stocks.")
            
            buy_or_sell =  input(" Input choice : ").lower()
            
            if buy_or_sell == 'buy':
                with open('buy_orders.csv', 'r') as csvfile:
                    csv_reader = csv.reader(csvfile)
                    for line in csv_reader:
                        print(line)
        
            
            elif buy_or_sell == 'sell':
                with open('sell_orders.csv', 'r') as csvfile:
                    csv_reader = csv.reader(csvfile)
                    for line in csv_reader:
                        print(line)
            else:
                print("Invalid choice. Please select buy or sell.")
                
        elif option == 4:
            manage_order = input(" Please enter if you would like to place a buy or sell order : ").lower()
            if manage_order == 'buy':
                stock_name = input("Stock name: ").upper()
                price = float(input("Price: "))
                quantity = int(input("Quantity: "))
                status = 'pending'
                order_manager.add_order(manage_order, stock_name, price, quantity)
                print(" Order added ")
            
            if manage_order == 'sell':
                stock_name = input("Stock name: ").upper()
                while stock_name not in stock_list:
                    print("Invalid stock name. Please enter a valid stock name from the list:")
                    break
                price = float(input("Price: "))
                quantity = int(input("Quantity: "))
                status = 'pending'
                order_manager.add_order(manage_order, stock_name, price, quantity)
                print(" Order added ")
            
            else:
                print("Invalid choice. Please select buy or sell.")
            
        elif option == 5:
            print("Please enter your order details to cancel an order")
            order_id = int(input("Order ID to cancel: "))
            stock_name = input("Stock name to cancel: ").upper()
            order_type = input("Order type (buy/sell): ").lower()
            price = float(input("Order price: "))
            quantity = int(input("Order quantity: "))
            status = 'cancelled'
            order_manager.cancel_order(order_id, order_type, stock_name, price, quantity)
            print("Order successfully cancelled.")
            
        elif option == 6:
            print("Please enter your order details to replace an order")
            order_id = int(input("Order ID to replace: "))
            stock_name = input("Stock name to replace: ").upper()
            order_type = input("Order type (buy or sell): ").lower()
            new_price = float(input("New price: "))
            new_quantity = int(input("New quantity: "))
            status = "pending (replaced order)"
            replace_order.replace_order(order_id, stock_name, order_type, new_price, new_quantity)   
            
        elif option == 7:
            print("Viewing your trade history/ Viewing the status of all orders...")
            matcher.match_order()
            with open('trade_history.csv', 'r') as csvfile:
                    csv_reader = csv.reader(csvfile)
                    for line in csv_reader:
                        print(line)
        
    else:
        print(" Please log in or register to access options. ")






