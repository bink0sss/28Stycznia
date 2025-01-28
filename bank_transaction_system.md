# Bank Transaction History System

### Description:
Write a system to handle a bank transaction history.

### Requirements:
- **Class `Transaction`** should contain the following fields:
  - `amount`: The amount of the transaction.
  - `type`: The type of transaction (debit or credit).
  - `date`: The date of the transaction.

- **Class `BankAccount`** should allow:
  - Adding transactions.
  - Displaying transaction history.
  - Calculating the current balance.
  - Logging each transaction.

### Example of Features:
- Add and display debit/credit transactions.
- Track and calculate the current balance based on transactions.

### Sample Code:

```python
import logging
from datetime import datetime

class Transaction:
    def __init__(self, amount, type_of_transaction):
        self.amount = amount
        self.type = type_of_transaction
        self.date = datetime.now()

    def __str__(self):
        return f"{self.date} - {self.type.capitalize()}: ${self.amount}"

class BankAccount:
    def __init__(self):
        self.balance = 0
        self.transactions = []
        self.logger = logging.getLogger(__name__)
        self.logger.setLevel(logging.INFO)
        
        handler = logging.FileHandler('bank_transactions.log')
        handler.setLevel(logging.INFO)
        formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
        handler.setFormatter(formatter)
        self.logger.addHandler(handler)

    def add_transaction(self, transaction):
        self.transactions.append(transaction)
        if transaction.type == "credit":
            self.balance += transaction.amount
        elif transaction.type == "debit":
            self.balance -= transaction.amount
        self.logger.info(f"Transaction added: {transaction}")

    def display_transactions(self):
        for transaction in self.transactions:
            print(transaction)

    def current_balance(self):
        print(f"Current balance: ${self.balance}")

# Menu for interacting with the user
def menu():
    print("Bank Transaction History")
    print("1. Add Transaction")
    print("2. Display Transactions")
    print("3. Display Current Balance")
    print("4. Exit")
    
    choice = input("Enter your choice: ")
    
    bank_account = BankAccount()
    
    if choice == '1':
        amount = float(input("Enter transaction amount: "))
        transaction_type = input("Enter transaction type (credit/debit): ")
        transaction = Transaction(amount, transaction_type)
        bank_account.add_transaction(transaction)
    elif choice == '2':
        bank_account.display_transactions()
    elif choice == '3':
        bank_account.current_balance()
    elif choice == '4':
        print("Exiting the program.")
        return
    else:
        print("Invalid choice, try again.")
        menu()

if __name__ == "__main__":
    menu()
