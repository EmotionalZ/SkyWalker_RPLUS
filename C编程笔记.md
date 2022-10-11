# 第三章，简单模拟

## 3.1 入门模拟-简单模拟

### **1002 写出这个数**

读入一个正整数 *n*，计算其各位数字之和，用汉语拼音写出和的每一位数字。

输入格式：

每个测试输入包含 1 个测试用例，即给出自然数 *n* 的值。这里保证 *n* 小于 10100。

输出格式：

在一行内输出 *n* 的各位数字之和的每一位，拼音数字间有 1 空格，但一行中最后一个拼音数字后没有空格。

输入样例：

```in
1234567890987654321123456789
```

输出样例：

```out
yi san wu
```

代码长度限制16 KB 时间限制400 ms 内存限制64 MB

#### test1，问题见1、2：

```c++
##include<cstdio>
int main(){
    int n, sum=0;
    char str[100] = {"yi", "er", "san", "si", "wu", "liu", 
                 "qi", "ba", "jiu"};
    scanf("%d", &n);
    while(n/10 != 0){
        sum = sum+(n%10);
        n=n/10;
    }
    while(sum/10 != 0){
        printf("%c ", str[sum%10-1]);
    }
    return 0;
}
```

#### test2，问题见3：

```c++
##include <cstdio>
##include <cstring>
int main(){
    char str[110];
    int len = strlen(str), sum = 0;
    //gets(str);//字符串输入方式有误get->gets,gets用不了
    //gets_s(str,110);//只有VS里能用
    fgets(str, 110, stdin);
    for(int n = 0; n < len; n++){//不能直接n<strlen(str)放在条件里，要建立一变量len
        sum += str[n];
    }
    char change[10][5] = {"ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu"};
    while(sum != 0){//对于存为整型的变量sum，每十位取余+每循环一次除去一位的方式循环遍历
        printf("%s ", change[sum%10]);//将末位取出并转换打印出来、、经典的错误，最后会多一个空格
        sum = sum / 10;//去掉一位
    }
    return 0;
}
```

#### test3, 最终版:

```c++
#include <cstdio>
#include <cstring>

int main(){
    char str[110];
    int len = strlen(str), sum = 0;
    fgets(str, 110, stdin);
    for(int n = 0; n < len; n++){//不能直接n<strlen(str)放在条件里，要建立一变量len
        sum += str[n] - '0';
    }
    //把sum转化成和数字同高低位的数组135->{5,3,1}
    int num =0, ans[10];
    while(num != 0){
        ans[num]= sum%10;
        num++;
        sum/=10;
    }
    //
    char change[10][5] = {"ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu"};
    //\1\num-1开始到i=0执行完结束\2\位数从最高位开始，即数组最高位对照原数字最高位
    for(int i = num -1; i >= 0; i--){
        printf("%s", change[ans[i]]);
        if(i!=0) printf(" ");//没到最后一次循环都输出空格
        else printf("\n");
    }
    return 0;
}
```



### 常见错误总结：

##### 1、scanf内没写&号、拼写错误

##### 2、字符串问题：

（1）字符串常量：

```c++
char str[25] = "wo ai de ren bu ai wo"//用%s输出
```

（2）字符数组：

```c++
char site[7] = {'R', 'U', 'N', 'O', 'O', 'B', '\0'};//用空格或者回车作为结束符,考虑\0的占位。
//等价写法：
char site[] = "RUNOOB";
```

（3）二维数组：

```c++
char str[10][5] = {"ling", "yi", "er", "san", "si", "wu", "liu", 
                 "qi", "ba", "jiu"}//二维：【几个单词】和【每个单词几个字母】
```

（4）字符串转整数：

1）、字符和字符相减的本质就是ASCII码的计算
2）、用%d打印出来的结果是对应的ASCII码值；所以C语言中单个字符减去‘0’，多是用于字符转数字的时候(如：‘8’ - ‘0’ 的计算结果就是8)
3）、ASCII码计算结果再次使用%c打印，就会转义为ASCII码对应的字符解释，如图1中的退格现象。

需要用到 sum = str[x] -'0'之类的写法。因为字符在计算机内用ASCII码存储，所以char-char得到的数字就是0-x之间的差值，正好为需要得到的整数



---

额外补充

```c++
#include <stdio.h>
#include <string.h>
 
int main ()
{
   char str1[14] = "runoob";
   char str2[14] = "google";
   char str3[14];
   int  len ;
 
   /* 复制 str1 到 str3 */
   strcpy(str3, str1);
   printf("strcpy( str3, str1) :  %s\n", str3 );
 
   /* 连接 str1 和 str2 */
   strcat( str1, str2);
   printf("strcat( str1, str2):   %s\n", str1 );
 
   /* 连接后，str1 的总长度 */
   len = strlen(str1);
   printf("strlen(str1) :  %d\n", len );
 
   return 0;
}
```

```
strcpy( str3, str1) :  runoob
strcat( str1, str2):   runoobgoogle
strlen(str1) :  12
```

(*^▽^*)重要提示：gets用不了了，因为这个函数不安全，从vs2015起gets()函数就没有了，因为可能会造成[缓冲区溢出](https://so.csdn.net/so/search?q=缓冲区溢出&spm=1001.2101.3001.7020), 甚至程序崩溃。故不推荐使用。vs2019会建议用gets_s()来代替 虽然用 gets() 时有空格也可以直接输入，但是 gets() 有一个非常大的缺陷，即它不检查预留存储区是否能够容纳实际输入的数据，换句话说，如果输入的字符数目大于[数组](https://so.csdn.net/so/search?q=数组&spm=1001.2101.3001.7020)的长度，gets 无法检测到这个问题，就会发生内存越界，所以编程时建议使用 fgets()。

fgets() 虽然比 gets() 安全，但安全是要付出代价的，代价就是它的使用比 gets() 要麻烦一点，有三个参数。它的功能是从 stream 流中读取 size 个字符存储到字符指针变量 s 所指向的[内存](https://so.csdn.net/so/search?q=内存&spm=1001.2101.3001.7020)空间。它的返回值是一个指针，指向字符串中第一个字符的地址。

```c++
## include <stdio.h>
char *fgets(char *s, int size, FILE *stream);
```

其中：s 代表要保存到的内存空间的首地址，可以是字符数组名，也可以是指向字符数组的字符指针变量名。size 代表的是读取字符串的长度。stream 表示从何种流中读取，可以是标准输入流 [stdin](https://so.csdn.net/so/search?q=stdin&spm=1001.2101.3001.7020)，也可以是文件流，即从某个文件中读取，这个在后面讲文件的时候再详细介绍。标准输入流就是前面讲的输入缓冲区。所以如果是从键盘读取数据的话就是从输入缓冲区中读取数据，即从标准输入流 stdin 中读取数据，所以第三个参数为 stdin。

！！！或者用cin.geline(str,strlen);

---

##### 3、程序逻辑：

（1）int sum输出格式应变成数组ans[]，并且应该对sum记位

（2）sum低位存入ans[]低位

（3）输出从高位开始输出123--》一二三

（4）空格输出用到if，巧妙解决机械样循环输出最后多一个空格的问题

——————————————————————————————————————————————————————

### **1011 A+B 和 C**

分数 15

全屏浏览题目切换布局

作者 HOU, Qiming

单位 浙江大学

给定区间 [−231,231] 内的 3 个整数 *A*、*B* 和 *C*，请判断 *A*+*B* 是否大于 *C*。

输入格式：

输入第 1 行给出正整数 *T* (≤10)，是测试用例的个数。随后给出 *T* 组测试用例，每组占一行，顺序给出 *A*、*B* 和 *C*。整数间以空格分隔。

输出格式：

对每组测试用例，在一行中输出 `Case #X: true` 如果 *A*+*B*>*C*，否则输出 `Case #X: false`，其中 `X` 是测试用例的编号（从 1 开始）。

输入样例：

```in
4
1 2 3
2 3 4
2147483647 0 2147483646
0 -2147483648 -2147483647
```

输出样例：

```out
Case #1: false
Case #2: true
Case #3: true
Case #4: false
```

代码长度限制16 KB时间限制400 ms内存限制64 MB

#### test1

```c++
#include <cstdio>
#include <iostream>
using namespace std;
int main(){
    int a, b, c;
    scanf("%d%d%d", &a, &b, &c);
    //输入为空怎么办，是否需要定一个范围呢，空格应该是结束输入
    if(a+b>c) printf("Case #X: true");
    else printf("Case #X: false");
    return 0;
}
```

#### 错误总结：

1、不用引用iostream

2、没好好读题，要循环T次

#### test2：

```c++
#include <cstdio>
int main(){
    int T, tcase = 1;
    scanf("%d", &T);//看数据给出的形式，需要先扫描次数，再看输出格式，每次输出需要输出case号。
    while(T--){//while判断主要是括号内为TRUE才可循环，高级写法就是T--次！
        long long a, b, c;
        scanf("%lld%lld%lld",&a, &b, &c);//绝对值超过10的9次方，需要用long long
        if(a + b > c) printf("Case #%d: true\n", tcase++);//格式中记得更改case号，并且记得换行！
        else printf("Case #%d: false\n", tcase++);
    }
    return 0;
}
```

错误总结：见注释！！！

---

### **1016 部分A+B**

正整数 *A* 的“*D**A*（为 1 位整数）部分”定义为由 *A* 中所有 *D**A* 组成的新整数 *P**A*。例如：给定 *A*=3862767，*D**A*=6，则 *A* 的“6 部分”*P**A* 是 66，因为 *A* 中有 2 个 6。

现给定 *A*、*D**A*、*B*、*D**B*，请编写程序计算 *P**A*+*P**B*。

输入格式：

输入在一行中依次给出 *A*、*D**A*、*B*、*D**B*，中间以空格分隔，其中 0<*A*,*B*<109。

输出格式：

在一行中输出 *P**A*+*P**B* 的值。

输入样例 1：

```in
3862767 6 13530293 3
```

输出样例 1：

```out
399
```

输入样例 2：

```in
3862767 1 13530293 8
```

输出样例 2：

```out
0
```

代码长度限制16 K时间限制150 ms内存限制64 MB

#### test1

```c++
#include <cstdio>
int main() {
    int a, b, Da, Db, Pa, Pb, flag=0, f=0;
    scanf("%d%d%d%d", a, Da, b, Db);
    while(a != 0){
        f = a % 10;
        if((f == Da)&(!flag)){
            Pa = f;
            flag++;
        }else if((f == Da)&(flag)){
            Pa += f*10^flag;
        }
        a /= 10;
    }
    while(b != 0){
        f = b % 10;
        if((f == Db)&(!flag)){
            Pb = f;
            flag++;
        }else if((f == Db)&(flag)){
            Pb += f*10^flag;
        }
        b /= 10;
    }
    printf("%d", Pa+Pb);
    return 0;
}
```

#### test2:

```c++
#include <cstdio>
int main() {
    long long a, b, Da, Db;
    scanf("%lld%lld%lld%lld", &a, &Da, &b, &Db);
    int Pa = 0, Pb = 0;
//     int flag=0, f=0;
    while(a != 0){
//         f = a % 10;
//         if((f == Da)&(!flag)){
//             Pa = f;
//             flag++;
//         }else if((f == Da)&(flag)){
//             Pa += f*10^flag;
//         }
        if(Pa%10==Da) Pa = Pa*10 + Da;
        a = a / 10;
    }
    while(b != 0){
//         f = b % 10;
//         if((f == Db)&(!flag)){
//             Pb = f;
//             flag++;
//         }else if((f == Db)&(flag)){
//             Pb += f*10^flag;
//         }
        if(b%10==Db) Pb = Pb*10 + Db;
        b = b / 10;
    }
    printf("%d", Pa+Pb);
    return 0;
}
```

#### test3：最终版

```c++
#include <cstdio>
int main() {
    long long a, b, Da, Db;//题目中范围超过10^9所以用lld
    scanf("%lld%lld%lld%lld", &a, &Da, &b, &Db);
    long long Pa = 0, Pb = 0;
    while(a != 0){
        if(a%10==Da) Pa = Pa*10 + Da;//整个程序的亮点
        a = a / 10;
    }
    while(b != 0){
        if(b%10==Db) Pb = Pb*10 + Db;
        b = b / 10;
    }
    printf("%lld\n", Pa + Pb);
    return 0;
}
```

### **1026 程序运行时间**

要获得一个 C 语言程序的运行时间，常用的方法是调用头文件 time.h，其中提供了 clock() 函数，可以捕捉从程序开始运行到 clock() 被调用时所耗费的时间。这个时间单位是 clock tick，即“时钟打点”。同时还有一个常数 CLK_TCK，给出了机器时钟每秒所走的时钟打点数。于是为了获得一个函数 *f* 的运行时间，我们只要在调用 *f* 之前先调用 clock()，获得一个时钟打点数 C1；在 *f* 执行完成后再调用 clock()，获得另一个时钟打点数 C2；两次获得的时钟打点数之差 (C2-C1) 就是 *f* 运行所消耗的时钟打点数，再除以常数 CLK_TCK，就得到了以秒为单位的运行时间。

这里不妨简单假设常数 CLK_TCK 为 100。现给定被测函数前后两次获得的时钟打点数，请你给出被测函数运行的时间。

输入格式：

输入在一行中顺序给出 2 个整数 C1 和 C2。注意两次获得的时钟打点数肯定不相同，即 C1 < C2，并且取值在 [0,107]。

输出格式：

在一行中输出被测函数运行的时间。运行时间必须按照 `hh:mm:ss`（即2位的 `时:分:秒`）格式输出；不足 1 秒的时间四舍五入到秒。

输入样例：

```in
123 4577973
```

输出样例：

```out
12:42:59
```

代码长度限制16 KB时间限制200 ms内存限64 MB

#### test1

```c++
#include <cstdio>
#define CLK_TCK 100
int main() {
    int c1, c2, time, ans, hh, mm, ss;
    scanf("%d%d", &c1, &c2);
    time = (c2 - c1)/CLK_TCK;
    ans = (c2 - c1)%CLK_TCK;

    hh=time/3600;
    mm = (time-hh*3600)/60;
    while(mm>=60){
        hh+=1;
        mm-=60;
    }
    ss = time-(hh*3600+mm*60);
    if(ans >= 50) ss += 1;//S位的四舍五入操作，>=50而不是5，因为对÷100求余 结果没差？
     while(ss>=60){
        mm+=1;
        ss-=60;
    }

    printf("%d:%d:%d", hh, mm, ss);
    return 0;
}
```

#### 错误总结：

可能是问题出在对秒求余上，应该一开始就四舍五入，但time减掉的是hhmm的整数部分和，应该没有误差才对，问题可能出在进位上

#### test2:

```c++
#include <cstdio>
#define CLK_TCK 100
int main() {
    int c1, c2;
    scanf("%d%d", &c1, &c2);
    int ans = c2 - c1;
    if(ans % 100 >=50) {
        ans = ans / 100 + 1;
    } else {
        ans = ans / 100;
    }
    printf("%02d:%02d:%02d\n", ans/3600, ans%3600/60, ans%60);//不加%02d和\n一样错误。每个位置上不足两位的用0补齐两位，如01
    return 0;
}
```

#### 错误原因：

%02d：%md %0md:

不足m位的int型变量以M位向右对齐输出其中高位用空格/0补齐

### **1046 划拳**

划拳是古老中国酒文化的一个有趣的组成部分。酒桌上两人划拳的方法为：每人口中喊出一个数字，同时用手比划出一个数字。如果谁比划出的数字正好等于两人喊出的数字之和，谁就赢了，输家罚一杯酒。两人同赢或两人同输则继续下一轮，直到唯一的赢家出现。

下面给出甲、乙两人的划拳记录，请你统计他们最后分别喝了多少杯酒。

输入格式：

输入第一行先给出一个正整数 *N*（≤100），随后 *N* 行，每行给出一轮划拳的记录，格式为：

```
甲喊 甲划 乙喊 乙划
```

其中`喊`是喊出的数字，`划`是划出的数字，均为不超过 100 的正整数（两只手一起划）。

输出格式：

在一行中先后输出甲、乙两人喝酒的杯数，其间以一个空格分隔。

输入样例：

```in
5
8 10 9 12
5 10 5 10
3 8 5 12
12 18 1 13
4 16 12 15
```

输出样例：

```out
1 2
```

代码长度限制16 KB时间限制400 ms内存限制64 MB

#### test1

```c++
#include <cstdio>
int main() {
    int N, jh, jhua, yh, yhua,j=0, y=0, sum=0;
    scanf("%d", &N);
    while(N--){
        scanf("%d%d%d%d", &jh, &jhua, &yh, &yhua);
        sum = jh+yh;
        if(jhua == sum) {
            if(yhua != sum) y++;
        }else{
            if(yhua == sum) j++;
        }
    }
    printf("%d %d", y, j);
    return 0;
}
```

#### 错误总结：

1、循环内取到的值，在循环内声明就行：jh yh jhua yhua

2、md甲乙输出反了，怪不得错！这个程序没问题，我脑子有问题

#### test2

```c++
##include <cstdio>
int main(){
    int N, j=0, y=0;//扫描次数、甲乙喝酒的次数
    scanf("%d", &N);
    while(N--){
        int jh, jhua, yh, yhua, sum=0;//分别为甲乙喊话和划拳的数字，甲乙喊话的和
        scanf("%d%d%d%d", &jh, &jhua, &yh, &yhua);
        sum = jh+yh;
        if(jhua == sum) {
            if(yhua != sum) y++;
        }else{
            if(yhua == sum) j++;
        }
    }
    printf("%d %d", j, y);
    return 0;
}
```

### **1008 数组元素循环右移问题**

一个数组*A*中存有*N*（>0）个整数，在不允许使用另外数组的前提下，将每个整数循环向右移*M*（≥0）个位置，即将*A*中的数据由（*A*0*A*1⋯*A**N*−1）变换为（*A**N*−*M*⋯*A**N*−1*A*0*A*1⋯*A**N*−*M*−1）（最后*M*个数循环移至最前面的*M*个位置）。如果需要考虑程序移动数据的次数尽量少，要如何设计移动的方法？

输入格式:

每个输入包含一个测试用例，第1行输入*N*（1≤*N*≤100）和*M*（≥0）；第2行输入*N*个整数，之间用空格分隔。

输出格式:

在一行中输出循环右移*M*位以后的整数序列，之间用空格分隔，序列结尾不能有多余空格。

输入样例:

```in
6 2
1 2 3 4 5 6
```

输出样例:

```out
5 6 1 2 3 4
```

#### test1

```c++
##include <cstdio>
int main(){
    int N, M;//数组中有N个整数，右移M个位置
    scanf("%d%d", &N, &m);
    int a[N];
    while(N--){
        int i=0;
        scanf("%d",a[i++]);
    }
}
int move(int a[N], int x, int y){//一次后移几位，一共后移几次
    int j = a[y];
    while(y--){
        int i=0;
        a[i]
        i = a[i+x];
        a[i+x] = a[i];
        i++;
    }
    
}
```

#### 错误总结：

还是想不明白数组是怎么移的。互换位置简单，都往后移一位

#### test2

```c++
##include <cstdio>

int move(int a[N], int x, int y){
    //数组中有x个整数，右移y个位置
    y =y % x;//重点：除去多余X周期数，保证右移数目小于数组长度
    for(int i = x-y; i<=(x-1); y--){
        //从N-M到N-1,因为这两个数会到最前方
        int temp = a[i];
        int j = i-y;
        for(; j+x == i; j-=y){
            //每一轮循环都提出该数从前每隔y位找对应的数来填空，走一圈到这个数结束
            a[j+y] = a[j];//相当于右移,第一次j+y的位置就是i，j是其前一y的数
        }
        a[j] = temp;
    }
    return 0;
}
int main(){
    int N, M;//数组中有N个整数，右移M个位置
    scanf("%d%d", &N, &M);
    int a[N];
    for(int j = 0; j<N; j++){
        scanf("%d", &a[j]);//数组scanf也要加&！！！
    }
    move(a[], N, M);
    for(int i = 0; i<N; i++){
        printf("%d", a[i]);
        if(i < (N-1)) printf(" ");//输出格式，每个数之间插空格，最后一个没有
    }
    return 0;
}
```

```
a.cpp:3:16: error: ‘N’ was not declared in this scope
 int move(int a[N], int x, int y){
                ^
a.cpp:3:18: error: expected ‘)’ before ‘,’ token
 int move(int a[N], int x, int y){
                  ^
a.cpp:3:20: error: expected unqualified-id before ‘int’
 int move(int a[N], int x, int y){
                    ^~~
a.cpp: In function ‘int main()’:
a.cpp:25:12: error: expected primary-expression before ‘]’ token
     move(a[], N, M);
            ^
a.cpp:20:10: warning: ignoring return value of ‘int scanf(const char*, ...)’, declared with attribute warn_unused_result [-Wunused-result]
     scanf("%d%d", &N, &M);
     ~~~~~^~~~~~~~~~~~~~~~
a.cpp:23:14: warning: ignoring return value of ‘int scanf(const char*, ...)’, declared with attribute warn_unused_result [-Wunused-result]
         scanf("%d", &a[j]);//数组scanf也要加&！！！
         ~~~~~^~~~~~~~~~~~~
```

#### 错误总结

1、 move(a[], N, M);这里直接a就好，数组名就是其地址，需要的是int*而a[]则是int

2、int move(int a[N], int x, int y){ 这里数组a[]不需要有长度，只要命名出来就行

3、核心思想：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220819231714318.png" alt="image-20220819231714318" style="zoom: 33%;" />

#### test3

```c++
##include <cstdio>

int move(int a[], int x, int y){
    //数组中有x个整数，右移y个位置
    y =y%x;
    for(int i = x-y; i<=(x-1); y--){
        //从N-M到N-1
        int temp = a[i];
        int j = i-y;
        for(; j+x == i; j-=y){
            //每一轮循环都提出该数从前每隔y位找对应的数来填空，走一圈到这个数结束
            a[j+y] = a[j];//相当于右移,第一次j+y的位置就是i，j是其前一y的数
        }
        a[j] = temp;
    }
    return 0;
}
int main(){
    int N, M;//数组中有N个整数，右移M个位置
    scanf("%d%d", &N, &M);
    int a[N];
    for(int j = 0; j<N; j++){
        scanf("%d", &a[j]);//数组scanf也要加&！！！
    }
    move(a, N, M);
    for(int i = 0; i<N; i++){
        printf("%d", a[i]);
        if(i < (N-1)) printf(" ");
    }
    return 0;
}
```



##### 逃课

先输出N-M~N-1然后再输出0~N-M-1(程序只检查输出格式，只要输出对就行)

```c++
##include <cstdio>
int main() {
	int a[110];
	int N, M, count = 0; //记录输出个数
	scanf("%d%d", &N, &M);
	M = M % N;
	for(int i = 0; i < N; i++) {
        scanf("%d", &a[i]);
    }
    for(int i = N-M; i < N;i++){
        printf("%d", a[i]);
        count++;
        if(count < N) printf(" ");
    }
    for(int i = 0; i < N - M - 1; i++){
        printf("%d", a[i]);
        count++;
        if(count < N) printf(" ");
    }
    return 0;
}
```



### **1012 数字分类**

给定一系列正整数，请按要求对数字进行分类，并输出以下 5 个数字：

- *A*1 = 能被 5 整除的数字中所有偶数的和；
- *A*2 = 将被 5 除后余 1 的数字按给出顺序进行交错求和，即计算 *n*1−*n*2+*n*3−*n*4⋯；
- *A*3 = 被 5 除后余 2 的数字的个数；
- *A*4 = 被 5 除后余 3 的数字的平均数，精确到小数点后 1 位；
- *A*5 = 被 5 除后余 4 的数字中最大数字。

输入格式：

每个输入包含 1 个测试用例。每个测试用例先给出一个不超过 1000 的正整数 *N*，随后给出 *N* 个不超过 1000 的待分类的正整数。数字间以空格分隔。

输出格式：

对给定的 *N* 个正整数，按题目要求计算 *A*1~*A*5 并在一行中顺序输出。数字间以空格分隔，但行末不得有多余空格。

若分类之后某一类不存在数字，则在相应位置输出 `N`。

输入样例 1：

```in
13 1 2 3 4 5 6 7 8 9 10 20 16 18
```

输出样例 1：

```out
30 11 2 9.7 9
```

输入样例 2：

```in
8 1 2 4 5 6 7 9 16
```

输出样例 2：

```out
N 11 2 N 9
```

#### test1

```c++
##include <cstdio>
##include <cstring>
int main(){
    int n;
    scanf("%d", &n);
    int a[n];
    for(int i = 0; i < n; i++){
        scanf("%d",&a[i]);
    }
    
    int flag = 1;//之后用到的要先赋值，要不就是存放地址了
    int sum = 0;//记录A1，能被 5 整除的数字中所有偶数
    int ans = 0;//记录A2
    int num= 0;//计数A3
    double avg = 0.0, count4 = 0.0;//记录A4平均数
    int max = 0;//记录A5
    
    for(int i = 0; i < n; i++){
        switch(a[i]%5){
            case 0: //A1
                if(a[i] % 2 == 0) sum += a[i];
                break;
            case 1: //A2
                if(flag % 2 == 0){
                    ans -= a[i];
                }else{
                    ans += a[i];
                }
                flag++;
                break;
            case 2: //A3
                num++;
                break;
            case 3: //A4
                avg += a[i];
                count4++;
                break;
            case 4: //A5
                if(a[i] > max) max = a[i];
        }
    }
    
    printf("%d %d %d %.1f %d", sum, ans, num, avg / count4, max);//double 了就用%.1f别%d了
    return 0;
}
```

#### 错误总结：

还是细节上出的问题，还有没考虑某一类不存在数字的情况，可以用if（）else的办法解决

```c++
if(A1==0) printf("N ");
 else printf("%d ",A1);
 
if(n==0) printf("N ");
else printf("%d ",A2);
 
if(A3==0)  printf("N");
else printf("%d ",A3);
 
if(num==0) printf("N ");
else printf("%.1f ",sum/num);
 
if(max==0) printf("N");
else printf("%d",max);

```



#### test2

```c++
##include <cstdio>
int main(){
    int n;
    scanf("%d", &n);
    int a[n], ans[5], count[5];//灵魂所在，两个数组一个存输出数，一个存每位计数
    int flag = 1;
    
    for(int i = 0; i < n; i++){
        scanf("%d", &a[i]);
        if(a[i] % 5 == 0){
            if(a[i] % 2 == 0){
                ans[0] += a[i]; 
                count[0]++;
            }
        }else if(a[i] % 5 == 1){
            if(flag % 2 == 0){
                ans[1] -= a[i];
            }else{
                ans[1] += a[i];
            }
            count[1]++;
        }else if(a[i] % 5 == 2){
            count[2]++;
        }else if(a[i] % 5 == 3){
            ans[3] += a[i];
            count[3]++;
        }else if(a[i] % 5 == 4){
            if(ans[4] < a[i]) ans[4] = a[i];
            count[4]++;
        }
    }
    if(count[0]==0) printf("N ");
    else printf("%d ", ans[0]);
    if(count[1]==0) printf("N ");
    else printf("%d ", ans[1]);
    if(count[2]==0) printf("N ");
    else printf("%d ", ans[2]);
    if(count[3]==0) printf("N ");
    else printf("%.1f ", (double)ans[3]/count[3]);
    if(count[4]==0) printf("N");
    else printf("%d", ans[4]);
    return 0;
}
```

#### 错误总结：

1、都处在同一行：

```c++
 int a[n], ans[5], count[5];//灵魂所在，两个数组一个存输出数，一个存每位计数
```

ans，count都会出现位数为0的判断，所以必须定义时初始化为0

```c++
int ans[5] = {0};
int count[5] = {0};
int temp;
```

换成这个数组思路的解法，就不用a[n]了，直接定义一个temp暂时存放每次读入的数据就行

2、pow(-1,n+1)：-1的n+1次方

3、还是无法通过所有的样本，tobecontinue



### **1018 锤子剪刀布**

大家应该都会玩“锤子剪刀布”的游戏：两人同时给出手势，胜负规则如图所示：

现给出两人的交锋记录，请统计双方的胜、平、负次数，并且给出双方分别出什么手势的胜算最大。

输入格式：

输入第 1 行给出正整数 *N*（≤105），即双方交锋的次数。随后 *N* 行，每行给出一次交锋的信息，即甲、乙双方同时给出的的手势。`C` 代表“锤子”、`J` 代表“剪刀”、`B` 代表“布”，第 1 个字母代表甲方，第 2 个代表乙方，中间有 1 个空格。

输出格式：

输出第 1、2 行分别给出甲、乙的胜、平、负次数，数字间以 1 个空格分隔。第 3 行给出两个字母，分别代表甲、乙获胜次数最多的手势，中间有 1 个空格。如果解不唯一，则输出按字母序最小的解。

输入样例：

```in
10
C J
J B
C B
B B
B C
C C
C B
J B
B C
J J
```

输出样例：

```out
5 3 2
2 3 5
B B
```

#### test1

```c++
##include <cstdio>
int main() {
    int n;//记录谁赢得最多
    scanf("%d", &n);
    
    int C1 = 0, J1 = 0, B1 = 0;
    int C2 = 0, J2 = 0, B2 = 0;
    int p1[3] = {0};//分别代表二人（胜、平、负）的次数
    int p2[3] = {0};
    char c1, c, c2;
    for(int i = 0; i < n; i++) {
        scanf("%c%c%c", c1, c, c2);
        if(c1 == 'C') {
            if(c2 == 'J') { //p1 win
                p1[0]++;
                p2[2]++;
                J1++;
            }else if(c2 == 'C') { //draw
                p1[1]++;
                p2[1]++;
            }else if(c2 == 'B') { //p2 win
                p1[2]++;
                p2[0]++;
                B2++;
            }
        } else if(c1 == 'J'){
            if(c2 == 'B') { //p1 win
                p1[0]++;
                p2[2]++;
                B1++;
            }else if(c2 == 'J') { //draw
                p1[1]++;
                p2[1]++;
            }else if(c2 == 'C') { //p2 win
                p1[2]++;
                p2[0]++;
                C2++;
            }
        } else {
            if(c2 == 'J') { //p1 win
                p1[0]++;
                p2[2]++;
                J1++;
            }else if(c2 == 'B') { //draw
                p1[1]++;
                p2[1]++;
            }else if(c2 == 'C') { //p2 win
                p1[2]++;
                p2[0]++;
                C2++;
            }
        }
    }
    for(int i = 0; i<3; i++) {
            printf("%d",p1[]);
        }
    printf("/n");
    for(int i = 0; i<3; i++) {
            printf("%d",p2[]);
        }
    printf("/n");
    printf("", );
    return 0;
}
```

#### test2

```c++
##include <cstdio>
int main() {
    int n;//记录谁赢得最多
    scanf("%d", &n);
    
    //分别代表二人（胜、平、负）的次数
    int p1[3] = {0};
    int p2[3] = {0};
    //分别代表每人用那种拳法赢了几次
//     int C1 = 0, J1 = 0, B1 = 0;
//     int C2 = 0, J2 = 0, B2 = 0;
    int T1[3] = {0};//J0,C1,B2
    int T2[3] = {0};
    //记录最大值
    int max1 = 0, max2 = 0;
    //输入交手信息，char读入的时候会读入空格，所以用变量c吸收空格
    char c1, c, c2;
    for(int i = 0; i < n; i++) {
        scanf("%c%c%c", &c, &c1, &c2);
        //输入的同时判断每次的胜负，具体到每种情况3*3=9
        if(c1 == 'C') {
            if(c2 == 'J') { //p1 win
                p1[0]++;
                p2[2]++;
//                 J1++;
                T1[1]++;
            }else if(c2 == 'C') { //draw
                p1[1]++;
                p2[1]++;
            }else { //p2 win
                p1[2]++;
                p2[0]++;
//                 B2++;
                T2[2]++;
            }
        } else if(c1 == 'J'){
            if(c2 == 'B') { //p1 win
                p1[0]++;
                p2[2]++;
//                 B1++;
                T1[0]++;
            }else if(c2 == 'J') { //draw
                p1[1]++;
                p2[1]++;
            }else { //p2 win
                p1[2]++;
                p2[0]++;
//                 C2++;
                T2[1]++;
            }
        } else {//p1 == "B"
            if(c2 == 'C') { //p1 win
                p1[0]++;
                p2[2]++;
//                 J1++;
                T1[2]++;
            }else if(c2 == 'B') { //draw
                p1[1]++;
                p2[1]++;
            }else { //p2 win
                p1[2]++;
                p2[0]++;
//                 C2++;
                T2[0]++;
            }
        }
    }
    //此处应该有两次三个数比大小，本别得到两个最合适的数
    for(int i = 0; i<3; i++) {
        printf("%d",p1[i]);
        if(i != 2) printf(" ");
        if(max1 < T1[i]) max1 = T1[i];
        }
    printf("\n");
    for(int i = 0; i<3; i++) {
        printf("%d",p2[i]);
        if(i != 2) printf(" ");
        if(max2 < T2[i]) max2 = T2[i];
        }
    printf("\n");
    //通过之前的最值判断输出最赢结果
    if(max1 == T1[0]){
        printf("J ");
    }else if(max1 == T1[1]){
        printf("C ");
    }else{
        printf("B ");
    }
    if(max2 == T2[0]){
        printf("J");
    }else if(max2 == T2[1]){
        printf("C");
    }else{
        printf("B");
    }
//     printf("%d %d", max1, max2); //输出最赢值
    return 0;
}
```

#### test2最后通关版本

```c++
##include <cstdio>
int main() {
    int n;//记录谁赢得最多
    scanf("%d", &n);
    
    //分别代表二人（胜、平、负）的次数
    int p1[3] = {0};
    int p2[3] = {0};
    //分别代表每人用那种拳法赢了几次
//     int C1 = 0, J1 = 0, B1 = 0;
//     int C2 = 0, J2 = 0, B2 = 0;
    int T1[3] = {0};//J0,C1,B2
    int T2[3] = {0};
    //记录最大值，及相同最大值序号
    int max1 = 0, max2 = 0;
    int count1 = 0, re1 = 0;
    int count2 = 0, re2 = 0;
    //输入交手信息，char读入的时候会读入空格，所以用变量c吸收空格
    char c1, c2;
    for(int i = 0; i < n; i++) {
        getchar();
        scanf("%c %c", &c1, &c2);
        //输入的同时判断每次的胜负，具体到每种情况3*3=9
        if(c1 == 'C') {
            if(c2 == 'J') { //p1 win
                p1[0]++;
                p2[2]++;
//                 J1++;
                T1[1]++;
            }else if(c2 == 'C') { //draw
                p1[1]++;
                p2[1]++;
            }else { //p2 win
                p1[2]++;
                p2[0]++;
//                 B2++;
                T2[2]++;
            }
        } else if(c1 == 'J'){
            if(c2 == 'B') { //p1 win
                p1[0]++;
                p2[2]++;
//                 B1++;
                T1[0]++;
            }else if(c2 == 'J') { //draw
                p1[1]++;
                p2[1]++;
            }else { //p2 win
                p1[2]++;
                p2[0]++;
//                 C2++;
                T2[1]++;
            }
        } else {//p1 == "B"
            if(c2 == 'C') { //p1 win
                p1[0]++;
                p2[2]++;
//                 J1++;
                T1[2]++;
            }else if(c2 == 'B') { //draw
                p1[1]++;
                p2[1]++;
            }else { //p2 win
                p1[2]++;
                p2[0]++;
//                 C2++;
                T2[0]++;
            }
        }
    }
    //此处应该有两次三个数比大小，本别得到两个最合适的数
    for(int i = 0; i<3; i++) {
        printf("%d",p1[i]);
        if(i != 2) printf(" ");
        if(max1 < T1[i]) {
            max1 = T1[i];
            count1 = i;
        } else if(max1 == T1[i]) {
            re1 = i;
        }
        }
    printf("\n");
    for(int i = 0; i<3; i++) {
        printf("%d",p2[i]);
        if(i != 2) printf(" ");
        //找最优解（赢次数最大值）同时，将同样最大值序号记下
        if(max2 < T2[i]) {
            max2 = T2[i];
            count2 = i;
        } else if(max2 == T2[i]) {
            re2 = i;
        }
        }
    printf("\n");
    //通过之前的最值判断输出最赢结果，如解不唯一，输出字母序最小的
    if(re1){//设置计赢次数 数组的时候应该按照字母序排，这样直接按照顺序就能输出，我写的正好是反序
        if(re1 > count1){
            if(re1 == 0){
                printf("J ");
            }else if(re1 == 1){
                printf("C ");
            }else{
                printf("B ");
            }
        }else {
            if(count1 == 0){
                printf("J ");
            }else if(count1 == 1){
                printf("C ");
            }else{
                printf("B ");
            }
        }
    }else {
        if(max1 == T1[0]){
            printf("J ");
        }else if(max1 == T1[1]){
            printf("C ");
        }else{
            printf("B ");
        }
    }
    
    if(re2){//设置计赢次数 数组的时候应该按照字母序排，这样直接按照顺序就能输出，我写的正好是反序
        if(re2 > count2){
            if(re2 == 0){
                printf("J");
            }else if(re2 == 1){
                printf("C");
            }else{
                printf("B");
            }
        }else {
            if(count2 == 0){
                printf("J");
            }else if(count2 == 1){
                printf("C");
            }else{
                printf("B");
            }
        }
    }else {
        if(max2 == T1[0]){
            printf("J");
        }else if(max2 == T1[1]){
            printf("C");
        }else{
            printf("B");
        }
    }
//     printf("%d %d", max1, max2); //输出最赢值
    return 0;
}
```



#### 错误总结：

1、本题中scanf()读入格式很重要，因为读入的char格式会自动吸收\n，解决方案：将原本

```c++
scanf("%c%c%c", &c1, &c, &c2);// 原本一位空格也会影响，但是直接在输入格式中就可声明存在空格
```

替换为：

```c++
getchar();//吸收回车
scanf("%c %c", &c1, &c2);
```

2、如果解不唯一，则输出按字母序最小的解。

怎么解决是个问题：

```C++
if(max1 == T1[0]){
    printf("J ");
}else if(max1 == T1[1]){
    printf("C ");
}else{
    printf("B ");
}
if(max2 == T2[0]){
    printf("J");
}else if(max2 == T2[1]){
    printf("C");
}else{
    printf("B");
}
```

```c++
for(int i = 0; i<3; i++) {
        printf("%d",p2[i]);
        if(i != 2) printf(" ");
        //找最优解（赢次数最大值）同时，将同样最大值序号记下
        if(max2 < T2[i]) {
            max2 = T2[i];
            count2 = i;
        } else if(max2 == T2[i]) {
            re2 = i;
        }
        }
    printf("\n");
    //通过之前的最值判断输出最赢结果，如解不唯一，输出字母序最小的
    if(re1){//设置计赢次数 数组的时候应该按照字母序排，这样直接按照顺序就能输出，我写的正好是反序
        if(re1 > count1){
            if(re1 == 0){
                printf("J ");
            }else if(re1 == 1){
                printf("C ");
            }else{
                printf("B ");
            }
        }else {
            if(count1 == 0){
                printf("J ");
            }else if(count1 == 1){
                printf("C ");
            }else{
                printf("B ");
            }
        }
    }else {
        if(max1 == T1[0]){
            printf("J ");
        }else if(max1 == T1[1]){
            printf("C ");
        }else{
            printf("B ");
        }
    }
    
    if(re2){//设置计赢次数 数组的时候应该按照字母序排，这样直接按照顺序就能输出，我写的正好是反序
        if(re2 > count2){
            if(re2 == 0){
                printf("J");
            }else if(re2 == 1){
                printf("C");
            }else{
                printf("B");
            }
        }else {
            if(count2 == 0){
                printf("J");
            }else if(count2 == 1){
                printf("C");
            }else{
                printf("B");
            }
        }
    }else {
        if(max2 == T1[0]){
            printf("J");
        }else if(max2 == T1[1]){
            printf("C");
        }else{
            printf("B");
        }
    }
```

能解决但是好麻烦啊，就这小程序写了150行代码

#### test3:最优解

```c++
##include <cstdio>
int change(char c){
    if(c == 'B') return 0;
    if(c == 'C') return 1;
    if(c == 'J') return 2;
}
int main(){
    char mp[3] = {'B', 'C', 'J'};//数组序号（数字）与字符联系起来
    int n;
    scanf("%d", &n);
    int T1[3] = {0}, T2[3] = {0};  //记录甲乙胜负平次数
    int w1[3] = {0}, w2[3] = {0};  //BCJ顺序分别记甲乙赢手
    char c1, c2;  //输入甲乙手势
    int k1, k2;  //将上面的转化为数字
    for(int i = 0; i< n; i++){
        getchar();
        scanf("%c %c", &c1, &c2);
        k1 = change(c1);
        k2 = change(c2);
        //利用循环相克顺序判断输赢
        if((k1 + 1 ) % 3 == k2) {  //如果甲赢
            T1[0]++;
            T2[2]++;
            w1[k1]++;
        } else if(k1 ==k2){
            T1[1]++;
            T2[1]++;
        } else {
            T1[2]++;
            T2[0]++;
            w2[k2]++;
        }
    }
    printf("%d %d %d\n", T1[0], T1[1], T1[2]);
    printf("%d %d %d\n", T2[0], T2[1], T2[2]);
    //找出获胜最多手势的数字（序号）
    int id1 = 0, id2 = 0;
    for(int i = 0; i < 3; i++){
        if(w1[i] > w1[id1]) id1 = i;
        if(w2[i] > w2[id1]) id2 = i;
    }
    printf("%c %c\n", mp[id1], mp[id2]); //数字序号状态转回BCJ字符
    return 0;
}
```

#### 总结优点：

1、将数字与字符链接在一起而且是从零开始（符合数组起始位置）、字符序从小到大，可以用change和mp[]往返变

2、在可以用数字序号的情况下，利用其循环相克的特性判断输赢，记录各手势获胜情况

```c++
if((k1 + 1 ) % 3 == k2)  //甲赢，循环相克，%3表示循环，k1+1表示取k1下一位，前克后
```

3、已知每位选手各个手势的赢次后，选择直接在循环中将序号带入数组，直接比赢次，记录每次比较大值的序号，并让其一直保持最大，并且因为定义时各手势与对应的字符从小到大是字符序增序，所以循环从小到大取，只有遇到相比大的数才会改变记录数，所以当遇到相等赢次的时候不会改变，记录的序号也自然而然是之前的小序号对应的小字符，喵

4、将上一步得到的数组序号转成字符形式

---

### **1010 一元多项式求导**

设计函数求一元多项式的导数。（注：*x**n*（*n*为整数）的一阶导数为*n**x**n*−1。）

输入格式:

以指数递降方式输入多项式非零项系数和指数（绝对值均为不超过 1000 的整数）。数字间以空格分隔。

输出格式:

以与输入相同的格式输出导数多项式非零项的系数和指数。数字间以空格分隔，但结尾不能有多余空格。注意“零多项式”的指数和系数都是 0，但是表示为 `0 0`。

输入样例:

```in
3 4 -5 2 6 1 -2 0
```

输出样例:

```out
12 3 -10 1 6 0
```

#### test1

```c++
##include <cstdio>
int main(){
    int n, a[110],count = 0;
    while(n) {
        scanf("%d", &n);
        a[count] = n;
        count++;
    }
    for(int i = 0; i < count; i += 2) {
        for(int j = 0; j < 2; j++) {
            a[i] *= a[i+1];
            a[i+1]--;
            printf("%d %d", a[i], a[i+1]);
            if(i != (count-1)) printf(" ");
        }
    }
    return 0;
}
```

#### 错误总结：

1、莫名其妙的for嵌套

2、数组用大了而且没有初始赋值

#### test2

```c++
##include <cstdio>
int main(){
    int n, a[10] = {0}, count = 0;
    while(n) {
        scanf("%d", &n);
        a[count] = n;
        count++;
    }
    count -= 2;
    for(int i = 0; i < count; i += 2) {
        a[i] *= a[i+1];
        a[i+1]--;
        printf("%d %d", a[i], a[i+1]);
        if(i != (count-1)) printf(" ");
    }
    return 0;
}
```

在Clion上可以编译，但是在平台上没效果

#### test3:最终版

```c++
##include <cstdio>
int main(){
    int a[1010] = {0};//留10位
    int k, e, count = 0; //k系数，e指数，count不为零的导数项个数
    while(scanf("%d%d", &k, &e) != EOF){//%d 不会读入空格
        a[e] = k; //将指数与系数形成对应顺次关系
    }
    //开始计算
    a[0] = 0;
    for(int i = 1; i <= 1000; i++) {
        a[i -1] = a[i] * i; //上一位指数对应的系数=此位指数的系数与其自身之积（等价于积后指数减一）
        a[i] = 0; //令此位系数清零，方便接收下一位传来的系数
        if(a[i-1] != 0)count++; //不为零导数
    }
    if(count == 0)printf("0 0"); //如为“零多项式”（指数和系数都是 0，但是表示为 0 0
    else{
        for(int i = 1000; i >= 0; i--) {
            if(a[i] != 0) { //这一位有系数的话
                printf("%d %d", a[i], i);
                count--;
                if(count != 0)printf(" ");
            }
        }
    }
    return 0;
}
```

#### 总结：

1、

2、

3、

---

## 3.2 查找元素

### **1041 考试座位号**

每个 PAT 考生在参加考试时都会被分配两个座位号，一个是试机座位，一个是考试座位。正常情况下，考生在入场时先得到试机座位号码，入座进入试机状态后，系统会显示该考生的考试座位号码，考试时考生需要换到考试座位就座。但有些考生迟到了，试机已经结束，他们只能拿着领到的试机座位号码求助于你，从后台查出他们的考试座位号码。

输入格式：

输入第一行给出一个正整数 *N*（≤1000），随后 *N* 行，每行给出一个考生的信息：`准考证号 试机座位号 考试座位号`。其中`准考证号`由 16 位数字组成，座位从 1 到 *N* 编号。输入保证每个人的准考证号都不同，并且任何时候都不会把两个人分配到同一个座位上。

考生信息之后，给出一个正整数 *M*（≤*N*），随后一行中给出 *M* 个待查询的试机座位号码，以空格分隔。

输出格式：

对应每个需要查询的试机座位号码，在一行中输出对应考生的准考证号和考试座位号码，中间用 1 个空格分隔。

输入样例：

```in
4
3310120150912233 2 4
3310120150912119 4 1
3310120150912126 1 3
3310120150912002 3 2
2
3 4
```

输出样例：

```out
3310120150912002 2
3310120150912119 1
```

#### test1

```c++
##include <cstdio>
int main(){
    int N;
    scanf("%d", &N);
    int Knum[16] = {0}, Js[N] ={0}, Ks[N] = {0};
    for(int i = 0; i < N; i++){
        for(int  i = 0; i<16; i++) {
            scanf("%d", &Knum[i]);
        }
        scanf("%d%d", &Js[i], &Ks[i]);
    }
    int M;
    scanf("%d", &M);
    int ans[M] = {0};
    for(int i = 0; i<M; i++){
        scanf("%d", &ans[i]);
    }
    //查找
    for(int i = 0; i < M; i++){
        for(int j = 0; j<N; j++){
            if(ans[i] == Js[j]) {
                for(int  i = 0; i<16; i++) {
                    printf("%d", &Knum[i]);
                }
                printf(" %d", Ks[j]);
            }
        }
    }
    return 0;
}
```

#### 错误总结：

1、超时，下标越界：减少for循环使用，用longlong变量代替数组存放

#### test2

```c++
##include <cstdio>
int main() {
    int N;
    scanf("%d", &N);
    int Js[N] ={0}, Ks[N] = {0};
    long long Knum = 0;//导致前后两人学号一样
    for(int i = 0; i < N; i++){
        scanf("%lld", &Knum);
        scanf("%d%d", &Js[i], &Ks[i]);
    }
    int M;
    scanf("%d", &M);
    int ans[M] = {0};
    for(int i = 0; i < M; i++){
        scanf("%d", &ans[i]);
    }
    //查找
    for(int i = 0; i < M; i++){
        for(int j = 0; j < N; j++){
            if(ans[i] == Js[j]) {
                printf("%lld", Knum);
                printf(" %d\n", Ks[j]);//换行记得加
            }
        }
    }
    return 0;
}
```

#### 错误总结：

1、输出格式问题:没有换行（就这样居然还对了一个，是不是只有一行的那种？）

2、前后两人学号一样了，原因大概是把Kum改成了单一变量，因为存储不止一个，所以应该是longlong数组

#### test3：答案

```

```



### **1004 成绩排名**

读入 *n*（>0）名学生的姓名、学号、成绩，分别输出成绩最高和成绩最低学生的姓名和学号。

输入格式：

每个测试输入包含 1 个测试用例，格式为

```
第 1 行：正整数 n
第 2 行：第 1 个学生的姓名 学号 成绩
第 3 行：第 2 个学生的姓名 学号 成绩
  ... ... ...
第 n+1 行：第 n 个学生的姓名 学号 成绩
```

其中`姓名`和`学号`均为不超过 10 个字符的字符串，成绩为 0 到 100 之间的一个整数，这里保证在一组测试用例中没有两个学生的成绩是相同的。

输出格式：

对每个测试用例输出 2 行，第 1 行是成绩最高学生的姓名和学号，第 2 行是成绩最低学生的姓名和学号，字符串间有 1 空格。

输入样例：

```in
3
Joe Math990112 89
Mike CS991301 100
Mary EE990830 95
```

输出样例：

```out
Mike CS991301
Joe Math990112
```

#### test1

```c++
##include <cstdio>
int main() {
    int N = 0;
    scanf("%d", &N);
//     vector<vector<char> > name(N, vector<char>(10, 0));
//     vector<vector<int> > num(N, vector<int>(10, 0));
    char name[N], num[N];
    int score[N];
    for(int i = 0; i < N; i++){
        score[i] = 0;
        name[i] = ' ';
        num[i] = ' ';
    }

    int max = 0, min = 0;//设置最大最小值

    for(int i = 0; i < N; i++) {
        //cin
        scanf("%c %c %d", &name[N], &num[N], &score[N]);
        //match
        if(score[N] > max) max = score[N];
        else if(score[N] < min) min = score[N];
    }
    printf("%c %c\n", name[max], num[max]);
    printf("%c %c", name[max], num[max]);
    return 0;
}
```

#### 错误总结：

1、编译越界，应该是数据读取不进来，没思路看答案了

#### test2

```c++
##include <cstdio>
struct Student {
    char name[15];
    char id[15];
    int score;
}temp, ans_max, ans_min;
//创建Student类变量三个，分别代表临时数据、最高分学生、最低分学生
int main(){
	int n;
    scanf("%d", n);
    //最高分最低分初始值设置
    ans_max.score = -1;
    ans_min.score = 101;
    for(int i = 0; i < n; i++){
        scanf("%s%s%d", temp.name, temp.id, temp.score);//读取信息，字符串数组不用带&和空格
        if(temp.score > ans_max.score) ans_max = temp;
        if(temp.score < ans_min.score) ans_min = temp;
    }
    printf("%s %s\n",ans_max.name, ans_max.id);
    printf("%s %s\n",ans_min.name, ans_min.id);
    return 0;
}
```

#### 错误总结

1、段错误

```
a.cpp: In function ‘int main()’:
a.cpp:10:18: warning: format ‘%d’ expects argument of type ‘int*’, but argument 2 has type ‘int’ [-Wformat=]
     scanf("%d", n);
                  ^
a.cpp:15:55: warning: format ‘%d’ expects argument of type ‘int*’, but argument 4 has type ‘int’ [-Wformat=]
         scanf("%s%s%d", temp.name, temp.id, temp.score);//读取信息，存临时，字符串数组不用带&
                                             ~~~~~~~~~~^
a.cpp:10:10: warning: ignoring return value of ‘int scanf(const char*, ...)’, declared with attribute warn_unused_result [-Wunused-result]
     scanf("%d", n);
     ~~~~~^~~~~~~~~
a.cpp:15:14: warning: ignoring return value of ‘int scanf(const char*, ...)’, declared with attribute warn_unused_result [-Wunused-result]
         scanf("%s%s%d", temp.name, temp.id, temp.score);//读取信息，存临时，字符串数组不用带&
         ~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
a.cpp:10:10: warning: ‘n’ is used uninitialized in this function [-Wuninitialized]
     scanf("%d", n);
     ~~~~~^~~~~~~~~
```

```c++
scanf("%d", n);//忘加&
scanf("%s%s%d", temp.name, temp.id, temp.score);//temp.score不是字符串数组，前面要加&
```



### 常见错误总结：

1、几个相关信息可以集团化应该选择创建结构体

2、字符数组要预留一个位置给”\n“

3、字符串数组：%s用来读取字符串且视空格换行为结束。每个字符串数组要给最后的结束符（\0，NULL）留一位。%c用来输单个字符，可识别空格和换行

### **1028 人口普查**

某城镇进行人口普查，得到了全体居民的生日。现请你写个程序，找出镇上最年长和最年轻的人。

这里确保每个输入的日期都是合法的，但不一定是合理的——假设已知镇上没有超过 200 岁的老人，而今天是 2014 年 9 月 6 日，所以超过 200 岁的生日和未出生的生日都是不合理的，应该被过滤掉。

输入格式：

输入在第一行给出正整数 *N*，取值在(0,105]；随后 *N* 行，每行给出 1 个人的姓名（由不超过 5 个英文字母组成的字符串）、以及按 `yyyy/mm/dd`（即年/月/日）格式给出的生日。题目保证最年长和最年轻的人没有并列。

输出格式：

在一行中顺序输出有效生日的个数、最年长人和最年轻人的姓名，其间以空格分隔。

输入样例：

```in
5
John 2001/05/12
Tom 1814/09/06
Ann 2121/01/30
James 1814/09/05
Steve 1967/11/20
```

输出样例：

```out
3 Tom John
```

#### test1

```c++
##include <cstdio>
struct person{
    char name[10];
    int date;
}max, min, left, right;
int main(){
    int N;
    scanf("%d", &N);
    while
}
```

#### 错误总结：

1、问题，日期格式读入怎么区别、比较？

#### test2：答案

```c++
##include <cstdio>
struct person{
    char name[10];
    int yy, mm, dd;
}max, min, left, right, temp;
bool LessEqu(person a, person b){
    if(a.yy != b.yy) return a.yy <= b.yy;
    else if(a.mm != b.mm) return a.mm <= b.mm;
    else return a.dd <= b.dd;
}
bool MoreEqu(person a, person b){
    if(a.yy != b.yy) return a.yy >= b.yy;
    else if(a.mm != b.mm) return a.mm >= b.mm;
    else return a.dd >= b.dd;
}
void init(){
    min.yy = left.yy = 1814;
    max.yy = right.yy = 2014;
    max.mm = min.mm = left.mm = right.mm = 9;
    max.mm = min.mm = left.mm = right.mm = 6;
}
int main(){
    init();
    int N, num= 0; //num存放合法日期人数
    scanf("%d", &N);
    for(int i = 0; i<N;i++) {
        scanf("%s %d/%d/%d", temp.name, &temp.yy, &temp.mm, &temp.dd);
        if(MoreEqu(temp, left) && LessEqu(temp, right)){
            num++;
            if(LessEqu(temp, min)) min = temp;
            if(MoreEqu(temp, max)) max = temp;
        }
    }
    if(num == 0) printf("0\n");
    else printf("%d %s %s\n", num, max.name, min.name);
    return 0;
}
```

#### test3：答案2

```c++
##include <cstdio>
struct person {
	char name[10];
	int yy, mm, dd;
}oldest,youngest,left,right,temp;

bool LessEqu(person a, person b) {
	if (a.yy != b.yy) return a.yy <= b.yy;
	else if (a.mm != b.mm) return a.mm <= b.mm;
	else return a.dd <= b.dd;
}
bool MoreEqu(person a, person b) {
	if (a.yy != b.yy) return a.yy >= b.yy;
	else if (a.mm != b.mm) return a.mm >= b.mm;
	else return a.dd >= b.dd;
}

void init() {
	youngest.yy = left.yy = 1814;
	oldest.yy = right.yy = 2014;
	youngest.mm = oldest.mm = left.mm = right.mm = 9;
	youngest.dd = oldest.dd = left.dd = right.dd = 6;
}

int main() {
	init();
	int n, num = 0;
	scanf("%d", &n);
	for (int i = 0; i < n; i++)
	{
		scanf("%s %d/%d/%d", temp.name, &temp.yy, &temp.mm, &temp.dd);
		if (MoreEqu(temp, left) && LessEqu(temp, right)) {
			num++;
			if (LessEqu(temp, oldest)) oldest = temp;
			if (MoreEqu(temp, youngest)) youngest = temp;
		}	
	}
	if (num == 0) printf("0\n");
	else printf("%d %s %s\n", num, oldest.name, youngest.name);
	return 0;
}
```



### **1032 挖掘机技术哪家强**

为了用事实说明挖掘机技术到底哪家强，PAT 组织了一场挖掘机技能大赛。现请你根据比赛结果统计出技术最强的那个学校。

输入格式：

输入在第 1 行给出不超过 105 的正整数 *N*，即参赛人数。随后 *N* 行，每行给出一位参赛者的信息和成绩，包括其所代表的学校的编号（从 1 开始连续编号）、及其比赛成绩（百分制），中间以空格分隔。

输出格式：

在一行中给出总得分最高的学校的编号、及其总分，中间以空格分隔。题目保证答案唯一，没有并列。

输入样例：

```in
6
3 65
2 80
1 100
2 70
3 40
3 0
```

输出样例：

```out
2 150
```

#### test1

```c++
##include <cstdio>
struct person {
    int id, score;
}temp, a;
void init() {
    a.id = a.score = 0;
}
int main(){
    init();
    int N;
    scanf("%d", &N);
    while(N--) {
        scanf("%d %d", temp.id, temp.score);
        if(a.score < temp.score) {
            a = temp;
        }
    }
    printf("%d %d", a.id, a.score);
    return 0;
}
```

#### 错误总结：

#### test2

```c++
##include <cstdio>
int main(){
    int N;
    scanf("%d", &N);
    int p[N]= {0};
    int id, num, max = 0, mid = 0;
    for(int i = 0; i < N; i++){
        scanf("%d %d", &id, &num);
        p[id - 1] += num;
        
    }
    for(int i = 0; i < N; i++){
        if(p[i] > max) {
            max = p[i];
            mid = i;
        }
    }
    printf("%d %d", mid + 1, max);
}
```



---

## 3.3 图形输出



### B1036 **跟奥巴马一起编程**

美国总统奥巴马不仅呼吁所有人都学习编程，甚至以身作则编写代码，成为美国历史上首位编写计算机代码的总统。2014 年底，为庆祝“计算机科学教育周”正式启动，奥巴马编写了很简单的计算机代码：在屏幕上画一个正方形。现在你也跟他一起画吧！

输入格式：

输入在一行中给出正方形边长 *N*（3≤*N*≤20）和组成正方形边的某种字符 C，间隔一个空格。

输出格式：

输出由给定字符 C 画出的正方形。但是注意到行间距比列间距大，所以为了让结果看上去更像正方形，我们输出的行数实际上是列数的 50%（四舍五入取整）。

输入样例：

```in
10 a
```

输出样例：

```out
aaaaaaaaaa
a        a
a        a
a        a
aaaaaaaaaa
```

#### test1

```c++
##include <cstdio>
int main(){
    int row, col;
    char shape;
    scanf("%d %c", &row, &shape);
    col = (int)((row+0.5)/5);
    while(row--){
        printf("%c", shape);
    }//第一行
    printf("\n");
    for(int j = 1; j < col-1; j++){
        printf("%c", shape);
        for(int o = 1; o < row-1; o++){
            printf(" ");
        }//两个字母中间的空格
        printf("%c", shape);
    }//中间n-2行
    printf("\n");
    while(row--){
        printf("%c", shape);
    }//最后一行
    return 0;
}
```

#### 错误总结：

1、操作超时

2、四舍五入思想：不够5的消去小数位，大等5的+1

#### test2:答案

```c++
##include <cstdio>
int main(){
    int row, col;//行数，列数
    char c;
    scanf("%d %c", &col, &c);
    //都是减半，根据四舍五入奇数取半后需要+1
    if(col % 2 == 1)row = col/2+1;
    else row = col /2;
    
    //第一行
    for(int i = 0; i< col; i++){
        printf("%c", c);
    }
    printf("/n");
    //中间行
    for(int i = 2; i<row; i++){
        printf("c",c);//每行第一个字母
        for(int j = 0; j <col - 2; j++){
            printf(" ");//中间的空格
        }
        printf("c\n", c);//每行最后一个字母
    }
    //最后一行
    for(int i = 0; i< col; i++){
        printf("%c", c);
    }
    
    return 0;
}
```



### B1027 打印沙漏

本题要求你写个程序把给定的符号打印成沙漏的形状。例如给定17个“*”，要求按下列格式打印

```
*****
 ***
  *
 ***
*****
```

所谓“沙漏形状”，是指每行输出奇数个符号；各行符号中心对齐；相邻两行符号数差2；符号数先从大到小顺序递减到1，再从小到大顺序递增；首尾符号数相等。

给定任意N个符号，不一定能正好组成一个沙漏。要求打印出的沙漏能用掉尽可能多的符号。

输入格式:

输入在一行给出1个正整数N（≤1000）和一个符号，中间以空格分隔。

输出格式:

首先打印出由给定符号组成的最大的沙漏形状，最后在一行中输出剩下没用掉的符号数。

输入样例:

```in
19 *
```

输出样例:

```out
*****
 ***
  *
 ***
*****
2
```

#### test1

```c++
##include <cstdio>
int main(){
    int N, ans;
    scanf("%d", &N);
    //x：除去每次必有的中间一层一个星星，再对折，一边还剩多少
    int x = (N - 1) / 2;
    //flag为一边除了一颗星星之外的第一层里的数量
    int flag = 3;
    //count为一边层数
    int count = 0;
    //剩余层数
    int num = 0;
    //算出一边有多少层
    while(x > flag) {
        x -= flag;
        flag += 2;
        count++;
    }
    //算出多余数目
    for(int i = 1; i <= count; i++) {
        num += i * 2 + 1;
    }
    ans = x - num;
    //上半部分
    for(int i = count; i > 0; i--) {
        while(i * 2 + 1){
            printf("*");
            i--;
        }
        printf("\n");
    }
    
    printf("*");//中间一*
    
    //下半部分
    for(int i = 1; i <= count; i++) {
        while(i * 2 + 1){
            printf("*");
            i--;
        }
        printf("\n");
    }
    printf("%d", ans);
    return 0;
}
```

#### 错误总结：

1、运行超时

2、

#### test2

```c++
##include <cstdio>
int main(){
    int N, ans;
    scanf("%d", &N);
    //x：除去每次必有的中间一层一个星星，再对折，一边还剩多少
    int x = (N - 1) / 2;
    //flag为一边除了中间一层单个*之外的第一层里的数量
    int flag = 3;
    //count为单边层数
    int count = 0;
    //num目前层数
    int num = 0;
    
    //计算环节：
    //算出一边有多少层
    while(x > flag) {
        x -= flag;
        flag += 2;
        count++;//count算出结果是对的，x=1，flag=7，导致ans不对
    }
    //算出多余数目
    for(int i = 1; i <= count; i++) {
        num += i * 2 + 1;
    }
    ans = x - num;
    ------------------------------------------------------------
    //输出环节：
    //上半部分
    //循环层数,从上到下，依次递减n~1
    for(int i = count; i > 0; i--) {
        //j:每一层多少个*
        int j = i * 2 + 1;
        //每层需要多少个空格,需要和层数挂钩
        if(i != count){
            for(int t = i; t > 0; t--) {
                printf(" ");
            }
        }
        //
        for(int t = j; t > 0; t--) {
            printf("*");
        }
        printf("\n");
    }

    //中间层,(1),空格+*
    int y = count;
    while(y--){
        printf(" ");
    }
    printf("*\n");

    //下半部分
    //循环层数，从上到下依次递增，1~n
    for(int i = 1; i <= count; i++) {
        //每层有多少个*
        int j = i * 2 + 1;
        //每层空格输出
        if(i != count){
            for(int t = i; t>0; t--) {
                printf(" ");
            }
        }
        //每层*输出
        for(int t = j; t>0; t--) {
            printf("*");
        }
        printf("\n");
    }
    printf("%d", ans);
    return 0;
}
```

#### 错误总结

1、运行超时，循环内变量问题：for（int i）内用while（i--），导致运行次数混乱

2、ans剩余*数错误，见前半部分：“算出多余数目”出错

```c++
    //计算环节：
    //算出一边有多少层
    while(x > flag) {
        x -= flag;
        flag += 2;
        count++;
    }
    //算出多余数目
    ans = x * 2;
//    for(int i = 1; i <= count; i++) {
//        num += i * 2 + 1;
//    }
//    ans = x - num;
```

```
19 *
*****
 ***
  *
 ***
*****
2
```

3、剩下的问题就是把“*”换成任何东西

#### test3

```c++
##include <cstdio>
int main(){
    int N, ans;
    char c;
    scanf("%d %c", &N, &c);

    //x：除去每次必有的中间一层一个星星，再对折，一边还剩多少
    int x = (N - 1) / 2;
    //flag为一边除了中间一层单个*之外的第一层里的数量
    int flag = 3;
    //count为单边层数
    int count = 0;

    //计算环节：
    //算出一边有多少层
    while(x > flag) {
        x -= flag;
        flag += 2;
        count++;//count算出结果是对的，x=1，flag=7，导致ans不对
    }
    //算出多余数目
    ans = x * 2;
//------------------------------------------------------------
    //输出环节：
    //上半部分
    //循环层数,从上到下，依次递减n~1
    for(int i = count; i > 0; i--) {
        //j:每一层多少个*
        int j = i * 2 + 1;
        //每层需要多少个空格,需要和层数挂钩
        if(i != count){
            for(int t = i; t > 0; t--) {
                printf(" ");
            }
        }
        //符号输出
        for(int t = j; t > 0; t--) {
            printf("%c", c);
        }
        printf("\n");
    }

    //中间层,(1),空格+*
    int y = count;
    while(y--){
        printf(" ");
    }
    printf("%c\n", c);

    //下半部分
    //循环层数，从上到下依次递增，1~n
    for(int i = 1; i <= count; i++) {
        //每层有多少个*
        int j = i * 2 + 1;
        //每层空格输出
        if(i != count){
            for(int t = i; t>0; t--) {
                printf(" ");
            }
        }
        //每层*输出
        for(int t = j; t>0; t--) {
            printf("%c", c);
        }
        printf("\n");
    }
    printf("%d", ans);
    return 0;
}
```



```
56 @
@@@@@@@@@
   @@@@@@@
  @@@@@
 @@@
    @
 @@@
  @@@@@
   @@@@@@@
@@@@@@@@@
6
```

#### 错误总结

1、看来空格输出有问题了，中间和最上下三行位置和每一行输出的符号数目没问题，目前的逻辑是将中间的空格输出和层数相关：应该把单区行数和列数与空格数同时相关。如，上半层随层数递增，下半层随层数递减

```c++
//------------------------------------------------------------
    //输出环节：
    //上半部分
    //循环层数,从上到下，依次递减n~1
    for(int i = count; i > 0; i--) {
        //j:每一层多少个*
        int j = i * 2 + 1;
        //每层需要多少个空格,上半层随层数递增加，下半递减
        int l = count - i;
        if(i != count){
            while(l--){
                printf(" ");
            }
        }
        //
        for(int t = j; t > 0; t--) {
            printf("%c", c);
        }
        printf("\n");
    }

    //中间层,(1),空格+*
    int y = count;
    while(y--){
        printf(" ");
    }
    printf("%c\n", c);

    //下半部分
    //循环层数，从上到下依次递增，1~n
    for(int i = 1; i <= count; i++) {
        //每层有多少个*
        int j = i * 2 + 1;
        //每层空格输出
        int l = count - i;
        if(i != count){
            while(l--){
                printf(" ");
            }
        }
        //每层*输出
        for(int t = j; t>0; t--) {
            printf("%c", c);
        }
        printf("\n");
    }
    printf("%d", ans);
    return 0;
}
```

```
56 @
@@@@@@@@@
 @@@@@@@
  @@@@@
   @@@
    @
   @@@
  @@@@@
 @@@@@@@
@@@@@@@@@
6
```

2、ans还是有错，56的话 56-(3+5+7+9)*2-1=7,

```c++
ans = x * 2 + 1;
```

这样搞会错一半，不加一对另一半，要想全对得换成下面这种
$$
1+3+5+7+...+(2n-1)=2^n
$$


```c++
ans = N - ((pow(count+1,2)-1)*2+1);
```



---

## 3.4 日期处理



---

## 3.5 进制转换

### **1022 D进制的A+B**

输入两个非负 10 进制整数 *A* 和 *B* (≤230−1)，输出 *A*+*B* 的 *D* (1<*D*≤10)进制数。

输入格式：

输入在一行中依次给出 3 个整数 *A*、*B* 和 *D*。

输出格式：

输出 *A*+*B* 的 *D* 进制数。

输入样例：

```in
123 456 8
```

输出样例：

```out
1103
```

#### test1

```

```

#### 错误总结：

### **1037 在霍格沃茨找零钱**

如果你是哈利·波特迷，你会知道魔法世界有它自己的货币系统 —— 就如海格告诉哈利的：“十七个银西可(Sickle)兑一个加隆(Galleon)，二十九个纳特(Knut)兑一个西可，很容易。”现在，给定哈利应付的价钱 *P* 和他实付的钱 *A*，你的任务是写一个程序来计算他应该被找的零钱。

输入格式：

输入在 1 行中分别给出 *P* 和 *A*，格式为 `Galleon.Sickle.Knut`，其间用 1 个空格分隔。这里 `Galleon` 是 [0, 107] 区间内的整数，`Sickle` 是 [0, 17) 区间内的整数，`Knut` 是 [0, 29) 区间内的整数。

输出格式：

在一行中用与输入同样的格式输出哈利应该被找的零钱。如果他没带够钱，那么输出的应该是负数。

输入样例 1：

```in
10.16.27 14.1.28
```

输出样例 1：

```out
3.2.1
```

输入样例 2：

```in
14.1.28 10.16.27
```

输出样例 2：

```out
-3.2.1
```

#### test1

```c++
##include <cstdio>

//1G=17S=493k，1S=29K
//4957 6931
//5229 6947 1718
//先都换算成k，再经过加减判断正负，再换算成G.S.K的形式
int main(){
    int bG, bS, bK, hG, hS, hK;
    int b = 0, h = 0;//应交的钱和哈利的钱，都是K为单位
    int check = 0;//找零
    scanf("%d.%d.%d %d.%d.%d",&bG, &bS, &bK, &hG, &hS, &hK);
    b = bG*493+bS*17+bK;
    h = hG*493+bS*17+hK;
    check = h-b;
    int G, S, K;
//     printf("%d", check);
    G = check / 493;
    printf("%d.", G);
    S = (check- G * 493) / 29;
    printf("%d.", S);
    K = check - G * 493 - S * 29;
    printf("%d", K);
}
```

#### 错误总结：

1、有对有错，测试输出为，应该是有条件没考虑

```
4.0.1
```

2、对比答案中间思路没问题，主要是引入

```c++
const int Galleon = 17 * 29;
const int Sickle = 29;
...
printf("%d.%d.%d\n", change / Galleon, change%Galleon / Sickle, change%Sickle);
```



#### test2

```c++
##include <cstdio>

const int Galleon = 17 * 29;
const int Sickle = 29;

int main() {
    int a1, b1, c1;
    int a2, b2, c2;
    scanf("%d.%d.%d %d.%d.%d", &a1, &b1, &c1, &a2, &b2, &c2);
    int price = a1 * Galleon + b1 * Sickle + c1;
    int money = a2 * Galleon + b2 * Sickle + c2;
    int change = money - price;
    if (change<0)
    {
        printf("-");
        change = -change;
    }
    printf("%d.%d.%d\n", change / Galleon, change%Galleon / Sickle, change%Sickle);
    return 0;
}
```

### 总结：

1、要活用const，为了让自己思路清晰点

2、进制就是从高位往低位，进几位/几位，再用%除掉多余部分，如此往复

## 3.6 字符串处理

### B1006换个格式输出整数



让我们用字母 `B` 来表示“百”、字母 `S` 表示“十”，用 `12...n` 来表示不为零的个位数字 `n`（<10），换个格式来输出任一个不超过 3 位的正整数。例如 `234` 应该被输出为 `BBSSS1234`，因为它有 2 个“百”、3 个“十”、以及个位的 4。

输入格式：

每个测试输入包含 1 个测试用例，给出正整数 *n*（<1000）。

输出格式：

每个测试用例的输出占一行，用规定的格式输出 *n*。

输入样例 1：

```in
234
```

输出样例 1：

```out
BBSSS1234
```

输入样例 2：

```in
23
```

输出样例 2：

```out
SS123
```

#### test1

```c++
//只有百位十位，个位数列，先输入数字分位
##include <cstdio>
int main(){
    int N;
    scanf("%d", &N);
    int b = 0,s = 0,g = 0;
    b = N / 100; 
    s = N % 100 / 10;
    g = N % 10;
    for(int i = 0; i < b; i++) {
        printf("B");
    }
    for(int i = 0; i < s; i++) {
        printf("S");
    }
    for(int i = 0; i < g; i++) {
        printf("%d",i + 1);
    }
    return 0;
}
```

#### 错误总结：

### B1021 **个位数统计**

给定一个 *k* 位整数 *N*=*d**k*−110*k*−1+⋯+*d*1101+*d*0 (0≤*d**i*≤9, *i*=0,⋯,*k*−1, *d**k*−1>0)，请编写程序统计每种不同的个位数字出现的次数。例如：给定 *N*=100311，则有 2 个 0，3 个 1，和 1 个 3。

输入格式：

每个输入包含 1 个测试用例，即一个不超过 1000 位的正整数 *N*。

输出格式：

对 *N* 中每一种不同的个位数字，以 `D:M` 的格式在一行中输出该位数字 `D` 及其在 *N* 中出现的次数 `M`。要求按 `D` 的升序输出。

输入样例：

```in
100311
```

输出样例：

```out
0:2
1:3
3:1
```

#### test1

```c++
##include <iostream>
using namespace std;
int main() {
    int n[10] = { 0 };
    int num;
    cin >> num;
    int M,N=1, count = 1, temp = 0;
    do {
        M = count;
        while (M--) {
            N *= 10;
        }
        temp = num % N;
        n[temp]++;
        count++;
    } while (num /= 10);
    for (int i = 0; i < count; i++) {
        if (n[i]) cout << i  << ":" << n[i] << endl;
    }
    return 0;
}
```

#### 错误总结：

#### test2:答案

```c++
##include <cstdio>
##include <cstring>
##include <iostream>
using namespace std;

int main() {
	char str[1010];
	cin.getline(str,1010);
	int len = strlen(str);
	int count[10] = { 0 };
	for (int i = 0; i < len; i++)
	{
		count[str[i] - '0']++;//字符串中数字转int格式必备
	}
	for (int i = 0; i < 10; i++)
	{
		if (count[i] != 0) {
			printf("%d:%d\n", i, count[i]);
		}
	}
	return 0;
}
```

#### 总结：

1、用cin.getline(数组名,读多长);比较好

### B1031 **查验身份证**

一个合法的身份证号码由17位地区、日期编号和顺序编号加1位校验码组成。校验码的计算规则如下：

首先对前17位数字加权求和，权重分配为：{7，9，10，5，8，4，2，1，6，3，7，9，10，5，8，4，2}；然后将计算的和对11取模得到值`Z`；最后按照以下关系对应`Z`值与校验码`M`的值：

```
Z：0 1 2 3 4 5 6 7 8 9 10
M：1 0 X 9 8 7 6 5 4 3 2
```

现在给定一些身份证号码，请你验证校验码的有效性，并输出有问题的号码。

输入格式：

输入第一行给出正整数*N*（≤100）是输入的身份证号码的个数。随后*N*行，每行给出1个18位身份证号码。

输出格式：

按照输入的顺序每行输出1个有问题的身份证号码。这里并不检验前17位是否合理，只检查前17位是否全为数字且最后1位校验码计算准确。如果所有号码都正常，则输出`All passed`。

输入样例1：

```in
4
320124198808240056
12010X198901011234
110108196711301866
37070419881216001X
```

输出样例1：

```out
12010X198901011234
110108196711301866
37070419881216001X
```

输入样例2：

```in
2
320124198808240056
110108196711301862
```

输出样例2：

```out
All passed
```

#### test1

```c++
//首先将id前17位按权重求和，Z对应M，M为id第18位,问题是18位有数字有字符
##include <cstdio>
##include <cstring>
##include <cmath>
##include <iostream>
using namespace std;
//a是id第18位，b是前17位加权求和得到的z--后对应的
bool isT(char a, int b){
    char m[11] = {'1','X','9','8','7','6','5','4','3','2'};
    if(m[b] == a) return true;
    else return false;
}

int main(){
    char id[18];
    int sum[100] = {0}, p[17] = {7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2};
    int n, z[100], count = 0;
    scanf("%d", &n);
    for(int j = 0; j<n; j++) {
        //循环n次，取每行id号，顺便求加权和
        for(int i = 0; i < 18; i++){
            scanf("%s", id);
            if(i != 17) sum[j] += (id[i] - '0')*p[i];
        }
        z[j] = sum[j] % 11;//对11取模都得到z
        if(isT(id[18], z[j])) count++;//计数，对的，全对再输出“All passed“
        else {
            for(int x = 0; x < 18; x++){
                printf("%s", id);
            }
        }
    }
    if(count == n) printf("All passed");
}
```

#### 错误总结：

段错误

```
a.cpp: In function ‘int main()’:
a.cpp:29:35: warning: format ‘%s’ expects argument of type ‘char*’, but argument 2 has type ‘int’ [-Wformat=]
                 printf("%s", id[x]);
                              ~~~~~^
a.cpp:18:10: warning: ignoring return value of ‘int scanf(const char*, ...)’, declared with attribute warn_unused_result [-Wunused-result]
     scanf("%d", &n);
     ~~~~~^~~~~~~~~~
a.cpp:22:18: warning: ignoring return value of ‘int scanf(const char*, ...)’, declared with attribute warn_unused_result [-Wunused-result]
             scanf("%s", id);
             ~~~~~^~~~~~~~~~
a.cpp:26:21: warning: array subscript is above array bounds [-Warray-bounds]
         if(isT(id[18], z[j])) count++;//计数，对的，全对再输出“All passed“
                ~~~~~^
```

#### test2 

```c++
//首先将id前17位按权重求和，Z对应M，M为id第18位,问题是18位有数字有字符
##include <cstdio>
##include <cstring>
##include <cmath>
##include <iostream>
using namespace std;
//a是id第18位，b是前17位加权求和得到的z--后对应的
bool isT(char a, int b){
    char m[11] = {'1','X','9','8','7','6','5','4','3','2'};
    if(m[b] == a) return true;
    else return false;
}

int main(){
    char id[18];
    //权重p，权重和sum
    int sum[100] = {0}, p[17] = {7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2};
    int n, z[100], count = 0;
    scanf("%d", &n);
    for(int j = 0; j < n; j++) {
        //取每行id号
        scanf("%s", id);
        //循环n次,求该id 前17位加权和
        for(int i = 0; i < 18; i++){
            if(i != 17) sum[j] += (id[i] - '0') * p[i];
        }
        //对11取模都得到z
        z[j] = sum[j] % 11;
        //计数，对的，全对再输出“All passed“
        if(isT(id[18], z[j])) count++;
        else printf("%s\n", id);
    }
    if(count == n) printf("All passed");
}
```

#### 错误总结：

1、部分正确，其余错误中有一条对的一条错的，应该是判断条件出了问题

#### testN：答案

```c++
##include <cstdio>
##include <cstring>

char change[11] = { '1', '0', 'X', '9', '8', '7', '6', '5', '4', '3' , '2' };
int w[17] = { 7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2 };

int main() {
	int n;
	scanf("%d", &n);
	bool flag = true;
	char str[20];
	for (int i = 0; i < n; i++)
	{
		scanf("%s", str);
		int j, last = 0;
		for (j = 0; j < 17; j++) // for (int j = 0; j < 17; j++) // 一旦加上int后 循环外不可读取循环中的j变量
		{
			if (!(str[j] >= '0' && str[j] <= '9')) break;
			last = last + (str[j] - '0')*w[j];
		}
		if (j<17)
		{
			flag = false;
			printf("%s\n", str);
		}
		else {
			if (change[last%11]!=str[17])
			{
				flag = false;
				printf("%s\n", str);
			}
		}
	}
	if (flag==true)
	{
		printf("All passed\n");
	}
	return 0;
}
```



### B1002 **写出这个数**

读入一个正整数 *n*，计算其各位数字之和，用汉语拼音写出和的每一位数字。

输入格式：

每个测试输入包含 1 个测试用例，即给出自然数 *n* 的值。这里保证 *n* 小于 10100。

输出格式：

在一行内输出 *n* 的各位数字之和的每一位，拼音数字间有 1 空格，但一行中最后一个拼音数字后没有空格。

输入样例：

```in
1234567890987654321123456789
```

输出样例：

```out
yi san wu
```

#### test1

```c++
##include <cstdio>
##include <cstring>
##include <iostream>
using namespace std;

int main(){
    char str[110];
//     fgets(str, 110, stdin);
    cin.getline(str,110);
    int len = strlen(str);
    int sum = 0;
    
    for(int n = 0; n < len; n++){//不能直接n<strlen(str)放在条件里，要建立一变量len
        sum += str[n] - '0';
    }
    //把sum转化成和数字同高低位的数组135->{5,3,1}
    //一开始的循环while条件是num！=0，后面都一样，造成循环不成立
    int num = 0, ans[10];
    while(sum){
        ans[num] = sum %10;
        sum /= 10;
        num++;
    }
    //二维数组存放拼音横向有0~9个单元，纵向5个空间存放单词里的每个字符
    char change[10][5] = {"ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu"};
    // \1\num-1开始到i=0执行完结束\2\位数从最高位开始，即数组最高位对照原数字最高位
    for(int i = num -1; i >= 0; i--){
        printf("%s", change[ans[i]]);
        if(i!=0) printf(" ");//没到最后一次循环都输出空格
        else printf("\n");
    }
    return 0;
}
```

#### 错误总结：

见注释

### B1009 **说反话**

给定一句英语，要求你编写程序，将句中所有单词的顺序颠倒输出。

输入格式：

测试输入包含一个测试用例，在一行内给出总长度不超过 80 的字符串。字符串由若干单词和若干空格组成，其中单词是由英文字母（大小写有区分）组成的字符串，单词之间用 1 个空格分开，输入保证句子末尾没有多余的空格。

输出格式：

每个测试用例的输出占一行，输出倒序后的句子。

输入样例：

```in
Hello World Here I Come
```

输出样例：

```out
Come I Here World Hello
```

#### test1

```c++
//首先是需要一个数组的，输入按空格结束，存放每个单词，然后再倒叙输出
//重点：1、读入句子 2、倒序
##include <cstdio>
int main() {
    char str[80][10];
    //读入句子
//     cin.getline(str,80);
    int i = 0;
    while(scanf("%s", str[i]) != EOF) {
        i++;
    }
    //倒叙
    char temp[10];
    int x = i;
    //为不影响循环中的i，=x；简单两两交换
    for(int j = 0; j < i; j++) {
        temp = str[j];
        str[j] = str[x];
        str[x] = temp;
        x--;
    }
    //输出倒叙str
    for(int j = 0; j < i; j++){
        printf("%c", str[j]);
        if(j != (i-1)) printf(" ");
    }
    return 0;
}
```

#### 错误总结：

1、数组互相赋值问题

#### test2

```c++
#include <cstdio>
int main()  {
    int num = 0;
    char ans[90][90];
    while(scanf("%s", ans[num]) != EOF) {
        num++;
    }
    for(int i = num-1; i >= 0; i--) {
        printf("%s", ans[i]);
        if(i>0) printf(" ");
    }
    return 0;
}
```

#### 总结：

读入的同时就可以计数用来做逆序输出时的循环次数

### B1014 **福尔摩斯的约会**(二维字符串和字符判断)

大侦探福尔摩斯接到一张奇怪的字条：

```
我们约会吧！ 
3485djDkxh4hhGE 
2984akDfkkkkggEdsb 
s&hgsfdk 
d&Hyscvnm
```

大侦探很快就明白了，字条上奇怪的乱码实际上就是约会的时间`星期四 14:04`，因为前面两字符串中第 1 对相同的大写英文字母（大小写有区分）是第 4 个字母 `D`，代表星期四；第 2 对相同的字符是 `E` ，那是第 5 个英文字母，代表一天里的第 14 个钟头（于是一天的 0 点到 23 点由数字 0 到 9、以及大写字母 `A` 到 `N` 表示）；后面两字符串第 1 对相同的英文字母 `s` 出现在第 4 个位置（从 0 开始计数）上，代表第 4 分钟。现给定两对字符串，请帮助福尔摩斯解码得到约会的时间。

输入格式：

输入在 4 行中分别给出 4 个非空、不包含空格、且长度不超过 60 的字符串。

输出格式：

在一行中输出约会的时间，格式为 `DAY HH:MM`，其中 `DAY` 是某星期的 3 字符缩写，即 `MON` 表示星期一，`TUE` 表示星期二，`WED` 表示星期三，`THU` 表示星期四，`FRI` 表示星期五，`SAT` 表示星期六，`SUN` 表示星期日。题目输入保证每个测试存在唯一解。

输入样例：

```in
3485djDkxh4hhGE 
2984akDfkkkkggEdsb 
s&hgsfdk 
d&Hyscvnm
```

输出样例：

```out
THU 14:04
```

#### test1

```c++
//前两个字符串是看相同的大写英文字母，第一个是星期，直接按英文字母顺序排，第二个是小时，按16进制数字加字母
//后两个字符串是看相同字母出现位置从0开始计数，第N分钟
##include <cstdio>
##include <cstring>
##include <iostream>
using namespace std;

int main() {
    char str1[60], str2[60], str3[60], str4[60];
    cin.getline(str1, 60);
    cin.getline(str2, 60);
    //在前两行中找,flag记录状态：是否小时是字母
    int num1, num2, counta = 0, flag = 0;
    for(int i = 0; i < 60; i++) {
            for(int j = 0; j < 60; j++) {
                //先查是否为大写字母
                if((str1[i] >= 'A')&&(str1[i] <= 'Z')){
                    if(str1[i] == str2[j]) {
                        //先分第几次发现大写字母，再检查第几个字母
                        if(!counta){
                            num1 = str1[i] - '@';
                            counta++;//判断是几个
                        } else {
                            //如果不是第一次出现字母
                            num2 = str1[i] - '@';
                            flag = 1;
                        }
                    }
                } else {
                    //如果不是大写字母而是其他字符
                    if((str1[i] >= '0')&&(str1[i] <= '9')&&(str1[i] == str2[j])) {
                        num2 = str1[i] - '0';
                    }
                }
            }
    }
    //在后两行中找分钟
    cin.getline(str3, 60);
    cin.getline(str4, 60);
    int num3 = 0, countb = 0;
    for(int i = 0; i < 60; i++) {
        if((str3[i] >= 'A')&&(str3[i] <='z')) {
            if((str3[i] <= 'Z')|(str3[i] >= 'a')) {
                for(int j = 0; j < 60; j++) {
                    countb++;
                    if(str4[j] == str3[i]) {
                        num3 = countb;
                        countb = 0;
                    }
                }
            }
        }
    }
    //对应关系，周，小时如果是字母+9，分钟（直接就是）
    char day[10][5] = {"MON", "TUE", "WED", "THU", "FRI", "SAT", "SUN"};
    if(flag) num2 += 9;
    printf("%s %d:%0md", day[num1-1], num2, num3);
    return 0;
}
```

#### 错误总结：

```
THU 81:Successd
```

1、小时有点离谱，一看就是数多了

```c++
 //如果不是大写字母而是其他字符----改了一下判断条件，加入了0<str[i]<9的设定
                    if((str1[i] >= '0')&&(str1[i] <= '9')&&(str1[i] == str2[j])) {
                        num2 = str1[i] - '0';
                    }
```

```
THU 14:Successd
```

2、int型怎么能输出Successd的？？？

```c++
printf("%s %d:%0md", day[num1-1], num2, num3);
```

此处的%md，m应该是整体长度，但是改完了，每次输出都不一样，奇诡啊。



#### testn：答案

```c++
##include <cstdio>
##include <cstring>
 int main(){
     char week[7][5] = {"MON", "TUE", "WEN", "THU", "FRI", "SAT", "SUN"};//星期四拼错了
     char str[4][70];
     int len[4];
     scanf("%s %s %s %s", str[0], str[1], str[2], str[3]);
     len[0] = strlen(str[0]);
     len[1] = strlen(str[1]);
     len[2] = strlen(str[2]);
     len[3] = strlen(str[3]);
     int i;
     for(i = 0; i < len[0] && i < len[1]; i++) {
         //不用在for中int i，后面还要用，应该把i设置成全局变量
         if(str[0][i] == str[1][i] && str[0][i] >= 'A' && str[0][i] <= 'G') {
             printf("%s", week[str[0][i] -'A']);
             //数组下标要保持整数形式
             break;
         }
     }
     for(i++; i < len[0] && i < len[1]; i++) {
         //不能再让int i = 0，要接着上面的，初始条件为i++
         if(str[0][i] == str[1][i]) {
             if(str[0][i] >= '0' && str[0][i] <= '9') {
                 printf(" %02d:", str[0][i] - '0');
                 break;
             } 
             else if(str[0][i]>='A' && str[1][i]<='N') {//这里切记不是else，还有别的可能呢，不然会错一半
                 printf(" %02d:", str[0][i] - 'A'+ 10);
                 //从A开始算是相当于10
                 break;
             }
         }
     }
     for(i = 0; i < len[2] && i < len[3]; i++) {
         //不用重新命名int i，但是得重置
         if(str[2][i] == str[3][i]) {
             if((str[2][i] <= 'Z' && str[2][i] >='A') || (str[2][i] <= 'z' && str[2][i] >='a')){
                 printf("%02d", i);
                 break;
             }
         }
     }
     return 0;
 }
```

```c++
##include <cstdio>
##include <cstring>
int main() {
	char week[7][5] = { "MON", "TUE", "WED", "THU", "FRI", "SAT", "SUN" };
	char str[4][70];
	int len[4];
	scanf("%s %s %s %s", &str[0], &str[1], &str[2], &str[3]);
	len[0] = strlen(str[0]);
	len[1] = strlen(str[1]);
	len[2] = strlen(str[2]);
	len[3] = strlen(str[3]);
	int i;
	for (i = 0; i < len[0] && i<len[1]; i++)
	{
		if (str[0][i]==str[1][i] && str[0][i]>='A' && str[0][i]<='G')
		{
			printf("%s ", week[str[0][i] - 'A']);
			break;
		}
	}
	for (i++; i < len[0] && i < len[1]; i++) {
		if (str[0][i]==str[1][i])
		{
			if (str[0][i] >= '0' && str[1][i] <= '9') {
				printf("%02d:", str[0][i] - '0');
				break;
			}
			else if (str[0][i]>='A' && str[1][i]<='N')
			{
				printf("%02d:", str[0][i] - 'A' + 10);
				break;
			}
		}
	}
	for (i = 0; i < len[2] && i < len[3]; i++) {
		if (str[2][i] == str[3][i]) {
			if ((str[2][i] >= 'A' && str[2][i] <= 'Z') || (str[2][i] >= 'a' && str[2][i] <= 'z')) {
				printf("%02d", i);
				break;
			}
		}
	}
	return 0;
}
```



#### 总结：

1、用二维数组存这几个字符串，没想到

2、用len存放数组长度，减少无效循环次数

3、没观察到最初俩字符串中相等的大写字母位置也在同样地方，可以少些麻烦判断，减少循环次数

4、想到了但是没用break；

5、没想到最后的秒数根本不用count，直接用循环计数用的i就行，节省资源！

### B1024  **科学计数法**（估计没掌握，再做一遍）

科学计数法是科学家用来表示很大或很小的数字的一种方便的方法，其满足正则表达式 [+-][1-9]`.`[0-9]+E[+-][0-9]+，即数字的整数部分只有 1 位，小数部分至少有 1 位，该数字及其指数部分的正负号即使对正数也必定明确给出。

现以科学计数法的格式给出实数 *A*，请编写程序按普通数字表示法输出 *A*，并保证所有有效位都被保留。

输入格式：

每个输入包含 1 个测试用例，即一个以科学计数法表示的实数 *A*。该数字的存储长度不超过 9999 字节，且其指数的绝对值不超过 9999。

输出格式：

对每个测试用例，在一行中按普通数字表示法输出 *A*，并保证所有有效位都被保留，包括末尾的 0。

输入样例 1：

```in
+1.23400E-03
```

输出样例 1：

```out
0.00123400
```

输入样例 2：

```in
-1.2E+10
```

输出样例 2：

```out
-12000000000
```

#### test1

```c++
#include <cstdio>
#include <cmath>
int main(){
    char flagA, flagB;
    double x;
    int dua, A;
    scanf("%c%f%c%d", &flagA, &x, &flagB, &dua);
    if(flagB == '+') {
        A = x * pow(10,dua);
    } else {
        A = x * pow(10,-dua);
    }
    printf("%c%d", flag, A);
}
```

#### 错误总结：

#### test2：答案

```c++
#include <cstdio>
#include <cstring>
#include <iostream>
using namespace std;

int main() {
    char str[10010];
    cin.getline(str, 10010);
    //!整个读入字符串
    int len = strlen(str);
    if (str[0] == '-') printf("-");
    //对正负号的处理思路一样

    int pos = 0;
    while (str[pos] != 'E') {
        pos++;
    }
    //pos为E之前有几位，也同样是E的位号
    int exp = 0;
    for (int i = pos + 2; i < len; i++)
    {
        exp = exp * 10 + (str[i] - '0');
    }
    //从E之后跳过符号位，数字顺序为个,十,百。。。到结束，以此类推获得exp
    //这里会出现负号吧？不用处理？
    if (exp == 0) {
        for (int i = 1; i < pos; i++)
        {
            printf("%c", str[i]);
        }
    }
    //特殊情况，E后面数字为0，直接出数

    if (str[pos + 1] == '-') {
        printf("0.");
        for (int i = 0; i < exp - 1; i++) {
            printf("0");
        }
        printf("%c", str[1]);//小数点前面的那个数
        for (int i = 3; i < pos; i++)
        {
            printf("%c", str[i]);//小数点后边的数
        }
    }//如果E后边有个负号，直接0.00..（0.后边有几个零取决于E后边数的大小）
    else {
        for (int i = 1; i < pos; i++)
        {
            if (str[i] == '.') continue;
            printf("%c", str[i]);
            if (i == exp + 2 && pos - 3 != exp)
            {
                printf(".");
//重点：小数点位置：i用作位置计数，
//因为从“1”开始且应到exp位置后的下一位输出小数点所以要+2
//且pos-3为去小数点及之前数（-2）后小数点需要移动次数，
//不可等于xep（小数点可移动次数），因为这样的话就不用输出小数点
            }
        }
        for (int i = 0; i < exp - (pos - 3); i++)
        {
            printf("0");
        }
    }//如果非负号，就直接循环出数字主体，if检查其小数点并用continue跳过
    return 0;
}
```



### B1048 **数字加密**(找机会再做)

本题要求实现一种数字加密方法。首先固定一个加密用正整数 A，对任一正整数 B，将其每 1 位数字与 A 的对应位置上的数字进行以下运算：对奇数位，对应位的数字相加后对 13 取余——这里用 J 代表 10、Q 代表 11、K 代表 12；对偶数位，用 B 的数字减去 A 的数字，若结果为负数，则再加 10。这里令个位为第 1 位。

输入格式：

输入在一行中依次给出 A 和 B，均为不超过 100 位的正整数，其间以空格分隔。

输出格式：

在一行中输出加密后的结果。

输入样例：

```in
1234567 368782971
```

输出样例：

```out
3695Q8118
```

#### test1

```c++
#include <cstdio>
#include <cstring>
#include <iostream>
int main() {
    char str[3][110];
    char sw[5] = {'J', 'Q', 'K'};
    scanf("%s %s", str[0], str[1]);
    int lenA = strlen(str[0]);
    int lenB = strlen(str[1]);
    for(int i = 0;i<lenB;i++)
    {
        if((i+1)%2)//jishuwei
        {
            str[2][i] = ((str[0][i] - '0') + (str[1][i] - '0'))%13;
            if(str[2][i] >= 10)
            {
                str[2][i] = sw[(str[2][i] - '0') - 10];
            }
        } 
        else//oushuwei 
        {
            str[2][i] = str[1][i] - str[0][i];
            if(str[2][i] < '0'){
                str[2][i] = (str[2][i] - '0') + 10;
            }
        }
        printf("%c", str[2][i]);
    }
}

```

#### 错误总结：

1、？？应该是打印前的处理环节有问题，或者不命初值就容易

```
�
```



#### testn：答案

```c++
#include <cstdio>
##include <cstring>
const int max = 110;
//因为从个位开始记为1，翻转过来好跟index+1相同
void reverse(char str[]) {
    int len = strlen(str);
    int temp;
    for(int i = 0; i < len/2; i++){
        temp = str[i];
        str[i] = str[len - i -1];
        str[len - i -1] = temp;
    }
}
int main() {
    char A[max], B[max], ans[max] = {0};
    scanf("%s %s", A, B);
    reverse(A);
    reverse(B);
    int lenA = strlen(A);
    int lenB = strlen(B);
//     if(lenA < lenB) {
//         swich(A,B)
//     }
    int len = lenA > lenB ? lenA : lenB;
    for(int i = 0; i < len; i++){
//         int numA = A[i] - '0';
//         int numB = B[i] - '0';
        int numA = i < lenA ? A[i] - '0' : 0;
        int numB = i < lenB ? B[i] - '0' : 0;
        //此三处的三元式需要注意
        if(i%2 == 0) {
            int temp = (numA+numB)%13;
            if (temp == 10) ans[i] = 'J';
            else if (temp == 11) ans[i] = 'Q';
            else if (temp == 12) ans[i] = 'K';
            else ans[i] = temp + '0';//从整型变回字符只需要+‘0’
        } else {
            int temp = numB - numA;
            if(temp < 0) temp += 10;
            ans[i] = temp + '0';
        }
    }
    reverse(ans);
    puts(ans);
    return 0;
}
```

#### 总结：

1、要区分什么时候用二维数组，本题中应该是把char转为int后及逆行计算再转回去更方便，

如果一边计算一边转格式容易出错

2、如果不设置初值，不管不对称的地方，把他们设为0，答案很容易出现乱码

3、三元式的应用，有的时候可以节省一些步骤空间，让代码更整洁易懂

# 第四章，算法初步

## 4.1 排序（难的可跳过）

#### 排序重点：本节介绍基础版

1、冒泡排序

2、选择排序

（1）进行（1 ~ =n）次循环

（2）

3、插入排序





### B1015 德才论

宋代史学家司马光在《资治通鉴》中有一段著名的“德才论”：“是故才德全尽谓之圣人，才德兼亡谓之愚人，德胜才谓之君子，才胜德谓之小人。凡取人之术，苟不得圣人，君子而与之，与其得小人，不若得愚人。”

现给出一批考生的德才分数，请根据司马光的理论给出录取排名。 

输入格式：

输入第一行给出 3 个正整数，分别为：*N*（≤105），即考生总数；*L*（≥60），为录取最低分数线，即德分和才分均不低于 *L* 的考生才有资格被考虑录取；*H*（<100），为优先录取线——德分和才分均不低于此线的被定义为“才德全尽”，此类考生按德才总分从高到低排序；才分不到但德分到优先录取线的一类考生属于“德胜才”，也按总分排序，但排在第一类考生之后；德才分均低于 *H*，但是德分不低于才分的考生属于“才德兼亡”但尚有“德胜才”者，按总分排序，但排在第二类考生之后；其他达到最低线 *L* 的考生也按总分排序，但排在第三类考生之后。

随后 *N* 行，每行给出一位考生的信息，包括：`准考证号 德分 才分`，其中`准考证号`为 8 位整数，德才分为区间 [0, 100] 内的整数。数字间以空格分隔。

输出格式：

输出第一行首先给出达到最低分数线的考生人数 *M*，随后 *M* 行，每行按照输入格式输出一位考生的信息，考生按输入中说明的规则从高到低排序。当某类考生中有多人总分相同时，按其德分降序排列；若德分也并列，则按准考证号的升序输出。

输入样例：

```in
14 60 80
10000001 64 90
10000002 90 60
10000011 85 80
10000003 85 80
10000004 80 85
10000005 82 77
10000006 83 76
10000007 90 78
10000008 75 79
10000009 59 90
10000010 88 45
10000012 80 100
10000013 90 99
10000014 66 60
```

输出样例：

```out
12
10000013 90 99
10000012 80 100
10000003 85 80
10000011 85 80
10000004 80 85
10000007 90 78
10000006 83 76
10000005 82 77
10000002 90 60
10000014 66 60
10000008 75 79
10000001 64 90
```

#### test1

```c++

```

#### 错误总结

1、

#### test2 答案

```c++
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;

/此题目中的algorithm中的sort带三个参数的用法需要了解

struct Student {
	char id[10];
	int de, cai, sum;
	int flag;
}stu[100010];

bool cmp(Student a, Student b) {
	if (a.flag != b.flag) return a.flag < b.flag;
	else if (a.sum != b.sum) return a.sum > b.sum;
	else if (a.de != b.de) return a.de > b.de;
	else return strcmp(a.id,b.id)<0;
}
int main() {
	int n, L, H;
	scanf("%d%d%d", &n, &L, &H);
	int m = n;
	for (int i = 0; i < n; i++)
	{
		scanf("%s%d%d", stu[i].id, &stu[i].de, &stu[i].cai);
		stu[i].sum = stu[i].de + stu[i].cai;
		if (stu[i].de<L || stu[i].cai<L)
		{
			stu[i].flag = 5;
			m--;
		}
		else if (stu[i].de >= H && stu[i].cai >= H) stu[i].flag = 1;
		else if (stu[i].de >= H && stu[i].cai < H) stu[i].flag = 2;
		else if (stu[i].de >= stu[i].cai) stu[i].flag = 3;
		else stu[i].flag = 4;
	}
	sort(stu, stu + n, cmp);
	printf("%d\n", m);
	for (int i = 0; i < m; i++)
	{
		printf("%s %d %d\n", stu[i].id, stu[i].de, stu[i].cai);
	}
	return 0;
}
```

#### 错误总结

1、

### A1012

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结

### A1016

#### test1

```c++

```

#### 错误总结

1、

#### test2 

```c++

```

#### 错误总结

### A1025

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结

### A1028

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结

### A1055

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结

### A1075

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结

### A1083

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结

### A1080

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结

### A1095

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结

## 4.2 散列

### B1029

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结

### B1033

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1038

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1039

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1042

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1043

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1047

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1041

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1050

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1005

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1048

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结





## 4.3 递归

## 4.4 贪心

### B1023

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1020-A70

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1033

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1037

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1067

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1038

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结





## 4.5 二分 (9.15做完)



### B1030-A85

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1010

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1044

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1048

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结





## D27~D29 _ 4.6two pointers

### B1030

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1035

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1029

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1048

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



## D29~D30 _ 4.7其他

### B1040

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1045

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



# 第五章，简单数学

## D30~D32  _ 5.1简单数学

### B1003

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1019-A69

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1049-A104

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1008

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1049

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



## D32~D32  _5.2最大公约数与最小公倍数

### B1008

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



## D32~D33  _5.3分数的四则运算

### B1081

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1034-A88

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



## D33~D35  _5.4素数

### B1007

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### B1013

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1015

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1078

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



## D35~D36  _5.5质因子分解

### A1096

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1059

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



## D36~D37  _5.6大整数运算

### A1017

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1023

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



### A1024

#### test1

```c++

```

#### 错误总结

1、

#### test2

```c++

```

#### 错误总结



## D37(9.25)~D3x  _5.7扩展欧几里得算法

X

## D3~D3  _5.8组合数

X

# 第六章 STL库常见用法总结

## 6.1 vector



## 6.2 set



## 6.3 string



## 6.4 map



## 6.5 queue



## 6.6 priority_queue



## 6.7 stack



## 6.8 pair



## 6.9 algorithm

# 第七章 数据结构！！

## 7.1 栈

## 7.2 队列

## 7.3 链表

## 7.4 深度优先(DFS)搜索专题

## 7.5 广度优先(BFS)搜索专题

## 7.6 树与二叉树

## 7.7 二叉树的遍历

## 7.8树的遍历

## 7.9 二叉查找树(BST)

## 7.10 平衡二叉树(AVL树)

## 7.11 并查集

## 7.12 堆

## 7.13 赫夫曼树



# 第八章 图算法！！！

# 第九章 动态规划！！！

# 第十章 字符串专题

# 第十一章 其他扩展专题