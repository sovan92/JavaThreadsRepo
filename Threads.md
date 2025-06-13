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
