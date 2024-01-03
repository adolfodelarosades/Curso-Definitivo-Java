# Appendix A. Bytecode

## WaitFor

```java
package jdk8.byte_code;

class WaitFor {
    static void waitFor(Condition condition) throws    
    InterruptedException {
        while (!condition.isSatisfied())
            Thread.sleep(250);
    }

    static <T> void waitFor(T input, Predicate<T> predicate)
            throws InterruptedException {
        while (!predicate.test(input))
            Thread.sleep(250);
    }
}
```

## Example 1

```java
package jdk8.byte_code;

import static jdk8.byte_code.WaitFor.waitFor;

@SuppressWarnings("all")
public class Example1 {

    // anonymous class
    void example() throws InterruptedException {
        waitFor(new Condition() {
            @Override
            public Boolean isSatisfied() {
                return true;
            }
        });
    }
}
```

```sh
Classfile Example1.class
 Last modified 08-May-2014; size 603 bytes
 MD5 checksum 7365ca98fe204fc9198043cef5d241be
 Compiled from "Example1.java"
public class jdk8.byte_code.Example1
  SourceFile: "Example1.java"
  InnerClasses:
         #2; //class jdk8/byte_code/Example1$1
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
  #1 = Methodref #6.#20 // java/lang/Object." <init>":()V 
  #2 = Class #21 // jdk8/byte_code/Example1$1 
  #3 = Methodref #2.#22 // jdk8/byte_code/Example1$1." <init>":(Ljdk8/byte_code/Example1;)V 
  #4 = Methodref #23.#24 // 
jdk8/byte_code/WaitFor.waitFor:(Ljdk8/byte_code/Condition;)V
 #5 = Class #25 // jdk8/byte_code/Example1
 #6 = Class #26 // java/lang/Object
 #7 = Utf8 InnerClasses
 #8 = Utf8 <init>
 #9 = Utf8 ()V 
 #10 = Utf8 Code
 #11 = Utf8 LineNumberTable
 #12 = Utf8 LocalVariableTable 
 #13 = Utf8 this 
 #14 = Utf8 Ljdk8/byte_code/Example1; 
 #15 = Utf8 example 
 #16 = Utf8 Exceptions 
 #17 = Class #27 // java/lang/InterruptedException 
 #18 = Utf8 SourceFile 
 #19 = Utf8 Example1.java 
 #20 = NameAndType #8:#9 // "":()V 
 #21 = Utf8 jdk8/byte_code/Example1$1 
 #22 = NameAndType #8:#28 // "":(Ljdk8/byte_code/Example1;)V 
 #23 = Class #29 // jdk8/byte_code/WaitFor 
 #24 = NameAndType #30:#31 // waitFor:(Ljdk8/byte_code/Condition;)V 
 #25 = Utf8 jdk8/byte_code/Example1 
 #26 = Utf8 java/lang/Object 
 #27 = Utf8 java/lang/InterruptedException 
 #28 = Utf8 (Ljdk8/byte_code/Example1;)V 
 #29 = Utf8 jdk8/byte_code/WaitFor 
 #30 = Utf8 waitFor 
 #31 = Utf8 (Ljdk8/byte_code/Condition;)V
{ 
 public jdk8.byte_code.Example1();
   descriptor: ()V
   flags: ACC_PUBLIC
   Code:
     stack=1, locals=1, args_size=1
        0: aload_0
        1: invokespecial #1 // Method java/lang/Object."<init>":()V 
        4: return
     LineNumberTable:
       line 6: 0 
     LocalVariableTable:
       Start Length Slot Name Signature
           0     5     0   this Ljdk8/byte_code/Example1;
 void example() throws java.lang.InterruptedException;
    descriptor: ()V
    flags:
    Code:
      stack=3, locals=1, args_size=1
         0: new           #2         // class  
         jdk8/byte_code/Example1$1
         3: dup
         4: aload_0
         5: invokespecial #3        // Method   
 jdk8/byte_code/Example1$1."<init>":(Ljdk8/byte_code/Example1;)V
         8: invokestatic  #4                  // Method jdk8/byte_code/WaitFor.waitFor:(Ljdk8/byte_code/Condition;)V
        11: return
      LineNumberTable:
        line 10: 0
        line 16: 11
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      12     0  this   Ljdk8/byte_code/Example1;
    Exceptions:
      throws java.lang.InterruptedException
}
```

## Example 2

```java
package jdk8.byte_code;

public interface Server {

  Boolean isRunning();

  public class HttpServer implements Server {
    @Override
    public Boolean isRunning() {
      return false;
    }
  }
}


package jdk8.byte_code;

import static jdk8.byte_code.Server.*;
import static jdk8.byte_code.WaitFor.waitFor;

public class Example2 {

    // anonymous class (closure)
    void example() throws InterruptedException {
        Server server = new HttpServer();
        waitFor(new Condition() {
            @Override
            public Boolean isSatisfied() {
                return !server.isRunning();
            }
        });
    }
}
```

```sh
Classfile Example2.class
 Last modified 08-May-2014; size 775 bytes
 MD5 checksum 2becf3c32e2b08abc50465aca7398c4b
 Compiled from "Example2.java"
 public class jdk8.byte_code.Example2
 SourceFile: "Example2.java"
 InnerClasses:
     #4; //class jdk8/byte_code/Example2$1
     public static #27= #2 of #25; //HttpServer=class jdk8/byte_code/Server$HttpServer of class jdk8/byte_code/Server
 minor version: 0
 major version: 52
 flags: ACC_PUBLIC, ACC_SUPER
 Constant pool:
 #1 = Methodref     #8.#24     // java/lang/Object."<init>":()V
 #2 = Class         #26        // jdk8/byte_code/Server$HttpServer
 #3 = Methodref     #2.#24     // jdk8/byte_code/Server$HttpServer."<init>":()V    
 #4 = Class         #28        // jdk8/byte_code/Example2$1 
 #5 = Methodref     #4.#29     // jdk8/byte_code/Example2$1."<init>":(Ljdk8/byte_code/Example2;Ljdk8/byte_code/Server;)V 
 #6 = Methodref     #30.#31    // jdk8/byte_code/WaitFor.waitFor:(Ljdk8/byte_code/Condition;)V 
 #7 = Class         #32        // jdk8/byte_code/Example2
 #8 = Class         #33        // java/lang/Object
 #9 = Utf8          InnerClasses
 #10 = Utf8         <init>
 #11 = Utf8         ()V
 #12 = Utf8         Code 
 #13 = Utf8         LineNumberTable 
 #14 = Utf8         LocalVariableTable 
 #15 = Utf8         this 
 #16 = Utf8         Ljdk8/byte_code/Example2; 
 #17 = Utf8         example 
 #18 = Utf8         server 
 #19 = Utf8         Ljdk8/byte_code/Server; 
 #20 = Utf8         Exceptions 
 #21 = Class        #34          // java/lang/InterruptedException 
 #22 = Utf8         SourceFile
 #23 = Utf8         Example2.java 
 #24 = NameAndType  #10:#11      // "<init>":()V 
 #25 = Class        #35          // jdk8/byte_code/Server 
 #26 = Utf8         jdk8/byte_code/Server$HttpServer 
 #27 = Utf8         HttpServer 
 #28 = Utf8         jdk8/byte_code/Example2$1 
 #29 = NameAndType  #10:#36      // "<init>":(Ljdk8/byte_code/Example2;Ljdk8/byte_code/Server;)V 
 #30 = Class        #37          // jdk8/byte_code/WaitFor 
 #31 = NameAndType  #38:#39      // waitFor:(Ljdk8/byte_code/Condition;)V
 #32 = Utf8         jdk8/byte_code/Example2 
 #33 = Utf8         java/lang/Object
 #34 = Utf8         java/lang/InterruptedException
 #35 = Utf8         jdk8/byte_code/Server
 #36 = Utf8         (Ljdk8/byte_code/Example2;Ljdk8/byte_code/Server;)V
 #37 = Utf8         jdk8/byte_code/WaitFor 
 #38 = Utf8         waitFor 
 #39 = Utf8         (Ljdk8/byte_code/Condition;)V 
{
 public jdk8.byte_code.Example2();
 descriptor: ()V
 flags: ACC_PUBLIC
 Code:
   stack=1, locals=1, args_size=1
     0: aload_0
     1: invokespecial #1        // Method java/lang/Object."<init>":()V
     4: return
     LineNumberTable:
       line 6: 0 
     LocalVariableTable:
     Start Length Slot Name Signature
         0      5    0 this Ljdk8/byte_code/Example2;
 void example() throws java.lang.InterruptedException;
    descriptor: ()V
    flags:
    Code:
      stack=4, locals=2, args_size=1
         0: new           #2   //class jdk8/byte_code/Server$HttpServer
         3: dup
         4: invokespecial #3   //Method jdk8/byte_code/Server$HttpServer."<init>":()V
         7: astore_1
         8: new           #4   // class jdk8/byte_code/Example2$1
        11: dup
        12: aload_0
        13: aload_1
        14: invokespecial #5   // Method jdk8/byte_code/Example2$1."<init>":(Ljdk8/byte_code/Example2;Ljdk8/byte_code/Server;)V
        17: invokestatic  #6   // Method jdk8/byte_code/WaitFor.waitFor:(Ljdk8/byte_code/Condition;)V
        20: return
      LineNumberTable:
        line 10: 0
        line 11: 8
        line 17: 20
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      21     0  this   Ljdk8/byte_code/Example2;
            8      13     1 server   Ljdk8/byte_code/Server;
    Exceptions:
      throws java.lang.InterruptedException
}
```

## Example 3

```java
package jdk8.byte_code;

import static jdk8.byte_code.WaitFor.waitFor;

public class Example3 {
    // simple lambda
    void example() throws InterruptedException {
        waitFor(() -> true);
    }
}
```

```sh
Classfile Example3.class
  Last modified 08-May-2014; size 1155 bytes
  MD5 checksum 22e120de85528efc921bb158588bbaa1
  Compiled from "Example3.java"
public class jdk8.byte_code.Example3
  SourceFile: "Example3.java"
  InnerClasses:
      public static final #50= #49 of #53; //Lookup=class java/lang/invoke/MethodHandles$Lookup of class java/lang/invoke/MethodHandles
  BootstrapMethods:
    0: #23 invokestatic java/lang/invoke/LambdaMetafactory.metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite;
  Method arguments:
    #24 ()Ljava/lang/Boolean;
    #25 invokestatic jdk8/byte_code/Example3.lambda$example$25:()Ljava/lang/Boolean;
    #24 ()Ljava/lang/Boolean;
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
 #1 = Methodref        #6.#21    // java/lang/Object."<init>":()V
 #2 = InvokeDynamic    #0:#26    // #0:isSatisfied:()Ljdk8/byte_code/Condition; 
 #3 = Methodref        #27.#28   // jdk8/byte_code/WaitFor.waitFor:(Ljdk8/byte_code/Condition;)V 
 #4 = Methodref        #29.#30   // java/lang/Boolean.valueOf:(Z)Ljava/lang/Boolean; 
 #5 = Class            #31       // jdk8/byte_code/Example3 
 #6 = Class            #32       // java/lang/Object 
 #7 = Utf8             <init> 
 #8 = Utf8             ()V 
 #9 = Utf8             Code 
 #10 = Utf8            LineNumberTable 
 #11 = Utf8            LocalVariableTable 
 #12 = Utf8            this 
 #13 = Utf8            Ljdk8/byte_code/Example3; 
 #14 = Utf8            example 
 #15 = Utf8            Exceptions 
 #16 = Class           #33       // java/lang/InterruptedException 
 #17 = Utf8            lambda$example$25 
 #18 = Utf8            ()Ljava/lang/Boolean; 
 #19 = Utf8            SourceFile 
 #20 = Utf8            Example3.java 
 #21 = NameAndType     #7:#8     // "<init>":()V 
 #22 = Utf8            BootstrapMethods 
 #23 = MethodHandle    #6:#34    // invokestatic java/lang/invoke/LambdaMetafactory.metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite; 
 #24 = MethodType      #18       // ()Ljava/lang/Boolean;
 #25 = MethodHandle    #6:#35    // invokestatic jdk8/byte_code/Example3.lambda$example$25:()Ljava/lang/Boolean; 
 #26 = NameAndType     #36:#37   // isSatisfied:()Ljdk8/byte_code/Condition;     
 #27 = Class           #38       // jdk8/byte_code/WaitFor 
 #28 = NameAndType     #39:#40   // waitFor:(Ljdk8/byte_code/Condition;)V 
 #29 = Class           #41       // java/lang/Boolean 
 #30 = NameAndType     #42:#43   // valueOf:(Z)Ljava/lang/Boolean; 
 #31 = Utf8            jdk8/byte_code/Example3 
 #32 = Utf8            java/lang/Object
 #33 = Utf8            java/lang/InterruptedException 
 #34 = Methodref       #44.#45   // java/lang/invoke/LambdaMetafactory.metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite; 
 #35 = Methodref       #5.#46    // jdk8/byte_code/Example3.lambda$example$25:()Ljava/lang/Boolean; 
 #36 = Utf8            isSatisfied 
 #37 = Utf8            ()Ljdk8/byte_code/Condition;
 #38 = Utf8            jdk8/byte_code/WaitFor
 #39 = Utf8            waitFor
 #40 = Utf8            (Ljdk8/byte_code/Condition;)V 
 #41 = Utf8            java/lang/Boolean 
 #42 = Utf8            valueOf 
 #43 = Utf8            (Z)Ljava/lang/Boolean;
 #44 = Class           #47       // java/lang/invoke/LambdaMetafactory    
 #45 = NameAndType     #48:#52   // metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite; 
 #46 = NameAndType     #17:#18  // lambda$example$25:()Ljava/lang/Boolean;  
 #47 = Utf8            java/lang/invoke/LambdaMetafactory 
 #48 = Utf8            metafactory 
 #49 = Class           #54      // java/lang/invoke/MethodHandles$Lookup 
 #50 = Utf8            Lookup 
 #51 = Utf8            InnerClasses 
 #52 = Utf8 (Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite; 
 #53 = Class           #55        // java/lang/invoke/MethodHandles
 #54 = Utf8            java/lang/invoke/MethodHandles$Lookup 
 #55 = Utf8            java/lang/invoke/MethodHandles 
{ 
public jdk8.byte_code.Example3();
 descriptor: ()V
 flags: ACC_PUBLIC
 Code:
  stack=1, locals=1, args_size=1
    0: aload_0
    1: invokespecial #1           // Method java/lang/Object."<init>":()V 
    4: return
 LineNumberTable:
 line 6: 0
 LocalVariableTable:
 Start Length Slot Name Signature
     0      5    0 this Ljdk8/byte_code/Example3;
void example() throws java.lang.InterruptedException;
    descriptor: ()V
    flags:
    Code:
      stack=1, locals=1, args_size=1
         0: invokedynamic #2,  0    // InvokeDynamic #0:isSatisfied:()Ljdk8/byte_code/Condition;
         5: invokestatic  #3       // Method jdk8/byte_code/WaitFor.waitFor:(Ljdk8/byte_code/Condition;)V
         8: return
      LineNumberTable:
        line 10: 0
        line 11: 8
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       9     0  this   Ljdk8/byte_code/Example3;
    Exceptions:
      throws java.lang.InterruptedException
}
```

## Example 4

```java
package jdk8.byte_code;

import static jdk8.byte_code.Server.HttpServer;
import static jdk8.byte_code.WaitFor.waitFor;

public class Example4 {
    // lambda with arguments
    void example() throws InterruptedException {
        waitFor(new HttpServer(), (server) -> server.isRunning());
    }
}
```

```sh
Classfile Example4.class
   Last modified 08-May-2014; size 1414 bytes
   MD5 checksum 7177f97fdf30b0648a09ab98109a479c
   Compiled from "Example4.java"
 public class jdk8.byte_code.Example4
   SourceFile: "Example4.java"
   InnerClasses:
     public static #21= #2 of #29; //HttpServer=class jdk8/byte_code/Server$HttpServer of class jdk8/byte_code/Server 
     public static final #65= #64 of #67; //Lookup=class java/lang/invoke/MethodHandles$Lookup of class java/lang/invoke/MethodHandles 
  BootstrapMethods:
    0: #32 invokestatic java/lang/invoke/LambdaMetafactory.metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite;
 Method arguments:
   #33 (Ljava/lang/Object;)Z
   #34 invokestatic jdk8/byte_code/Example4.lambda$example33 : (L
j
d
k8/b
y
t
e
c
o
d
e/S
e
r
v
e
rHttpServer;)Z
   #35 (Ljdk8/byte_code/Server$HttpServer;)Z
 minor version: 0
 major version: 52
 flags: ACC_PUBLIC, ACC_SUPER
 Constant pool:
 #1 = Methodref         #9.#28     //java/lang/Object."<init>":()V    
 #2 = Class             #30        //jdk8/byte_code/Server$HttpServer   
 #3 = Methodref         #2.#28     //jdk8/byte_code/Server$HttpServer."<init>":()V 
 #4 = InvokeDynamic     #0:#36     // #0:test:()Ljava/util/function/Predicate;   
 #5 = Methodref         #37.#38    // jdk8/byte_code/WaitFor.waitFor:(Ljava/lang/Object;Ljava/util/function/Predicate;)V 
 #6 = Methodref         #2.#39    // jdk8/byte_code/Server$HttpServer.isRunning:()Ljava/lang/Boolean;
 #7 = Methodref         #40.#41   // java/lang/Boolean.booleanValue:()Z   
 #8 = Class             #42       // jdk8/byte_code/Example4 
 #9 = Class             #43       //java/lang/Object 
 #10 = Utf8             <init>
 #11 = Utf8             ()V 
 #12 = Utf8             Code 
 #13 = Utf8             LineNumberTable 
 #14 = Utf8             LocalVariableTable 
 #15 = Utf8             this 
 #16 = Utf8             Ljdk8/byte_code/Example4; 
 #17 = Utf8             example 
 #18 = Utf8             Exceptions 
 #19 = Class            #44       // java/lang/InterruptedException 
 #20 = Utf8             lambda$example$33 
 #21 = Utf8             HttpServer 
 #22 = Utf8             InnerClasses 
 #23 = Utf8             (Ljdk8/byte_code/Server$HttpServer;)Z 
 #24 = Utf8             server 
 #25 = Utf8             Ljdk8/byte_code/Server$HttpServer; 
#26 = Utf8              SourceFile 
#27 = Utf8              Example4.java 
#28 = NameAndType       #10:#11   // "<init>":()V 
#29 = Class             #45       // jdk8/byte_code/Server 
#30 = Utf8              jdk8/byte_code/Server$HttpServer 
#31 = Utf8              BootstrapMethods 
#32 = MethodHandle      #6:#46    // invokestatic java/lang/invoke/LambdaMetafactory.metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite; 
#33 = MethodType        #47       // (Ljava/lang/Object;)Z 
#34 = MethodHandle      #6:#48    // invokestatic jdk8/byte_code/Example4.lambda$example33 : (L
j
d
k8/b
y
t
e
c
o
d
e/S
e
r
v
e
rHttpServer;)Z 
#35 = MethodType        #23       // (Ljdk8/byte_code/Server$HttpServer;)Z 
#36 = NameAndType       #49:#50   // test:()Ljava/util/function/Predicate; 
#37 = Class             #51       // jdk8/byte_code/WaitFor 
#38 = NameAndType       #52:#53   // waitFor:(Ljava/lang/Object;Ljava/util/function/Predicate;)V 
#39 = NameAndType       #54:#55   // isRunning:()Ljava/lang/Boolean; 
#40 = Class             #56       // java/lang/Boolean 
#41 = NameAndType       #57:#58   // booleanValue:()Z 
#42 = Utf8              jdk8/byte_code/Example4 
#43 = Utf8              java/lang/Object 
#44 = Utf8              java/lang/InterruptedException 
#45 = Utf8              jdk8/byte_code/Server 
#46 = Methodref         #59.#60   // java/lang/invoke/LambdaMetafactory.metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite; 
#47 = Utf8              (Ljava/lang/Object;)Z 
#48 = Methodref         #8.#61    // jdk8/byte_code/Example4.lambda$example33 : (L
j
d
k8/b
y
t
e
c
o
d
e/S
e
r
v
e
rHttpServer;)Z 
#49 = Utf8              test 
#50 = Utf8              ()Ljava/util/function/Predicate; 
#51 = Utf8              jdk8/byte_code/WaitFor 
#52 = Utf8              waitFor 
#53 = Utf8           (Ljava/lang/Object;Ljava/util/function/Predicate;)V 
#54 = Utf8              isRunning 
#55 = Utf8              ()Ljava/lang/Boolean; 
#56 = Utf8              java/lang/Boolean 
#57 = Utf8              booleanValue 
#58 = Utf8              ()Z 
#59 = Class             #62       // java/lang/invoke/LambdaMetafactory 
#60 = NameAndType #63:#66 // metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite; 
#61 = NameAndType       #20:#23   // lambda$example33 : (L
j
d
k8/b
y
t
e
c
o
d
e/S
e
r
v
e
rHttpServer;)Z 
#62 = Utf8              java/lang/invoke/LambdaMetafactory 
#63 = Utf8              metafactory 
#64 = Class             #68       // java/lang/invoke/MethodHandles$Lookup 
#65 = Utf8              Lookup 
#66 = Utf8 (Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite; 
#67 = Class             #69       // java/lang/invoke/MethodHandles 
#68 = Utf8              java/lang/invoke/MethodHandles$Lookup 
#69 = Utf8              java/lang/invoke/MethodHandles
{
 public jdk8.byte_code.Example4();
 descriptor: ()V
 flags: ACC_PUBLIC
 Code: stack=1, locals=1, args_size=1
 0: aload_0 1:
 invokespecial #1                 // Method java/lang/Object."":()V 
 4: return
 LineNumberTable:
 line 9: 0
 LocalVariableTable:
 Start Length Slot Name Signature
     0      5    0 this Ljdk8/byte_code/Example4;
 void example() throws java.lang.InterruptedException;
   descriptor: ()V
   flags:
   Code:
     stack=2, locals=1, args_size=1
         0: new           #2      //class jdk8/byte_code/Server$HttpServer
         3: dup
         4: invokespecial #3      //Method jdk8/byte_code/Server$HttpServer."<init>":()V
         7: invokedynamic #4,  0  //InvokeDynamic #0:test:()Ljava/util/function/Predicate;
        12: invokestatic  #5      // Method jdk8/byte_code/WaitFor.waitFor:(Ljava/lang/Object;Ljava/util/function/Predicate;)V
        15: return
      LineNumberTable:
        line 13: 0
        line 15: 15
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      16     0  this   Ljdk8/byte_code/Example4;
    Exceptions:
      throws java.lang.InterruptedException
}
```

## Example 4 (with Method Reference)

```java
package jdk8.byte_code;

import static jdk8.byte_code.Server.HttpServer;
import static jdk8.byte_code.WaitFor.waitFor;

@SuppressWarnings("all")
public class Example4_method_reference {
    // lambda with method reference
    void example() throws InterruptedException {
        waitFor(new HttpServer(), HttpServer::isRunning);
    }

}
```

```sh
Classfile Example4_method_reference.class
  Last modified 08-May-2014; size 1271 bytes
  MD5 checksum f8aef942361f29ef599adfec7a594948
  Compiled from "Example4_method_reference.java"
public class jdk8.byte_code.Example4_method_reference
 SourceFile: "Example4_method_reference.java"
 InnerClasses:
   public static #23= #2 of #21; //HttpServer=class jdk8/byte_code/Server$HttpServer of class jdk8/byte_code/Server 
   public static final #52= #51 of #56; //Lookup=class java/lang/invoke/MethodHandles$Lookup of class java/lang/invoke/MethodHandles 
  BootstrapMethods:
    0: #26 invokestatic java/lang/invoke/LambdaMetafactory.metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite; 
    Method arguments:
       #27 (Ljava/lang/Object;)Z 
       #28 invokevirtual jdk8/byte_code/Server$HttpServer.isRunning:()Ljava/lang/Boolean;
       #29 (Ljdk8/byte_code/Server$HttpServer;)Z
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
 #1 = Methodref       #7.#20         //java/lang/Object."<init>":()V    
 #2 = Class           #22            //jdk8/byte_code/Server$HttpServer    
 #3 = Methodref       #2.#20         //  jdk8/byte_code/Server$HttpServer."<init>":()V 
 #4 = InvokeDynamic   #0:#30         // #0:test:()Ljava/util/function/Predicate; 
 #5 = Methodref       #31.#32        // jdk8/byte_code/WaitFor.waitFor:(Ljava/lang/Object;Ljava/util/function/Predicate;)V 
 #6 = Class           #33            // jdk8/byte_code/Example4_method_reference 
 #7 = Class           #34            // java/lang/Object 
 #8 = Utf8            <init> 
 #9 = Utf8            ()V 
 #10 = Utf8           Code 
 #11 = Utf8           LineNumberTable 
 #12 = Utf8           LocalVariableTable 
 #13 = Utf8           this  
 #14 = Utf8           Ljdk8/byte_code/Example4_method_reference; 
 #15 = Utf8           example 
 #16 = Utf8           Exceptions 
 #17 = Class          #35            // java/lang/InterruptedException   
 #18 = Utf8           SourceFile
 #19 = Utf8           Example4_method_reference.java
 #20 = NameAndType    #8:#9         // "<init>":()V
 #21 = Class          #36           // jdk8/byte_code/Server 
 #22 = Utf8           jdk8/byte_code/Server$HttpServer 
 #23 = Utf8           HttpServer 
 #24 = Utf8           InnerClasses 
 #25 = Utf8           BootstrapMethods 
 #26 = MethodHandle   #6:#37        // invokestatic java/lang/invoke/LambdaMetafactory.metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite; 
#27 = MethodType      #38           // (Ljava/lang/Object;)Z 
#28 = MethodHandle    #5:#39        // invokevirtual jdk8/byte_code/Server$HttpServer.isRunning:()Ljava/lang/Boolean;
#29 = MethodType      #40           // (Ljdk8/byte_code/Server$HttpServer;)Z 
#30 = NameAndType     #41:#42       // test:()Ljava/util/function/Predicate; 
#31 = Class           #43           //jdk8/byte_code/WaitFor 
#32 = NameAndType     #44:#45       // waitFor:(Ljava/lang/Object;Ljava/util/function/Predicate;)V 
#33 = Utf8            jdk8/byte_code/Example4_method_reference
#34 = Utf8            java/lang/Object 
#35 = Utf8            java/lang/InterruptedException 
#36 = Utf8            jdk8/byte_code/Server 
#37 = Methodref       #46.#47       // java/lang/invoke/LambdaMetafactory.metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite; 
#38 = Utf8            (Ljava/lang/Object;)Z 
#39 = Methodref       #2.#48        // jdk8/byte_code/Server$HttpServer.isRunning:()Ljava/lang/Boolean; 
#40 = Utf8            (Ljdk8/byte_code/Server$HttpServer;)Z
#41 = Utf8            test 
#42 = Utf8            ()Ljava/util/function/Predicate;
#43 = Utf8            jdk8/byte_code/WaitFor 
#44 = Utf8            waitFor 
#45 = Utf8           (Ljava/lang/Object;Ljava/util/function/Predicate;)V 
#46 = Class           #49           // java/lang/invoke/LambdaMetafactory 
#47 = NameAndType     #50:#53       // metafactory:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite;
#48 = NameAndType     #54:#55       // isRunning:()Ljava/lang/Boolean; #49 = Utf8            java/lang/invoke/LambdaMetafactory 
#50 = Utf8            metafactory 
#51 = Class           #57           // java/lang/invoke/MethodHandles$Lookup 
#52 = Utf8            Lookup 
#53 = Utf8 (Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodType;Ljava/lang/invoke/MethodHandle;Ljava/lang/invoke/MethodType;)Ljava/lang/invoke/CallSite; 
#54 = Utf8            isRunning 
#55 = Utf8            ()Ljava/lang/Boolean;
#56 = Class           #58 // java/lang/invoke/MethodHandles 
#57 = Utf8            java/lang/invoke/MethodHandles$Lookup
#58 = Utf8            java/lang/invoke/MethodHandles 
{
 public jdk8.byte_code.Example4_method_reference();
   descriptor: ()V 
   flags: ACC_PUBLIC
   Code:
     stack=1, locals=1, args_size=1
      0: aload_0
      1: invokespecial #1           // Method java/lang/Object."":()V    
      4: return 
LineNumberTable:
 line 7: 0 
LocalVariableTable:
 Start Length Slot Name Signature
     0      5    0 this Ljdk8/byte_code/Example4_method_reference;  void example() throws java.lang.InterruptedException;
    descriptor: ()V
    flags:
    Code:
      stack=2, locals=1, args_size=1
         0: new           #2                  // class jdk8/byte_code/Server$HttpServer
         3: dup
         4: invokespecial #3                  // Method jdk8/byte_code/Server$HttpServer."<init>":()V
         7: invokedynamic #4,  0              // InvokeDynamic #0:test:()Ljava/util/function/Predicate;
        12: invokestatic  #5                  // Method jdk8/byte_code/WaitFor.waitFor:(Ljava/lang/Object;Ljava/util/function/Predicate;)V
        15: return
      LineNumberTable:
        line 11: 0
        line 12: 15
      LocalVariableTable:
      Start Length Slot Name Signature
      0     16     0    this Ljdk8/byte_code/Example4_method_reference;  
    Exceptions:
      throws java.lang.InterruptedException
  }
```

## 
