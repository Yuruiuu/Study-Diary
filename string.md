# std::string的初始化
```
stdstring 变量名称{“字符串”,要截取的长度};
stdstring str{"123456”，3};
str= "123"
//注*中文时支持不佳
    

stdstring 变量名称{“字符串”,起始位置，截取长度};
stdstring str{"123456”，1，4};
str= "234"
//注*中文时支持不佳
    

std:string变量名称(要复制的个数,'字符');
stdstring str(6, 'a')；
str= "aaaaaa"
```

# std:string连接字符串

```
std::string str(6, 'a’);
str=str+"123 ";
str= "aaa123"
str="sss"+"sss";//错误  右边一定要有一个str变量
str=str+"sss"+"sss";
```

# std:string连接字符串和数字
语法
```
std:string std:to_string(数字);
std::string str=std:to_string(123);
str=“123"


int age;cin age;
string ls="用户的年龄是:";
string str=ls+to_string(age);
```

# 深入探讨std::string的连接
```
std:string str;
str=“123"+"456";//错误
//变通方法:
str=string{“123"}+”456";
str="456"+"123"+string{"123"};//错误   先算"456"+"123"就出错了
str=“456”+("123"+string{" 123"});


str+="abc"+"sss";//出错，+优先级高于+=


//变通方法
str="123""456";//两个都得是常量
str="123"string{"456"};//错误


string str{"123"};
char a;cin>>a;
str+=a+'o';//等价于str+=char(a+'o'}，char类型可以相加
```
# .append()连接字符串
```
std:string str;
.append();方法可以连接一个字符串
str.append("s");


string str{"123"};
str.append("456"}.append{"789"};//可以无限append


//append可以使用初始化的方式
string str{"123"};
str.append("456",1);//1234
str.append("456789",2,5);//1236789
```

# .substr()截取字符串
.substr(起始位置,要截取的长度);
```
std:string str{ "123456"};
//不输入终止位置，默认截取到最后
std::string strsub{str.substr(1)};//strsub=“23456"
std::string strsubA{str.substr(1,3)};//strsubA=“234”

//可以连环截取
str.substr(7).substr(3);
.length()长度
std:string.length();可以得到string的长度
std:string str{"123”};
str.length()将回返回3
//注*中文支持不良
```

# string字符串的比较
```
//string字符串可以用比较运算符和另外一个string字符串或者C标准的字符串(char*)来进行比较
//支持的比较运算符有==!=>< >=<=
//比较的原则为依次进行字符大小的比较
//比如
string strA{ "abcd"string strB{ "bcde”};
//结果为strB>strA
```

# .compare()字符串的比较
.compare()是string类型目带的一个方法,可以用来比较与另一个string或C标准字符串的大小, 比较完成后返回的是一个int类型的值;
比如:
```
string strA{ "abcd"};
strA.compare( "abcd”);//返回0
strA.compare( "bcdef");//返回负数
strA.compare( "ABCD");//返回正数
//因此这样的写法是得不到你想要的结果的
if(strA.compare("abcd”))//错误
```
# .find()字符串的搜索
.find()是string类型自带的一个方法,用来搜索字符串种的内容,并且返回内容所在的位置,当返回值为std:string:npos时,表示未找到
语法:
```
.find(要搜索的内容，开始搜索的位置);
.find(要搜索的内容,开始搜索的位置,要纳入搜索的长度);
string strA{"abcd cdef ghijk” };
strA.find("cdef”);//返回值为5
strA.find("cdef”,6);//返回值std:string::npos
strA.find("cdef”,0.2);//返回值为2等同于从位置0开始搜索cd
```
> std::string::npos是一个常量，它表示std::string中的一个特殊值。它通常用于表示查找操作失败或没有匹配项的情况。
> npos的值是一个std::string::size_type类型的静态常量，其值通常是一个非常大的数（例如，4294967295或18446744073709551615，取决于实现）。
> 在std::string的成员函数中，如果查找操作没有找到所需的子串，那么它将返回std::string::npos。
# .insert()插入字符串
.insert() insert是string类型的一个方法,可以在一个string字符串的指定位置插入另一个字符串;
基本语法:
```
.insert(要插入的位置,要插入的字符串);
string id{ "id=;"};
id.insert(3,"testid");//id的值为id=testid;
.insert(要插入的位置,要插入的字符个数,要插入的字符);
string id{"id=;"};
id.insert(3,6,'*');//id的值为id=******;
```
# .insert()的扩展用法
基本语法:
```
.insert(要插入的位置,要插入的字符串,要插入的字符串的起始位置,要插入的大小);
string id{"id=;"};
id.insert(3,"killertestid123456",6,6);//id的值为id=testid;
.insert(要插入的位置,要插入的字符串,要插入的大小);
string id{"id=;"};
id.insert(3,"killertestid123456",6);//id的值为id=killer;
```
# string内存重新分配
```
int main()
{
	string str{ "123" };
	cout << (int)&str <<" "<< (int)&str[0] <<" "<< (int)&str[1] << endl;
	str += "45678910111213141516";
	cout << (int)&str << " " << (int)&str[0] << " " << (int)&str[1] << endl;
}

19920928 19920932 19920933
19920928 24296568 24296569
//str地址不变，之后因为加了字符串之后放不下，内存重新分配了
```
# .c_str()   .data()
在内存里的布局,与C字符串(char*)不同的是,C字符串以0结尾, 而string字符串由于有专门记录其长度的属性, 在实现的时候,并没有严格要求是否以0结尾!
但是在C++11标准推出后,要求string字符串也以0结尾
通过.c_str()方法可以得到一个const char* 的指针,指向str储存字符数据的内存空间(常量指针，值不能修改）
通过.data()方法可以得到一个const char* 的指针,指向str储存字符数据的内存空间(常量指针，值不能修改）
在C++17标准下，.data()方法得到的是一个char* 的指针
# .replace()替换字符串的内容
.replace()是string类型提供的一种方法,可以替换string字符串中的内容;
语法
```
.replace(要替换的内容起始位置,要替换的长度,“替换后的内容");
string str{"id=user;"];
str.replace(3,4,"zhangsan");//str的内容为:id=zhangsan;
.replace(要替换的内容起始位置,要替换的长度,替换后的字符长度,'字符);
str.replace(3,4,6,'*');
str的内容为:id=******。
```
# .erase(), .clear()删除字符串的内容
.erase()是string类型提供的一种方法,可以删除string字符串中的内容;
语法

```.erase(要删除的起始位置,要删除的起始长度);
string str{"id=user;"};
str.erase(3,4);
str的内容为:id=;
.erase(要删除的起始位置);从起始位置开始删除所有内容
string str{ "id=user;"};
str.erase(3);
str的内容为:id=
```
.erase();删除字符串所有内容.
clear();删除字符串所有内容

# 字符串
字符的储存：字符→编码表→二进制数字
字符的读取：二进制数字→编码表→字符
字符编码表:
UNICODE GBK ASCII等等
