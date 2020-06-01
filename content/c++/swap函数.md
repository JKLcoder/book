&emsp;&emsp;标准库中已经为我们定义了 swap 函数，用于完成两个元素交换,其基本实现如下：

```c++
T temp = v1;
v1 = v2;
v2 = temp;
```

&emsp;&emsp;但是考虑存在下面这个类：

```c++
class HasPtr{
private:
std::string *ps;
int i;
};
```

&emsp;&emsp;很明显，由于 std::string \*ps 的存在，如果我们对 HasPtr 使用 swap 函数,由于调用了拷贝赋值运算符对指针成员进行深拷贝，我们将会为 temp 重新分配一个 std::string 空间，并将其赋值给了 v2，但这并不是我们所需要的，我们所需要的应该是将 v1 中 ps 所指向的空间赋值给 v2 中的 ps；   
&emsp;&emsp;所以，标准库中的 swap 函数并不能实现我们的目的，因此我们需要为 HasPtr 自定义一个 swap 函数；  
&emsp;&emsp;在 c++中，如果我们为类定义了自定义版本的 swap 函数，那么将会使用自定义版本的 swap 函数，否则就睡使用标准库版本的 swap 函数；但这应当是基于我们能够正确的定义 swap 函数的基础上；

# 为类自定义 swap 函数

&emsp;&emsp;针对前面的 HasPtr 类，我们可以像下面这样定义 swap 函数：

```c++
Class HasPtr{
    firend void swap(HasPtr &lhs,HasPtr &rhs);
};
inline void swap(HasPtr &lhs,Has &rhs)
{
    using std::swap; //#1
    swap(lhs.ps,rhs.ps);
    swap(lhs.i,rhs.i);
}
```

&emsp;&emsp;需要注意的是，上述代码中的每个 swap 调用都应该是未加限定的；在 c++中，如果存在类型特定的 swap 版本，其匹配程度会优于 std 中定义的版本，同时我们也不需要担心上述代码中的#1 会隐藏 HasPtr 的 swap 版本；

# 在赋值运算符中使用 swap 函数

&emsp;&emsp;swap 函数在赋值运算符有很好的用武之地，看下述代码：

```c++
HasPtr& HasPtr::operator=(HasPtr rhs)
{
    swap(*this,rhs);
    return *this;
}
```

&emsp;&emsp;在上述代码中，我们调用 swap 函数来交换 rhs 和\*this 中的数据成员，swap 函数将 rhs 中原来保存的指针存入了\*this 中，而 rhs 则指向了本对象曾静使用的内存，但是由于其是临时变量，所以赋值运算符结束时，rhs 被销毁，析构函数执行，rhs 现在指向的内存就被 delete 掉了；  
&emsp;&emsp;其相对于传统的赋值运算符的优势在于：其天然就是异常安全的，并且不需要进行自赋值判断；

# swap 函数的应用场景

&emsp;&emsp;与拷贝控制成员不同，swap 并不是必须要的，但是对于分配了资源的类，定义 swap 函数是一种很重要的优化手段；
