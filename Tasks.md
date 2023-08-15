# Processes

Concurrency is achieved through processes. Processes are lightweight, isolated, and can run simultaneously. They communicate with each other through message passing.

## Tasks
Tasks provide a way to perform computations concurrently. They are useful for running a computation in parallel with your current process.

## Starting the Bank Account GenServer
Here's how you can start a GenServer with an initial balance for our bank account example:

```
{:ok, pid} = BankAccount.start_link(1000)
```

## Depositing Money
Deposit an amount into the account:

```
BankAccount.deposit(pid, 200)
```

## Withdrawing Money
Withdraw an amount from the account:
```
BankAccount.withdraw(pid, 200)
```

## Checking the Balance
Retrieve the current balance:
```
balance = BankAccount.balance(pid)
```

## Processes and spawn
In Elixir, processes are the building blocks of concurrency. They are lightweight and run independently of each other. They can be created using the spawn function.

### spawn
- The spawn function creates a new process to execute a given function. It returns a process identifier (PID) that can be used to refer to the process.

Example:
```
pid = spawn(fn -> IO.puts("Hello from a new process!") end)
```
This example spawns a new process that prints a message. Since the processes are independent, they might execute in any order, and their behavior might vary between runs.

### Concurrent Deposits and Withdrawals
### Using spawn
You can use spawn to create a new process:
```
spawn(fn -> BankAccount.deposit(pid, 100) end)
spawn(fn -> BankAccount.withdraw(pid, 50) end)
```

## Tasks
Tasks provide a more convenient way to run functions concurrently. They are built on top of processes and provide additional functionality.

### Task.async
Task.async is used to run a function concurrently with the current process. It returns a task struct that can be used later to retrieve the result.

Example:

task = Task.async(fn -> 2 + 2 end)
This example creates a new task that calculates 2 + 2. The result is not immediately available but can be retrieved later using Task.await.

### Task.await
Task.await is used to retrieve the result of a task created with Task.async. It takes the task struct and waits for the result. If the task finishes within the given timeout (defaults to 5000ms), it returns the result. If not, it raises an error.

Example:

```
result = Task.await(task)
IO.puts(result) # Outputs: 4
```

Using Tasks for Concurrent Deposits and Withdrawals
Tasks can be used to simulate concurrent deposits and withdrawals in our bank account example. 

Here's a step-by-step explanation:

Create Tasks for Deposits and Withdrawals: Use Task.async to create tasks for deposits and withdrawals.

Wait for Results with Task.await: Use Task.await to wait for the results of each task. This ensures that you get the final balance after all deposits and withdrawals have been processed.

Check the Final Balance: Use an assertion to check that the final balance is as expected.

Example Test:

```
test "concurrent deposits and withdrawals" do
  {:ok, pid} = BankAccount.start_link(1000)

  tasks =
    1..10
    |> Enum.map(fn _ ->
      Task.async(fn ->
        BankAccount.deposit(pid, 100)
        BankAccount.withdraw(pid, 50)
      end)
    end)

  Enum.each(tasks, &Task.await/1)

  assert BankAccount.balance(pid) == 1500
end
```

Processes enable concurrency in Elixir and can be created using spawn.
Tasks provide a higher-level abstraction for concurrency and are created using Task.async.
Task.await is used to retrieve the result of a task, waiting for it to complete.

### Using Tasks

Tasks provide a more convenient way to perform concurrent operations:
```
task1 = Task.async(fn -> BankAccount.deposit(pid, 100) end)
task2 = Task.async(fn -> BankAccount.withdraw(pid, 50) end)
Task.await(task1)
Task.await(task2)
```
### Writing a Concurrent Test with Tasks

```
test "concurrent deposits and withdrawals" do
  {:ok, pid} = BankAccount.start_link(1000)

  tasks =
    1..10
    |> Enum.map(fn _ ->
      Task.async(fn ->
        BankAccount.deposit(pid, 100)
        BankAccount.withdraw(pid, 50)
      end)
    end)

  Enum.each(tasks, &Task.await/1)

  assert BankAccount.balance(pid) == 1500
end
```

### Understanding Synchronization
Concurrency can lead to race conditions where the outcome depends on the timing of the processes. GenServer ensures that call is handled synchronously and cast is handled asynchronously but in the order they were sent, providing controlled concurrency.

Concurrency is a powerful feature of Elixir, but it requires understanding of processes, GenServer, and tasks to use effectively. Experimenting with these concepts in iEX and writing tests will reinforce your understanding.

Remember:

Processes are lightweight and can run concurrently.
GenServer provides a structure for client-server architecture.
Tasks enable parallel computations.
Synchronization is vital to prevent race conditions.
Explore more in the Elixir documentation, especially the guides on GenServer and Tasks.

## Exercises - Please do these in a separate Exercises Branch.

#### 1. Basic Operations
- Task: Start a BankAccount with an initial balance of 500, deposit 300, and withdraw 200. Check the balance at each step.
- Hint: Use BankAccount.start_link, BankAccount.deposit, BankAccount.withdraw, and BankAccount.balance.

#### 2. Concurrent Deposits
Task: Start a BankAccount with an initial balance of 1000. Spawn 5 processes to deposit 100 each concurrently. Check the final balance.
Hint: Use spawn or Task.async to perform the deposits concurrently.

#### 3. Concurrent Withdrawals
Task: Start a BankAccount with an initial balance of 1000. Spawn 5 processes to withdraw 100 each concurrently. Check the final balance.
Hint: Think about synchronization and how GenServer handles concurrent withdrawals.

#### 4. Mixed Concurrent Deposits and Withdrawals
Task: Start a BankAccount with an initial balance of 1000. Spawn 10 processes, 5 for depositing 100 each, and 5 for withdrawing 50 each. Check the final balance.
Hint: You can use tasks to manage the concurrent operations.

#### 5. Error Handling
Task: Start a BankAccount with an initial balance of 500. Try to withdraw 600 and handle the "Insufficient funds" error gracefully.
Hint: Pay attention to the return value when the withdrawal amount exceeds the balance.

#### 6. Writing Tests for Concurrency
Task: Write an ExUnit test that verifies the concurrent handling of deposits and withdrawals in the BankAccount module.
Hint: You can use Task.async and Task.await as shown in the previous explanation.

#### 7. Logger
Task: Explore the logged messages by performing various operations. Try concurrent deposits and withdrawals and observe the log messages.
Hint: Logger messages are used in the BankAccount module to log different operations. Use Logger.info and Logger.warn in iEX to see the messages.

####8. Challenge - Implement a Transfer Function
Task: Implement a function that transfers money between two different bank accounts. Ensure that the operation is atomic and handles concurrency correctly.
Hint: Think about how to ensure that the transfer operation is atomic and how GenServer can be leveraged to synchronize the operations.
^^Maybe lets add this to the TSBank app as a feature

## Additional Resources
[Elixir School: Concurrency](https://elixirschool.com/en/lessons/advanced/concurrency/)

[Elixir Getting Started: Processes](https://elixir-lang.org/getting-started/processes.html)

[Elixir Guides]: Tasks(https://elixir-lang.org/getting-started/mix-otp/task-and-gen-tcp.html)






