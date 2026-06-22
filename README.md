# 机器人ros开发实习面试八股文

##c++语法速通
   ###数据类型 ：  bool 1  char 1  int 4  float 4  double 8
 数组：  
```
 // 定义并初始化数组
    int arr[5] = {1, 2, 3, 4, 5};
    
    // 访问数组元素
    cout << "arr[2] = " << arr[2] << endl; // 输出3
    
    // 遍历数组
    for (int i = 0; i < 5; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
    
    // C++11范围for循环
    for (int num : arr) {
        cout << num << " ";
    }
    cout << endl;
```
 字符串：
```
// C风格字符串
    char c_str[] = "Hello C";
    cout << c_str << endl;
    
    // C++风格字符串
    string cpp_str = "Hello C++";
    cout << cpp_str << endl;
    
    // 字符串操作
    cout << "长度：" << cpp_str.length() << endl;
    cout << "拼接：" << cpp_str + " World" << endl;
    cout << "子串：" << cpp_str.substr(6, 3) << endl; // 从索引6开始取3个字符
```

结构体：
```
struct Student {
     string name;
     int age;
     float score;
};
int main(){
//创建结构体对象
Student s1;
s1.name="张三";
s1.age = 20;
s1.score = 95.5;

//访问结构体成员
cout<<s1.name<<s1.age<<s1.score<<endl;
```

###指针和引用
   指针是一个变量  用于存储另一个变量的内存地址
```
int main(){
int a =10;
int *p = &a;
cout<<a<<&a<<p<<*P<<endl;
*p=20;
cout<<a<<enl;
```
数组与指针 ：数组名本质是一个指向数组首元素的常量指针
```
int arr[5]={1,2,3,4,5};
int *p=arr;
int *q=&arr[0];
cout<<*p<<*q<<endl;
```

引用：  引用是一个变量的别名，即同一个变量的另一个名字
```
int main(){
int a =10;
int &ref=a;  //定义引用，必须在定义时初始化
cout<<a<<ref<<endl;
//通过引用修改a的值
ref=20;
cout<<a<<endl;
renturn 0;
```
指针和引用的核心区别：
                       指针：  存储内存地址的变量 有自己的地址   可以先定义后初始化  可以为nullptr  可以指向其他变量  占用4字节  可以有多级指针
                       引用：  变量的别名  没有自己独立的地址    定义是必须初始化    不能引用空置   一旦绑定不能改变  不占用额外内存  只有一级引用
常见误区：
           野指针：指向未初始化的指针   悬空指针：  曾经指向有效内存，但内存被销毁，释放了
           数组越界：  指针访问超出数组范围


###cosnt关键字
         const修饰的变量成为常量，其值在初始化后不能被修改
```
int main(){
const int MAX = 100;  //const常量不能修改
//const与指针
int a=10;
int b=20;
//￥￥￥找到一个快速的记忆方法  *p代表数据  p代表指针  const int* p  :（指向常量int类型数据的指针）const 修饰*p 也就是修饰数据  所以指针指向的数据不能改   （常量指针）指向常量的指针
                                                 //int const *p  :（指向常量int类型数据的指)   const 修饰*p 也就是修饰数据  所以指针指向的数据不能改     （常量指针）指向常量的指针
                                                //int*  const p  :(int类型的常量指针)        const修饰p   也就是指针   所以指针本身不能变    （指针常量）指针是个常量
const int* p1 =&a;  //指向常量的指针，不能通过指针修改指向的值
*p1=30;  //error
p1=&b;   // right

int* const p2=&a;   //指针常量：指针本身不能修改
*p=30; //right
p=&b;  //error

const int* const p3;  //指向常量的常量指针： 指针和指向的值都不能修改
*p3=30;//error
 p3=&b; //error

return 0;
```
          const修饰成员函数：表示该函数不会修改类的任何成员变量

```
class Person{
    private:
        string name;
        int age;
    public:
        Person(string n,int a):name(n),age(a){}
    //const成员函数
        string get_name() const {
            age=30;//error
            return name;
            }
       int get_age() const{
         return age;
        }
        void set_age(int a){
            age=a;
            }
};
int main(){
       const Person p("tom",25);
       cout<<p.get_name<<p.get_age<<endl;
      p.set_age(30);//error const对象只能调用const成员对象
        return 0；
}
```

   //const与#define的区别：
     |         | const | #define |
     |---------|-------|---------|
    | 是否有类型检查 | 有     | 无       |
    | 作用域     | 限制    | 全局      |
    | 可否调试    | 可     | 否       |
//const对象要保证对象的状态不被改变   所以const对象只能调用const成员函数

###函数
      函数定义与调用
```
int add(int a ,int b);  //函数声明
int main(){
int result = add(3 ,5);  //函数调用
int add(int a,int b){    //函数定义
return a+b;
}
     函数的参数传递方式
值传递:将参数的值拷贝一份传递给函数，函数内部对参数的修改不会影响原变量
     ```
     void swap_by_value(int a ,int b){
int temp =a;
a=b;
b=temp;
}

int main(){
int x=10,y=20;
swap_by_value(x,y);
cout<<x<<y<<endl;
return 0;
}
```
地址传递：将参数的地址传递给函数，函数通过指针可以修改原变量的值
```
void swap_by_pointer(int *a ,int *b){
int temp =*a;
*a=*b;
*b=temp;
}

int main(){
int x=10,y=20;
swap_by_value(&x,&y);
cout<<x<<y<<endl;
return 0;
}
```

引用传递：将参数的引用传递给函数，函数通过引用可以修改原变量的值
``` 
 void swap_by_reference(int &a ,int &b){
int temp =a;
a=b;
b=temp;
}

int main(){
int x=10,y=20;
swap_by_reference(x,y);
cout<<x<<y<<endl;
return 0;
}
```
函数重载：同一作用域内，函数名相同但参数列表不同的多个函数称为函数重载
函数重载原理：编译器在编译时会根据参数列表对函数名进行名字修饰，生成不同的内部函数名
```
int add(int a, int b){
return a+b;
}

int add(int a,int b,int c){   //参数个数不同
return a+ba+c;
}

double add(double a ,double b){
return a+b;
}
```
函数默认参数： 函数参数可以设置默认值，调用时如果没有提供该参数就是用默认值
```
int add(int a,int b=10){  //设置默认参数值必须从右向左
return a+b;
}
```
内联函数：用inline修饰的函数，编译器会在调用处直接展开函数体，避免函数调用的开销
```
inline int max(int a,int b){
return a>b?a:b;
}
```
###类与对象
     类的定义与对象的创建
```
class Car{
private:
   string brand;
   int price;
public:
   void set_brand(string b){
   brand=b;
}
string get_brand(){
return brand;
}
}；
int main(){
Car car1;
car1.set_brand("奔驰")；
return 0;
}
```
    访问控制
public:类内外都能访问
private：只有类内部和友元能访问
protected：类内部和派生类能访问
封装原则：所有成员变量都应该设为private 通过public的函数来访问和修改

    继承：  继承允许我们创建一个新类，从现有类中继承属性和方法，实现代码复用
```
class Animal{
protected:string name;
public:
    void set_name(string n){
name = n;
}
};
class Dog:public Animal{
public:
   void bark(){
 cout<<name<<endl;
}
};
int main(){
Dog dog;
dog.set_name("旺财");
dog.bark();
return 0;
}
```
多态： 多态是指同一接口 不同实现  通过虚函数来实现 普通虚函数提供了默认实现，派生类可以选择重写或继承；纯虚函数没有实现，相当于一个接口，强制派生类必须提供自己的实现。包含纯虚函数的类叫做抽象类，不能被实例化，只能作为基类被继承。
 ```
class Animal{
protected:
   string name;
public:
    Animal(string n):name(n){}
//虚函数
   virtual void make_sound(){
cout<<"动物发出声音"<<endl;
}
};
class Dog:public Animal{
public:
    Dog(string n):Animal(n){}
   //重写基类的虚函数
    void make_sound() override{
             cout<<name<<"汪汪叫"<<endl;
}
};
class Cat:public Animal{
public:
    Cat(string n):Animal(n){}
   //重写基类的虚函数
    void make_sound() override{
             cout<<name<<"喵喵叫"<<endl;
}
};
int main(){
//多态：  基类指针指向派生类对象
Animal* animal1 = new Dog("旺财");
Animal* animal2 = new Cat("咪咪");
animal1->make_sound();
animal2->make_sound();
delete animal1;
delete animal2;
return 0;
}
```
虚函数的实现原理：  每个有虚函数的类都有一个虚函数表（vtable），对象中存储一个指向虚函数表的指针（vptr），调用函数
时，通过vptr找到vtable，再调用相应的函数
函数重载与虚函数区别：
函数重载是编译时的静态行为，编译器根据参数列表选择调用哪个函数；多态是运行时的动态行为，程序运行时根据对象的实际类型选择调用哪个函数。函数重载解决的是 "同名函数处理不同类型参数" 的问题，多态解决的是 "同一接口处理不同类型对象" 的问题。

###构造函数与析构函数
    构造函数 ： 在创建对象时自动调用，用于初始化对象的成员变量   函数名与类名相同  没有返回值  可以重载
```
class Person {
private:
string name;
int age;
public:
  //默认构造函数
   Person(){}  
  //含参构造函数
  Person(string n,int a){
name=n; 
age=a;
}  
  //初始化列表
  Person(string n):name(n),age(0);
{}
};
```
析构函数：  在销毁对象时自动调用   没有返回值  没有参数  不能重载
```
class MyString{
 private: 
    char* data;
public:
   MyString(const char* str){
    data = new char[strlen(str)+1];
strcpy(data,str);
}
~MyString(){
dalete[] data;
}
};
```
拷贝构造函数：  用一个已存在的对象来初始化一个新对象时调用的构造函数
```
class MyString {
private:
    char* data;
public:
    MyString(const char* str) {
        data = new char[strlen(str) + 1];
        strcpy(data, str);
    }
     //拷贝构造函数
     MyString(const MyString& other){
        // 深拷贝：重新分配内存，复制数据
        data = new char[strlen(other.data) + 1];
        strcpy(data, other.data);
}
 ~MyString() {
        delete[] data;
    }
int main(){
MyString s1("hello");
Mystring s2=s1;  //调用拷贝构造函数
```
深拷贝与浅拷贝：   浅拷贝：   只复制指针的值，不复制指针指向的内容，多个对象共享一块内存，当一个对象释放内存时，另一个对象会变成野指针
                 深拷贝：  重新分配内存，复制指针指向的内容，每个对象拥有独立的内存，互不影响

移动构造函数： 移动构造函数用于转移资源所有权，避免不必要的深拷贝，提高程序性能
```
class MyString {
    private:
         char* data;
    public:
      MyString(const char* str){
        data = new char[strlen(str)+1];
        strcpy(data,str);
}
//深拷贝构造函数
MyString(const MyString& str){
data = new char [strlen(str)+1];
strcpy(data,str.data);
}
//移动构造函数
MyString (MyString && other) noexcept{
   //接管资源
   data = other.data;
  //将原对象置空
other.data = nullptr;
}
//调用移动构造函数 
MyString s2 = move(s1);
```
###函数模板和类模板
   函数模板：定义一个通用的函数，其参数和返回值可以是任意类型  template<class T>
```
  template<typename T>
  void swap(T& a,T& b){
   T temp =a;
   a=b;
   b=temp;
}
  
  T max(const T& a,const T& b){
    return a>b?a:b;
} 
  int main(){
int a=10,b=20;
swap(a,b);
double c=3.14,b=2.71;
swap(c,d);
max<int>(3.14,2.71);
return 0;
}
```
类模板：  定义一个通用的类，其成员变量和成员函数可以是任意类型
```
  template<typename T>
class Stack{
  private:
     vertor<T>elements; 
 public:
    void push(const T& elem){
       element.push_back(elem);
}
    void pop(){
   if(elements.empty()){
    throw out_of_range("Stack is empty");
}
  elements.pop_back();
}
   T top() const{
   if (elements.empty()){
      throw out_of_range("Stack is empty");
}
return elements.back();
}
   bool empty() const {
    return elements.empty();
}
    size_t size() const {
   return elements.size();
}
};
```
//模板的编译原理  ：模板是编译时机制，编译器会根据传入的类型生成对应的具体函数或类，因此模板的定义和实现必须放在同一个头文件中；

###智能指针
        核心思想： rall（资源获取即初始化） 将资源的生命周期与对象的生命周期绑定。对象创建时即获取资源，对象销毁时自动释放资源
        三种智能指针：<memory>头文件中  std::unique_ptr  std::shared_ptr  std::weak_ptr
        std::unique_ptr （独占所有权）
              核心特性：  独占所指向的对象，同一时间只能有一个unique_ptr指向同一个对象
                         不能拷贝  只能移动  
                         当unique_ptr被销毁时， 它所指向的对象会被自动删除
```
class MyClass{
public:
     MyClass(){ cout<<"constructor"<<endl;}
     ~MyString() {cout<<"destructor}<<endl;}
     void do_something(){cout<<"doing something"<<endl;}
};
int main(){
    unique_ptr<MyClass> ptr1 = make_unique<MyClass>();
    ptr1->do_something();
    unique_ptr<MyClass> p2=p1;//error   不能拷贝unique_ptr;
   unique_ptr<MyClass> ptr2 = move(ptr1);   //right 可以转移所有权
ptr2->do_something();
   //ptr1现在是空的   并且程序结束时   ptr2被销毁  自动删除指向的对象
return 0;
}
```
  std::shared_ptr (共享所有权)
  核心特性： 多个shared_ptr可以指向同一个对象
            使用引用计数机制，每增加一个shared_ptr 引用计数加1  每销毁一个 shared_ptr，引用计数 - 1 
            当引用计数变为0时 ，对象被自动删除
```
class MyClass{
public:
     MyClass(){ cout<<"constructor"<<endl;}
     ~MyString() {cout<<"destructor}<<endl;}
};
int main(){
     shared_ptr<MyClass> ptr1 = make_shared<MyClass>();
     //拷贝  引用计数加1
       shared_ptr<MyString> ptr2 = ptr1;
       cout<<ptr1.use_count()<<endl;  
```
  std:：weak_ptr(弱引用)
weak_ptr<MyString> ptr1 = make_weak<MyString>();
核心特征：  不拥有对象的所有权 不会增加引用计数
           用于解决shared_ptr的循环引用的问题
           循环引用： 两个对象内部的shared_ptr互相持有对方的控制块的引用，导致当所有外部shared_ptr都销毁后，他们的强引用计数仍然是1.
           需要通过lock（）方法获取shared_ptr才能访问对象
```
    class B;  //前向声明
    class A{
  public:
      shared_ptr<B> b_ptr;
      ~A() {cout<<a destructor<<endl;}
};
class B{
    public: 
       weak_ptr<A> a_ptr; //用weak_ptr解决循环引用
       ~B（）
  }；
    int main(){
      shared_ptr<A> a = make_shared<A>();
      shared_ptr<B> b = make_shared<B>();
      a-> b_ptr = b;
      b->a->ptr = a;
      return 0;
}
```
 lambda表达式：  匿名函数  可以作为参数传递给算法  也可以作为返回值
```
[capture_list] (parameters) mutable-> return_type{
       function_body;
}
 int main(){
      auto add = [](int a,int b) {return a+b;};
       cout<<add(a,b);

int x=10;  //按值捕获外部变量
auto print_x = [x](){cout<<x<<endl; };
x=20;   //输出10(捕获的是x的拷贝)；

//按引用捕获外部变量
auto print_x_ref = [&x](){cout<<x<<endl;};
x=30;   //输出30   捕获的是x的引用

//STL算法中的应用
  vector<int> vec = {3,1,4,5,7,8,9,8,4};

//排序
 sort(vec.begin(),vec.end(),[](int a,int b){return a>b;});

//遍历
for_each(vec.begin(),vec.end(),[](int num){cout<<num<<endl;});

//查找第一个大于5的元素：
auto it = find_if(vec.begin(),vec.end(),[](int num){return num>5};);
```
lamba的本质：  编译器会为每个lambda生成一个唯一的匿名类，重载了operator（）运算符
              捕获的变量会变成匿名类的成员变量

移动语义：它允许我们转移对象的资源所有权，避免不必要的深拷贝
左值： 可以取地址 有名字的表达式
右值： 不能取地址，没有名字的表达式  （临时对象，函数返回值）
右值引用：T&& 专门用来绑定右值
std：：move：  将左值强制转换为右值引用，触发移动操作

```
实现一个支持移动语义的完整的MyString的类：
    #include<iostream>
 #include<cstring>
#include<utility>
using namespace std;
  Class MyString {
   private:  
        char* data;
       int len;
public:
   //默认构造函数
    MyString():data(nullptr),len(0){
    cout<<"default construct"<<endl;
   
//普通构造函数
     MyString(const char* str){
    data = new char[(len)+1]；
    strcpy(data,str);
  cout<<"parameterized construct"<<endl;

//拷贝构造函数
MyString(cosnt MyString& other){
len=other.len;
data = new char[other.len+1];
strcpy(data,other.data);
cout<<<"copy construct"<<endl;

//拷贝构造运算符
MyString& operator = (cosnt MyString& other){
if(*this = other)
return *this;
delete[] data;
len = other.len
data = new char[len+1];
strcpy(data,other.data);
return *this;

//移动构造函数
MyString(MyString&& other) noexcept{
cout<<"move construct"<<endl;
data = other.data;
len = other.len;
other.data = nullptr;
other.len = 0;

//移动赋值运算符
MyString& operator=(cosnt MyString&& other){
if(*this == other)
return *this;
delete[] data;
data = other.data;
len = other.len;
other.data = nullptr;
other.len  = 0;
return *this;
```

迭代器：  一种泛化的指针，用来指向容器中的元素 ，通过它可以读取修改元素  也可以在容器中移动位置
          统一接口：  所有STL容器的遍历方式完全一致  换容器不需要改遍历逻辑
          屏蔽底层： 不用管是数组  链表  还是红黑树  都用同样的方式访问元素
输入迭代器  输出迭代器  前向迭代器  双向迭代器  随机访问迭代器
```
//最基础迭代器
int main(){
    vector<int> vec = {1,2,3,4};
    for(vector::iterator it = vec.begin();it!=vec.end();++it){
         cout<<*ir<<endl;
   }
     list<int> lst = {1,2,3,4};
      for(auto it = lst.begin();it!=lst.end;++it)；

   auto it = vec.begin();
*it = 100;
return 0;
}

//const迭代器
   int main(){
     vector<int> vec = {1,2,3};
     const vector<int> cosnt_vec = {10 ,20,30};
     //cbegin()/ cend() 直接返回const迭代器
     for (auto it = cbegin();it!=vec.cend()l;++it){
cout<<*it<<endl;
}
    //const容器只能用cosnt迭代器
for (auto it = const_vec.begin(); it != const_vec.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;

    return 0;
}
//反向迭代器  ：rbegin（）指向最后一个元素  rend（）指向第一个元素的前一个位置
int main(){
   vector<int> vec = {1,2,3,4,5}
    for(auto it = vec.rbegin();it!=vec.rend();++it){
        cout<<*it<<endl;
return 0;
}

//随机访问和双向迭代器的区别：
   vector<int> vec = {1, 2, 3, 4, 5};
    list<int> lst = {1, 2, 3, 4, 5};
    auto vec_it = vec.begin();
   vec_it+=3;
cout<<*vec_it<<endl;
cout<<vec_it[1]<<endl;
auto list_it = lst.begin();
advance(lst_it,3); //list是双向迭代器  不支持+   所以只能用advance

//迭代器失效问题
    当容器的结构发生变化（插入  删除  扩容），部分或全部迭代器会变成野指针 
     vector的迭代器失效：
         尾部插入push_back  触发扩容 ：所有迭代器 指针 引用 全部失效 
                           不触发扩容：  仅尾部迭代器失效，前面的有效
         中间/头部插入：  插入位置及之后的所有的迭代器全部失效
         删除元素erase：  被删除元素及之后的所有迭代器全部失效
// 错误！erase后it已经失效，再执行it++会崩溃
for (auto it = vec.begin(); it != vec.end(); ++it) {
    if (*it == 3) {
        vec.erase(it);
    }
}
//正确写法：
      erase会返回指向被删除元素下一个位置的有效迭代器
        int main() {
    vector<int> vec = {1, 2, 3, 3, 4, 3, 5};

    // 正确：删除所有值为3的元素
    auto it = vec.begin();
    while (it != vec.end()) {
        if (*it == 3) {
            it = vec.erase(it); // erase返回下一个有效迭代器
        } else {
            ++it;
        }
    }

    // 输出结果：1 2 4 5
    for (int num : vec) {
        cout << num << " ";
    }
    cout << endl;

    return 0;
}
  //deque迭代器只要不是在头尾操作  那么他的迭代器都会失效
      list  map  set的迭代器  只有指向被删除元素的迭代器失效  其他所有迭代器都有效
```
 迭代器常用的辅助函数  <iterator>头文件
   ```
     #include<iostream>
#include<vector>
#include<list>
#include<iterator>
using namespace std;

int main(){
    vector<int >vec = {10,20,30,40,50};
    list<int > lst = {1,2,3,4,5}
    //使用advance（it，n）迭代器向后移动n步
    auto vec_it = vec.bagin();
    advance(vec_it,3);
   auto list_it=lst.begin();
   advance(list_it,3);
//distance（it1，it2）；  计算两个迭代器之间的元素个数
int vec_dis = distance(vec.begin(),vec.end());
//next(it,n):返回向后移动n步的迭代器，不改变原迭代器
auto next_it = next(vec.begin(),2);
//prev(it,n):返回向前移动n步的迭代器，不改变原迭代器
auto prev_it = prev(vec.end(),2);
     return ;
}
```
###STL算法库高频核心函数
       STL算法都定义在<algorithm>头文件中，通过迭代器和所有容器配合使用
  find   线性查找第一个等于目标值的元素  on
  find——if   线性查找第一个满足条件的元素  on
  cout/cout——if  统计满足条件的元素个数   on
binary——search 有序区间二分查找是否存在  ologn
lower_bound  有序区间查找第一个》=目标的位置  ologn

```
int  mian(){
    vector<int > vec = {3,1,4,1,5,9,2,6};
    //find   查找值为5的元素
    auto it1 = find(vec.begin(),vec.end(),5)
   //find_if 查找第一个大于5的元素
   auto it2 = find_if(vec.begin(),vec.end(),[](int x){return x>5});
  //cout_if  统计偶数的个数
       int even_cnt = count_if(vec.begin(),vec.end(),[](int x)return {x%2==0});
  //二分查找 ：  
    sort(vec.begin(),vec.end());
    bool exist = binary_search(vec.begin(),vec.end(),4);
//lower_bound 第一个》3的位置
  auto it3 = lower_bound(vec.begin(), vec.end(), 3);
    cout << "第一个≥3的元素：" << *it3 << endl;
return 0;
}
```
int main(){
  vector<int >vec = {1,2,3,4,5}
  //默认升序排序
sort(vec.begin(),vec.end());
  //自定义排序  降序
sort(vec.begin(),vec.end(),greater<int >());
//自定义排序  按绝对值 从大到小
    sort(vec.begin(),vec.end(),[](int a,int b){return a>b;});
//反转区间
reverse(vec.begin(),vec.end());
//stable_sort 稳定排序  
 stable_sort(vec.begin(),vec.end());
return 0;
}
```
 删除与去重
```
 remove/remove_if 并不会真正删除元素  只要把要删除的元素移到末尾  返回新的尾迭代器  真正的删除需要配合容器的erase成员函数
   int main(）{
     //删除所有值为2的元素
     auto new_end = remove(vec.begin(),vec.end(),2);
      vec.erase(new_end,vec.end());
    // 删除所有偶数  
      vec.erase(remove_if(vec.begin（），vec.end(),[](int x){return x%2==0;}),vec.end());
    //去重
       vector<int > v ={1,2,2,3,4,8};
       sort(v.begin(),v.end());
   auto last = unique(v.begin(),v.end());
       v.erase(last,v.end());
  for(int num : v){
cout<<num<<endl;
}
return 0;

```
  //遍历与变换:
        int main(){
              vector<int > vec = {1,2,3,4,5};
            //for_each： 遍历每个元素执行操作
              for_each(vec.begin(),vec.end(),[](ing&x ){
x*=2});
//transform：交换到另一个容器
vector<int > res(vec.size());
 transform(vec.begin(),vec.end(),res.begin(),[](int x){return x+1;});
//accumulate:求和 累积  
  int sum = accumulate(vec.begin(),vec.end(),0)
  return 0;
}
```
集合set与映射map：
                 set：唯一  不可重复                    map： key唯一 键值对
   int main (){
     set<int> s;
    //插入
       s.insert(3);
s.insert(1);
s.insert(2);
s.insert(2);//重复元素插入无效

//查找
if(s.find(2)!=s.end()){
cout<<s.count(2)<<endl;}

//删除
s.erase(3);//按值删除
auto it = s.find(1);
if(it!=s.end())
s.erase(it); // 按迭代器删除

//遍历：
   for(int num:s)    //自动升序输出
  cout<<num<<endl;
```
map的常用操作：
    ```
   int main(){
      map<int ,string > m;
    //插入键值对
      m[1]="one";
 m[2] = "two";     // 常用写法，key不存在则自动插入
    m[3] = "three";

//访问与修改
    cout<<m[2]<<endl;
    m[2]="TWO";

//查找
if(m.find(3)!=m.end()){
   cout<<m[3]<<endl;}

//遍历（自动按key升序）
  for(auto& pair: m){
cout<<pair.first<<pair.second<<endl;}}

//删除
m.erase(1); //按key删除

//统计字符出现的次数
  string str ="hello world";
  unordered_map<string, int > cnt;
  for(char c: str){
    cnt[c]++;
}
  for(auto& pair : cnt){
   cout<< pair.first<<pair.second<<endl;
}
```
手撕MyVector类（指向堆上的数组，当前元素个数， 当前总容量， // 1. 默认构造函数， // 2. 初始化列表构造（C++11），// 3. 拷贝构造函数（深拷贝）， // 4. 拷贝赋值运算符，// 5. 移动构造函数， // 6. 移动赋值运算符， // 7. 析构函数， // 8. 尾插元素（含2倍扩容逻辑）， // 9. 尾删元素，// 10. 下标访问， // 11. 常用接口，// 迭代器支持（原生指针就是天然的随机访问迭代器））
```
#include<iostream>
#include<initializer_list>
using namespace std;

typelate<typename T>
class MyVector {
        private:
           T* data_;
           size_t size_;
           size_t capcity;
        public:
            // 1. 默认构造函数
            MyVector():data_(nullptr),size_(0),capcity(0){}
            // 2. 初始化列表构造（C++11)
            MyVector (initializer_list<T> l){
                          size_=l.size_;
                          capcity=size_;
                          data_=new T[l.size_];
   size_t i=0;                
for(const T& val : l){
         data_[i++]=val;
}
}

// 3. 拷贝构造函数（深拷贝)
      MyVector(cosnt MyVector& v){
          size_=v.size_;
          capcity = v.capcity; 
          data_ = new T[capcity]; 
       for (size_t i = 0; i < _size; i++) {
            _data[i] = other._data[i];
        }
 }
// 4. 拷贝赋值运算符
        MyVector& operator=(cosnt  MyVector& v){
             if(this==&v){
return *this;
 }
delete[] data_;
           size_=v.size_;
          capcity = v.capcity; 
          data_ = new T[capcity];
for (size_t i = 0; i < _size; i++) {
            _data[i] = other._data[i];
        }

 
// 5. 移动构造函数
    MyVector (MyVector&& v2 )  {
         size_=v2.size_;
         capcity=v2.capcity;
         data_ = v2.data_;
         v2.data_=nullptr;
         v2.capcity=0;
         v2.size_=0;   
}

// 6. 移动赋值运算符
        Mystring& operator=( MyVector&& v2) 
              if(this==&v2){
return *this;
delete[] data_;
size_=v2.size_;
         capcity=v2.capcity;
         data_ = v2.data_;
         v2.data_=nullptr;
         v2.capcity=0;
         v2.size_=0;
        return *this;
 }  
         
// 7. 析构函数
     ~MyVector(){
          delete[] data_;  
}   

// 8. 尾插元素（含2倍扩容逻辑）
     void insert_back(const T& val){
           if(new_capcity==capcity){
             size_t new_cap =capcity;
                 capcity*=2;
            T* new_data=new T[new_cap];
             for(size_t i=0;i<size;i++){
                   new_data[i]=data_[i];
                
}
delete [] data_;
          data_ = new_data;
          capcity = new_cap;
          }
           data_[size_++]=val;
}
       // 9. 尾删元素
     void    pop_back(){
if(size_>0){size_--};
  
// 10. 下标访问
   T& operator[]( size_t index){
          return data_[index];
}
```
     
       
手撕MyString类（ // 1. 默认构造， // 2. C字符串构造， // 3. 拷贝构造（深拷贝），// 4. 拷贝赋值， // 5. 移动构造，// 6. 移动赋值，// 7. 析构， // 8. 常用接口，// 9. 字符串拼接）
```
#include<iostream>
#include<string>
#include<utility>
using namespace std;
class MyString{
     private: 
         char* data_;
         size_t len_;
    public:
         // 1. 默认构造
         MyString():data_(nullptr),len_(0){}
         // 2. C字符串构造
         MyString(const char* str){
if (str == nullptr)
    {
        data_ = nullptr;
        len_ = 0;
        return;
    }
            len_ = strlen(str);
            data_=new   char[len_+1];
            strcpy(data_,str);
          }
         //3. 拷贝构造（深拷贝)
               MyString (const MyString& other ){                  
           len_=other.len_;
           data_=new char[other.len_+1];
           strcpy(data_,other.data_);
               

        // 4. 拷贝赋值运算符
             MyString& operator=(cosnt MyString& other){
                       if(this==&other){
                        return *this;
                                  }
                         delete[] data_;
                         len_=other.len_;
                         data_ =new char[other.len_+1];
                         strcpy(data_,other.data_);
                        return *this;
                          }
// 5. 移动构造
     MyString(MyString&& other) noexcept{
             
       len_= other.len_;
       data_ = other.data_;
            other.len=0;
            other.data_=nullptr;
 }

// 6. 移动赋值
        MyString& operator=(MyString&& other){
                    if(this==&other){
                        return *this;
                                  }
delete [] data_; 
                len_=other.len_;
                data_=other.data_;
              
                other.len_=0;
               other.data_=nullptr;
            return *this;
}
// 7. 析构
      ~MyString(){
             delete []data_;
             }
         
// 8. 常用接口
      cosnt char* c_str() cosnt {return data_};
      size_t size() cosnt {return len_;};
      bool empty() cosnt {return len_ ==0;}
      char& operator[](size_t index){return _data[]index]};
      const char& operator[] (size_t index)const {return _data[]index]};
// 9. 字符串拼接
   MyString operator+(cosnt MyString& str )const{
          MyString res;
          res.len=len_+str.len_;
          res.data_=new  char[res.len_+1];
          strcpy(res.data_,str.data_);
          strcat(data_.res.data_);
          return res;
}
};
   int main() {
         MyString s1("hello");
         MySting s2("world");
         Mystring s3 =s1+s2;
         MyString s4=move(s3);
          return 0;
}
```
