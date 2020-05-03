# Programming Fundamentals 2 - Lab Session 7

## Disclamer
Below you'll find a verbal description of a system. Natural language is way less strict then programming language and easily may contain ambiguities. It is a long process to convert such a description to a set of deterministic algorithms interacting with each other. It involves a lot of people of different responsibilities: from executives and business analysts to quality assurance and software engineers. In exercises like this, we ask you to try all these job titles. If some requirement is not immediately clear to you, feel free to interpret it based on your impression of "how this should work". There is absolutely nothing wrong with that as long as your approach does not contradict directly requirements you've extracted from natural language description. Experiment and have fun!

This time you will develop a simplified banking system. This is another exercise on Object-Oriented design, and you will create a lot of classes. Don't be overwhelmed - just take your time thinking about the design first. Throughout the implementation you may find useful concepts like Optional Binding, Optional Chaining, Early Exit - please refresh your knowledge on them using official [Swift 5 documentation](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html#ID525). 

## Exercise 1 - Conceptual Design with UML
The Banking System allows a customer to communicate with a clerk and to execute transactions on her bank accounts. It also provides services with ATMs. The customer uses an ATM to perform transactions on bank accounts using cards. The customer may use the card to get cash from the ATM or to pay for some services.

You'll perform three steps to complete this assignment:

- Identifying the properties and behavior of the bank, the clerk, the customer, and bank accounts.
- Extending the banking system with the ATM that can work with different types of cards and execute different operations.
- Implementing what you've just designed.

### Exercise 1.1 - Bank

#### Tasks
Read the description below and complete the UML class diagram. Put your solution in the folder Exercise11. 

### Description
A Bank is a financial institution and usually is defined by its BIC code. Any institution is located at some address. Banks have customers and hold all their bank accounts. Customers, in turn, check regularly the bank accounts and update them every month.

Each month the bank performs a monthly update on all accounts and applies the interests, fees & prints out the details for the corresponding accounts. Depending on the type of account, different monthly updates will be performed.

Clerks are working for a bank. They are only allowed to retrieve the accounts and perform operations on the bank accounts by withdrawing and deposing money.

The bank's customers can ask the following questions to a clerk:
- **deposit :** "Could I put 400 € on my account?"
- **withdraw :** "Could I get 200 € from my account?"
- **money transfer :** "Could I transfer 100 € from account 1 to account 2?"
- **account details :** "Could You give me some details for my account 1?"

The **Clerk** handles the requests given by the customer. He can access all accounts held by a bank. Moreover, he can perform the transactions to change the balances on the accounts.

The following transactions are done by a clerk
- **transfer** : The customer provides a clerk with his account number and a target account number. Then the clerk executes a transfer of the asked amount of money. 
- **deposit** : The customer gives her account number and some money to the clerk. Then the clerk deposits the money on the balance of the bank account.
- **withdraw** : The customer gives his bank account and asks for money to be withdrawn from his balance.
- **account details** : The customer gives his account number to the clerk and in return receives account details.

The **BankAccount** is held by a bank for a specific customer. The bank account belongs to exactly one customer. Each bank account is identified by an account number (IBAN number). We use bank accounts to control our money. Each bank account has a balance, that represents the amount of money on the account. Customers may ask to withdraw or deposit money on/of their balance. Each month, the bank prints out the account details and its balance of bank accounts.

There are several types of bank accounts:
- **CheckingAccount** : A checking account has no interests. The bank permits five free transactions per month. Balance inquiry is free of charge. A fee is deducted from the balance at the end of the month by the bank for each additional transaction beyond the limit. Each additional transaction costs 2€.
- **SavingAccount** : A saving account is used for saving money. Every month, some specific interests are applied to saving accounts. These interests are added to the balance at the end of the month.
- **TimeDepositAccount** : A TimeDepositAccount is used for saving money. The length of the deposit period must be specified. The period is specified by some number of months, where the money should not be touched. Same as for SavingAccounts, interests are applied on the time deposit accounts. Every month, the interests are compounded and the saving period is updated. Note, that there is a penalty for early withdrawal.

### Exercise 1.2 - Adding ATM

#### Tasks
Complete the UML class diagram from the previous exercise by adding the concepts related to ATMs & cards. Put your solution in the folder Exercise11.

#### Description
The bank wants to extend its system with an ATM and cards. A customer can use the ATM to get cash from her bank account. The customer uses the card as an authentication method. The customer puts the card in the atm, enters a password to log in and asks for cash. After login, he might abort to get the card back.

The **Card** belongs to a customer, who uses the card to authenticate to an ATM. He uses a card to get some cash at the ATM or to pay for something. The customer knows the password of the card. A card is usually defined by its card number and is mapped to exactly one checking account. Usually, a card has an expiration date. The user has only 3 attempts max to authenticate herself. For some cards, the bank performs monthly updates to book money from the balance of the checking account or to apply some fees. A customer can withdraw an amount of money from the balance of his checking account only if there is enough money.

There are two types of cards:
- **Debit Card** : A debit card does transactions directly on your bank account: the money is directly withdrawn from the bank account in real-time. Using the debit card a customer can exceed the limit of the account. However, if he has a negative balance, overdraft fees of 10% are applied every month by the bank and removed from the account. The customer will have to pay more an more fees if he doesn't get out of the negative balance.

- **Credit Cards** : The customer can use this card as long he does not reach the credit limit. The used credit is withdrawn every month by the bank from the balance. Afterward, the used credit is reset to 0 for the next month. Fees of 2% are only applied if the customer uses the credit card to get some cash. For payments, no fees applied.

The **ATM** takes the card of the customer and asks for a password. The ATM can detect if the card is inserted. Moreover, it knows if the customer is already authenticated. After the customer has entered the password, the atm authenticates her. For authenticating the customer, the following conditions must be satisfied:
- the ATM must have a card inserted
- the customer has not exceeded the maximal attempts to log in
- if the password provided by the customer is correct
- the customer is authenticated

If all the tests are passed, then the customer is correctly authenticated. Note that if he aborts the process or receives the card back, he's considered as logged out and not authenticated. In case the password isn't correct, the number of attempts is incremented. If the maximal number of attempts is reached the user cannot use his card to connect anymore. After being correctly authenticated, the customer can get some cash resp. abort the process.

#### Hints
- Make use of [Umletino](http://www.umletino.com/) to complete the UML diagrams.
- Train your abstraction and identify the concepts needed for defining a Bank.
- The number of attempts, the max number of attempts and the password are usually stored on the card.


## Exercise 2 - Implementation in Swift

#### Tasks
Look at your class diagram and implement the banking system. This is your first and only Swift project for the assignment, name it Exercise2.

### Hints (in no particular order)
- Reuse the methods of your superclasses to avoid having redundant code in multiple classes and to ensure better maintenance
- Guards might be useful to assert the values of the passed parameters in some methods of your class
- Dictionaries are useful for defining all cards that a customer got. They might be used to retrieve the accounts in a Bank by mapping the account number to the account
- Use Double to model money on a bank account (and never do that in a real-life)
- Make it simple and use only one bank. The transactions can be done only on accounts of the same ban
- Please use access controls for your method and properties

### Notes
1. Sometimes, if you are not using an output of a method of a class in your boilerplate code, a warning shows up. To remove this warning put the following annotation annotations before the signature of the concerned method  : **@discardableResult**

### Console Output - Example
```
***QUESTION*** : Could You give me some details for my account 1?
Your balance is currently set to 0.0
***QUESTION*** : Could You give me some details for my account 2?
Your balance is currently set to 0.0
***QUESTION*** : Could You give me some details for my account 3?
Your balance is currently set to 0.0
***QUESTION*** : Could I put 2000.0 € on my account?
2000.0 € has been deposit to your account
***QUESTION*** : Could I transfer 150.0 € from account 1 to account 2 ?
150.0 € has been transferred to your account
***QUESTION*** : Could I put 2000.0 € on my account?
2000.0 € has been deposit to your account
***QUESTION*** : Could I get 100.0 € from my account?
100.0 € has been withdrawn from your account
***QUESTION*** : Could You give me some details for my account 1?
Your balance is currently set to 1850.0
***QUESTION*** : Could You give me some details for my account 2?
Your balance is currently set to 150.0
***QUESTION*** : Could You give me some details for my account 3?
Your balance is currently set to 0.0
To continute please, enter your password!
You are not authenticated!
Your have successfully logged in!
Your are already logged in!
Customer has now 50.0
```
