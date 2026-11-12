# My-coding-project

import csv
import os
from datetime import datetime

FILE_NAME = "expenses.csv"


def create_file():
    if not os.path.exists(FILE_NAME):
        with open(FILE_NAME, "w", newline="") as file:
            writer = csv.writer(file)
            writer.writerow(["Date", "Category", "Description", "Amount"])


def add_expense():
    date = input("Enter date (YYYY-MM-DD): ")

    try:
        datetime.strptime(date, "%Y-%m-%d")
        category = input("Enter category: ").strip()
        description = input("Enter description: ").strip()
        amount = float(input("Enter amount: "))

        if not category or not description or amount <= 0:
            raise ValueError

        with open(FILE_NAME, "a", newline="") as file:
            writer = csv.writer(file)
            writer.writerow([date, category, description, amount])

        print("Expense successfully recorded!")

    except ValueError:
        print("Please enter valid details and a positive amount.")


def view_expenses():
    with open(FILE_NAME, "r", newline="") as file:
        reader = csv.reader(file)
        rows = list(reader)

    if len(rows) == 1:
        print("No expenses recorded yet.")
        return

    print("\nYOUR EXPENSE HISTORY")
    for row in rows:
        print(" | ".join(row))


def show_total():
    total = 0

    with open(FILE_NAME, "r", newline="") as file:
        reader = csv.DictReader(file)

        for row in reader:
            total += float(row["Amount"])

    print(f"\nTotal spending: £{total:.2f}")


def main():
    create_file()

    while True:
        print("\nPERSONAL EXPENSE TRACKER")
        print("1. Add expense")
        print("2. View expenses")
        print("3. View total spending")
        print("4. Exit")

        choice = input("Select an option: ")

        if choice == "1":
            add_expense()
        elif choice == "2":
            view_expenses()
        elif choice == "3":
            show_total()
        elif choice == "4":
            print("Thank you for using the tracker!")
            break
        else:
            print("Invalid choice. Please try again.")


if __name__ == "__main__":
    main()
