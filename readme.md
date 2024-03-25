# 1 program
```yard
# 1. Read your name and print Hello message with name
# Read your name from the user
name = input("Enter your name: ")
# Print a Hello message with your name
print("Hello, " + name + "!")

# 2. Read two numbers and print their sum, difference, product and division.
# Read two numbers from the user
num1 = float(input("Enter the first number: "))
num2 = float(input("Enter the second number: "))
# Perform arithmetic operations
sum_result = num1 + num2
difference_result = num1 - num2
product_result = num1 * num2
try:
    division_result = num1 / num2
except ZeroDivisionError:
    division_result = "Cannot divide by zero"
# Print the results
print("Sum:", sum_result)
print("Difference:", difference_result)
print("Product:", product_result)
print("Division:", division_result)

# 3. Read two numbers and print their sum, difference, product and division.
# Read two numbers from the user
num1 = float(input("Enter the first number: "))
num2 = float(input("Enter the second number: "))
# Perform arithmetic operations and print the results
print("Sum:", num1 + num2)
print("Difference:", num1 - num2)
print("Product:", num1 * num2)
# Handle division by zero
if num2 != 0:
    print("Division:", num1 / num2)
else:
    print("Cannot divide by zero")

# 4. Word and character count of a given string
# Read a string from the user
input_string = input("Enter a string: ")
# Calculate word count
word_count = len(input_string.split())
# Calculate character count
char_count = len(input_string)
# Print the results
print("Word Count:", word_count)
print("Character Count:", char_count)

# 5. Area of a given shape (rectangle, triangle and circle) reading shape and appropriate values from standard input
import math
# Read the shape from the user
shape = input("Enter the shape (rectangle, triangle, or circle): ")
# Calculate area based on the shape
if shape == "rectangle":
    length = float(input("Enter the length: "))
    width = float(input("Enter the width: "))
    area = length * width
    print("Area of the rectangle:", area)
elif shape == "triangle":
    base = float(input("Enter the base: "))
    height = float(input("Enter the height: "))
    area = 0.5 * base * height
    print("Area of the triangle:", area)
elif shape == "circle":
    radius = float(input("Enter the radius: "))
    area = math.pi * radius ** 2
    print("Area of the circle:", area)
else:
    print("Invalid shape entered.")

# 6. Print a name „n‟ times, where name and n are read from standard input, using for and while loops.
# Read the name and n from the user
name = input("Enter a name: ")
n = int(input("Enter the number of times to print the name: "))
# Using a for loop
print("Using for loop:")
for _ in range(n):
    print(name)
# Using a while loop
print("\nUsing while loop:")
count = 0
while count < n:
    print(name)
    count += 1

# 7. Handle Divided by Zero Exception.
# Read two numbers from the user
numerator = float(input("Enter the numerator: "))
denominator = float(input("Enter the denominator: "))
# Handle division by zero
try:
    result = numerator / denominator
    print("Result:", result)
except ZeroDivisionError as e:
    print(f"Error: {e}. Cannot divide by zero.")
except Exception as e:
    print(f"An unexpected error occurred: {e}")

# 8. Print current time for 10 times with an interval of 10 seconds.
import time
from datetime import datetime
# Print the current time 10 times with a 10-second interval
for _ in range(10):
    current_time = datetime.now().strftime("%H:%M:%S")
    print("Current Time:", current_time)
    time.sleep(10)

```
