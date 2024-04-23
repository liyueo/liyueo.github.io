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

## 4.适配器模式（结构型模式）
c++ stl容器适配器使用了适配器模式，，栈（stack）、队列（queue）和优先队列（priority_queue），底层都是双端队列(deque)
**这里双端队列就扮演了适配器的角色。队列用到了它的后端插入，前端删除。而栈用到了它的后端插入，后端删除。**
UML图如下：
![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/bc1e3c65-cc5c-4bff-b7bd-128c289b65cf)
c++代码 如下；
```c++
class Deque{
public:
    void push_front(){};
    void pop_front(){};
    void push_back(){};
    void pop_back(){};
};

class Sequence{
public:
    virtual void pop() = 0;
    virtual void push() = 0;
};

class Stack : public Sequence{
public:
    void pop(){deque.pop_back();}
    void push(){deque.push_back();}
private:
    Deque deque;
};

class Queue : Sequence{
public:
    void pop(){deque.pop_front();}
    void push(){deque.push_back();};
private:
    Deque deque;
};
```

## 5.原型模式（创建型模式），模板方法模式（行为型模式）

原型模式：![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/3d71e8c0-38a2-46bd-868c-d38dbfca7570)
原型模式实现的关键就是深拷贝：
```c++
//父类  
class Resume  
{  
protected:  
    char *name;  
public:  
    Resume() {}  
    virtual ~Resume() {}  
    virtual Resume* Clone() { return NULL; }  
    virtual void Set(char *n) {}  
    virtual void Show() {}  
};

class ResumeA : public Resume  
{  
public:  
    ResumeA(const char *str);  //构造函数  
    ResumeA(const ResumeA &r); //拷贝构造函数  
    ~ResumeA();                //析构函数  
    ResumeA* Clone();          //克隆，关键所在  
    void Show();               //显示内容  
};  
ResumeA::ResumeA(const char *str)   
{  
    if(str == NULL) {  
        name = new char[1];   
        name[0] = '\0';   
    }  
    else {  
        name = new char[strlen(str)+1];  
        strcpy(name, str);  
    }  
}  
ResumeA::~ResumeA() { delete [] name;}  
ResumeA::ResumeA(const ResumeA &r) {  
    name = new char[strlen(r.name)+1];  
    strcpy(name, r.name);  
}  
ResumeA* ResumeA::Clone() {  
    return new ResumeA(*this);  
}  
void ResumeA::Show() {  
    cout<<"ResumeA name : "<<name<<endl;   
}

// 使用：
int main()  
{  
    Resume *r1 = new ResumeA("A");  
    Resume *r2 = new ResumeB("B");  
    Resume *r3 = r1->Clone();  
    Resume *r4 = r2->Clone();  
    r1->Show(); r2->Show();  
    //删除r1,r2  
    delete r1; delete r2;     
    r1 = r2 = NULL;  
    //深拷贝所以对r3,r4无影响  
    r3->Show(); r4->Show();  
    delete r3; delete r4;  
    r3 = r4 = NULL;  
} 
```
**模板方法模式**
父类构造一些方法，子类去实现这些方法
```c++
//简历  
class Resume  
{  
protected: //保护成员  
    virtual void SetPersonalInfo() {}  
    virtual void SetEducation() {}  
    virtual void SetWorkExp() {}  
public:  
    void FillResume()   
    {  
        SetPersonalInfo();  
        SetEducation();  
        SetWorkExp();  
    }  
};  
class ResumeA: public Resume  
{  
protected:  
    void SetPersonalInfo() { cout<<"A's PersonalInfo"<<endl; }  
    void SetEducation() { cout<<"A's Education"<<endl; }  
    void SetWorkExp() { cout<<"A's Work Experience"<<endl; }  
};  
class ResumeB: public Resume  
{  
protected:  
    void SetPersonalInfo() { cout<<"B's PersonalInfo"<<endl; }  
    void SetEducation() { cout<<"B's Education"<<endl; }  
    void SetWorkExp() { cout<<"B's Work Experience"<<endl; }  
};  
```

## 6.建造者模式（创建型模式）

建造者模式的定义将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示（DP）。
《大话设计模式》举了一个很好的例子——建造小人，一共需建造6个部分，头部、身体、左右手、左右脚。与工厂模式不同，建造者模式是在导向者的控制下一步一步构造产品的。建造小人就是在控制下一步步构造出来的。创建者模式可以能更精细的控制构建过程，从而能更精细的控制所得产品的内部结构。下面给出建造者模式的UML图，以建造小人为实例。

![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/59acf893-7206-4940-a31f-671e37b4d5ba)
对于客户来说，只需知道导向者就可以了，通过导向者，客户就能构造复杂的对象，而不需要知道具体的构造过程。下面给出小人例子的代码实现。
```c++
class Builder    
{  
public:  
    virtual void BuildHead() {}  
    virtual void BuildBody() {}  
    virtual void BuildLeftArm(){}  
    virtual void BuildRightArm() {}  
    virtual void BuildLeftLeg() {}  
    virtual void BuildRightLeg() {}  
};  
//构造瘦人  
class ThinBuilder : public Builder  
{  
public:  
    void BuildHead() { cout<<"build thin body"<<endl; }  
    void BuildBody() { cout<<"build thin head"<<endl; }  
    void BuildLeftArm() { cout<<"build thin leftarm"<<endl; }  
    void BuildRightArm() { cout<<"build thin rightarm"<<endl; }  
    void BuildLeftLeg() { cout<<"build thin leftleg"<<endl; }  
    void BuildRightLeg() { cout<<"build thin rightleg"<<endl; }  
};  
//构造胖人  
class FatBuilder : public Builder  
{  
public:  
    void BuildHead() { cout<<"build fat body"<<endl; }  
    void BuildBody() { cout<<"build fat head"<<endl; }  
    void BuildLeftArm() { cout<<"build fat leftarm"<<endl; }  
    void BuildRightArm() { cout<<"build fat rightarm"<<endl; }  
    void BuildLeftLeg() { cout<<"build fat leftleg"<<endl; }  
    void BuildRightLeg() { cout<<"build fat rightleg"<<endl; }  
};  
//构造的指挥官  
class Director    
{  
private:  
    Builder *m_pBuilder;  
public:  
    Director(Builder *builder) { m_pBuilder = builder; }  
    void Create(){  
        m_pBuilder->BuildHead();  
        m_pBuilder->BuildBody();  
        m_pBuilder->BuildLeftArm();  
        m_pBuilder->BuildRightArm();  
        m_pBuilder->BuildLeftLeg();  
        m_pBuilder->BuildRightLeg();  
    }  
};

```
```c++
//使用方法
int main()
{
{
    FatBuilder thin;  
    Director director(&thin);  
    director.Create();  
}
    
{
    FatBuilder thin;  
    Director director(&thin);  
    director.Create();  
}
    return 0;
}  
```

## 7.外观模式（结构性模式）、组合模式（结构性模式）
