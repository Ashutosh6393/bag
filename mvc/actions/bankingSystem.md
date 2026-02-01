________________________Banking-System

AccountBase.cs
 
using dotnet_banking_system.Model;

using System;

using System.Collections.Generic;

using System.Linq;

using System.Net.NetworkInformation;

using System.Text;

using System.Threading.Tasks;
 
namespace dotnet_banking_system.Account

{

    public class AccountBase : IAccount

    {

        private AccountHolder AccountHolder { get; set; }

        private long AccountNumber { get; set; }

        private int Pin { get; set; }

        private double Balance { get; set; }
 
        public AccountBase(AccountHolder accountHolder, long accountNumber, int pin, double balance)

        {

            // complete the constructor

            AccountHolder = accountHolder;

            AccountNumber = accountNumber;

            Pin = pin;

            Balance = balance;

        }
 
        public double CheckBalance()

        {

            // complete the method

           // return 0;

           return Balance;

        }
 
        public void Deposit(double amount)

        {

            // complete the method

            Balance = Balance + amount;  

        }
 
        public AccountHolder GetAccountHolder()

        {

            // complete the method

           // return null;

           return AccountHolder;

        }
 
        public long GetAccountNumber()

        {

            // complete the method

            //return 0;

            return AccountNumber;

        }
 
        public bool ValidatePin(int attemptedPin)

        {

            // complete the method

            if (Pin == attemptedPin)

            {

                return true;

            }

            return false;

        }
 
        public bool Withdraw(double amount)

        {

            // complete the method

            // return false;

            if (Balance >= amount)

            {

                Balance = Balance - amount;

                return true;

            }

            return false;

        }
 
        public override string ToString()

        {

            return "Account{" +

                "accountHolder=" + AccountHolder +

                ", accountNumber=" + AccountNumber +

                ", pin=" + Pin +

                ", balance=" + Balance +

                '}';

        }

    }

}

_________________________________________________________________________________________________________

CommercialAccounts.cs
 
using dotnet_banking_system.Model;

using System;

using System.Collections.Generic;

using System.Linq;

using System.Text;

using System.Threading.Tasks;
 
namespace dotnet_banking_system.Account

{

    public class CommercialAccount : AccountBase

    {

        public List<Person> AuthorizedUsers { get; set; }

        public CommercialAccount(Company company, long accountNumber, int pin, double startingDeposit)

            : base(company, accountNumber, pin, startingDeposit)

        {

            // complete the function

            AuthorizedUsers = new List<Person>();
 
        }
 
        /**

         * @param person The authorized user to add to the account.

         */

        public void AddAuthorizedUser(Person person)

        {

            // complete the function

            if (!AuthorizedUsers.Contains(person))

            {

                AuthorizedUsers.Add(person);

            }

        }
 
        /**

         * @param person

         * @return true if person matches an authorized user in {@link #authorizedUsers}; otherwise, false.

         */

        public bool IsAuthorizedUser(Person person)

        {

            // complete the function

            if (AuthorizedUsers.Contains(person))

            {

                return true;

            }

            return false;

        }

    }

}

________________________________________________________________________________________________

Banks.cs
 
 
using dotnet_banking_system.Account;

using dotnet_banking_system.Model;

using System;

using System.Collections.Generic;

using System.Linq;

using System.Text;

using System.Threading.Tasks;
 
namespace dotnet_banking_system.Banking

{

    public class Bank : IBank

    {

        private List<AccountBase> accounts { get; set; }

        public int CurrentAccountNumber { get; set; }
 
        public Bank()

        {

            // complete the constructor

            accounts = new List<AccountBase>();

            CurrentAccountNumber = 1;

        }

        public bool AuthenticateUser(long accountNumber, int pin)

        {

            // complete the function

            var acc = GetAccount(accountNumber);

            if (acc == null)

            {

                return false;

            }

            return acc.ValidatePin(pin);

        }
 
        public double CheckBalance(long accountNumber)

        {

            // complete the function

            // return 0;

            var acc = GetAccount(accountNumber);

            return acc.CheckBalance();

        }
 
        public void Deposit(long accountNumber, double amount)

        {

            // complete the function

            var account = GetAccount(accountNumber);

            if (account == null)

            {

                throw new Exception();

            }

            account.Deposit(amount);  

        }
 
        public AccountBase GetAccount(long id)

        {

            //complete the function

            return accounts.FirstOrDefault(x => x.GetAccountNumber() == id);

           // return null;

        }
 
        public long OpenCommercialAccount(Company company, int pin, double startingDeposit)

        {

            // complete the function

           // return 0;

           var acc = new CommercialAccount(company, CurrentAccountNumber, pin, startingDeposit);

            accounts.Add(acc);

            return CurrentAccountNumber++;

        }
 
        public long OpenConsumerAccount(Person person, int pin, double startingDeposit)

        {

            // complete the function

            //return 0;

            var acc = new ConsumerAccount(person, CurrentAccountNumber, pin, startingDeposit);

            accounts.Add(acc);

            return CurrentAccountNumber++;

        }
 
        public bool Withdraw(long accountNumber, double amount)

        {

            // complete the function

            //return false;

            var account = GetAccount(accountNumber);

            if (account == null)

            {

                return false;

            }

           return account.Withdraw(amount);

            //return true;

        }
 
        public override string ToString()

        {

            return "Bank{" +

                "accounts=" + accounts +

                '}';

        }

    }

}

__________________________________________________________________________________________________

Transaction.cs
 
 
using dotnet_banking_system.Banking;

using System;

using System.Collections.Generic;

using System.Linq;

using System.Text;

using System.Threading.Tasks;
 
namespace dotnet_banking_system.Transactions

{

    public class Transaction : ITransaction

    {

        private long AccountNumber { get; set; }

        private Bank Bank { get; set; }

        private int AttemptedPin { get; set; }
 
        public Transaction(Bank bank, long accountNumber, int attemptedPin)

        {

            // complete the constructor

            Bank = bank;

            AccountNumber = accountNumber;

            AttemptedPin = attemptedPin;
 
            if(!bank.AuthenticateUser(AccountNumber, AttemptedPin))

            {

                throw new Exception();

            }

        }
 
        public double CheckBalance()

        {

            // complete the method

            //return 0;

            return Bank.CheckBalance(AccountNumber);

        }

        public void Deposit(double amount)

        {

            // complete the method

            Bank.Deposit(AccountNumber, amount);

        }
 
 
        public bool Withdraw(double amount)

        {

            // complete the method

           // return false;

           return Bank.Withdraw(AccountNumber, amount);

        }

    }

}
 
 
 