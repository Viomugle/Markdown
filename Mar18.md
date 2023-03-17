---
marp: true
theme: default
paginate: false
size: 16:9
---
<!-- _class: lead -->
#  <!-- fit --> Insider the Qt Multithread Mechanism
#  <!-- fit --> and What Trapped Me
```
王冠林
Mar.18 2023
```
---
# Multithread by QThread
- Which thread
- Cross thread operation
- Summary
---
## Which Thread
>It is important to remember that a QThread instance lives in the **old thread** that instantiated it, **not in the new thread** that calls run(). This means that all of QThread's queued slots and invoked methods will execute **in the old thread**. Thus, a developer who wishes to invoke slots in the new thread must use the worker-object approach; new slots should not be implemented directly into a subclassed QThread.
---
```cpp
#include <QThread>
class MyThread:public QThread
{
    MyThread();
    ~MyThread();
    void run() override;//in the new thread;
    void doSomething();//in the old thread;
    public signals:
        void MySignal();
    pulic slots:
        void MySlot();//in the old thread;
}
void Mythread::run()
{
    connect(this,SIGNAL(MySignal),this,SLOT(MySolt));//MySlot will run in the old thread;
    connect(this,SIGNAL(MySignal),this,[](){
        //do something
    });//lambda express will run in the new thread;
}
void MyThread::doSomething()
{
    //doSomething() still run in the old thread;
}
```
---
## Cross Thread Operation
>If a member variable is **accessed from both functions**, then the variable is accessed from **two different threads**. Check that it is safe to do so.
---
```cpp
#include <QThread>
#include <QTimer>
class ThreadwithTimer:public QThread
{
    ThreadwithTimer();
    ~ThreadwithTimer();
public:
    QTimer TimerInOld; //Qtimer in old thread
    QTimer pTimerInOld; //Qtimer pointer in old thread;
    void run() override;
    void doSomething();
}
```
---
```cpp
void ThreadwithTimer::run()
{
    QTimer TimerInNew();//QTimer in new thread;
    pTimerInOld=new QTimer();//Wrong,QTimer cannnot access in another thread;
    TimerinOld.start();//Wrong,QTimer cannnot access in another thread;
    TimerInNew.start();//Correct
    pTimerInOlder.start();//Wrong
    doSomething();//switch to the old thread;
}
void ThreadwithTimer::doSomething()
{
    TimerInOld.stop();//Correct,access in old thread;
    pTimerInOlder.stop();//Wrong
    TimerInNew.stop();//Wrong
}
```
---
<br>

<center> QThread instance lives in the thread that instantiated it to manage other thread by calling run(); </center>
<center>If you want to deal events by multithread, you'll need to write very long lambda expresses in run(), which is conflicted with OOP.
</center>

---
# Multithread by moveToThread
- Which thread
- Summary
---
## Which Thread

```cpp
class Controll:public Object
{
    Controll();
    ~Controll();
    MyObject MemObj;
    QThread MemThread;
    signals:
    void MemSignal()；
}
```
---
<br>

```cpp
class MyObject:public QObject
{
    MyObject();//Always in old thread;
    ~MyObject();//Up to how this function is called
public:
    void MemFun();
public slots:
    void MemSlot();//Up to how this function is called
}
Controll::Controll()
{
    MenObj.MenFun();//In old thread;
    MemObj.moveToThread(&MemThread);
    connect(this,SIGNAL(MenSignal),&MenObj,SLOT(MenSlot));//MemSlot will in the new thread;
    MemObj.MemSlot();//if directly called,MenSlot will in the old thread;
}
```
---
<br>


- Before moveToThread, all methods of subclasses run in the main thread, whether they are called directly or through signals and slots.
- After moveToThread, the method called by the subclass through the signal and slot system runs in the sub-thread; the direct call is also runnable, and all the work brought by it takes place in the main process.

---
<br>


- After moveToThread, whether to call start has no effect on which thread the method is in. It just start to listen evetloop.
- Calling a static function in a sub-thread will run in a temporary thread that is neither a sub-thread nor the main thread, and the new thread will be destroyed after running.

---
## Summary
<br>

<center>
Multithread by moveToThread method is easy to understand and not conflicted with OOP, and in this method we can hide the operation of the thread behind the scenes. But the problem of cross thread operation is still existing there so it's better to accomplish the task in the child thread's slots, and control the whole process by the signals of the main thread.
</center>

---

<!-- footer: <font size="3px"><font color="black">Copyright to 王冠林 and all right reserved.</front></font> -->
<!-- _class: lead -->
# <!-- fit --> Vielen Dank! 