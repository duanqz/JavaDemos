Demo WhereIsString 用来回答Java的字符串存储在哪里的问题

结论: Java的字符串都是存储在常量区(Constant Pool), 即存储在Class文件之中.

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

执行make dumpclass后,可以打印出class文件,截取片段如下:

在Constant Pool中可以看到一个String类型的常量,其内容索引至UTF8类型, UTF8类型的常量值为
"IamString", 就是我们在源码中反复使用的字符串:

```class
#2 = String             #20            //  IamString
#20 = Utf8              IamString
```

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

在构造函数中,可以看到字符串的初始化代码,第5条指令用于初始化strA,第15条指令用于初始化strB,
可以看到都从常量池(Constants Pool)的2号索引来取值, 即从同样的地方取到了"IamString";
在成员函数中,也使用了"IamString",同样是从常量池的2号索引取到了"IamString".
