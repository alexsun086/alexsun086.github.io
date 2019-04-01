### How thread pool works in java
- A thread pool is a collection of pre-initialized threads. Generally the size of collection is fixed, but it is not mandatory. It facilitates the execution of N number of tasks using same threads. If thread are more tasks than threads, then tasks need to wait in a queue like structure (FIFO – First in first out).
- When any thread completes it’s execution, it can pickup a new task from queue and execute it. When all tasks are completed the threads remain active and wait for more tasks in thread pool.

### ThreadPoolExecutor
- Since Java 5, the Java concurrency API provides a mechanism Executor framework. This is around the Executor interface, its sub-interface ExecutorService, and the ThreadPoolExecutor class that implements both interfaces.
- ThreadPoolExecutor separates the task creation and its execution. With ThreadPoolExecutor, you only have to implement the Runnable objects and send them to the executor. It is responsible for their execution, instantiation, and running with necessary threads.

### How to create ThreadPoolExecutor
1. Fixed thread pool executor
````
ThreadPoolExecutor executor = (ThreadPoolExecutor) 
Executors.newFixedThreadPool(10);
````
2. Cached thread pool executor
DO NOT use this thread pool if tasks are long running. It can bring down the system if number of threads goes beyond what system can handle.
````
ThreadPoolExecutor executor = (ThreadPoolExecutor) 
Executors.newCachedThreadPool();
````
3. Scheduled thread pool executor
````
ThreadPoolExecutor executor = (ThreadPoolExecutor) 
Executors.newScheduledThreadPool(10);
````
4. Single thread pool executor 
````
ThreadPoolExecutor executor = (ThreadPoolExecutor) 
Executors.newSingleThreadExecutor();
````
5. Work stealing thread pool executor
````
ThreadPoolExecutor executor = (ThreadPoolExecutor) 
Executors.newWorkStealingPool(4);
````
### ThreadPoolExecutor Example
- Create Task
````
import java.util.concurrent.TimeUnit;
 
public class Task implements Runnable {
    private String name;
 
    public Task(String name) {
        this.name = name;
    }
 
    public String getName() {
        return name;
    }
 
    public void run() {
        try {
            Long duration = (long) (Math.random() * 10);
            System.out.println("Executing : " + name);
            TimeUnit.SECONDS.sleep(duration);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
````

- Execute tasks with thread pool executor
````
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;
 
public class ThreadPoolExample
{
    public static void main(String[] args)
    {
        ThreadPoolExecutor executor = (ThreadPoolExecutor) Executors.newFixedThreadPool(2);
         
        for (int i = 1; i <= 5; i++)
        {
            Task task = new Task("Task " + i);
            System.out.println("Created : " + task.getName());
 
            executor.execute(task);
        }
        executor.shutdown();
    }
}
````

Output:
````
Created : Task 1
Created : Task 2
Created : Task 3
Created : Task 4
Created : Task 5
Executing : Task 1
Executing : Task 2
Executing : Task 3
Executing : Task 4
Executing : Task 5
````
### ScheduledThreadPoolExecutor Example
Fixed thread pools or cached thread pools are good when you have to execute one unique task only once. 
When you need to execute a task, repeatedly N times, either N fixed number of times or infinitively after fixed delay, 
you should be using ScheduledThreadPoolExecutor.

ScheduledThreadPoolExecutor provides 4 methods which provide different capabilities to execute the tasks in repeated manner.

- ScheduledFuture schedule(Runnable command, long delay, TimeUnit unit) – Creates and executes a task that becomes enabled after the given delay.
- ScheduledFuture schedule(Callable callable, long delay, TimeUnit unit) – Creates and executes a ScheduledFuture that becomes enabled after the given delay.
- ScheduledFuture scheduleAtFixedRate(Runnable command, long initialDelay, long delay, TimeUnit unit) – Creates and executes a periodic action that becomes enabled first after the given initial delay, and subsequently with the given delay period. If any execution of this task takes longer than its period, then subsequent executions may start late, but will not concurrently execute.
- ScheduledFuture scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit) – Creates and executes a periodic action that becomes enabled first after the given initial delay, and subsequently with the given delay period. No matter how much time a long running task takes, there will be a fixed delay time gap between two executions.
````
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledThreadPoolExecutor;
import java.util.concurrent.TimeUnit;
 
public class ScheduledThreadPoolExecutorExample
{
    public static void main(String[] args)
    {
        ScheduledThreadPoolExecutor executor = (ScheduledThreadPoolExecutor) Executors.newScheduledThreadPool(2);
         
        Task task = new Task("Repeat Task");
        System.out.println("Created : " + task.getName());
         
        executor.scheduleWithFixedDelay(task, 2, 2, TimeUnit.SECONDS);
    }
}
 
class Task implements Runnable {
    private String name;
 
    public Task(String name) {
        this.name = name;
    }
 
    public String getName() {
        return name;
    }
 
    public void run() {
        System.out.println("Executing : " + name + ", Current Seconds : " + new Date().getSeconds());
    }
}
````
Output:
````
Created : Repeat Task
Executing : Repeat Task, Current Seconds : 36
Executing : Repeat Task, Current Seconds : 38
Executing : Repeat Task, Current Seconds : 41
Executing : Repeat Task, Current Seconds : 43
Executing : Repeat Task, Current Seconds : 45
Executing : Repeat Task, Current Seconds : 47
````

### Custom thread pool implementation in java

### Summary