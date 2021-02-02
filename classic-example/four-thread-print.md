###  实现 四个线程分别打印 ABCD 5次 

> 在涉及线程的问题的时候，我们首要的思路就是实现变量的共享。利用变量来更新线程的状态。

* 首先定义共享的变量

```java
class MyDemo {
    private volatile int stamp = 1;
    private ReentrantLock lock = new ReentrantLock();
    Condition conditionA = lock.newCondition();
    Condition conditionB = lock.newCondition();
    Condition conditionC = lock.newCondition();
    Condition conditionD = lock.newCondition();
    public void loopA(int i) {
        lock.lock();
        try {
            if (stamp != 1) {
                conditionA.await();
            }
            System.out.print(Thread.currentThread().getName()+"\t");
            stamp = 2;
            conditionB.signal();
        } catch (Exception e) {

        } finally {
            lock.unlock();
        }
    }

    public void loopB(int i) {
        lock.lock();
        try {
            if (stamp != 2) {
                conditionB.await();
            }
            System.out.print(Thread.currentThread().getName()+"\t");
            stamp = 3;
            conditionC.signal();
        } catch (Exception e) {

        } finally {
            lock.unlock();
        }
    }


    public void loopC(int i) {
        lock.lock();
        try {
            if (stamp != 3) {
                conditionC.await();
            }
            System.out.print(Thread.currentThread().getName()+"\t");

            stamp = 4;
            conditionD.signal();
        } catch (Exception e) {

        } finally {
            lock.unlock();
        }
    }

    public void loopD(int i) {
        lock.lock();
        try {
            if (stamp != 4) {
                conditionD.await();
            }
            System.out.print(Thread.currentThread().getName()+"\t");
            stamp = 1;
            conditionA.signal();
        } catch (Exception e) {

        } finally {
            lock.unlock();
        }
    }
}
```

> 上面所展示的代码是我们分别实现打印ABCD的方法，利用 Condition 来约束代码执行的顺序。stamp 属于标志 而且他的类型是 volatile 的，这就是说明，可以实现 防止指令重排序 以及 内存的可见性。

* 定义4个线程，分别执行循环语句

```java
/**
 * @author :breakpoint/赵立刚
 * @date : 2020/07/14
 */
public class ThreadTest01 {
    /*
            分别打印 ABCD 循环五次
     */
    public static void main(String[] args) throws Exception {
        final MyDemo myDemo = new MyDemo();

        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 5; i++) {
                    myDemo.loopA(i);
                }
            }
        }, "A").start();
        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 5; i++) {
                    myDemo.loopB(i);
                }
            }
        }, "B").start();

        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 5; i++) {
                    myDemo.loopC(i);
                }
            }
        }, "C").start();

        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 5; i++) {
                    myDemo.loopD(i);
                }
            }
        }, "D").start();
        TimeUnit.SECONDS.sleep(2);
    }
}
```

> 分别创建线程ABCD 之后分别的打印出需要的结果的信息。