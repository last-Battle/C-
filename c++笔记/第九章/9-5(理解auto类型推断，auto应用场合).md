# auto类型常规推断

```c++
#include<iostream>
#include<cstdlib>

#include <boost/type_index.hpp>

using namespace std;

//显示参数类型，这里不研究boost库
template<typename T>
void myFunction01(T&tem)//T是类型模板参数，T是有类型的,tem是形参，tem也是有类型的
{
	using boost::typeindex::type_id_with_cvr;
	cout << "T type=" << type_id_with_cvr<T>().pretty_name() << endl;//显示T类型
	cout << "tem type=" << type_id_with_cvr<decltype(tem)>().pretty_name() << endl;//显示T类型

}

int main(void)
{
	int number01 = 18;
	const int number02 = number01;
	const int&number03 = number01;
	myFunction01(number01);//T=int  tem=int&类型
	myFunction01(number02);//T=int const tem=const int&
	myFunction01(number03);//T=int const tem=const int& 引用会被忽略，传递不过去
	/*
	T type=int
	tem type=int &
	T type=int const
	tem type=int const &
	T type=int const
	tem type=int const &
	 */

	// (1)auto类型常规推断
	auto number04 = 23;//number04是int类型，auto是int类型占位符
	
	//1.指针或者引用类型，但不是万能引用
	const auto&number07 = number01;  //auto =int,number07=const int&
	auto number08 = number07;//传值方式，引用和const属性会被抛弃，把对方当作新副本， number08=int ,auto =int
	auto &number09 = number07;//auto =const int		number09=const int&  引用属性被丢弃，const属性被保留
	const auto*p1 = &number01;//auto =int		p1=const int*
	auto*p2 = &number01;//auto=int	p2=int*
	auto p3 = &number01;//auto=int*		p3=int*
	


	
	//2.万能引用类型  --函数模板万能引用是T&&   auto万能引用是auto&&
	auto&&number11 = number01;//number11是左值，auto =int&,number11=int&,这里产生引用折叠
	const auto number12 = 234;
	auto&&number13 = number12;//autu=int&，number13=const int&
	auto&&number14 = 234;//赋值给处置是右值  --auto=int,number14=int&&
	

	
	//3.传值方式，非指针，非引用--
	auto number05 = 234;
	const auto number06 = number05;//auto =int number06=const int


	//auto可以使用new操作符
	auto number10 = new auto(2343);//auto=int
	
	system("pause");
	return 0;
}
/*
* (1)auto类型常规推断
* c++11中，auto英语变量的自动类型推断，在声明变量的时候根据变量的初值的类型自动为此变量选择匹配的类型，
*	而不需要我们显示指定类型
*特点：
*1.自动类型推断发生在编译期间，不会影响程序执行期间的性能。
*2.auto定义的变量必须立即初始化，这样编译器才能自动推断它的类型。确定auto的类型和它所定义变量的类型。
*	然后在编译期间使用真正的类型替换掉auto这个类型占位符。
*3.auto使用灵活，可以和指针，引用，const等限定符结合使用。
*4.auto类型推断和函数模板推断非常类似。autu推导后代表一个具体类型。
*	auto实际上是一个类型，实际上相当于函数模板推导中的类型模板参数T。auto也是类型声明中的一部分。
*	可以把auto当作类型占位符
*
*
* (2)auto类型推断分类
*		1.指针或者引用类型，但不是万能引用--不会抛弃const属性，但是会抛弃引用等限定符
*		2.万能引用类型
*		3.传值方式，非指针，非引用--传值方式会抛弃引用和const属性。
* (3)
* (4)
* (5)
* (6)(7)
*/


```

# auto针对数组和函数的推断
```c++
#include<iostream>
#include<cstdlib>

#include <boost/type_index.hpp>

using namespace std;

//显示参数类型，这里不研究boost库
template<typename T>
void myFunction01(T&tem)//T是类型模板参数，T是有类型的,tem是形参，tem也是有类型的
{
	using boost::typeindex::type_id_with_cvr;
	cout << "T type=" << type_id_with_cvr<T>().pretty_name() << endl;//显示T类型
	cout << "tem type=" << type_id_with_cvr<decltype(tem)>().pretty_name() << endl;//显示T类型

}

void myFunction(double a,int b)
{
	
}


int main(void)
{
	const char myStr[] = "jisuanjizuchengyaunli";//mystr=const char[长度]类型
	auto mystr02 = myStr;//const char*
	auto&mystr03 = myStr;//const char(&)[数组长度]类型

	int a[2] = { 1,2 };
	auto myIntarry = a;//auto =int* myIntArray=int*

	//函数类型
	auto myf = myFunction;//
	auto &myf02 = myFunction;//
	using boost::typeindex::type_id_with_cvr;
	cout << "myf type=" << type_id_with_cvr<decltype(myf)>().pretty_name() << endl;//显示类型
	cout << "myf02 type=" << type_id_with_cvr<decltype(myf02)>().pretty_name() << endl;
	/*
	    myf type=void (__cdecl*)(double,int)//函数指针
		myf02 type=void (__cdecl&)(double,int)//函数引用
	 */

	
	system("pause");
	return 0;
}
/*
* (1)auto针对数组和函数的推断
* (3)
* (4)
* (5)
* (6)(7)
*/

```

# auto类型initializer_list的特殊推断

```c++
#include<iostream>
#include<cstdlib>

#include <boost/type_index.hpp>

using namespace std;

//显示参数类型，这里不研究boost库
template<typename T>
void myFunction01(T&tem)//T是类型模板参数，T是有类型的,tem是形参，tem也是有类型的
{
	using boost::typeindex::type_id_with_cvr;
	cout << "T type=" << type_id_with_cvr<T>().pretty_name() << endl;//显示T类型
	cout << "tem type=" << type_id_with_cvr<decltype(tem)>().pretty_name() << endl;//显示T类型

}



int main(void)
{
	//c++98支持的初始化方式
	int number01 = 10;
	int number02(23);
	//c++11支持的初始化方式
	int number03 = { 23 };
	int number04{ 234 };
	//替换为auto可以吗？？

	auto number05 = 23;//ok
	auto number06(23);
	auto number07 = { 23 };//class std::initializer_list<int>  auto遇见{}推导规则不一样
	auto number08{ 23 };
	using boost::typeindex::type_id_with_cvr;
	cout << "number07 type=" << type_id_with_cvr<decltype(number07)>().pretty_name() << endl;//显示T类型
	cout << "number08 type=" << type_id_with_cvr<decltype(number08)>().pretty_name() << endl;//显示T类型
	/*
	 *  number07 type=class std::initializer_list<int>一个类模板
		number08 type=int
	 */

	auto number09 = { 23,345 };//所有的类型要一致，需要进行二次推导，先推导auto=int,再推导出类模板类型
	
	
	system("pause");
	return 0;
}
/*
* (1)auto类型std::initializer_list的特殊推断
*	class std::initializer_list<int> c++中引入的类模板，表示某种特定的值的数组
* (2)auto还可以放在函数返回值类型，以后讲。
* (4)
* (5)
* (6)(7)
*/


```

# auto不适用场合举例

```c++
#include<iostream>
#include<cstdlib>

#include <boost/type_index.hpp>

using namespace std;

//显示参数类型，这里不研究boost库
template<typename T>
void myFunction01(T&tem)//T是类型模板参数，T是有类型的,tem是形参，tem也是有类型的
{
	using boost::typeindex::type_id_with_cvr;
	cout << "T type=" << type_id_with_cvr<T>().pretty_name() << endl;//显示T类型
	cout << "tem type=" << type_id_with_cvr<decltype(tem)>().pretty_name() << endl;//显示T类型

}

class TescClass
{
public:
	//auto m;//普通成员变量不可以是auto
	static  const auto m2 = 23;//static const 静态成员可以使用auto，其值必须在类内初始化
};


int main(void)
{

	system("pause");
	return 0;
}
/*
* (1)auto不适用场合举例
*	1.atuo不能使用与函数形参，比如void myfunction(auto x)
*	2.类中普通成员变量不能使用auto 。static const 允许，但是必须在类内初始化
*	
* (4)
* (5)
* (6)(7)
*/

```

# auto适用场合举例

```c++
#include<iostream>
#include<cstdlib>

#include <boost/type_index.hpp>
#include <map>
#include <string>

using namespace std;

//显示参数类型，这里不研究boost库
template<typename T>
void myFunction01(T&tem)//T是类型模板参数，T是有类型的,tem是形参，tem也是有类型的
{
	using boost::typeindex::type_id_with_cvr;
	cout << "T type=" << type_id_with_cvr<T>().pretty_name() << endl;//显示T类型
	cout << "tem type=" << type_id_with_cvr<decltype(tem)>().pretty_name() << endl;//显示T类型

}

class TescClassA
{
public:
	static int test()
	{
		return 0;
	}
};

class TescClassB
{
public:
	static double test()
	{
		return 0.12;
	}
};

//通过类模板调用上面两个类中的函数,返回值不确定
template<typename T>
auto testFunction()
{
	auto value = T::test();//调用哪个函数不确定，返回值也不确定
	return value;
}

int main(void)
{
	std::map<string, int>myMap;
	myMap.insert(make_pair("aaa", 3454));
	myMap.insert({ "bbb",222 });
	myMap.insert({ "ccc",3434 });
	for (map<string,int>::iterator iter=myMap.begin();iter!=myMap.end();iter++)
	{
		cout << iter->first << "=" << iter->second << endl;
	}
	//可以使用auto代替类型
	for (auto iter=myMap.begin();iter!=myMap.end();iter++)
	{
		cout << iter->first << "=" << iter->second << endl;
	}

	//2.当没有办法确定类型的时候，使用auto
	cout << testFunction<TescClassA>() << endl;//0
	cout << testFunction<TescClassB>() << endl;//0.12
	

	system("pause");
	return 0;
}
/*
* (1)auto适用场合举例
*	1.代替很长的类型名字，如迭代器
*	2.当没有办法确定类型的时候，使用auto
* (2)总结：
*	auto表面上是一个关键字，也可以看作一个强大的工具，善于利用。
* (5)
* (6)(7)
*/

```

```c++
/******************************************************************************

Welcome to GDB Online.
GDB online is an online compiler and debugger tool for C, C++, Python, PHP, Ruby, 
C#, VB, Perl, Swift, Prolog, Javascript, Pascal, HTML, CSS, JS
Code, Compile, Run and Debug online from anywhere in world.

*******************************************************************************/
#include <stdio.h>
#include <iostream>
#include <boost/type_index.hpp>

using namespace std;

namespace _nmsp1
{
    template<typename T>
    void myfunc(T && v)
    {
        
    }
    
    void func()
    {
        // // std::forward 用法补充
        
        // int i = 14;
        // myfunc(i); // i 是左值。T = int &, t = int &
        // myfunc(188); // 188 是右值。T = int, t = int &&
        
        // int nx = 13; // nx 左值
        // int &&def = std::move(nx);
        // int &&def2 = std::forward<int>(nx); // forward这里也能成功的把左值转为右值
        // // 所以这里forward转为什么值，取决于尖括号里传递的是什么，尖括号里就表示上文的 T 
        
    }
}

namespace _nmsp2
{
    template<typename T>
    void myfunc(T& tmpv)
    {
        std::cout << "---------------begin-----------------" << std::endl;
        using boost::typeindex::type_id_with_evr;
        std::cout << "T = " << type_id_with_evr<T>().pretty_name() << std::endl;
        // 显示T的类型
        std::cout << "tmpv = " << type_id_with_evr<decltype(tmpv)>().pretty_name() << std::endl;
        // 显示形参tmpv的类型
        std::cout << "----------------end------------------" << std::endl;
        
        return;
    }
    
    void funcTest(double, int) {}
    
    void func()
    {
        // auto常规类型推断
        
        int i = 18;         // int
        const int j = i;    // const int
        const int &k = i;   // const int &
        myfunc(i);          // T = int;             t = int &
        myfunc(j);          // T = const int;       t = const int &
        myfunc(k);          // T = const int;       t = const int &
        
        // auto c++11 用于变量的自动类型推断，在声明变量的时候根据变量初始值的类型自动为此变量选择匹配的类型
        // 而不需要我们显示的指定变量类型
        
        // 特点：
        // 1）auto自动类型推断发生在编译期间，所以不会影响程序执行期间的性能
        // 2）auto定义的变量必须立即初始化，这样编译器才能在编译期间推断他的实际类型
        // 那么编译器这时候才会确定auto 的类型和整个变量的类型，然后在编译期间就可以用真正的类型替换掉auto这个类型占位符
        // 3）auto使用灵活，可以和指针，引用，const等限定符结合使用
        
        // auto类型推断和函数模板推断非常类似
        // auto推断出来代表的是一个具体类型
        // auto实际上是一个类型，那么这里的auto实际上就相当于上面函数模板推导里的类型模板参数 T
        // 所以auto是类型声明的一部分（类型占位符）
        
        // auto x = 20;    // auto = int ； x = int
        // 形参分为3类，1）指针或者引用类型，但不是万能引用 2）万能引用类型，3）传值方式（非指针，非引用）
        // 1）传值方式 （传值方式的auto会抛弃引用，const等限定符）
        auto x = 20;    // auto = int ； x = int
        const auto x2 = x; // auto = int ; x2 = const int
        
        // 2) 指针或者引用类型，但不是万能引用
        // 这种情况不会抛弃const限定符，但是会丢弃引用
        const auto &x3 = x; // auto = int ; x3 = const int&
        auto xyz = x3;  // auto = int, xyz = int // 这个属于传值方式（传值凡是引用类型和const属性都会被抛弃，对方会将其看做新副本）
        // （注意： auto后面直接接变量名属于传值方式， auto后面接一个引用符，就叫引用）
        
        auto &x4 = x3;  // x4 = const int &;    auto = const int  引用属性被丢弃，但是const属性被保留
        auto y = new auto(100);     // y = int *, auto = int *;(auto可以用于new操作符)
        const auto *app = &x;   // app = const int *; auto = int
        auto *app2 = &x;    // app2 = int *; auto = int
        auto app3 = &x;     // app3 = int *; auto = int *; (虽然app3我们没有把它声明为指针，但是auto也还是把它推导为指针类型)
        
        // 3）万能引用类型 T&&
        auto&& w1 = x;      // x是左值，auto = int &; w1 = int &(出现了引用折叠，系统帮助我们处理掉了)
        auto&& w2 = x2;     // x2(const int)左值， auto = int &, w2 = const int &;
        auto&& w3 = 100;    // 100 右值，auto = int; w3 = int &&(右值引用)
        
        
        
        // auto类型针对数组和函数类型的推断
        cosnt char myatr[] = "oooooooooo";  // 定义一个字符数组 mystr = const char[11]   注意：【这里是字符数加1（有一个结束标记符\0）】
        auto myarr = mystr; // myarr = const char *; auto = const char *
        auto &myarr2 = mystr;   // myarr2 = const char (&)[11]  数组引用
        
        int a[2] = {1, 2};
        auto a1 = a;    // a1 = int *; auto = int *
        
        auto temf = funcTest; // 给函数名，应该推断出来的是函数指针
        // temf = void (*)(double, int)
        auto & temf2 = funcTest; // 加引用符，给函数名，应该推断出来的是函数地址（函数引用）
        // temf2 = void (&)(double, int)
        
        temf(2,3);
        temf2(2,3);
        
        return;
    }
}

namespace _nmsp3
{
    template<typename T>
    // void tmfunc(T t)
    void tmfunc(std::initializer_list<T> t)
    {
        
    }
    
    // auto也可以作为函数返回类型
    auto funcs()
    {
        return 123;
    }
    
    class CT
    {
        public:
            auto m_i = 12; // 编译报错，非静态成员不能包含auto
            static const auto m_si = 121; // static const （注意，这里static 和 const缺一不可）静态成员可以使用auto,注意，使用auto，其值必须在类内初始化
                                        // static const和普通静态成员不一样，普通静态成员在这里相当于声明，然后在.cpp中定义和初始化
                                        // static const 静态成员就表示在这里定义和初始化了
    };
    
    class A
    {
        public:
            static int teset()
            {
                return 0;
            }
    };
    
    class B
    {
        public:
            static int teset()
            {
                return 12.4f;
            }
    };
    
    template<typename T>
    auto ftestClass()
    {
        auto value = T::teset();
        return auto;
    }
    
    void func()
    {
        // auto 类型 std::initializer_list的特殊推断
        // 初始化变量
        int x = 100;    // C++98
        int x2(100);    // c++98
        int x3 = {300}; // c++11    x3 = int 
        int x4{400}; // c++11
        // 替换为auto
        auto x = 100;       //  x = int;值为100
        auto x2(100);       //  x2 = int; 值为100； 
        auto x3 = {300};    //  x3 = std::initializer_list<int>; 值300。auto遇到 = {}的时候，推导规则就变得不一样了
        auto x4{400};       //  x4 = int； 值400
        
        // std::initializer_list是c++11引入的新类型（他也是一个类模板）
        // 用来表示某种特定的值的数组，（和vector有点类似）
        // auto x6 = {20, 323, 34}; // 可以推导
        // auto x7 = {20, 45.32, 23}; // 类型不一致，无法推导
        // auto在遇到 auto x7 = {20, 45.32, 23};这种情况的时候，实际上是进行了两次推导，
        // 第一次将x7推导为 std::initializer_list
        // 第二次根据实参进行类型模板参数 类型推断，所以在遇到大括号里的实参类型不一致的情况，就会推导失败
        
        // auto x3 = {30};  这是一个针对auto的特殊推导，能推导出std::initializer_list这种类型来
        // 这种推导只适合auto，不适合模板类型。这一点是auto类型推导和模板类型推导不一样的地方
        // 其他反面auto类型推导和模板类型推导都差不多
        
        // tmfunc({2324}); // 编译报错。
        // 但是如果在函数模板上做文章，将其改为
        // void tmfunc(std::initializer_list<T> t)
        // 就可以正常编译通过  此时t的类型就是std::initializer_list<int>； T的类型就是 int
        tmfunc({2324});
        
        // auto还可以放在函数返回位置上
        
        
        
        // auto不适用场合举例
        // 1）auto不能用于函数参数； 比如 void func(auto v1, int v2) {} // 非法
        // 2）普通成员变量不可以是auto类型，static const auto m_si = 12；静态成员类型是被允许的
        
        
        // auto适用场合举例
        std::map<string, int> mymaop;
        mymaop.insert({"sds", 12});
        mymaop.insert({"sds1", 12});
        mymaop.insert({"sds2", 12});
        mymaop.insert({"sds3", 12});
        mymaop.insert({"sds4", 12});
        std::map<string, int>::iterator iter;
        for(iter = mymaop.begin(); iter != mymaop.end(); ++iter)
        {
            std::cout << iter->first << iter->second << std::endl;
        }
        for(auto iter2 = mymaop.begin(); iter2 != mymaop.end(); ++iter2)
        {
            std::cout << iter2->first << iter2->second << std::endl;
        }
        
        // 当有些场合我们无法确定类型时，我们此时使用auto也能派上用场
        std::cout << ftestClass<A>() << std::endl;  // 调用A类的teset()，并返回int
        std::cout << ftestClass<B>() << std::endl;  // 调用B类的teset()，并返回double
        
    }
}

int main()
{
    
    // _nmsp1::func();
    _nmsp2::func();
    
    return 0;
}


```