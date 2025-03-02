# Task4
Banking system using cpp
#include <iostream>
#include <map>
#include <limits>  // Required for clearing input buffer

using namespace std;

class BankAccount {
public:
    string name;
    double balance;

    // default constructor (Needed for std::map)
    BankAccount() : name(""), balance(0.0) {}

    // parameterized constructor
    BankAccount(string n, double b) : name(n), balance(b) {}

    void deposit(double amount) {
        if (amount > 0)
            balance += amount;
        else
            cout << "Invalid deposit amount!\n";
    }

    bool withdraw(double amount) {
        if (amount > balance) {
            cout << "Insufficient balance!\n";
            return false;
        }
        if (amount <= 0) {
            cout << "Invalid withdrawal amount!\n";
            return false;
        }
        balance -= amount;
        return true;
    }

    void display() {
        cout << "Account Holder: " << name << " | Balance: " << balance << endl;
    }
};

// Global variables
map<int, BankAccount> accounts;
int accountNumber = 1001;

// function to clear input buffer
void clearInput() {
    cin.clear();  // Clear error flags
    cin.ignore(numeric_limits<streamsize>::max(), '\n');  // Remove invalid input
}

// function to create an account
void createAccount() {
    string name;
    double initialDeposit;

    cout << "Enter name: ";
    cin.ignore();  // Clear previous input buffer before taking full name
    getline(cin, name);

    cout << "Enter initial deposit: ";
    cin >> initialDeposit;
    if (cin.fail() || initialDeposit < 0) {
        cout << "Invalid input! Please enter a valid amount.\n";
        clearInput();
        return;
    }

    accounts[accountNumber] = BankAccount(name, initialDeposit);
    cout << "Account created! Your Account Number: " << accountNumber << endl;
    accountNumber++;  // Increment account number after successful creation
}

// function for deposit/withdrawal transactions
void transaction() {
    int accNum, choice;
    double amount;

    cout << "Enter account number: ";
    cin >> accNum;
    if (cin.fail()) {
        cout << "Invalid input! Please enter a valid account number.\n";
        clearInput();
        return;
    }

    if (accounts.find(accNum) == accounts.end()) {
        cout << "Invalid Account!\n";
        return;
    }

    cout << "1. Deposit\n2. Withdraw\nChoose: ";
    cin >> choice;
    if (cin.fail() || (choice != 1 && choice != 2)) {
        cout << "Invalid choice! Please enter 1 or 2.\n";
        clearInput();
        return;
    }

    cout << "Enter amount: ";
    cin >> amount;
    if (cin.fail()) {
        cout << "Invalid input! Please enter a valid amount.\n";
        clearInput();
        return;
    }

    if (choice == 1) {
        accounts[accNum].deposit(amount);
    } else {
        accounts[accNum].withdraw(amount);
    }

    accounts[accNum].display();
}

// main function
int main() {
    int choice;
    while (true) {
        cout << "\n1. Create Account\n2. Transaction\n3. Exit\nChoose: ";
        cin >> choice;
        if (cin.fail()) {
            cout << "Invalid input! Please enter 1, 2, or 3.\n";
            clearInput();
            continue;
        }

        if (choice == 1)
            createAccount();
        else if (choice == 2)
            transaction();
        else if (choice == 3)
            break;
        else
            cout << "Invalid choice! Please enter 1, 2, or 3.\n";
    }

    return 0;
}
