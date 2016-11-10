### Deadlock
***
* 死锁停止的截图
<img src="https://raw.githubusercontent.com/liaohl25/MarkdownImg/master/res/Deadlock.png" width="226." alt="configure"/>

可以看到程序在跑第11遍的时候停止了。


* 死锁产生的条件

	产生死锁有四个条件：

	1. 互斥条件：一个资源每次只能被一个进程使用,不能共享
	2. 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放，等待该资源的释放
	3. 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺，只能在进程完成任务后自动释放
	4. 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系

</b>

* 对实验中程序产生死锁的解释

	如下是实验中使用到的代码：
	
		package thread;

		class A {
			synchronized void methodA(B b) {
				b.last();
			}
			synchronized void last() {
				System.out.println("Inside A.last()");
			}
		} 

		class B {
			synchronized void methodB(A a) {
				a.last();
			}
			synchronized void last() {
				System.out.println("Inside B.last()");
			}
		}

		class Deadlock implements Runnable{
			A a = new A();
			B b = new B();
	
			Deadlock(){
				Thread t = new Thread(this);
				int count = 20000;
		
				t.start();
				while(count-- > 0);
				a.methodA(b);
			}
			public void run(){
				b.methodB(a);
			}
			public static void main(String args[]){
				new Deadlock();
			}
		}

	
    我们在 java 文件中定义了两个类 A 和 B , A 对象执行的任务是让类B的对象执行 last() 函数，B 对象执行的任务是让类A的对象执行 last() 函数。然而这两个任务和两个 last() 函数都是被关键字 synchronized 修饰的，那么同一时刻只能执行其中的一个任务。
	
	程序开始就执行 main 函数，里面直接调用 Deadlock() 函数。来看Deadlock() 函数，简单地声明变量后，执行 t.start() 语句，此时就会执行 run() 函数，执行 a.methodA(b) 语句。但同时 deadlock() 中，会继续执行 count-- 语句，然后执行 b.methodB(a)。前面我们知道这两个语句同一时刻只能执行一条，如果 count 的值设置成合适的值，那么两条语句会同时执行，如果其中 a 想要 b 执行 last() 的同时 b 也想要 a 执行 last() ，此时就会发生死锁，因为我们不知道该让谁先执行任务。 