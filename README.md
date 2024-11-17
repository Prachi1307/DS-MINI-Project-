# DS-MINI-Project-
Mini Project for Internal II 

HR Analytics to track employee Performance  

 

1. Define problem statement. 

The goal is to use HR data to track employee performance, identify key factors affecting productivity, and use data-driven insights to improve workforce management. The analysis will focus on understanding relationships between variables like training hours, job satisfaction, and performance scores. 

2. Select suitable dataset. 

import os 
for dirname, _, filenames in os.walk('/kaggle/input'): 
    for filename in filenames: 
        print(os.path.join(dirname, filename)) 

3. Implement Project using Python 

Code-  

import mysql.connector 
 
# Database connection 
con = mysql.connector.connect( 
    host="localhost", 
    user="root", 
    password="password", 
    database="emp" 
) 
 
cursor = con.cursor() 
 
# Function to check if an employee exists 
def check_employee(employee_id): 
    sql = 'SELECT * FROM employees WHERE id=%s' 
    cursor.execute(sql, (employee_id,)) 
    return cursor.rowcount == 1 
 
# Function to add an employee 
def add_employee(): 
    Id = input("Enter Employee Id: ") 
    if check_employee(Id): 
        print("Employee already exists. Please try again.") 
        return 
     
    Name = input("Enter Employee Name: ") 
    Post = input("Enter Employee Post: ") 
    Salary = input("Enter Employee Salary: ") 
 
    sql = 'INSERT INTO employees (id, name, position, salary) VALUES (%s, %s, %s, %s)' 
    data = (Id, Name, Post, Salary) 
    try: 
        cursor.execute(sql, data) 
        con.commit() 
        print("Employee Added Successfully") 
    except mysql.connector.Error as err: 
        print(f"Error: {err}") 
        con.rollback() 
 
# Function to remove an employee 
def remove_employee(): 
    Id = input("Enter Employee Id: ") 
    if not check_employee(Id): 
        print("Employee does not exist. Please try again.") 
        return 
     
    sql = 'DELETE FROM employees WHERE id=%s' 
    data = (Id,) 
    try: 
        cursor.execute(sql, data) 
        con.commit() 
        print("Employee Removed Successfully") 
    except mysql.connector.Error as err: 
        print(f"Error: {err}") 
        con.rollback() 
 
# Function to promote an employee 
def promote_employee(): 
    Id = input("Enter Employee's Id: ") 
    if not check_employee(Id): 
        print("Employee does not exist. Please try again.") 
        return 
     
    try: 
        Amount = float(input("Enter increase in Salary: ")) 
 
        sql_select = 'SELECT salary FROM employees WHERE id=%s' 
        cursor.execute(sql_select, (Id,)) 
        current_salary = cursor.fetchone()[0] 
        new_salary = current_salary + Amount 
 
        sql_update = 'UPDATE employees SET salary=%s WHERE id=%s' 
        cursor.execute(sql_update, (new_salary, Id)) 
        con.commit() 
        print("Employee Promoted Successfully") 
 
    except (ValueError, mysql.connector.Error) as e: 
        print(f"Error: {e}") 
        con.rollback() 
 
# Function to display all employees 
def display_employees(): 
    try: 
        sql = 'SELECT * FROM employees' 
        cursor.execute(sql) 
        employees = cursor.fetchall() 
        for employee in employees: 
            print("Employee Id : ", employee[0]) 
            print("Employee Name : ", employee[1]) 
            print("Employee Post : ", employee[2]) 
            print("Employee Salary : ", employee[3]) 
            print("------------------------------------") 
 
    except mysql.connector.Error as err: 
        print(f"Error: {err}") 
 
# Function to display the menu 
def menu(): 
    while True: 
        print("\nWelcome to Employee Management Record") 
        print("Press:") 
        print("1 to Add Employee") 
        print("2 to Remove Employee") 
        print("3 to Promote Employee") 
        print("4 to Display Employees") 
        print("5 to Exit") 
         
        ch = input("Enter your Choice: ") 
 
        if ch == '1': 
            add_employee() 
        elif ch == '2': 
            remove_employee() 
        elif ch == '3': 
            promote_employee() 
        elif ch == '4': 
            display_employees() 
        elif ch == '5': 
            print("Exiting the program. Goodbye!") 
            break 
        else: 
            print("Invalid Choice! Please try again.") 
 
if __name__ == "__main__": 
    menu() 

Output-  

 

4. Visualize data with box plot, Histogram, Scatter Plot. 

1. Plot Histograms for Numerical Variables 

numerical_columns = ['Age', 'No_of_Trainings', 'Previous_Year_Rating', 'Length_of_Service', 'Avg_Training_Score'] 
 
# Plot histograms for numerical variables 
for column in numerical_columns: 
    plt.figure(figsize=(8, 6)) 
    sns.histplot(hr[column], kde=True) 
    plt.title(f'Histogram of {column}') 
    plt.xlabel(column) 
    plt.ylabel('Frequency') 
    plt.show() 

 

 

 

 

 

2. Plot boxplots for numerical variables 

for column in numerical_columns: 
    plt.figure(figsize=(8, 6)) 
    sns.boxplot(x=hr[column]) 
    plt.title(f'Boxplot of {column}') 
    plt.xlabel(column) 
    plt.show() 

 

 

