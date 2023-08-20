# matlab基础语法

## 变量

### 特殊变量

| Name    | Meaning                                  |
| ------- | ---------------------------------------- |
| **ans** | 默认的变量名，以应答最近依次操作运算结果 |
| **eps** | 浮点数的相对误差                         |
| **i,j** | 虚数单位，定义为 i2 = j2 = -1            |
| **Inf** | 代表无穷大                               |
| **NaN** | 代表不定值（不是数字）                   |
| **pi**  | 圆周率                                   |

每个**MATLAB变量**可以是数组或者矩阵。

用一个简单的方法指定变量。例如：

```matlab
x = 3	       % defining x and initializing it with a value
```

MATLAB执行上述语句，并返回以下结果：

```matlab
x =
     3
```

上述的例子创建了一个1-1的矩阵名为x和的值存储在其元素中。我们可以看看另外的例子，

```matlab
x = sqrt(16) 	% defining x and initializing it with an expression
```

MATLAB执行上述语句，并返回以下结果：

```matlab
x =
     4
```

**MATLAB注意事项：**

- 在使用变量之前，必须进行赋值。
- 当系统接收到一个变量之后，这个变量可以被引用。
- 例如：

- ```matlab
  x = 7 * 8;
  y = x * 7.89
  ```

- MATLAB将执行上面的语句，并返回以下结果：

- ```matlab
  y =
    441.8400
  ```

- 当表达式返回一个结果，不分配给任何变量，系统分配给一个变量命名ans，以后可以继续使用。
- 例如：

- ```
  sqrt(78)
  ```

- MATLAB将执行上面的语句，并返回以下结果：

- ```
  ans =
      8.8318
  ```

- 变量 ans 可以被继续使用:

- ```
  9876/ans
  ```

- MATLAB将执行上面的语句，并返回以下结果：

- 

- ```
  ans =
     1.1182e+03
  ```

在MATLAB中可以使用 who 命令显示所有已经使用的变量名。

```
who
```

MATLAB将执行上面的语句，并返回以下结果：

```
Your variables are:
a    ans  b    c    x    y    
```

whos 命令则显示多一点有关变量：

- 当前内存中的变量
- 每个变量的类型
- 内存分配给每个变量
- 无论他们是复杂的变量与否

```
whos
```

MATLAB将执行上面的语句，并返回以下结果：

```
  Name      Size            Bytes  Class     Attributes

  a         1x1                 8  double              
  ans       1x1                 8  double              
  b         1x1                 8  double              
  c         1x1                 8  double              
  x         1x1                 8  double              
  y         1x1                 8  double      
```

clear命令删除所有（或指定）从内存中的变量（S）。

```
clear x     % it will delete x, won't display anything
clear	     % it will delete all variables in the workspace
             %  peacefully and unobtrusively 
```

### 长任务

长任务可以通过使用省略号（...）延伸到另一条线路。例如，

```
initial_velocity = 0;
acceleration = 9.8;
time = 20;
final_velocity = initial_velocity ...
    + acceleration * time
```

MATLAB将执行上面的语句，并返回以下结果：

```
final_velocity =
   196
```

### MATLAB创建向量

向量是一维数组中的数字。MATLAB允许创建两种类型的矢量：

- 行向量
- 列向量

创建行向量括在方括号中的元素的集合，用空格或逗号分隔的元素。

例如，

```
r = [7 8 9 10 11]
```

MATLAB将执行上面的语句，并返回以下结果：

```matlab
r =
  Columns 1 through 4
       7              8              9             10       
  Column 5
      11    
```

另外一个例子，

```matlab
r = [7 8 9 10 11];
t = [2, 3, 4, 5, 6];
res = r + t
```

MATLAB将执行上面的语句，并返回以下结果：

```
res =
  Columns 1 through 4
       9             11             13             15       
  Column 5
      17
```

创建列向量通过内附组方括号中的元素，使用分号（;)分隔的元素。

```
c = [7;  8;  9;  10; 11]
```

MATLAB将执行上面的语句，并返回以下结果：

```
c =
       7       
       8       
       9       
      10       
      11  
```

### MATLAB创建矩阵

矩阵是一个二维数字阵列。

在MATLAB中，创建一个矩阵每行输入空格或逗号分隔的元素序列，最后一排被划定一个分号。

例如，下面创建了一个3×3的矩阵：

```
m = [1 2 3; 4 5 6; 7 8 9]
```

MATLAB执行上述语句，并返回以下结果：

```
m =
       1              2              3       
       4              5              6       
       7              8              9     
```

## 命令

## 数据类型

默认情况下，MATLAB ®存储所有数值变量为双精度浮点值。其他数据类型存储文本，整数或单精度值或单个变量中相关数据的组合。

MATLAB不需要任何类型声明或维度语句。当MATLAB遇到新的变量名称时，它将创建变量并分配适当的内存空间。

如果变量已经存在，则MATLAB将使用新内容替换原始内容，并在必要时分配新的存储空间。

### MATLAB数据类型

MATLAB提供`15`种基本数据类型，分别是8种整型数据、单精度浮点型、双精度浮点型、逻辑型、字符串型、单元数组、结构体类型和函数句柄。每种数据类型存储矩阵或数组形式的数据。矩阵或数组的最小值是`0`到`0`，并且是可以到任何大小的矩阵或数组。

下表显示了MATLAB中最常用的数据类型：

| 数据类型   | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| `int8`     | `8`位有符号整数                                              |
| `uint8`    | `8`位无符号整数                                              |
| `int16`    | `16`位有符号整数                                             |
| `uint16`   | `16`位无符号整数                                             |
| `int32`    | `32`位有符号整数                                             |
| `uint32`   | `32`位无符号整数                                             |
| `int64`    | `64`位有符号整数                                             |
| `uint64`   | `64`位无符号整数                                             |
| `single`   | 单精度数值数据                                               |
| `double`   | 双精度数值数据                                               |
| `logical`  | 逻辑值为`1`或`0`，分别代表`true`和`false`                    |
| `char`     | 字符数据(字符串作为字符向量存储)                             |
| 单元格阵列 | 索引单元阵列，每个都能够存储不同维数和数据类型的数组         |
| 结构体     | C型结构，每个结构具有能够存储不同维数和数据类型的数组的命名字段 |
| 函数处理   | 指向一个函数的指针                                           |
| 用户类     | 用户定义的类构造的对象                                       |
| Java类     | 从Java类构造的对象                                           |

### 数据类型转换

------

MATLAB提供了各种用于将一种数据类型转换为另一种数据类型的函数。 下表显示了数据类型转换函数：

| 函数             | 描述说明                                       |
| :--------------- | :--------------------------------------------- |
| `char`           | 转换为字符数组(字符串)                         |
| `int2str`        | 将整数数据转换为字符串                         |
| `mat2str`        | 将矩阵转换为字符串                             |
| `num2str`        | 将数字转换为字符串                             |
| `str2double`     | 将字符串转换为双精度值                         |
| `str2num`        | 将字符串转换为数字                             |
| `native2unicode` | 将数字字节转换为Unicode字符                    |
| `unicode2native` | 将Unicode字符转换为数字字节                    |
| `base2dec`       | 将基数N字符串转换为十进制数                    |
| `bin2dec`        | 将二进制数字串转换为十进制数                   |
| `dec2base`       | 将十进制转换为字符串中的N数字                  |
| `dec2bin`        | 将十进制转换为字符串中的二进制数               |
| `dec2hex`        | 将十进制转换为十六进制数字                     |
| `hex2dec`        | 将十六进制数字字符串转换为十进制数             |
| `hex2num`        | 将十六进制数字字符串转换为双精度数字           |
| `num2hex`        | 将单数转换为IEEE十六进制字符串                 |
| `cell2mat`       | 将单元格数组转换为数组                         |
| `cell2struct`    | 将单元格数组转换为结构数组                     |
| `cellstr`        | 从字符数组创建字符串数组                       |
| `mat2cell`       | 将数组转换为具有潜在不同大小的单元格的单元阵列 |
| `num2cell`       | 将数组转换为具有一致大小的单元格的单元阵列     |
| `struct2cell`    | 将结构转换为单元格数组                         |

### 数据类型确定

------

MATLAB提供了用于识别变量数据类型的各种函数。

下表提供了确定变量数据类型的函数：

| 函数                 | 描述说明                         |
| :------------------- | :------------------------------- |
| `is`                 | 检测状态                         |
| `isa`                | 确定输入是否是指定类的对象       |
| `iscell`             | 确定输入是单元格数组             |
| `iscellstr`          | 确定输入是字符串的单元格数组     |
| `ischar`             | 确定项目是否是字符数组           |
| `isfield`            | 确定输入是否是结构数组字段       |
| `isfloat`            | 确定输入是否为浮点数组           |
| `ishghandle`         | 确定是否用于处理图形对象句柄     |
| `isinteger`          | 确定输入是否为整数数组           |
| `isjava`             | 确定输入是否为Java对象           |
| `islogical`          | 确定输入是否为逻辑数组           |
| `isnumeric`          | 确定输入是否是数字数组           |
| `isobject`           | 确定输入是否为MATLAB对象         |
| `isreal`             | 检查输入是否为实数数组           |
| `isscalar`           | 确定输入是否为标量               |
| `isstr`              | 确定输入是否是字符数组           |
| `isstruct`           | 确定输入是否是结构数组           |
| `isvector`           | 确定输入是否为向量               |
| `class`              | 确定对象的类                     |
| `validateattributes` | 检查数组的有效性                 |
| `whos`               | 在工作区中列出变量，其大小和类型 |

## 数组

### MATLAB中的特殊阵列

MATLAB中会使用一些函数来建立一些特殊的阵列，对于所有这些函数，一个参数创建一个正方形阵列，双参数创建矩形阵列。

使用 zeros() 函数建立一个元素为零的数组：

例如：

```
zeros(5)
```

MATLAB 执行上述语句，返回以下结果：

```matlab
ans =
     0     0     0     0     0
     0     0     0     0     0
     0     0     0     0     0
     0     0     0     0     0
     0     0     0     0     0
```

使用 ones() 函数建立一个数组：

例如：

```matlab
ones(4,3)
```

MATLAB执行上述语句，返回以下结果：

```
ans =
     1     1     1
     1     1     1
     1     1     1
     1     1     1
```

使用 eye() 函数创建一个矩阵：

例如：

```
eye(4)
```

MATLAB执行上述语句，返回以下结果：

```
ans =
     1     0     0     0
     0     1     0     0
     0     0     1     0
     0     0     0     1
```

使用 rand() 函数建立一个数组（0,1）上均匀分布的随机数：

例如：

```
rand(3, 5)
```

MATLAB执行上述语句，返回以下结果：

```
ans =
    0.8147    0.9134    0.2785    0.9649    0.9572
    0.9058    0.6324    0.5469    0.1576    0.4854
    0.1270    0.0975    0.9575    0.9706    0.8003
```

### 单元阵列

单元阵列的阵列中每个单元格可以存储不同的维度和数据类型的数组的索引单元格。

单元格函数用于建立一个单元阵列。

单元格函数的语法如下：

```matlab
C = cell(dim)
C = cell(dim1,...,dimN)
D = cell(obj)
```

#### 注意

- C 是单元阵列；
- dim 是一个标量整数或整数向量，指定单元格阵列C的尺寸；
- dim1, ... , dimN 是标量整数指定尺寸的C；
- obj 是以下内容之一
  - Java 数组或对象
  - .NET阵列 System.String 类型或 System.Object

#### 详细例子

在MATLAB中建立一个脚本文件，输入下述代码：

```matlab
c = cell(2, 5);
c = {'Red', 'Blue', 'Green', 'Yellow', 'White'; 1 2 3 4 5}
```

运行该文件，显示以下结果：

```matlab
c = 
    'Red'    'Blue'    'Green'    'Yellow'    'White'
    [  1]    [   2]    [    3]    [     4]    [    5]
```

#### MATLAB在单元格上阵列访问数据

使用两种方法来引用单元阵列的元素：

- 封闭的索引在第一个 bracket ()，是指一组单元格
- 封闭的在大括号{}，的索引单个单元格内的数据

括在第一支架的索引，它指的是单元格的集。

单元阵列索引平稳括号单元格集合。

例如：

```
c = {'Red', 'Blue', 'Green', 'Yellow', 'White'; 1 2 3 4 5};
c(1:2,1:2)
```

MATLAB执行上述语句，返回以下结果：

```
ans = 
    'Red'    'Blue'
    [  1]    [   2]
```

同样可以用花括号“**{ }**”索引访问单元格的内容。

例如：

```
c = {'Red', 'Blue', 'Green', 'Yellow', 'White'; 1 2 3 4 5};
c{1, 2:4}
```

MATLAB执行上述语句，返回以下结果：

```
ans =
   Blue
ans =
   Green
ans =
   Yellow
```

## matlab函数

> 在MATLAB中，函数定义在单独的文件。函数和函数的文件名应该是相同的。

函数是一起执行任务的一组语句。

函数在自己的工作空间进行操作，被称为本地工作区，独立的工作区；在 MATLAB 命令提示符访问，这就是所谓的基础工作区的变量。

函数可以接受多个输入参数和可能返回多个输出参数。

函数语句的语法是：

```
function [out1,out2, ..., outN] = myfun(in1,in2,in3, ..., inN)
```

### 详细例子

下述有个 mymax 函数，它需要五个数字作为参数并返回最大的数字。

建立函数文件，命名为 mymax.m 并输入下面的代码：

```matlab
function max = mymax(n1, n2, n3, n4, n5)
%This function calculates the maximum of the
% five numbers given as input
max =  n1;
if(n2 > max)
    max = n2;
end
if(n3 > max)
   max = n3;
end
if(n4 > max)
    max = n4;
end
if(n5 > max)
    max = n5;
end
```

每个函数的第一行要以 function 关键字开始。它给出了函数的名称和参数的顺序。

在我们的例子中，mymax 函数有5个输入参数和一个输出参数。

注释行语句的功能后提供的帮助文本。这些线条打印，当输入：

```
help mymax
```

MATLAB执行上述语句，返回以下结果：

```
This function calculates the maximum of the
 five numbers given as input
```

可以调用该函数：

```
mymax(34, 78, 89, 23, 11)
```

MATLAB执行上述语句，返回以下结果：

```
ans =
    89
```

### MATLAB匿名函数

一个匿名的函数就像是在传统的编程语言，在一个单一的 MATLAB 语句定义一个内联函数。

它由一个单一的 MATLAB 表达式和任意数量的输入和输出参数。

在MATLAB命令行或在一个函数或脚本可以定义一个匿名函数。

这种方式，可以创建简单的函数，而不必为他们创建一个文件。

建立一个匿名函数表达式的语法如下：

```
f = @(arglist)expression
```

### 详细例子

在这个例子中，我们将编写一个匿名函数 power，这将需要两个数字作为输入并返回第二个数字到第一个数字次幂。

在MATLAB中建立一个脚本文件，并输入下述代码：

```
power = @(x, n) x.^n;
result1 = power(7, 3)
result2 = power(49, 0.5)
result3 = power(10, -10)
result4 = power (4.5, 1.5)
```

运行该文件时，显示结果：

```
result1 =
   343
result2 =
     7
result3 =
   1.0000e-10
result4 =
    9.5459
```

### 主要函数和子函数

在一个文件中，必须定义一个匿名函数以外的任何函数。每个函数的文件包含一个必需的主函数和首先出现的任何数量的可选子函数，在主要函数之后使用。

主要函数可以调用的文件，它定义之外，无论是从命令行或从其他函数，但子功能不能被称为命令行或其他函数，外面的函数文件。

子功能可见函数内的文件，它定义它们的主要函数和其他函数。

### 详细例子

我们写一个 quadratic 函数来计算一元二次方程的根。

该函数将需要三个输入端，二次系数，线性合作高效的和常数项，它会返回根。 

函数文件 quadratic.m 将包含的主要 quadratic 函数和子函数 disc 来计算判别。

在MATLAB中建立一个函数文件 quadratic.m 并输入下述代码：

```
function [x1,x2] = quadratic(a,b,c)
%this function returns the roots of 
% a quadratic equation.
% It takes 3 input arguments
% which are the co-efficients of x2, x and the 
%constant term
% It returns the roots
d = disc(a,b,c); 
x1 = (-b + d) / (2*a);
x2 = (-b - d) / (2*a);
end % end of quadratic

function dis = disc(a,b,c) 
%function calculates the discriminant
dis = sqrt(b^2 - 4*a*c);
end % end of sub-function
```

可以从命令提示符调用上述函数为：

```
quadratic(2,4,-4)
```

MATLAB执行上面的语句，返回以下结果：

```
ans =
    0.7321
```

### MATLAB嵌套函数

在这个机体内另一个函数，可以定义函数。这些被称为嵌套函数。

嵌套函数包含任何其他函数的任何或所有的组件。

嵌套函数被另一个函数的范围内定义他们共享访问包含函数的工作区。

嵌套函数的语法如下：

```
function x = A(p1, p2)
...
B(p2)
   function y = B(p3)
   ...
   end
...
end
```

### 详细例子

我们重写前面例子的 quadratic 函数，但是，这一次的 disc 函数将是一个嵌套函数。 

在MATLAB中建立一个函数文件 quadratic2.m，并输入下述代码：

```
function [x1,x2] = quadratic2(a,b,c)
function disc  % nested function
d = sqrt(b^2 - 4*a*c);
end % end of function disc
disc;
x1 = (-b + d) / (2*a);
x2 = (-b - d) / (2*a);
end % end of function quadratic2
```

可以从命令提示符调用上面的函数为：

```
quadratic2(2,4,-4)
```

MATLAB执行上面的语句，返回以下结果：

```
ans =
    0.7321
```

### MATLAB私有函数

一个私有函数是一个主要的函数，是只看得见一组有限的其它函数。

如果不想公开的执行的一个函数，可以创建私有函数。

私有函数驻留特殊的名字私人的子文件夹中。

他们是可见的，只有在父文件夹的函数。

### 详细例子

重写 quadratic 函数。然而，这时计算的判别式 disc 函数，是一个私有函数。

在MATLAB中建立一个子文件夹命名为私人工作目录。它存储在以下函数文件 disc.m：

```
function dis = disc(a,b,c) 
%function calculates the discriminant
dis = sqrt(b^2 - 4*a*c);
end % end of sub-function
```

在工作目录，创建一个函数 quadratic3.m ，输入下述代码：

```
function [x1,x2] = quadratic3(a,b,c)
%this function returns the roots of 
% a quadratic equation.
% It takes 3 input arguments
% which are the co-efficients of x2, x and the 
%constant term
% It returns the roots
d = disc(a,b,c); 
x1 = (-b + d) / (2*a);
x2 = (-b - d) / (2*a);
end % end of quadratic3
```

可以从命令提示符调用上面的函数为：

```
quadratic3(2,4,-4)
```

MATLAB执行上面的语句，返回以下结果：

```
ans =
    0.7321
```

### MATLAB全局变量

全局变量可以由一个以上的函数共享。对于这一点，需要将变量声明为global ，就可以在所有的函数可使用。

如果想访问该变量。从基工作区，然后在命令行声明的变量。

全局声明必须出现在变量中。实际上是使用功能。这是一个很好的做法是使用大写字母为全局变量的名称，以区别于其他变量。

### 详细例子

创建一个函数文件名为 average.m ，输入下述代码：

```
function avg = average(nums)
global TOTAL
avg = sum(nums)/TOTAL;
end
```

在MATLAB中建立一个脚本文件，输入下面的代码：

```
global TOTAL;
TOTAL = 10;
n = [34, 45, 25, 45, 33, 19, 40, 34, 38, 42];
av = average(n)
```

运行该文件，显示以下结果：

```
av =
   35.5000
```