### Execution Mechanism
Both interfaces are designed to represent a task that can be executed by multiple threads. Runnable tasks can be run using the Thread class or ExecutorService whereas Callables can be run only using the latter.

### Return Values
#### Runnable

```
public interface Runnable {
    public void run();
}

public class EventLoggingTask implements  Runnable{
    private Logger logger
      = LoggerFactory.getLogger(EventLoggingTask.class);
 
    @Override
    public void run() {
        logger.info("Message");
    }
}

public void executeTask() {
    executorService = Executors.newSingleThreadExecutor();
    Future future = executorService.submit(new EventLoggingTask());
    executorService.shutdown();
}
```
In this case, the Future object will not hold any value.

#### Callable
```
public interface Callable<V> {
    V call() throws Exception;
}

public class FactorialTask implements Callable<Integer> {
    int number;
 
    // standard constructors
 
    public Integer call() throws InvalidParamaterException {
        int fact = 1;
        // ...
        for(int count = number; count > 1; count--) {
            fact = fact * count;
        }
 
        return fact;
    }
}

@Test
public void whenTaskSubmitted_ThenFutureResultObtained(){
    FactorialTask task = new FactorialTask(5);
    Future<Integer> future = executorService.submit(task);
  
    assertEquals(120, future.get().intValue());
}
```
The result of call() method is returned within a Future object.

### Exception Handling
#### Runnable
Since the method signature does not have the “throws” clause specified, there is no way to propagate further checked exceptions.

#### Callable
Callable’s call() method contains “throws Exception” clause so we can easily propagate checked exceptions further:
```
public class FactorialTask implements Callable<Integer> {
    // ...
    public Integer call() throws InvalidParamaterException {
 
        if(number < 0) {
            throw new InvalidParamaterException("Number should be positive");
        }
    // ...
    }
}
```

In case of running a Callable using an ExecutorService, the exceptions are collected in the Future object, which can be checked by making a call to the Future.get() method. This will throw an ExecutionException – which wraps the original exception:

```
@Test(expected = ExecutionException.class)
public void whenException_ThenCallableThrowsIt() {
  
    FactorialCallableTask task = new FactorialCallableTask(-5);
    Future<Integer> future = executorService.submit(task);
    Integer result = future.get().intValue();
}
```

If we don’t make the call to the get() method of Future class – then the exception thrown by call() method will not be reported back, and the task will still be marked as completed:
```
@Test
public void whenException_ThenCallableDoesntThrowsItIfGetIsNotCalled(){
    FactorialCallableTask task = new FactorialCallableTask(-5);
    Future<Integer> future = executorService.submit(task);
  
    assertEquals(false, future.isDone());
}
```