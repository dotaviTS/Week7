# Lesson: More Concurrency, Supervision, and Registry in Elixir

## Part 1: Supervisors

### Introduction to Supervisors

Supervisors are specialized processes that monitor other processes (called child processes). If a child process crashes, the supervisor will restart it, thus providing resilience to your system.

Exercise 1: Implementing a Supervisor for BankAccount

1. Create a Supervisor Module: Define a supervisor that will manage BankAccount processes.

```
defmodule BankSupervisor do
  use Supervisor

  def start_link(_) do
    Supervisor.start_link(__MODULE__, :ok, name: :bank_supervisor)
  end

  def init(:ok) do
    children = [
      {BankAccount, 1000} # Example child specification
    ]
    Supervisor.init(children, strategy: :one_for_one)
  end
end
```
Modify the BankAccount Module: Ensure that the BankAccount module is suitable for supervision.

Exercise 1A: Implement the BankSupervisor.

Step 1: Modify the BankAccount Module
Before we can supervise the BankAccount, we need to make sure it's suitable for supervision. 
Ensure that the BankAccount module's start_link function is set up correctly. Here's a general outline:
```
defmodule BankAccount do
  use GenServer

  def start_link(initial_balance) do
    GenServer.start_link(__MODULE__, initial_balance, name: :bank_account)
  end

  # ... rest of the code ...
end
```

Step 2: Implement the BankSupervisor

Create a new module named BankSupervisor to manage the BankAccount processes. This supervisor will restart the child process if it crashes.
```
defmodule BankSupervisor do
  use Supervisor

  def start_link(_) do
    Supervisor.start_link(__MODULE__, :ok, name: :bank_supervisor)
  end

  def init(:ok) do
    children = [
      {BankAccount, 1000} # Example child specification
    ]
    Supervisor.init(children, strategy: :one_for_one)
  end
end
```

Step 3: Start the Supervisor and BankAccount

- Exercise 1A: Implement the BankSupervisor module as described above.

- Exercise 1B: Start the BankSupervisor and then start a BankAccount under its supervision. 
You can do this in an IEx session or within your application's start function:
```
{:ok, _} = BankSupervisor.start_link([])
BankAccount.start_link(1000) # Starts a bank account with an initial balance of 1000
```

# Part 2: Registry
## Introduction to Registry

The Registry module in Elixir allows processes to be registered under a unique key. This is particularly useful for identifying processes, such as bank accounts, by their account numbers.

Exercise 2: Implementing Registry for BankAccount

1. Create a Registry Module: Define a registry for bank accounts.

```
defmodule BankRegistry do
  use Registry

  def start_link do
    Registry.start_link(keys: :unique, name: BankRegistry)
  end
end
```

Modify BankAccount for Registration: Update the BankAccount start_link function to register with the account number.
```
def start_link(account_number, initial_balance) do
  GenServer.start_link(__MODULE__, initial_balance, name: {:via, Registry, {BankRegistry, account_number}})
end
```

Example: Starting two accounts with different numbers.
```
BankAccount.start_link("ACC123", 1000)
BankAccount.start_link("ACC456", 500)
```

Exercise 2A: Modify the BankAccount application to use Registry.

Step 1: Create a Registry Module
First, create a new module named BankRegistry that will act as the registry for bank accounts.
```
defmodule BankRegistry do
  use Registry

  def start_link do
    Registry.start_link(keys: :unique, name: BankRegistry)
  end
end
```

This module uses Elixir's built-in Registry module, specifying that keys (in our case, account numbers) must be unique.

Step 2: Modify the BankAccount Module
Next, update the BankAccount module's start_link function to register each new bank account with a unique account number in the BankRegistry.
```
defmodule BankAccount do
  # ... existing code ...

  def start_link(account_number, initial_balance) do
    GenServer.start_link(__MODULE__, initial_balance, name: {:via, Registry, {BankRegistry, account_number}})
  end

  # ... rest of the code ...
end
```

Step 3: Start the Registry and Test the BankAccount
- Exercise 2A: Modify the BankAccount application to use Registry as described above. You'll also need to ensure that the BankRegistry is started when your application starts.
- Exercise 2B: Test by creating accounts with unique numbers and perform some transactions. Here's a sample sequence you can try in an IEx session:
```
{:ok, _} = BankRegistry.start_link()
{:ok, _} = BankAccount.start_link("ACC123", 1000)
{:ok, _} = BankAccount.start_link("ACC456", 500)
BankAccount.deposit("ACC123", 200)
BankAccount.withdraw("ACC456", 100)
```

# Part 3: Multiple Accounts and Transfers
## Supporting Multiple Accounts

With the Registry in place, as implemented in the previous exercise, you can now create multiple accounts with unique account numbers. 
This allows the system to differentiate between various customer accounts and operate on them individually.

### Implementing Transfers
A common banking operation is transferring money from one account to another. This operation must be atomic, meaning that it either completes entirely or not at all. 
If any error occurs during a transfer, such as insufficient funds, the entire operation should be rolled back.

Add a Transfer Function: Implement a function in BankAccount to perform transfers.
```
def transfer(from_account, to_account, amount) do
  # Implementation here
end
```

- Exercise 3A: Write a Function to Perform a Transfer Between Two Accounts

Sample skeleton implementation:

```
def transfer(from_account, to_account, amount) do
  with {:ok, _} <- withdraw(from_account, amount),
       {:ok, _} <- deposit(to_account, amount) do
    {:ok, "Transfer successful"}
  else
    error -> error
  end
end
```

- Exercise 3B: Handle Possible Errors Such as Insufficient Funds
The above implementation is a good start, but it lacks proper error handling. If the withdrawal from the from_account fails due to insufficient funds, the entire operation should be rolled back.

Handle Insufficient Funds: If the withdrawal from the from_account fails, you must not proceed with the deposit into the to_account.

Provide Informative Errors: Return descriptive error messages that indicate what went wrong, such as {:error, "Insufficient funds"}.

Here's an enhanced version of the transfer function:
```
def transfer(from_account, to_account, amount) do
  from_account_pid = Registry.lookup(BankRegistry, from_account) |> List.first()
  to_account_pid = Registry.lookup(BankRegistry, to_account) |> List.first()

  case BankAccount.withdraw(from_account_pid, amount) do
    {:ok, _} ->
      case BankAccount.deposit(to_account_pid, amount) do
        {:ok, _} -> {:ok, "Transfer successful"}
        error -> error
      end
    {:error, reason} -> {:error, reason}
  end
end
```

This version checks the result of the withdrawal and only proceeds with the deposit if the withdrawal was successful. If there's an error, it returns that error immediately.

Exercise Questions:
- How does a Supervisor restart a process? What happens if the Supervisor itself crashes?
- Describe the purpose of the Registry module in Elixir. How can it be used in managing processes?
- Discuss the advantages and disadvantages of using a global Registry vs. a local Registry in an Elixir application.

# Part 4: Concurrency and Error Handling

## Handling Concurrent Operations

Concurrency enables multiple operations to occur simultaneously. In a bank system, this means handling simultaneous deposits, withdrawals, and transfers.

Error Handling
Proper error handling ensures that your application behaves correctly under various scenarios.
Add Guard Clauses: Include guard clauses to validate inputs.


- Exercise 4B: Add error handling for scenarios like insufficient funds.


Extra REading:

[Elixir Supervisors](https://elixir-lang.org/getting-started/mix-otp/supervisor-and-application.html)

(Elixir Registry)[https://hexdocs.pm/elixir/Registry.html]
