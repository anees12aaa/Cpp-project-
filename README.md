#include <iostream>
#include <string>
#include <map>

using namespace std;

{
    string accountNumber;
    string accountHolderName;
    double balance;
};

map<string, Account> accounts;


void createAccount() {
    Account newAccount;
    cout << "Enter account number: ";
    cin >> newAccount.accountNumber;
    cout << "Enter account holder name: ";
    cin.ignore();
    getline(cin, newAccount.accountHolderName);
    newAccount.balance = 0.0;
    accounts[newAccount.accountNumber] = newAccount;
    cout << "Account created successfully!" << endl;
}

// Function to deposit money
void deposit() {
    string accountNumber;
    double amount;
    cout << "Enter account number: ";
    cin >> accountNumber;
    if (accounts.find(accountNumber) != accounts.end()) {
        cout << "Enter amount to deposit: ";
        cin >> amount;
        accounts[accountNumber].balance += amount;
        cout << "Deposit successful. New balance: " << accounts[accountNumber].balance << endl;
    } else {
        cout << "Account not found!" << endl;
    }
}

void withdraw() {
    string accountNumber;
    double amount;
    cout << "Enter account number: ";
    cin >> accountNumber;
    if (accounts.find(accountNumber) != accounts.end()) {
        cout << "Enter amount to withdraw: ";
        cin >> amount;
        if (accounts[accountNumber].balance >= amount) {
            accounts[accountNumber].balance -= amount;
            cout << "Withdrawal successful. New balance: " << accounts[accountNumber].balance << endl;
        } else {
            cout << "Insufficient balance!" << endl;
        }
    } else {
        cout << "Account not found!" << endl;
    }
}


void checkBalance() {
    string accountNumber;
    cout << "Enter account number: ";
    cin >> accountNumber;
    if (accounts.find(accountNumber) != accounts.end()) {
        cout << "Account balance: " << accounts[accountNumber].balance << endl;
    } else {
        cout << "Account not found!" << endl;
    }
}

 information
void displayAccountInfo() {
    string accountNumber;
    cout << "Enter account number: ";
    cin >> accountNumber;
    if (accounts.find(accountNumber) != accounts.end()) {
        cout << "Account Number: " << accounts[accountNumber].accountNumber << endl;
        cout << "Account Holder Name: " << accounts[accountNumber].accountHolderName << endl;
        cout << "Account Balance: " << accounts[accountNumber].balance << endl;
    } else {
        cout << "Account not found!" << endl;
    }
}

int main() {
    int choice;
    while (true) {
        cout << "Bank Management System" << endl;
        cout << "1. Create Account" << endl;
        cout << "2. Deposit" << endl;
        cout << "3. Withdraw" << endl;
        cout << "4. Check Balance" << endl;
        cout << "5. Display Account Info" << endl;
        cout << "6. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1:
                createAccount();
                break;
            case 2:
                deposit();
                break;
            case 3:
                withdraw();
                break;
            case 4:
                checkBalance();
                break;
            case 5:
                displayAccountInfo();
                break;
            case 6:
                return 0;
            default:
                cout << "Invalid choice. Please try again." << endl;
        }
    }
    return 0;
}