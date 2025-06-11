
#include <iostream>
#include <fstream>
#include <vector>
#include <sstream>

using namespace std;

struct Account {
    string username;
    string password;
    string pin;
    int balance;
};

// Function declarations
void createAccount();
bool login(Account &loggedInAccount);
void deposit(Account &acc);
void withdraw(Account &acc);
void checkBalance(const Account &acc);
void showMenu();
vector<Account> loadAccounts();
void saveAccounts(const vector<Account> &accounts);

int main() {
    vector<Account> accounts = loadAccounts();
    Account loggedInAccount;

    cout << "\n========== Welcome to Multi-User Bank System ==========" << endl;

    int option;
    cout << "\n1. Create Account\n2. Login\nChoose option: ";
    cin >> option;

    if (option == 1) {
        createAccount();
    } else if (option == 2) {
        if (login(loggedInAccount)) {
            int choice;
            while (true) {
                showMenu();
                cout << "Enter your choice: ";
                cin >> choice;

                switch (choice) {
                    case 1:
                        deposit(loggedInAccount);
                        break;
                    case 2:
                        withdraw(loggedInAccount);
                        break;
                    case 3:
                        checkBalance(loggedInAccount);
                        break;
                    case 4:
                        saveAccounts(accounts); // save previous data
                        accounts = loadAccounts(); // reload
                        for (auto &acc : accounts) {
                            if (acc.username == loggedInAccount.username)
                                acc = loggedInAccount;
                        }
                        saveAccounts(accounts);
                        cout << "Goodbye!\n";
                        return 0;
                    default:
                        cout << "Invalid choice!\n";
                }
            }
        } else {
            cout << "Login failed.\n";
        }
    } else {
        cout << "Invalid option.\n";
    }

    return 0;
}

// File operations
vector<Account> loadAccounts() {
    vector<Account> accounts;
    ifstream file("accounts.txt");
    string line;

    while (getline(file, line)) {
        stringstream ss(line);
        Account acc;
        ss >> acc.username >> acc.password >> acc.pin >> acc.balance;
        accounts.push_back(acc);
    }
    return accounts;
}

void saveAccounts(const vector<Account> &accounts) {
    ofstream file("accounts.txt");
    for (const auto &acc : accounts) {
        file << acc.username << " " << acc.password << " " << acc.pin << " " << acc.balance << "\n";
    }
}

// Create a new account
void createAccount() {
    vector<Account> accounts = loadAccounts();
    Account newAcc;
    cout << "Enter username: ";
    cin >> newAcc.username;

    // Check for existing username
    for (const auto &acc : accounts) {
        if (acc.username == newAcc.username) {
            cout << "Username already exists!\n";
            return;
        }
    }

    cout << "Create password: ";
    cin >> newAcc.password;
    cout << "Set 4-digit PIN: ";
    cin >> newAcc.pin;
    newAcc.balance = 0;

    accounts.push_back(newAcc);
    saveAccounts(accounts);
    cout << "Account created successfully!\n";
}

// Login and PIN verification
bool login(Account &loggedInAccount) {
    vector<Account> accounts = loadAccounts();
    string username, password, pin;

    cout << "Username: ";
    cin >> username;
    cout << "Password: ";
    cin >> password;

    for (const auto &acc : accounts) {
        if (acc.username == username && acc.password == password) {
            cout << "Enter 4-digit PIN: ";
            cin >> pin;
            if (pin == acc.pin) {
                loggedInAccount = acc;
                cout << "Login successful!\n";
                return true;
            } else {
                cout << "Incorrect PIN.\n";
                return false;
            }
        }
    }
    return false;
}

// Menu display
void showMenu() {
    cout << "\n=========== Main Menu ===========\n";
    cout << "1. Deposit\n";
    cout << "2. Withdraw\n";
    cout << "3. Check Balance\n";
    cout << "4. Exit\n";
}

// Deposit
void deposit(Account &acc) {
    int amount;
    cout << "Enter deposit amount: ";
    cin >> amount;
    if (amount > 0) {
        acc.balance += amount;
        cout << "Deposit successful. New Balance: " << acc.balance << "\n";
    } else {
        cout << "Amount must be greater than 0.\n";
    }
}

// Withdraw
void withdraw(Account &acc) {
    int amount;
    cout << "Enter withdrawal amount: ";
    cin >> amount;
    if (amount > 0 && amount <= acc.balance) {
        acc.balance -= amount;
        cout << "Withdrawal successful. Remaining Balance: " << acc.balance << "\n";
    } else {
        cout << "Invalid or insufficient balance.\n";
    }
}

// Check balance
void checkBalance(const Account &acc) {
    cout << "Your current balance is: " << acc.balance << "\n";
}
