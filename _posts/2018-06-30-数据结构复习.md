---
title: 数据结构复习
comment: true
copyright: true
date: 2018-06-30 10:10:49
categories: 学科复习
tags:  数据结构
password: xkfx
---

由于临近夏令营，所以将之前的知识整理一下，好面试和笔试。本文是根据《数据结构--使用c++语言描述(第二版)》来进行复习的。  

<!--more-->

# 基础知识  

## 算法分析的基本方法  

### 渐进时间复杂度  

常见的渐进时间复杂度有`O(1)<O(log2n)<O(n)<O(nlog2n)<O(n^2)<O(n^3)`。  


# 线性表  

## 线性表ADT  

```cpp
#include<iostream> 

template <class T>
class LinearList
{
public:
    virtual bool IsEmpty();
    virtual int Length() const = 0;
    virtual bool Find(int i, T&x) const = 0;
    virtual int Search(T x) const = 0;
    virtual bool Insert(int i, T x) = 0;
    virtual bool Delete(int i) = 0;
    virtual bool Update(int i, T x) = 0;
    virtual void Output(ostream& out) const = 0;
protected:
    int n;                       //线性表的长度
};
```

## 线性表的顺序表示(数组)

```cpp
#include "linearList.h"
template <class T>
class SeqList :public LinearList<T>
{
public:
    SeqList(int mSize);
    ~SeqList() { delete[] elements };
    bool IsEmpty()const;
    int Length()const;
    bool Find(int i, T&x)const;
    int Search(T x)const;
    bool Insert(int i, T x);
    bool Delete(int i);
    bool Update(int i, T x);
    void Output(ostream & out)const;
private:
    int maxLength;                        //顺序表的最大长度
    T * elements;                         //动态一维数组的指针
};

template<class T>
SeqList<T>::SeqList(int mSize)
{
    maxLength=mSize;
    elements=new T[maxLength];            //动态分配顺序表的存储空间
    n=0;
}

template <class T>
bool SeqList<T>::IsEmpty()const                     //不改变顺序表里面的元素
{
    return n == 0;
}

template <class T>
int SeqList<T>::Length()const
{
    return n;
}

template <class T>
bool SeqList<T>::Find(int i,T& x)const
{
    if(i<0||i>n-1){
        cout<<"Out of Bounds"<<endl;
        return false;
    }
    x=elements[i];
    return true;
}

template <class T>
int SeqList<T>::Search(T x)const
{
    for(int j=0;j<n;j++)
        if(elements[j] == x)
            return j;
    return -1;
}

template <class T>
bool SeqList<T>::Insert(int i,T x)
{
    if(i<-1||i>n-1){
        cout<<"Out of Bounds"<<endl;
        return false;
    }

    if(n==maxLength){
        cout<<"OverFlow"<<endl;
        return false;
    }
    for(int j=n-1;j>i;j--){
        elements[j+1]=elements[j];
    }
    elements[i+1]=x;
    n++;
    return true;
}

template <class T>
bool SeqList<T>::Delete(int i)
{
    if(!n){
        cout<<"UnderFlow"<<endl;
        return false;
    }
    if(i<0||i>n-1){
        cout<<"Out of Bounds"<<endl;
        return false;
    }
    for(int j=i+1;j<n;j++){
        elements[j-1]=elements[j];
    }
    n--;
    return true;
}

template <class T>
bool SeqList<T>::Update(int i,T x)
{
    if(i<0||i>n-1){
        cout<<"Out of Bounds"<<endl;
        return false;
    }
    elements[i]=x;
    return true;
}

template <class T>
void SeqList<T>::Output(ostream &out)const
{
    for(int i =0;i<n;i++){
        out<<elements[i]<<' ';
    }
    out<<endl;
}
```
顺序表可以随机存取表中元素，但是插入和删除时效率极低，时间主要消耗在移动元素上，移动元素的个数取决于插入和删除元素的位置。  
`Insert`和`Delete`函数的时间复杂度均为`O(n)`。  

## 线性表的链接表示(链表)  


