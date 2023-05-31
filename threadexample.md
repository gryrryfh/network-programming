```
import java.util.Random;

public class MyThread extends Thread implements Runnable {
    int id = -1;

    public MyThread(int id) {
        this.id = id;
    }

    public void run() {
        System.out.println(id + "empty thread working ...");
        Random r = new Random(System.currentTimeMillis());
        try {
            long s = r.nextInt(3000);
            Thread.sleep(s);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println(id + " empty thread working end...");
    }

    public static void main(String[] args) {
        System.out.println("Start main method.");
        for (int i = 0; i < 10; i++) {
            MyThread test = new MyThread(i);
            test.start();
        }
        
        System.out.println("End main method.");
    }
}
```
```
import java.util.Random;

public class ThreadTest extends Thread {
    int id = -1;

    public ThreadTest(int id) {
        this.id = id;
    }

    public void run() {
        System.out.println(id + "empty thread working ...");
        Random r = new Random(System.currentTimeMillis());
        try {
            long s = r.nextInt(3000);
            Thread.sleep(s);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println(id + " empty thread working end...");
    }

    public static void main(String[] args) {
        System.out.println("Start main method.");
        for (int i = 0; i < 10; i++) {
            ThreadTest test = new ThreadTest(i);
            test.start();
        }
        
        System.out.println("End main method.");
    }
}
```
```
public class ThreadExample implements Runnable {
    int threadNum;

    public ThreadExample(int threadNum) {
        this.threadNum = threadNum;
    }

    public static void main(String[] args) throws InterruptedException {
        for (int i = 0; i < Integer.parseInt(args[0]); i++) {
            ThreadExample f = new ThreadExample(i);
            Thread t = new Thread(f);
            t.start();
            Thread.sleep(200);
        }
        System.out.println("ho~");
    }

    @Override
    public void run() {
        long tStart = System.currentTimeMillis();
        for (int i = 0; i < 5; i++) {
            System.out.print("Thread " + ThreadExample.this.threadNum + ": ");
            for (int k = 0; k < i; k++)
                System.out.print("o");
            for (int j = 0; j < 5 - i; j++)
                System.out.print("x");
            System.out.println();
            try {
                Thread.sleep((int) (Math.random() * 1000));
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        long tEnd = System.currentTimeMillis();
        System.out.println("Thread " + ThreadExample.this.threadNum + " end " + "Process exited after " + (double) (tEnd - tStart) / 1000 + " seconds");
    }
}
```
```
import java.util.Random;

public class ThreadExample implements Runnable {
    int threadNum;

    public ThreadExample(int threadNum) {
        this.threadNum = threadNum;
    }

    public static void main(String[] args) throws InterruptedException {
        for (int i = 0; i < Integer.parseInt(args[0]); i++) {
            ThreadExample f = new ThreadExample(i);
            Thread t = new Thread(f);
            t.start();
            Thread.sleep(200);
        }
        System.out.println("ho~");
    }

    @Override
    public void run() {
        System.out.println(threadNum + " empty thread working ...");
        Random r = new Random(System.currentTimeMillis());
        try {
            long s = r.nextInt(3000);
            Thread.sleep(s);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        for (int i = 0; i < 5; i++) {
            System.out.print("Thread " + ThreadExample.this.threadNum + ": ");
            for (int k = 0; k < i; k++)
                System.out.print("o");
            for (int j = 0; j < 5 - i; j++)
                System.out.print("x");
            System.out.println();
            try {
                Thread.sleep((int) (Math.random() * 1000));
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        
        long tEnd = System.currentTimeMillis();
        System.out.println("Thread " + ThreadExample.this.threadNum + " end " + "Process exited after " + (double) (tEnd - tStart) / 1000 + " seconds");
    }
}
```
```
import java.util.Random;

public class ThreadExample implements Runnable {
    int threadNum;

    public ThreadExample(int threadNum) {
        this.threadNum = threadNum;
    }

    public static void main(String[] args) throws InterruptedException {
        for (int i = 0; i < Integer.parseInt(args[0]); i++) {
            ThreadExample f = new ThreadExample(i);
            Thread t = new Thread(f);
            t.start();
            Thread.sleep(200);
        }
        System.out.println("ho~");
    }

    @Override
    public void run() {
        System.out.println(threadNum + " empty thread working ...");
        Random r = new Random(System.currentTimeMillis());
        try {
            long s = r.nextInt(3000);
            Thread.sleep(s);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(threadNum + " empty thread working end...");

        long tStart = System.currentTimeMillis();
        for (int i = 0; i < 5; i++) {
            System.out.print("Thread " + ThreadExample.this.threadNum + ": ");
            for (int k = 0; k < i; k++)
                System.out.print("o");
            for (int j = 0; j < 5 - i; j++)
                System.out.print("x");
            System.out.println();
            try {
                Thread.sleep((int) (Math.random() * 1000));
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        long tEnd = System.currentTimeMillis();
        System.out.println("Thread " + ThreadExample.this.threadNum + " end " + "Process exited after " + (double) (tEnd - tStart) / 1000 + " seconds");
    }
}

```
