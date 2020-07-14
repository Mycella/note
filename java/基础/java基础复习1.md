### 一.环境变量配置

>JAVA_HOME=C:\Program Files\Java\jdk1.8.0_231
>
>CLASSPATH=.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar
>
>path    %JAVA_HOME%\bin   %JAVA_HOME%\jre\bin

JAVA_HOME相当于配置一个参数

CLASSPATH代表java程序执行要加载的类库，.代表当前路径下的，也就是自己编写，另外两部分是jdk内置类库

path指执行的java或者javac的路径，配置完可在任意目录执行

### 二.javadoc使用

> javadoc dirName -author -version filename

### 三.变量的定义

1.先声明再使用

2.需要初始化    **全局变量会有默认值，成员变量必须初始化**

### 四.基本类型

1.由于存储方式，float虽然只占4个字节但是比long的取值范围要大，float在内存中是科学计数法的表现形式。

2.小于int类型的运算会自动转换为int