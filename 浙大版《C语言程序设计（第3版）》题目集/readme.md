## 习题5-6 使用函数输出水仙花数 (20 分)

水仙花数是指一个*N*位正整数（*N*≥3），它的每个位上的数字的*N*次幂之和等于它本身。例如：$153=1^{3}+5^{3}+3^{3}$。 本题要求编写两个函数，一个判断给定整数是否水仙花数，另一个按从小到大的顺序打印出给定区间(*m*,*n*)内所有的水仙花数。

### 函数接口定义：

```c++
int narcissistic( int number );
void PrintN( int m, int n );
```

函数`narcissistic`判断`number`是否为水仙花数，是则返回1，否则返回0。

函数`PrintN`则打印开区间(`m`, `n`)内所有的水仙花数，每个数字占一行。题目保证100≤`m`≤`n`≤10000。

### 裁判测试程序样例：

```c++
#include <stdio.h>

int narcissistic( int number );
void PrintN( int m, int n );

int main()
{
    int m, n;

    scanf("%d %d", &m, &n);
    if ( narcissistic(m) ) printf("%d is a narcissistic number\n", m);
    PrintN(m, n);
    if ( narcissistic(n) ) printf("%d is a narcissistic number\n", n);

    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例：

```in
153 400
```

### 输出样例：

```out
153 is a narcissistic number
370
371
```



### 题解

```C
int narcissistic( int number )
{
    int temp=number;
    int num=0;
    while(temp!=0)
    {
        num++;//计算位数
        temp/=10;
    }
    int n=number;
    
    int sum=0;
    while(n!=0)
    {
        int mul=1;
        int i=n%10;
        int temp2=num;
        while(temp2--)
        {
            mul*=i;//N次方
        }
        sum+=mul;
        n/=10;
    }
    
    if(sum==number)
        return 1;
    return 0;

}
void PrintN( int m, int n )
{

    for(int i=m+1;i<n;i++)
    {
        if(narcissistic(i))
        {
            printf("%d\n",i);
        }
    }
    
}
```



### 总结

1.一开始的时候看错题目了以为水仙花数是每个数字的三次方之和，wa了好几遍之后才发现是N次方，有些粗心。

2.mul变量应该在每一次进入while循环的时候都重新变成1，要不然就会越乘越大了。





## 习题5-7 使用函数求余弦函数的近似值 (15 分)

本题要求实现一个函数，用下列公式求cos(*x*)的近似值，精确到最后一项的绝对值小于*e*：

$cos(x)=x^{0}/0!−x^{2}/2!+x^{4}/4!−x^{6}/6!+⋯$

### 函数接口定义：

```c++
double funcos( double e, double x );
```

其中用户传入的参数为误差上限`e`和自变量`x`；函数`funcos`应返回用给定公式计算出来、并且满足误差要求的cos(*x*)的近似值。输入输出均在双精度范围内。

### 裁判测试程序样例：

```c++
#include <stdio.h>
#include <math.h>

double funcos( double e, double x );

int main()
{    
    double e, x;

    scanf("%lf %lf", &e, &x);
    printf("cos(%.2f) = %.6f\n", x, funcos(e, x));

    return 0;
}

/* 你的代码将被嵌在这里 */
```

### 输入样例：

```in
0.01 -3.14
```

### 输出样例：

```out
cos(-3.14) = -0.999899
```

### 题解

```C
double funcos( double e, double x )
{
    double re=0;
    double num=0;//分子
    double den=1;//分母！=0
    double sum=0;
    int i=0;
    while(1)
    {
        int temp=2*i;
        if(i==0)
        {
            re=1.0;
            sum+=re;
        }
        else
        {
            int flag;
            if(i%2==0)//偶次项是正数，奇次项是负数
            {
                flag=0;
            }
            else
            {
                flag=1;
            }
            num=pow(x,temp);
            den=1;
            while(temp!=0)
            {
                den*=temp;
                temp--;
            }
            re=num/den;
            if(flag==1)
            {
                re=0-re;
            }
            sum+=re;
        }
        if(re<0)
        {
        	double tem=0-re;
        	if(tem<e)
        		break;
		}
		else
		{
			if(re<e)
				break;
		}
//         if(abs(re)<e)
//             break;
        i++;
    }

    return sum;
}
```

### 总结

1.浮点数的绝对值函数是double fabs(double x)