### 3.1 Hello World

```java
public class Hello {
	public static void main(String[] args) {
		System.out.println("Hello world!");
	}
}
```

驼峰式命名，class首字母大写，文件名与class相同

### 3.2 注释

与c++一致

### 3.3 数据类型

* **整型** int 4B, short 2B, long 8B, byte 1B
* **浮点** float 4B, double 8B
* **char** 设计UTF编码，不建议使用
* **boolean** true / false

### 3.4 变量与常量

* **final** 常量，类似const
* **枚举**  `enum Size { SMALL, MEDIUM, LARGE, EXTRA_LARGE };`

### 3.5 运算符

* 算数
* 数学**Math**类 `Math.pow(x, a)`

### 3.6 String

* Java的String是immutable的，不存在修改字符串的方法。
* 使用`StringBuilder`快速构建String

```java
StringBuilder builder = new StringBuilder();
builder.append(str);
String completedString = builder.toString();
```

### 3.7 IO

```java
// 标准输入流
Scanner in = new Scanner(System.in);
String name = in.nextLine();

// 格式化输出类似c++
System.out.printf("Hello, $s, you have %8.2f", name, balance);
String msg = String.format("Hello, $s, you have %8.2f", name, balance);

// 文件读入
Scanner in = new Scanner(Path.of("myfile.txt"), StandardCharsets.UTF_8);
// 文件写入
PrintWriter out = new PrintWriter("myfile.txt", StandardCharsets.UTF_8);
```

### 3.8 控制流程

条件、循环

### 3.9 大数

`java.math`中的`BigInteger`和`Bigdecimal`可以处理任意长度数字序列的数值。

### 3.10 数组

```java
int[] a = new int[100]; // 所有值会被初始化
// 数组内存空间在heap上分配
int[] values = {1, 5, 10, 50, 100};
// for each 循环
for (int v : values) {
	res += v;
}
```

