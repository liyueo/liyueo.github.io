---
layout:     post
title:      手动实现一个智能指针
subtitle:   智能指针-20240409
date:       2024-04-17
author:     BY
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - Smart Pointer
    - HandWritten
---

# 手写智能指针

```
实现思路：
1. 实现一个计数器类，在智能指针类中声明一个计数器类的指针，负责引用计数
2. 实现拷贝构造函数和拷贝赋值
3. 实现移动构造函数和移动赋值
4. 实现一些常用接口、
5. 实现运算符重载 -> 和 *
6. 重载 bool()
```

```c++
#include <iostream>
#include <atomic>
using namespace std;

class sharedCount{
public:
    sharedCount():count_(1) {}
    
    void add(){++count_;}
    
    void minus(){--count_;}

    int get() const{
        return count_;
    }
private:
    atomic<int> count_;
};

template <class T>
class sharedPtr{
public:
    sharedPtr(T* ptr) : ptr_(ptr), ref_count_(new sharedCount){
        cout<<"sharedptr construct"<<endl;
    }

    sharedPtr() : ptr_(nullptr), ref_count_(new sharedCount){}
    ~sharedPtr(){
        clean();
    }
    // 拷贝构造和复制引用计数加一  
    // 拷贝构造函数和拷贝赋值
    sharedPtr(const sharedPtr& p){
        this->ptr_ = p.ptr_;
        this->ref_count_ = p.ref_count_;
        cout<<"copy construct"<<endl;
        ref_count_->add();
    }
    
    sharedPtr& operator=(const sharedPtr& p){
        clean();
        this->ptr_ = p.ptr_;
        this->ref_count_ = p.ref_count_;
        ref_count_->add();
        cout<<"copy assign"<<endl;
        // 返回值是引用类型，对指针解引用
        return *this;
    }

    // 上面是拷贝语义，下面是移动语义
    // 移动构造函数
    sharedPtr(sharedPtr&& p){
        this->ptr_ = p.ptr_;
        this->ref_count_ = p.ref_count_;
        p->ptr_ = nullptr;
        p->ref_count_ = nullptr;
        cout<<"move construct"<<endl;
    }

    //移动赋值函数
    sharedPtr& operator=(sharedPtr&& p){
        clean();
        this->ptr_ = p.ptr_;
        this->ref_count_ = p.ref_count_;
        p->ref_count_ = nullptr;
        p->ptr_ = nullptr;
        cout<<"move assign"<<endl;
        return *this;
    }


    // 上面是构造函数+赋值函数
    // 下面是常用的接口
    int use_count(){
        return ref_count_->get();
    }

    T* get(){
        return ptr_;
    }

    // 运算符重载
    // 指针有-> 自己定义的智能指针类对象没有->
    // 需要重载一下运算符
    T* operator->() const {
        return ptr_;
    }

    T& operator*() const {
        return *ptr_;
    }

    operator bool() const {
        return ptr_;
    }

private:
    void clean(){
        if(ref_count_){
            ref_count_->minus();
            if(ref_count_->get() == 0){
                if(ptr_)delete ptr_;
                delete ref_count_;
            }
        }
    }
    T* ptr_;
    sharedCount* ref_count_;
};

class A{
public:
    A(){cout<<"construct A"<<endl;};
    ~A(){cout<<"deconstruct A"<<endl;};
};

int main(){
    A* a = new A();
    // sharedPtr<A> *sp = new sharedPtr(a);
    sharedPtr<A> sp(a);
    {
        cout<<sp.use_count()<<endl;
        sharedPtr<A> sp2 = sp;
    }
}
```
