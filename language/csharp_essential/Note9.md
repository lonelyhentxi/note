# 第8章 值类型

---

## 8.2 装箱

值类型变量直接包含它们的数据，引用类型的变量包含对一个储存位置的引用。当值类型的变量转换成为它实现的某个接口或者object时，转换的结果表面上是对一个储存位置的引用，实际上包含值类型的值。这种特殊的行为：称为装箱。

转换涉及以下的步骤：

1. 在堆上分配内存，用于存放值类型的数据以及少量的额外开销（SyncBlockIndex和方法表指针）。这些额外开销使得对象看起来像引用类型的托管对象实例。
2. 接着发生一次内存复制，当前储存位置的值类型数据被复制到堆上分配好的位置。
3. 转换的结果是对堆上的新储存位置的引用。

相反的过程称之为拆箱（UNboxing）。拆箱转换先检查已装箱的值得类型兼容于要拆箱成的类型的值。

装箱和拆箱之所以重要，是因为它们会影响性能和行为：如果装箱和拆箱行为进行的不是非常频繁，那么它们实现的性能问题不大。但如果装箱问题在被忽视的情况下频繁发生，那么这就可能大幅度影响性能。ArrayList类型维护的是对象引用列表，所以在其中添加值类型对值进行装箱再引用。

```csharp
//容易忽视的box和unbox指令
class DisplayFibonacci
{
    static void Main()
    {
        int totalCount;
        System.Collections.ArrayList list=
            new System.Collections.ArrayList();

        Console.Write("Enter a number between 2 and 1000:");
        totalCount = int.Parse(Console.ReadLine());

        //Excution-time error;
        //list.Add(0); //Cast to double or 'D' suffix required.
                       //Whether cast or using 'D' suffix;
                       //CIL is identical.
        list.Add((double)0);
        list.Add((double)1);
        for(int count = 2; count < totalCount; count++)
        {
            list.Add((double)list[count-1]+(double)list[count-2]);
        }

        foreach(double count in list)
        {
            Console.Write("{0},",count);
        }
    }
}
```

- 在以上的代码中，含有很多的装箱操作，每个操作都涉及内存的分配和复制，造成了很大的资源浪费，可以使用泛型或者对象来避免内存分配和类型检查。
- 和c++类似（雾？），在涉及隐式转换的时候，只能发生有限次的转换。显式的声明也有限制，在装箱后类型转换为其基础类型可以转换成的类型时，需要先转换成该基础类型。否则会引发InvalidCastException异常。

**高级主题：lock语句中的值类型**

c#支持用于同步代码的lock语句：
- 该语句实际会编译成为System.Threading.Monitor的Enter()和Exit()方法。
- 这两个方法必须成对调用。Enter()记录由其唯一的引用类型参数传递的一个lock。
- 值类型的问题在于装箱。每次调用Enter()或者Exit()时，都会在堆上创建新的值，因此是将一个副本的引用同另一个副本的引用进行比较，总是返回false，因此无法将对应的两者钩到一起，因此，不允许在lock()语句中使用值类型。

ps：在修改装箱后的值类型时，修改的是值在堆上的副本，而不会引起自己本身的变化，而且在拆箱时，这个副本会被丢弃，这就是编码规范要求值类型为不可变类型的最重要原因之一。

**高级主题：如何避免方法在调用期间装箱**

在接收者是值类型的情况下调用方法，接收者必须是变量而不是值本身，因为方法可能尝试修改接收者。显然，它必须修改接收者的储存位置而不是副本的位置再丢弃该副本。



