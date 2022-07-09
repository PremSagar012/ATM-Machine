# ATM-Machine
This Project is to make an Automated Teller Machine with user's Account Number,Password,and bank account.Using this data,users can withdraw, deposit, and view their account balance.

ATM.java

import java.io.IOException;

public class ATM {

public static void main(String[] args) throws IOException {
	OptionMenu optionMenu = new OptionMenu();
	introduction();
	optionMenu.mainMenu();
}

public static void introduction() {
	System.out.println("Welcome to the ATM Project!");
}
}

It is used as main class where we used other classes to go through the input and transactions. Also it is responsible to display the "Welcome" output.

OptionMenu.java

Scanner menuInput = new Scanner(System.in); DecimalFormat moneyFormat = new DecimalFormat("'$'###,##0.00"); HashMap<Integer, Account> data = new HashMap<Integer, Account>();

We used scanner to take the inputs from the user. HashMap is used so that after entering data we can match the id and the pin linked to it. We used DecimalFormat to format numbers of the pattern declared. Here, it starts with dollar sign and ends with two 0's after decimal.

public void getLogin() throws IOException { boolean end = false; int customerNumber = 0; int pinNumber = 0; while (!end) { try { System.out.print("\nEnter your customer number: "); customerNumber = menuInput.nextInt(); System.out.print("\nEnter your PIN number: "); pinNumber = menuInput.nextInt(); Iterator it = data.entrySet().iterator(); while (it.hasNext()) { Map.Entry pair = (Map.Entry) it.next(); Account acc = (Account) pair.getValue(); if (data.containsKey(customerNumber) && pinNumber == acc.getPinNumber()) { getAccountType(acc); end = true; break; } } if (!end) { System.out.println("\nWrong Customer Number or Pin Number"); } } catch (InputMismatchException e) { System.out.println("\nInvalid Character(s). Only Numbers."); } } }

getLogin() class is used for login. It checks if the credentials entered is correct or not. If not, it throws an Exception and Error is displayed.

public void getAccountType(Account acc) { boolean end = false; while (!end) { try { System.out.println("\nSelect the account you want to access: "); System.out.println(" Type 1 - Checkings Account"); System.out.println(" Type 2 - Savings Account"); System.out.println(" Type 3 - Exit"); System.out.print("\nChoice: ");

			int selection = menuInput.nextInt();

			switch (selection) {
			case 1:
				getChecking(acc);
				break;
			case 2:
				getSaving(acc);
				break;
			case 3:
				end = true;
				break;
			default:
				System.out.println("\nInvalid Choice.");
			}
		} catch (InputMismatchException e) {
			System.out.println("\nInvalid Choice.");
			menuInput.next();
		}
	}
}
Here, we choose which type of account we want to login, including Exception if any incorrect option is entered.

public void getChecking(Account acc) { boolean end = false; while (!end) { try { System.out.println("\nCheckings Account: "); System.out.println(" Type 1 - View Balance"); System.out.println(" Type 2 - Withdraw Funds"); System.out.println(" Type 3 - Deposit Funds"); System.out.println(" Type 4 - Transfer Funds"); System.out.println(" Type 5 - Exit"); System.out.print("\nChoice: ");

			int selection = menuInput.nextInt();

			switch (selection) {
			case 1:
				System.out.println("\nCheckings Account Balance: " + moneyFormat.format(acc.getCheckingBalance()));
				break;
			case 2:
				acc.getCheckingWithdrawInput();
				break;
			case 3:
				acc.getCheckingDepositInput();
				break;

			case 4:
				acc.getTransferInput("Checkings");
				break;
			case 5:
				end = true;
				break;
			default:
				System.out.println("\nInvalid Choice.");
			}
		} catch (InputMismatchException e) {
			System.out.println("\nInvalid Choice.");
			menuInput.next();
		}
	}
}
Here we are provided options for Checkings Account to View Balance, Deposit Funds, Transfer Funds and Withdraw Funds also Exit option to go to the previous window.

public void getSaving(Account acc) { boolean end = false; while (!end) { try { System.out.println("\nSavings Account: "); System.out.println(" Type 1 - View Balance"); System.out.println(" Type 2 - Withdraw Funds"); System.out.println(" Type 3 - Deposit Funds"); System.out.println(" Type 4 - Transfer Funds"); System.out.println(" Type 5 - Exit"); System.out.print("Choice: "); int selection = menuInput.nextInt(); switch (selection) { case 1: System.out.println("\nSavings Account Balance: " + moneyFormat.format(acc.getSavingBalance())); break; case 2: acc.getsavingWithdrawInput(); break; case 3: acc.getSavingDepositInput(); break; case 4: acc.getTransferInput("Savings"); break; case 5: end = true; break; default: System.out.println("\nInvalid Choice."); } } catch (InputMismatchException e) { System.out.println("\nInvalid Choice."); menuInput.next(); } } }

Same as the previous class, here we are provided options for Savings Account to View Balance, Deposit Funds, Transfer Funds and Withdraw Funds also Exit option to go to the previous window.

public void createAccount() throws IOException { int cst_no = 0; boolean end = false; while (!end) { try { System.out.println("\nEnter your customer number "); cst_no = menuInput.nextInt(); Iterator it = data.entrySet().iterator(); while (it.hasNext()) { Map.Entry pair = (Map.Entry) it.next(); if (!data.containsKey(cst_no)) { end = true; } } if (!end) { System.out.println("\nThis customer number is already registered"); } } catch (InputMismatchException e) { System.out.println("\nInvalid Choice."); menuInput.next(); } } System.out.println("\nEnter PIN to be registered"); int pin = menuInput.nextInt(); data.put(cst_no, new Account(cst_no, pin)); System.out.println("\nYour new account has been successfuly registered!"); System.out.println("\nRedirecting to login............."); getLogin(); }

It creates a new account

public void mainMenu() throws IOException { data.put(952141, new Account(952141, 191904, 1000, 5000)); data.put(123, new Account(123, 123, 20000, 50000)); boolean end = false; while (!end) { try { System.out.println("\n Type 1 - Login"); System.out.println(" Type 2 - Create Account"); System.out.print("\nChoice: "); int choice = menuInput.nextInt(); switch (choice) { case 1: getLogin(); end = true; break; case 2: createAccount(); end = true; break; default: System.out.println("\nInvalid Choice."); } } catch (InputMismatchException e) { System.out.println("\nInvalid Choice."); menuInput.next(); } } System.out.println("\nThank You for using this ATM.\n"); menuInput.close(); System.exit(0); } }

This class is like main menu which appears firt with 2 options, either to login or create a new account.

Account.java

It is used to enter all the details of the customer and update it as per the options selected from the Option Menu.

import java.text.DecimalFormat; import java.util.InputMismatchException; import java.util.Scanner;

public class Account { // variables private int customerNumber; private int pinNumber; private double checkingBalance = 0; private double savingBalance = 0;

Scanner input = new Scanner(System.in);
DecimalFormat moneyFormat = new DecimalFormat("'$'###,##0.00");

public Account() {
}

public Account(int customerNumber, int pinNumber) {
	this.customerNumber = customerNumber;
	this.pinNumber = pinNumber;
}

public Account(int customerNumber, int pinNumber, double checkingBalance, double savingBalance) {
	this.customerNumber = customerNumber;
	this.pinNumber = pinNumber;
	this.checkingBalance = checkingBalance;
	this.savingBalance = savingBalance;
}

public int setCustomerNumber(int customerNumber) {
	this.customerNumber = customerNumber;
	return customerNumber;
}

public int getCustomerNumber() {
	return customerNumber;
}

public int setPinNumber(int pinNumber) {
	this.pinNumber = pinNumber;
	return pinNumber;
}

public int getPinNumber() {
	return pinNumber;
}

public double getCheckingBalance() {
	return checkingBalance;
}

public double getSavingBalance() {
	return savingBalance;
}

public double calcCheckingWithdraw(double amount) {
	checkingBalance = (checkingBalance - amount);
	return checkingBalance;
}

public double calcSavingWithdraw(double amount) {
	savingBalance = (savingBalance - amount);
	return savingBalance;
}

public double calcCheckingDeposit(double amount) {
	checkingBalance = (checkingBalance + amount);
	return checkingBalance;
}

public double calcSavingDeposit(double amount) {
	savingBalance = (savingBalance + amount);
	return savingBalance;
}

public void calcCheckTransfer(double amount) {
	checkingBalance = checkingBalance - amount;
	savingBalance = savingBalance + amount;
}

public void calcSavingTransfer(double amount) {
	savingBalance = savingBalance - amount;
	checkingBalance = checkingBalance + amount;
}

public void getCheckingWithdrawInput() {
	boolean end = false;
	while (!end) {
		try {
			System.out.println("\nCurrent Checkings Account Balance: " + moneyFormat.format(checkingBalance));
			System.out.print("\nAmount you want to withdraw from Checkings Account: ");
			double amount = input.nextDouble();
			if ((checkingBalance - amount) >= 0 && amount >= 0) {
				calcCheckingWithdraw(amount);
				System.out.println("\nCurrent Checkings Account Balance: " + moneyFormat.format(checkingBalance));
				end = true;
			} else {
				System.out.println("\nBalance Cannot be Negative.");
			}
		} catch (InputMismatchException e) {
			System.out.println("\nInvalid Choice.");
			input.next();
		}
	}
}

public void getsavingWithdrawInput() {
	boolean end = false;
	while (!end) {
		try {
			System.out.println("\nCurrent Savings Account Balance: " + moneyFormat.format(savingBalance));
			System.out.print("\nAmount you want to withdraw from Savings Account: ");
			double amount = input.nextDouble();
			if ((savingBalance - amount) >= 0 && amount >= 0) {
				calcSavingWithdraw(amount);
				System.out.println("\nCurrent Savings Account Balance: " + moneyFormat.format(savingBalance));
				end = true;
			} else {
				System.out.println("\nBalance Cannot Be Negative.");
			}
		} catch (InputMismatchException e) {
			System.out.println("\nInvalid Choice.");
			input.next();
		}
	}
}

public void getCheckingDepositInput() {
	boolean end = false;
	while (!end) {
		try {
			System.out.println("\nCurrent Checkings Account Balance: " + moneyFormat.format(checkingBalance));
			System.out.print("\nAmount you want to deposit from Checkings Account: ");
			double amount = input.nextDouble();
			if ((checkingBalance + amount) >= 0 && amount >= 0) {
				calcCheckingDeposit(amount);
				System.out.println("\nCurrent Checkings Account Balance: " + moneyFormat.format(checkingBalance));
				end = true;
			} else {
				System.out.println("\nBalance Cannot Be Negative.");
			}
		} catch (InputMismatchException e) {
			System.out.println("\nInvalid Choice.");
			input.next();
		}
	}
}

public void getSavingDepositInput() {
	boolean end = false;
	while (!end) {
		try {
			System.out.println("\nCurrent Savings Account Balance: " + moneyFormat.format(savingBalance));
			System.out.print("\nAmount you want to deposit into your Savings Account: ");
			double amount = input.nextDouble();

			if ((savingBalance + amount) >= 0 && amount >= 0) {
				calcSavingDeposit(amount);
				System.out.println("\nCurrent Savings Account Balance: " + moneyFormat.format(savingBalance));
				end = true;
			} else {
				System.out.println("\nBalance Cannot Be Negative.");
			}
		} catch (InputMismatchException e) {
			System.out.println("\nInvalid Choice.");
			input.next();
		}
	}
}

public void getTransferInput(String accType) {
	boolean end = false;
	while (!end) {
		try {
			if (accType.equals("Checkings")) {
				System.out.println("\nSelect an account you wish to tranfers funds to:");
				System.out.println("1. Savings");
				System.out.println("2. Exit");
				System.out.print("\nChoice: ");
				int choice = input.nextInt();
				switch (choice) {
				case 1:
					System.out.println("\nCurrent Checkings Account Balance: " + moneyFormat.format(checkingBalance));
					System.out.print("\nAmount you want to deposit into your Savings Account: ");
					double amount = input.nextDouble();
					if ((savingBalance + amount) >= 0 && (checkingBalance - amount) >= 0 && amount >= 0) {
						calcCheckTransfer(amount);
						System.out.println("\nCurrent Savings Account Balance: " + moneyFormat.format(savingBalance));
						System.out.println(
								"\nCurrent Checkings Account Balance: " + moneyFormat.format(checkingBalance));
						end = true;
					} else {
						System.out.println("\nBalance Cannot Be Negative.");
					}
					break;
				case 2:
					return;
				default:
					System.out.println("\nInvalid Choice.");
					break;
				}
			} else if (accType.equals("Savings")) {
				System.out.println("\nSelect an account you wish to tranfers funds to: ");
				System.out.println("1. Checkings");
				System.out.println("2. Exit");
				System.out.print("\nChoice: ");
				int choice = input.nextInt();
				switch (choice) {
				case 1:
					System.out.println("\nCurrent Savings Account Balance: " + moneyFormat.format(savingBalance));
					System.out.print("\nAmount you want to deposit into your savings account: ");
					double amount = input.nextDouble();
					if ((checkingBalance + amount) >= 0 && (savingBalance - amount) >= 0 && amount >= 0) {
						calcSavingTransfer(amount);
						System.out.println("\nCurrent checkings account balance: " + moneyFormat.format(checkingBalance));
						System.out.println("\nCurrent savings account balance: " + moneyFormat.format(savingBalance));
						end = true;
					} else {
						System.out.println("\nBalance Cannot Be Negative.");
					}
					break;
				case 2:
					return;
				default:
					System.out.println("\nInvalid Choice.");
					break;
				}
			}
		} catch (InputMismatchException e) {
			System.out.println("\nInvalid Choice.");
			input.next();
		}
	}
}
}

Thank You!
