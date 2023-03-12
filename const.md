# const
+ const
+ const_cast<>
+ mutable
  
## const
const修饰的对象不能修改其成员变量的值，只能调用const成员函数

+ const修饰的类的成员变量不能修改
```c++
const Role user;
user.hp=2;//报错
```
+ 常量指针指向的值不能修改
```c++
const Role* puser{&user};//常量指针
puser->damage=2;//报错

Role monster;
const Role* p{&monster};
p->damage=2;//报错，虽然Role没有被const修饰，但指针是常量指针
```
+ const 对象只能调用const成员函数
```c++
//Role.h
class Role
{
public:
	int hp;
	int GetHP() const;  //const修饰
};


//Role.cpp
int Role::GetHP() const
{
	return hp;
}


//test.cpp
int main()
{
	Role user;
    const Role* puser{&user};
    puser->GetHp();//√
}
```
const原则：
const对象不能以任何方式改变,这是const的原则,在这个基本原则下,产生了一些列效应,比如const对象只能调用const成员函数;

另外一个我们不注意的变化是,在const成员函数下,this指针也变成了const指针

```c++
int Role::GetHp() const
{
    this->damage=2;//报错
    return hp;
}
```
```c++
Role.c
class Role
{
	int lv;
	int& GetLv() const;
};

Role.cpp
int& Role::GetLv() const
{
    return lv;  //报错，因为返回引用可能导致值被修改
}
_____________________________________分割线________________________________________

解决方法1：不返回引用，返回值

_____________________________________分割线________________________________________

解决方法2：返回const引用

Role.c
class Role
{
	int lv;
	const int& GetLv() const;
};

Role.cpp
const int& Role::GetLv() const//返回值被const修饰就不能被修改了
{
    return lv;
}
```
+ 利用函数重载
```c++
Role.c
class Role
{
	int lv;
	int GetLv() const;
	int GetLv();
};

Role.cpp
int Role::GetLv() const
{
    return lv; 
}
int Role::GetLv()
{
    return lv;
}
```
+ 强行改变const
```c++
void test(Role* p)
{
    p->SetHp(5000);
}

int main()
{
    const Role user;
    test((Role*)(&user));
}
```
+ const_cast<type>强制转换
```c++
void test(Role* p)
{
    p->SetHp(5000);
}

int main()
{
    const Role user;
    test(const_cast<Role*>(&user));//&user转换成Role*
}
```

## const_cast<>
```c++
//const类型转换
const_cast<类型(变量);
Role user;
const_cast<Role*> (puser);
//const_cast 可以将一个const变量的常量属性去掉,只有在极少数情况下需要用到const_cast
例如:
void test(Role* role)
{}
const Role user;
const Role* puser{&user};
test(const_cast<Role*>(puser))
//注*确保test函数不会改变对象的内容
```
## mutable
mutable声明的成员变量可以被const成员函数修改
```c++
//mutable声明的成员变量可以被const成员函数修改
class Role
{
private:
	int hp;
	mutable int getHPCount;
public:
	int GetHP() const
} ;
int Role::GetHP() const
{
	getHPCont++;
    return hp;
}
```
