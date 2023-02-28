---
布局：图案
标题：监视器
文件夹：监视器
永久链接：/patterns/monitor/
类别：并发
语言：英文
标签：
- 表现
---

## 意图
监视器模式用于创建线程安全的对象并防止并发应用程序中线程之间的冲突。

## 解释

通俗地说

> 监视器模式用于强制对数据进行单线程访问。 一次只允许一个线程执行监视器对象中的代码。

维基百科说

> 在并发编程（也称为并行编程）中，监视器是一种同步构造，它允许线程具有互斥和等待（阻塞）特定条件变为假的能力。 监视器也有一种机制，用于向其他线程发送信号，表明它们的条件已得到满足。

**编程示例**

考虑有一家银行使用 transfer method 将钱从一个帐户转移到另一个帐户。 它是“同步的”意味着只有一个线程可以访问此方法，因为如果多个线程访问它并在同一时间将资金从一个帐户转移到另一个帐户余额发生变化！
 
```
class Bank {

     private int[] accounts;
     Logger logger;
 
     public Bank(int accountNum, int baseAmount, Logger logger) {
         this.logger = logger;
         accounts = new int[accountNum];
         Arrays.fill(accounts, baseAmount);
     }
 
     public synchronized void transfer(int accountA, int accountB, int amount) {
         if (accounts[accountA] >= amount) {
             accounts[accountB] += amount;
             accounts[accountA] -= amount;
             logger.info("Transferred from account :" + accountA + " to account :" + accountB + " , amount :" + amount + " . balance :" + getBalance());
         }
     }
```

getBalance 始终返回总金额，每次转账后总金额应相同

```
     private synchronized int getBalance() {
         int balance = 0;
         for (int account : accounts) {
             balance += account;
         }
         return balance;
     }
 }
```

## 类图
![alt text](./etc/monitor.urm.png "Monitor class diagram")

## 适用性
在以下情况下使用 Monitor 模式

* 我们有一个共享资源并且有临界区。
* 你想创建线程安全的对象。
* 你想在高级编程语言中实现互斥。
