# process (进程)& thread 线程

## 一、进程与线程的概念：

​	   **程序**：	  是指令和数据的有序集合，本身没有任何运行的含义，是一个静态的概念；

**process**：	  执行程序的一次过程，是一个动态的概念，是系统资源分配的单位；

  **thread**：	  cpu调度和执行的最**小**单位

线程的实现

### 1、extends Threadnbvn

 以及 callable

**1.1、方法的定义**

start()方法在java.lang.Thread类中定义；而，run()方法在java.lang.Runnable接口中定义，必须在实现类中重写。

**1.2、新线程创建**

当程序调用start()方法时，会创建一个新线程，然后执行run()方法。但是如果我们直接调用run()方法，则不会创建新的线程，run()方法将作为当前调用线程本身的常规方法调用执行，并且不会发生多线程。

### 2、 implements runnable

new Thread(new TestThread()).start();

**注意**：

<u>invoke方法前必须实例化线程对象，否则会报异常：NullPointerException</u>

<u>线程开启并不一定执行，由cpu来调度执行</u>

观测线程的状态

Thread.state state = thread.getState();

线程的状态可以用enum获取

eg: 

Thread.State.NEW						    <u>==》尚未启动的线程处于此状态</u>

Thread.State.RUNNABLE   	 	    <u>==》在JAVA虚拟机中执行的线程处于此状态</u>

Thread.State.BLOCKED		   	    <u>==》被阻塞等待监视器锁定的线程处于此状态</u>

Thread.State.WAITING			 	    <u>==》正在等待另一个线程执行特定动作的线程处于此状态</u>

Thread.State.TIMED_WAITIND 	  <u>==》正在等待另一个线程执行特定动作达到等待时间的线程处于此状态</u>

Thread.State.TERMINATED   		  <u>==》已退出的线程处于此状态</u>

## 线程同步

1.给方法加锁**<u>synchronized</u>** public **synchronized** void buy(){

​	if(ticketNums<=0){

​		flag=false;

​		return;

​	}

​	*模拟延时*

​	Thread.sleep(1000) ==>throws InterruptedException

​	sout.(Thread.currentThread().getName()+"买到了第"+ticketNums+"号票");

2.在重写的run方法里面加上synchronized块

synchronized(**account**){   //accout ===》<u>*给多个线程要操作的共享资源对象加把锁*</u>

//code……

}

## 避免死锁

##### **互斥条件：一个资源每次只能被一个线程使用**

##### **请求与保持条件：一个线程因请求资源而阻塞时，对已获得的资源保持不放**

##### **不剥夺条件：线程已获得的资源，在未使用完之前，不能强行剥夺**

##### **循环等待条件：若干线程之间形成一种头尾相接的循环等待资源关系**

注意：再次强调！！Thread.sleep不会释放锁