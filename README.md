# Bank-app

import streamlit as st

# Define the SavingsAccount class
class SavingsAccount:
    def __init__(self, username, balance):
        self.username = username
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount
        return f"Deposited ${amount} successfully."

    def withdraw(self, amount):
        if amount > self.balance:
            return "Insufficient funds."
        self.balance -= amount
        return f"Withdrew ${amount} successfully."

# Define the CurrentAccount class (inherits SavingsAccount behavior here)
class CurrentAccount(SavingsAccount):
    pass  # You can add CurrentAccount specific features here if needed

# Streamlit app
st.set_page_config(page_title="Bank App", page_icon="🏦")
st.title("🏦 WELCOME")

account_type = st.sidebar.selectbox("Choose Account Type", ["Savings", "Current"])
username = st.sidebar.text_input("Enter your name")
balance = st.sidebar.number_input("Initial Balance", min_value=0, step=50)

if 'account' not in st.session_state:
    st.session_state.account = None

if st.sidebar.button("Create Account"):
    if not username:
        st.sidebar.error("Please enter your name to create an account.")
    else:
        if account_type == "Savings":
            st.session_state.account = SavingsAccount(username, balance)
        else:
            st.session_state.account = CurrentAccount(username, balance)
        st.sidebar.success(f"{account_type} Account created for {username} with balance ${balance}")

if st.session_state.account:
    st.write(f"Account Holder: **{st.session_state.account.username}**")
    st.write(f"Current Balance: **${st.session_state.account.balance}**")

    action = st.selectbox("Choose Action", ["Deposit", "Withdraw"])
    amount = st.number_input("Amount", min_value=1, step=10)

    if st.button("Submit Transaction"):
        if action == "Deposit":
            message = st.session_state.account.deposit(amount)
        else:
            message = st.session_state.account.withdraw(amount)
        st.info(message)
        st.write(f"Updated Balance: **${st.session_state.account.balance}**")
else:
    st.write("Please create an account first.")


    class CurrentAccount:
    def _init_(self, owner, balance=0, overdraft_limit=500):
        self.owner = owner
        self.balance = balance
        self.overdraft_limit = overdraft_limit

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            return f"Deposited ${amount}. New balance: ${self.balance}"
        return "Invalid deposit amount."

    def withdraw(self, amount):
        if 0 < amount <= self.balance + self.overdraft_limit:
            self.balance -= amount
            return f"Withdrew ${amount}. New balance: ${self.balance}"
        return "Overdraft limit exceeded or invalid amount."


        class SavingsAccount:
    def _init_(self, owner, balance=0):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            return f"Deposited ${amount}. New balance: ${self.balance}"
        return "Invalid deposit amount."

    def withdraw(self, amount):
        if 0 < amount <= self.balance:
            self.balance -= amount
            return f"Withdrew ${amount}. New balance: ${self.balance}"
        return "Insufficient balance or invalid amount."
