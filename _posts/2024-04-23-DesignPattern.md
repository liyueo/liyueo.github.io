---
layout:     post
title:      设计模式
subtitle:   设计模式-202404023
date:       2024-04-23
author:     BY
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - DesignPattern
    - summary
---

# 一、 设计模式分类

总体来说设计模式分为三大类

创建型模式，共五种：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。

结构型模式，共七种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。

行为型模式，共十一种：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

其实还有两类：并发型模式和线程池模式。

# 二、设计模式原则
总原则： 开闭原则：在程序需要进行拓展的时候，不能去修改原有的代码，而是要扩展原有代码

1. 单一职责原则：
   不存在多余一个导致类变更的原因，每个类应该实现单一的职责，如若不然，应该把类拆分
2. 里氏替换原则
   任何基类可以出现的地方，子类一定可以出现。只有当衍生类替换掉基类，软件单位的功能不受影响的时候，基类才能真正的被复用
3. 依赖倒置原则
   具体内容：面向接口编程，依赖于抽象而不依赖于具体。写代码时用到具体类时，不与具体类交互，而与具体类的上层接口交互。
4. 接口隔离原则
   每个接口中不存在子类用不到却必须实现的方法，如果不然，就要将接口拆分。使用多个隔离的接口，比使用单个接口（多个接口方法集合到一个的接口）要好。
5. 迪米特法则（最少知道原则）
   个类对自己依赖的类知道的越少越好。
6. 合成复用原则
   尽量首先使用合成/聚合的方式，而不是使用继承。

## 1. 工厂模式

### 1.1 简单工厂模式
它的主要特点是需要在工厂类中做判断，从而创造相应的产品。当增加新的产品时，就需要修改工厂类。
举个例子;
**有一家生产处理器核的厂家，它只有一个工厂，能够生产两种型号的处理器核。客户需要什么样的处理器核，一定要显示地告诉生产工厂。下面给出一种实现方案。**
例如：
```c++
enum CTYPE {COREA, COREB};     
class SingleCore    
{    
public:    
    virtual void Show() = 0;  
};    
//单核A    
class SingleCoreA: public SingleCore    
{    
public:    
    void Show() { cout<<"SingleCore A"<<endl; }    
};    
//单核B    
class SingleCoreB: public SingleCore    
{    
public:    
    void Show() { cout<<"SingleCore B"<<endl; }    
};    
//唯一的工厂，可以生产两种型号的处理器核，在内部判断    
class Factory    
{    
public:     
    SingleCore* CreateSingleCore(enum CTYPE ctype)    
    {    
        if(ctype == COREA) //工厂内部判断    
            return new SingleCoreA(); //生产核A    
        else if(ctype == COREB)    
            return new SingleCoreB(); //生产核B    
        else    
            return NULL;    
    }    
}; 
```
缺点： 要增加新的类型C的时候，，需要修改工厂类，违反了 开闭原则
**于是有了工厂方法模式**

###.1.2 工厂方法模式
所谓工厂方法模式，是指定义一个用于创建对象的接口，让子类决定实例化哪一个类。Factory Method使一个类的实例化延迟到其子类。
举个例子：
```c++
class SingleCore    
{    
public:    
    virtual void Show() = 0;  
};    
//单核A    
class SingleCoreA: public SingleCore    
{    
public:    
    void Show() { cout<<"SingleCore A"<<endl; }    
};    
//单核B    
class SingleCoreB: public SingleCore    
{    
public:    
    void Show() { cout<<"SingleCore B"<<endl; }    
};    
class Factory    
{    
public:    
    virtual SingleCore* CreateSingleCore() = 0;  
};    
//生产A核的工厂    
class FactoryA: public Factory    
{    
public:    
    SingleCoreA* CreateSingleCore() { return new SingleCoreA; }    
};    
//生产B核的工厂    
class FactoryB: public Factory    
{    
public:    
    SingleCoreB* CreateSingleCore() { return new SingleCoreB; }    
}; 
```
优化了简单工厂模式，但是每增加一个产品就要增加一个类。

缺点： 这家公司的技术不断进步，不仅可以生产单核处理器，也能生产多核处理器。现在简单工厂模式和工厂方法模式都鞭长莫及。抽象工厂模式登场了。

### 1.3 抽象工厂模式
```c++
//单核    
class SingleCore     
{    
public:    
    virtual void Show() = 0;  
};    
class SingleCoreA: public SingleCore      
{    
public:    
    void Show() { cout<<"Single Core A"<<endl; }    
};    
class SingleCoreB :public SingleCore    
{    
public:    
    void Show() { cout<<"Single Core B"<<endl; }    
};    
//多核    
class MultiCore      
{    
public:    
    virtual void Show() = 0;  
};    
class MultiCoreA : public MultiCore      
{    
public:    
    void Show() { cout<<"Multi Core A"<<endl; }    
    
};    
class MultiCoreB : public MultiCore      
{    
public:    
    void Show() { cout<<"Multi Core B"<<endl; }    
};    
//工厂    
class CoreFactory      
{    
public:    
    virtual SingleCore* CreateSingleCore() = 0;  
    virtual MultiCore* CreateMultiCore() = 0;  
};    
//工厂A，专门用来生产A型号的处理器    
class FactoryA :public CoreFactory    
{    
public:    
    SingleCore* CreateSingleCore() { return new SingleCoreA(); }    
    MultiCore* CreateMultiCore() { return new MultiCoreA(); }    
};    
//工厂B，专门用来生产B型号的处理器    
class FactoryB : public CoreFactory    
{    
public:    
    SingleCore* CreateSingleCore() { return new SingleCoreB(); }    
    MultiCore* CreateMultiCore() { return new MultiCoreB(); }    
}; 
```

![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/4b3c5b4a-78a5-4ef4-9ba7-01db4665a748)
**简单工厂UML图**
![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/84299da8-b89e-4f27-9526-bca22f7e7fd7)
**工厂方法UML图**
![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/0dc6b735-0937-4de1-b4ab-c096c2e47ce1)
**抽象工厂UML图**


## 2. 单例模式

一个类只能由一个对象，不提供创建对象的接口，并且提供一个可以访问它的全局接口，分为懒汉模式和饿汉模式
### 2.1 懒汉模式
什么时候用到，什么时候去实例化对象

```c++
class Singleton{
public:
    static Singleton* getInstance();
    ~Singleton();

private:
    Singleton(){};
    Singleton(const Singleton& obj) = delete;
    Singleton& operator=(const Singleton& obj) = delete;

    static Singleton* instance; // 静态成员，不属于任何对象
};

Singleton* Singleton::instance = nullptr;

Singleton* Singleton::getInstance(){
    if(instance == nullptr){
        instance = new Singleton();
    }
    return instance;
}
```
### 2.2饿汉模式
创建的时候直接实例化
```c++
class Singleton{
public:
    static Singleton* getInstance();
    ~Singleton();

private:
    Singleton(){};
    Singleton(const Singleton& obj) = delete;
    Singleton& operator=(const Singleton& obj) = delete;

    static Singleton* instance; // 静态成员，不属于任何对象
};

Singleton* Singleton::instance = new Singleton();

Singleton* Singleton::getInstance(){
    return instance;
}
```

## 3. 策略模式（行为型模式）
策略模式是指定义一系列的算法，把它们一个个封装起来，并且使它们可相互替换。本模式使得算法可独立于使用它的客户而变化。也就是说这些算法所完成的功能一样，对外的接口一样，只是各自实现上存在差异。
例如： 页面置换算法，有先进先出，LRU，随机替换
![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/05c3709c-236f-4d0a-9c57-f22e27a6b099)
ReplaceAlgorithm是一个抽象类，定义了算法的接口，有三个类继承自这个抽象类，也就是具体的算法实现。Cache类中需要使用替换算法，因此维护了一个 ReplaceAlgorithm的对象。这个UML图的结构就是策略模式的典型结构。
```c++
class ReplaceAlgorithm{
public:
    virtual void Replace() = 0;
};

class LRU : public ReplaceAlgorithm{
public:
    void Replace(){
        cout<<"LRU"<<endl;
    }
};

class FIFO: public ReplaceAlgorithm{  
public:  
    void Replace(){
        cout<<"First in First out replace algorithm"<<endl;
    }  
};

class Random : public ReplaceAlgorithm  {  
public:  
    void Replace(){
        cout<<"Random replace algorithm"<<endl;
    }  
};  

```
接着给出cache的实现
```c++
class Cache{
public:
    Cache(ReplaceAlgorithm* ra){
        m_ra = ra;
    }
    ~Cache(){
        delete m_ra;
    }
    void Replace(){
        m_ra->Replace();
    }
private:
    ReplaceAlgorithm *m_ra;
};

int main()  
{  
    Cache cache(new LRU_ReplaceAlgorithm()); //暴露了算法的定义  
    cache.Replace();  
    return 0;  
}  
```

模板实现
```c++
template <class RA>  
class Cache  
{  
private:  
    RA m_ra;  
public:  
    Cache() { }  
    ~Cache() { }  
    void Replace() { m_ra.Replace(); }  
};

int main()  
{  
    Cache<Random_ReplaceAlgorithm> cache; //模板实参  
    cache.Replace();  
    return 0;  
}  
```

## 4.适配器模式
