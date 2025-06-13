## Atomic Operations
An operation or a set of operations is considered atomic . if it appears to the rest of the system as if it occured at once. Single step ; all or nothing. 

## Problems with threads

Threads enter the critical sections without having knowledge of other threads at hand. Therefore non atomic operations 

## Syncronized is Monitor . 

Montor lock provides one thread at a time. 

```java
public static class InventoryCounter{
    private int items = 0;

    public syncronized void increment(){
        items++;
    }

    public syncronized void decrement(){
        items--;
    }  

    public syncronized int getItems(){
        return items;
    }
}

```
Generarally you want less code in in the critical sections. 


```java

public static class InventoryCounter{
    private int items = 0;
    Object lock = new Object();
    public void increment(){
        syncronized(this.lock){
            items++;
        }
    }

    public void decrement(){
        syncronized(this.lock){
            items--;
        }
    }  

    public int getItems(){
        return items;
    }
}

```
- What is a Critical Section .
- What is an Atomic operation .
- How to Guard critical section - Locking syncronization .
- 2 ways to use sync keyword .
- Syncronized is applied per object. Syncronised is used per lock.
- All assignments to objects are atomic
- assignment to 32 bit is atomic (byte, short, int)
- asignment to (long and double) are not atomic .
- java.util.concurrent.atomic brings atomic operations.


- lets look at a class called Metrics and determine where do we need syncronization

```java
class Main{

     public static class Metrics {
        private long count = 0;
        private double average = 0.0;

        // Syncronization needed . 
        public void addSample(long sample){
            double currentSum = average * count;
            count++;
            average = (currentSum+sample)/count;
        }

        // Assignment requires no syncs. 
        public double getAverage(){
            return average;
        }
    }
}
```
- We see that add sample is not atomic operation . So you need to add the syncronized keyword.
- Then the write to variable average is not atomic and the getAverage method therefore is not atomic. So you need to add volatile keyword inorder to ensure atomicity.

```java
class Main{

     public static class Metrics {
        private volatile long count = 0;
        private volatile double average = 0.0;

        // Syncronization needed . 
        public syncronized void addSample(long sample){
            double currentSum = average * count;
            count++;
            average = (currentSum+sample)/count;
        }

        // Assignment requires no syncs. 
        public double getAverage(){
            return average;
        }
    }
}
```
Summary 
- Assignement to primitive types without using long and double is atomic.
- Assignement to reference.
- AAssignment to double and long using volatile keyword.
- Knowledge of atomic operation is key to high performance.

## Race Condition and Data Race 

- When multiple threads are accessing the shared resource. Atleast one of them is modifying the resource. THe timing of the thread can cause incorrect result. 

### Data Race. 
Somtime the CPU executes staatements for performance and hardware utilizations. 
They will do so while maintaining the correctness of the code. 
Doing so can cause data race. 
```java
x = 1;
y = x+2;
```
Now these two may not be executed out of order, as one is dependent on other. 
```java
x++;
y++;
```
For these two can't be guarenteed to execute in order, so data race can happen . Here declaring the shared variable as volatile, will not cause compiler to take precidence of one variable to other, so variable's order of execution is maintained. 



