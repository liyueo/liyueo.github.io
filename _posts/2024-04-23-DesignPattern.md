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
    ThinBuilder thin;  
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

**外观模式**
特别是当一个系统很复杂时，系统提供给客户的是一个简单的对外接口，而把里面复杂的结构都封装了起来。客户只需使用这些简单接口就能使用这个系统，
比如，编译的过程包括：词法分析、语法分析、中间代码生成、机器码生成，，只需要一个类把这四个过程包装起来使用

```c++
class Scanner  
{  
public:  
    void Scan() { cout<<"词法分析"<<endl; }  
};  
class Parser  
{  
public:  
    void Parse() { cout<<"语法分析"<<endl; }  
};  
class GenMidCode  
{  
public:  
    void GenCode() { cout<<"产生中间代码"<<endl; }  
};  
class GenMachineCode  
{  
public:  
    void GenCode() { cout<<"产生机器码"<<endl;}  
};  
//高层接口  
class Compiler  
{  
public:  
    void Run()   
    {  
        Scanner scanner;  
        Parser parser;  
        GenMidCode genMidCode;  
        GenMachineCode genMacCode;  
        scanner.Scan();  
        parser.Parse();  
        genMidCode.GenCode();  
        genMacCode.GenCode();  
    }  
};

int main()  
{  
    Compiler compiler;  
    Console Compiler();  
    return 0;  
}  
```
特点：（1）它对客户屏蔽子系统组件，因而减少了客户处理的对象的数目并使得子系统使用起来更加方便。（2）它实现了子系统与客户之间的松耦合关系，而子系统内部的功能组件往往是紧耦合的。（3）如果应用需要，它并不限制它们使用子系统类。


**组合模式**
外观模式是组合在了一起。。
定义：DP书上给出的定义：将对象组合成树形结构以表示“部分-整体”的层次结构。组合使得用户对单个对象和组合对象的使用具有一致性。
注意两个字“树形”。这种树形结构在现实生活中随处可见，比如一个集团公司，它有一个母公司，下设很多家子公司。不管是母公司还是子公司，都有各自直属的财务部、人力资源部、销售部等。对于母公司来说，不论是子公司，还是直属的财务部、人力资源部，都是它的部门。整个公司的部门拓扑图就是一个树形结构。
UML图：
![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/f10e224d-0ed1-438c-8785-bcce5222a057)

```c++
// 双向链表的形式
class Company    
{  
public:  
    Company(string name) { m_name = name; }  
    virtual ~Company(){}  
    virtual void Add(Company *pCom){}  
    virtual void Show(int depth) {}  
protected:  
    string m_name;  
};  
//具体公司  
class ConcreteCompany : public Company    
{  
public:  
    ConcreteCompany(string name): Company(name) {}  
    virtual ~ConcreteCompany() {}  
    void Add(Company *pCom) { m_listCompany.push_back(pCom); } //位于树的中间，可以增加子树  
    void Show(int depth)  
    {  
        for(int i = 0;i < depth; i++)  
            cout<<"-";  
        cout<<m_name<<endl;  
        list<Company *>::iterator iter=m_listCompany.begin();  
        for(; iter != m_listCompany.end(); iter++) //显示下层结点  
            (*iter)->Show(depth + 2);  
    }  
private:  
    list<Company *> m_listCompany;  
};  
//具体的部门，财务部  
class FinanceDepartment : public Company   
{  
public:  
    FinanceDepartment(string name):Company(name){}  
    virtual ~FinanceDepartment() {}  
    virtual void Show(int depth) //只需显示，无限添加函数，因为已是叶结点  
    {  
        for(int i = 0; i < depth; i++)  
            cout<<"-";  
        cout<<m_name<<endl;  
    }  
};  
//具体的部门，人力资源部  
class HRDepartment :public Company    
{  
public:  
    HRDepartment(string name):Company(name){}  
    virtual ~HRDepartment() {}  
    virtual void Show(int depth) //只需显示，无限添加函数，因为已是叶结点  
    {  
        for(int i = 0; i < depth; i++)  
            cout<<"-";  
        cout<<m_name<<endl;  
    }  
};  
```

使用：
```c++
// 树形
int main()  
{  
    Company *root = new ConcreteCompany("总公司");  
    Company *leaf1=new FinanceDepartment("财务部");  
    Company *leaf2=new HRDepartment("人力资源部");  
    root->Add(leaf1);  
    root->Add(leaf2);  
  
    //分公司A  
    Company *mid1 = new ConcreteCompany("分公司A");  
    Company *leaf3=new FinanceDepartment("财务部");  
    Company *leaf4=new HRDepartment("人力资源部");  
    mid1->Add(leaf3);  
    mid1->Add(leaf4);  
    root->Add(mid1);  
    //分公司B  
    Company *mid2=new ConcreteCompany("分公司B");  
    FinanceDepartment *leaf5=new FinanceDepartment("财务部");  
    HRDepartment *leaf6=new HRDepartment("人力资源部");  
    mid2->Add(leaf5);  
    mid2->Add(leaf6);  
    root->Add(mid2);  
    root->Show(0);  
  
    delete leaf1; delete leaf2;  
    delete leaf3; delete leaf4;  
    delete leaf5; delete leaf6;   
    delete mid1; delete mid2;  
    delete root;  
    return 0;  
}  
```
上面的实现方式有缺点，就是内存的释放不好，需要客户自己动手，非常不方便。有待改进，比较好的做法是让ConcreteCompany类来释放。因为所有的指针都是存在ConcreteCompany类的链表中。C++的麻烦，没有垃圾回收机制。

## 7.代理模式
[DP]上的定义：为其他对象提供一种代理以控制对这个对象的访问。有四种常用的情况：（1）远程代理，（2）虚代理，（3）保护代理，（4）智能引用。本文主要介绍虚代理和智能引用两种情况。
考虑一个可以在文档中嵌入图形对象的文档编辑器。有些图形对象的创建开销很大。但是打开文档必须很迅速，因此我们在打开文档时应避免一次性创建所有开销很大的对象。这里就可以运用代理模式，在打开文档时，并不打开图形对象，而是打开图形对象的代理以替代真实的图形。待到真正需要打开图形时，仍由代理负责打开。

虚代理：
```c++
class Image  
{  
public:  
    Image(string name): m_imageName(name) {}  
    virtual ~Image() {}  
    virtual void Show() {}  
protected:  
    string m_imageName;  
};  
class BigImage: public Image  
{  
public:  
    BigImage(string name):Image(name) {}  
    ~BigImage() {}  
    void Show() { cout<<"Show big image : "<<m_imageName<<endl; }  
};  
class BigImageProxy: public Image  
{  
private:  
    BigImage *m_bigImage;  
public:  
    BigImageProxy(string name):Image(name),m_bigImage(0) {}  
    ~BigImageProxy() { delete m_bigImage; }  
    void Show()   
    {  
        if(m_bigImage == NULL)  
            m_bigImage = new BigImage(m_imageName);  
        m_bigImage->Show();  
    }  
};  
```
客户调用： 在调用的使用再打开，，在show里面创建的对象
```c++
int main()  
{  
    Image *image = new BigImageProxy("proxy.jpg"); //代理  
    image->Show(); //需要时由代理负责打开  
    delete image;  
    return 0;  
}
```

**还有代理服务器的情况**
以代理服务器为例进行代码编写，访问真正的服务器，需要通过代理服务器，代理服务器进行用户名密码校验，通过才允许访问真实服务器。
```c++

#include <bits/stdc++.h>

//抽象类，抽象的主题类
class AbstractServer {
public:
    virtual void Request() = 0;

    virtual ~AbstractServer() = default;
};

//真正主题类，具体提供服务的类
class RealServer : public AbstractServer {
    void Request() override {
        std::cout << "服务器启动..." << std::endl;
    }

};

//代理服务器，非真正的服务器，访问真正服务器必须通过代理服务器
class ProxyServer : public AbstractServer {
public:
    ProxyServer(const std::string &name, const std::string &pwd)
            : m_name(name), m_pwd(pwd) {
        m_pserver = std::make_shared<RealServer>();
    }

    // 和 真正主题类实现共同的接口，对外可以提供一致的接口！
    void Request() override {
        if (!CheckUser()) {
            std::cout << "用户名或者密码错误..." << std::endl;
            return;
        }
        std::cout << "校验用户名及密码成功..." << std::endl;
        PreRequest();//额外附加的操作

        m_pserver->Request();

        PostRequest();//额外附加的操作
    }

private:
    //访问服务器前 进行的动作，可以控制对真实主题类的访问
    bool CheckUser() {
        if ("admin" == m_name && "123456" == m_pwd) {
            return true;
        }
        return false;
    }

    //真正访问服务器前 进行的动作
    void PreRequest() {
        std::cout << "进入代理服务器..." << std::endl;
    }

    //访问服务器之后 进行的动作
    void PostRequest() {
        std::cout << "服务器访问完毕..." << std::endl;
    }


private:
    std::shared_ptr<AbstractServer> m_pserver;
    std::string m_name;
    std::string m_pwd;
};

//客户端 通过登录代理服务器 访问 真实服务器
int main(int argc, char *argv[]) {
    std::shared_ptr<AbstractServer> proxy = std::make_shared<ProxyServer>("admin", "123456");//登录代理服务器
    proxy->Request();//通过代理服务器 访问真正服务器

    return 0;
    //运行结果如下：
    //校验用户名及密码成功...
    //进入代理服务器...
    //服务器启动...
    //服务器访问完毕...
}
```

**下面介绍智能引用**
auto_ptr(c++11已经摒弃，并提出unique_ptr来代替)
 auto_ptr 类就是一个代理，客户只需操作auto_prt的对象，而不需要与被代理的指针pointee打交道。
 ```c++
template<class T>    
class auto_ptr {    
public:    
    explicit auto_ptr(T *p = 0): pointee(p) {}    
    auto_ptr(auto_ptr<T>& rhs): pointee(rhs.release()) {}    
    ~auto_ptr() { delete pointee; }    
    auto_ptr<T>& operator=(auto_ptr<T>& rhs)    
    {    
        if (this != &rhs) reset(rhs.release());    
        return *this;    
    }    
    T& operator*() const { return *pointee; }    
    T* operator->() const { return pointee; }    
    T* get() const { return pointee; }    
    T* release()    
    {    
        T *oldPointee = pointee;    
        pointee = 0;    
        return oldPointee;    
    }    
    void reset(T *p = 0)    
    {    
        if (pointee != p) {    
               delete pointee;    
               pointee = p;    
            }    
        }    
private:    
    T *pointee;    
};    
```
同理还有shared_ptr
```c++
template <typename T>  
class smart_ptr  
{  
public:  
    smart_ptr(T *p = 0): pointee(p), count(new size_t(1)) { }  //初始的计数值为1  
    smart_ptr(const smart_ptr &rhs): pointee(rhs.pointee), count(rhs.count) { ++*count; } //拷贝构造函数，计数加1  
    ~smart_ptr() { decr_count(); }              //析构，计数减1，减到0时进行垃圾回收，即释放空间  
    smart_ptr& operator= (const smart_ptr& rhs) //重载赋值操作符  
    {  
        //给自身赋值也对，因为如果自身赋值，计数器先减1，再加1，并未发生改变  
        ++*count;  
        decr_count();  
        pointee = rhs.pointee;  
        count = rhs.count;  
        return *this;  
    }    
    //重载箭头操作符和解引用操作符，未提供指针的检查  
    T *operator->() { return pointee; }  
    const T *operator->() const { return pointee; }  
    T &operator*() { return *pointee; }  
    const T &operator*() const { return *pointee; }  
    size_t get_refcount() { return *count; } //获得引用计数器值  
private:   
    T *pointee;       //实际指针，被代理    
    size_t *count;    //引用计数器  
    void decr_count() //计数器减1  
    {  
        if(--*count == 0)   
        {  
            delete pointee;  
            delete count;  
        }  
    }  
};  
```

## 9.享元模式（结构型模式）Flyweight Pattern
享元模式是一种结构型的设计模式，，它允许你在消耗少量内存的情况下支持大量的对象。
享元模式只有一个目的，将内存的消耗最小化，
其定义为：运用共享技术有效地支持大量细粒度的对象。
**用共享技术来有効地支持大量细粒度对象的复用。它通过共享已经存在的对象来大幅度减少需要创建的对象数量、避免大量相似类的开销，从而提高系统资源的利用率。**


## 10.桥接模式（结构性设计模式）
可将一个大类或一系列紧密相关的类拆分为抽象和实现两个独立的层次结构， 从而能在开发时分别使用。
举两个例子
1. 形状和颜色
![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/2bf75303-52e5-4803-94e2-63d0977996c7)

    可以分成
![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/fd536605-7f7c-4871-8099-f73bf267b0e4)

2. 计算机和操作系统
![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/83efd646-15da-4ffb-890c-f574d7f0e534)

```c++
//操作系统  
class OS  
{  
public:  
    virtual void InstallOS_Imp() {}  
};  
class WindowOS: public OS  
{  
public:  
    void InstallOS_Imp() { cout<<"安装Window操作系统"<<endl; }   
};  
class LinuxOS: public OS  
{  
public:  
    void InstallOS_Imp() { cout<<"安装Linux操作系统"<<endl; }   
};  
class UnixOS: public OS  
{  
public:  
    void InstallOS_Imp() { cout<<"安装Unix操作系统"<<endl; }   
};  
//计算机  
class Computer  
{  
public:  
    virtual void InstallOS(OS *os) {}  
};  
class DellComputer: public Computer  
{  
public:  
    void InstallOS(OS *os) { os->InstallOS_Imp(); }  
};  
class AppleComputer: public Computer  
{  
public:  
    void InstallOS(OS *os) { os->InstallOS_Imp(); }  
};  
class HPComputer: public Computer  
{  
public:  
    void InstallOS(OS *os) { os->InstallOS_Imp(); }  
};  
```
使用方法
```c++
int main()  
{  
    OS *os1 = new WindowOS();  
    OS *os2 = new LinuxOS();  
    Computer *computer1 = new AppleComputer();  
    computer1->InstallOS(os1);  
    computer1->InstallOS(os2);  
}  
```

## 11.装饰模式(结构型设计模式)
装饰模式：动态地给一个对象添加一些额外的职责。就增加功能来说，装饰模式相比生成子类更为灵活。**有时我们希望给某个对象而不是整个类添加一些功能**。比如有一个手机，允许你为手机添加特性，比如增加挂件、屏幕贴膜等。一种灵活的设计方式是，将手机嵌入到另一对象中，由这个对象完成特性的添加，我们称这个嵌入的对象为装饰。这个装饰与它所装饰的组件接口一致，因此它对使用该组件的客户透明。


手机类：
```c++
//公共抽象类  
class Phone  
{  
public:  
    Phone() {}  
    virtual ~Phone() {}  
    virtual void ShowDecorate() {}  
};  

//具体的手机类  
class iPhone : public Phone  
{  
private:  
    string m_name; //手机名称  
public:  
    iPhone(string name): m_name(name){}  
    ~iPhone() {}  
    void ShowDecorate() { cout<<m_name<<"的装饰"<<endl;}  
};  
//具体的手机类  
class NokiaPhone : public Phone  
{  
private:  
    string m_name;  
public:  
    NokiaPhone(string name): m_name(name){}  
    ~NokiaPhone() {}  
    void ShowDecorate() { cout<<m_name<<"的装饰"<<endl;}  
};  
```

装饰类
```c++
//装饰类  
class DecoratorPhone : public Phone  
{  
private:  
    Phone *m_phone;  //要装饰的手机  
public:  
    DecoratorPhone(Phone *phone): m_phone(phone) {}  
    virtual void ShowDecorate() { m_phone->ShowDecorate(); }  
};  
//具体的装饰类  
class DecoratorPhoneA : public DecoratorPhone  
{  
public:  
    DecoratorPhoneA(Phone *phone) : DecoratorPhone(phone) {}  
    void ShowDecorate() { DecoratorPhone::ShowDecorate(); AddDecorate(); }  
private:  
    void AddDecorate() { cout<<"增加挂件"<<endl; } //增加的装饰  
};  
//具体的装饰类  
class DecoratorPhoneB : public DecoratorPhone  
{  
public:  
    DecoratorPhoneB(Phone *phone) : DecoratorPhone(phone) {}  
    void ShowDecorate() { DecoratorPhone::ShowDecorate(); AddDecorate(); }  
private:  
    void AddDecorate() { cout<<"屏幕贴膜"<<endl; } //增加的装饰  
};  
```

调用,,只对某个对象进行装饰
```c++
int main()  
{  
    Phone *iphone = new NokiaPhone("6300");  
    Phone *dpa = new DecoratorPhoneA(iphone); //装饰，增加挂件  
    Phone *dpb = new DecoratorPhoneB(dpa);    //装饰，屏幕贴膜  
    dpb->ShowDecorate();  
    delete dpa;  
    delete dpb;  
    delete iphone;  
    return 0;  
}  
```

## 12.备忘录模式（行为型模式）
定义：在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可将该对象恢复到原先保存的状态
举个简单的例子，我们玩游戏时都会保存进度，所保存的进度以文件的形式存在。这样下次就可以继续玩，而不用从头开始。这里的进度其实就是游戏的内部状态，而这里的文件相当于是在游戏之外保存状态。这样，下次就可以从文件中读入保存的进度，从而恢复到原来的状态。这就是备忘录模式。

```c++
//需保存的信息  
class Memento    
{  
public:  
    int m_vitality; //生命值  
    int m_attack;   //进攻值  
    int m_defense;  //防守值  
public:  
    Memento(int vitality, int attack, int defense):   
      m_vitality(vitality),m_attack(attack),m_defense(defense){}  
    Memento& operator=(const Memento &memento)   
    {  
        m_vitality = memento.m_vitality;  
        m_attack = memento.m_attack;  
        m_defense = memento.m_defense;  
        return *this;  
    }  
};  
//游戏角色  
class GameRole    
{  
private:  
    int m_vitality;  
    int m_attack;  
    int m_defense;  
public:  
    GameRole(): m_vitality(100),m_attack(100),m_defense(100) {}  
    Memento Save()  //保存进度，只与Memento对象交互，并不牵涉到Caretake  
    {   
        Memento memento(m_vitality, m_attack, m_defense);  
        return memento;  
    }  
    void Load(Memento memento)  //载入进度，只与Memento对象交互，并不牵涉到Caretake  
    {  
        m_vitality = memento.m_vitality;  
        m_attack = memento.m_attack;   
        m_defense = memento.m_defense;  
    }  
    void Show() { cout<<"vitality : "<< m_vitality<<", attack : "<< m_attack<<", defense : "<< m_defense<<endl; }  
    void Attack() { m_vitality -= 10; m_attack -= 10;  m_defense -= 10; }  
};  
//保存的进度库  
class Caretake    
{  
public:  
    Caretake() {}  
    void Save(Memento menento) { m_vecMemento.push_back(menento); }  
    Memento Load(int state) { return m_vecMemento[state]; }  
private:  
    vector<Memento> m_vecMemento;  
};  
```
使用方法
```c++
//测试案例  
int main()  
{     
    Caretake caretake;  
    GameRole role;   
    role.Show();   //初始值  
    caretake.Save(role.Save()); //保存状态  
    role.Attack();     
    role.Show();  //进攻后  
    role.Load(caretake.Load(0)); //载入状态   
    role.Show();  //恢复到状态0  
    return 0;  
}  
```

## 13.中介者模式（行为型模式）
用一个中介对象来封装一系列的对象交互。中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。中介者模式的例子很多，大到联合国安理会，小到房屋中介，都扮演了中间者的角色，协调各方利益。

举个例子：
中介对象（房屋中介），，对象交互（租客，房东）

## 14.职责链模式
职责链模式：使多个对象都有机会处理请求，从而避免请求的发送者和接收者之前的耦合关系，将这些对象连成一条链，并沿着这条链传递请求，直到有一个对象处理它为止。

## 15.观察者模式(行为型模式)
定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。它还有两个别名，依赖(Dependents)，发布-订阅(Publish-Subsrcibe)。可以举个博客订阅的例子，当博主发表新文章的时候，即博主状态发生了改变，那些订阅的读者就会收到通知，然后进行相应的动作，比如去看文章，或者收藏起来。博主与读者之间存在种一对多的依赖关系。下面给出相应的UML图设计。
![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/722ca758-7ccc-4c1e-9994-1ea4a5a84f68)


## 16.状态模式（行为型模式)
状态模式：允许一个对象在其内部状态改变时改变它的行为。对象看起来似乎修改了它的类。它有两种使用情况：（1）一个对象的行为取决于它的状态, 并且它必须在运行时刻根据状态改变它的行为。（2）一个操作中含有庞大的多分支的条件语句，且这些分支依赖于该对象的状态。本文的例子为第一种情况，以战争为例，假设一场战争需经历四个阶段：前期、中期、后期、结束。当战争处于不同的阶段，战争的行为是不一样的，也就说战争的行为取决于所处的阶段，而且随着时间的推进是动态变化的。下面给出相应的UML图。

![image](https://github.com/liyueo/liyueo.github.io/assets/119725085/966473dc-08ed-4995-9c05-49173425fc8d)


参考：
    https://zhuanlan.zhihu.com/p/431714886
    https://refactoringguru.cn/design-patterns/state
    https://www.cnblogs.com/schips/p/common-design-pattern-about-cpp.html
