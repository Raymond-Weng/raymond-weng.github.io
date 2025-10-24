---
title: std::optional，return null的cpp解法
date: 2025-10-24 15:50:00 +0800
categories: [競賽程式, C++]
tags: [cpp, std, tutorial]     # TAG names should always be lowercase
description: "寫Leetcode用到的小方法，稍微記錄一下"
---

> 本文介紹`std::optional`，為C++17開始的功能，另在C++23, C++26都有做更新，有興趣的讀者

之前寫Java為主，在Java上常常在找不到值的時後直接`return null;`並用`if(f()==null)`直接檢查，但是在C++裡面並沒有這個功能，但相對的就出現了`optional`來補足了。

以Java來說可能會有以下程式
```java
public String f(String s){
  boolean exist = false;
  String res;
  // ...
  if(exist){
    return res;
  }else{
    return null;
  }
}
```
但這種寫法在C++是不存在的，你只能回傳空指針，但這在記憶體管理上會變的很複雜（除非你是用大物件，不然沒有那個必要），於是就出現了`std::optional`
```c++
using namespace std;
optional<string> f(string s){
  bool exist = false;
  string res;
  // ...
  if(exist){
    return res; // 不需要另外包進optional裡面！
  }else{
    return nullopt; // 等價於return null
  }
}
```
`optional`的好處在於他不需要改變`return`語句，只需要在原本邏輯上需要回傳空值的地方改成`return nullopt`就好。  
使用上也很簡單，程式看了應該就懂了
```c++
using namespace std;
optional<string> f(string s){
  // ...
}

int main(){
  auto res = f("test"); // 用auto省略長類別，可以查一下auto怎麼使用
  if(res){ // 用if可以判斷是不是nullopt
    cout << "YES: " << res.value(); // 用.value()取值
  }else{
    cout << "NO"; // nullopt
  }
}
```
如果是像`pair`之類有帶值或是函式的類別可以直接當作**指針**使用，例如
```cpp
using namespace std;
optional<pair<int, int>> f(){
  // ...
}

int main(){
  auto res = f();
  if(res){
    cout << "YES: " << res->first; // 用->取值
  }else{
    cout << "NO";
  }
}
```
在美化程式上是很大的幫助！

