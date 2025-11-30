# Lambda表达式

基本语法：[捕获变量]（参数列表）可选限定符->返回类型{ 函数体 }

本质：是一个匿名函数对象，auto p =[捕获变量]（传入变量）->返回类型{ 函数体 }就相当于：
```c++
struct{

	成员变量；

	返回类型 operator ()(传入变量形参){函数体}；//重载运算符，使得函数对象可以像函数一样使用

	构造函数；

}p{捕获变量};
```



> 相当于用捕获变量构造了一个匿名函数对象，具有捕获到的变量作为成员变量，指定的参数列表，然后声明了这种函数类型的对象p，但是由于没有声明类型名称（匿名）所以节省了不必要的类型声明代码。

1. 捕获变量

   显式捕获变量 :[&a] [a] 

   > 前者是**按引用**捕获变量，后者是**按值拷贝**当前变量值

   隐式捕获当前作用域的变量：

   1. 值传入：[=] 表示**默认按值**拷贝作用域内的变量
   2. 引用传入：[&]表示默认**按引用**传入作用域内的变量

2. 传入变量：

3. 可选限定符->返回类型 

   1. 这块可以省略。省略则默认函数为const函数 返回类型由函数体内返回值类型决定
   2. 可选限定符：mutable 则函数不再为const函数

4. 函数体：略

【应用】：自定义数据，如pair<int,int>的最小堆，其排序规则未被定义

```c++
class solution{
public:
	vector<vector<int>> kminpairs(vector<int> &nums1,vector<int> &nums2){
       //这里应用了lambda表达式，定义了一个匿名函数对象cmp，定义了关于两个pair<int,int>变量的比较方法，pair<int,int> 作为索引对应的两个数组元素值之和，谁的和更大，谁的优先级更低（priority_queue认为，cmp(a,b)函数返回true表示a的优先级更低应该排后面）
        auto cmp = [&nums1,&nums2](pair<int,int> &a,pair<int,int> &b){
            return nums1[a.first] + nums2[a.second] > nums1[b.first] + nums2[b.second];
        }
        //所以当cmp这个函数类型作为模板的第三个参数、并将其函数对象作为构造函数的参数时，将定义一个自定义的最小堆
        priority_queue<pair<int,int>,vector<pair<int,int>>,decltype(cmp)> pq(cmp);
    }     
} 

```

