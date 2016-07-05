# WhereIsString 

**Java的字符串存储在哪里呢？**

Java源码:

```java
class WhereIsString {

  private String strA = "IamString";
  private String strB = new String("IamString");

  public void foo() {
    String strC = new String("IamString");
  }
}
```

执行`make dumpclass`后,可以打印出class文件, 在常量池中，我们可以找到如下常量:

```class
#2 = String             #20            //  IamString
#20 = Utf8              IamString
```

一个String类型的常量，其内容索引至UTF8类型, 而UTF8类型的常量值为"IamString",
就是在源码中反复被使用的字符串.

这个字符串是如何被使用的呢？

```class
WhereIsString();
  flags:
  Code:
    stack=4, locals=1, args_size=1
       0: aload_0       
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: aload_0       
       5: ldc           #2                  // String IamString
       7: putfield      #3                  // Field strA:Ljava/lang/String;
      10: aload_0       
      11: new           #4                  // class java/lang/String
      14: dup           
      15: ldc           #2                  // String IamString
      17: invokespecial #5                  // Method java/lang/String."<init>":(Ljava/lang/String;)V
      20: putfield      #6                  // Field strB:Ljava/lang/String;
      23: return        
...
public void foo();
  flags: ACC_PUBLIC
  Code:
    stack=3, locals=2, args_size=1
       0: new           #4                  // class java/lang/String
       3: dup           
       4: ldc           #2                  // String IamString
       6: invokespecial #5                  // Method java/lang/String."<init>":(Ljava/lang/String;)V
       9: astore_1      
      10: return  
```

涉及到"IamString"字符串使用的地方都有这么一条指令：

```
ldc  #2
```

表示从常量池(Constants Pool)的2号位置加载数据, 即从同样的地方取到了"IamString"。
这表示，该类中不同的String虽然是不同对象，但其内容却是相同的，都指向常量池的同一个地方。
