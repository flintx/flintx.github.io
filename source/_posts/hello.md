---
title: Hello, My Blog
date: 2017-02-02 12:23:44
tags:
	- 博客相关

---

## 图片插入测试

![a image](http://okqi2ipwh.bkt.clouddn.com/39163751_p0.jpg)



<!--more-->

## 代码高亮测试
```python

print("hello, my blog")

```

```java


import java.math.BigInteger;
import java.util.Scanner;


public class test{

	public static void main(String[] args) {
		Scanner cin = new Scanner(System.in);
		int t = cin.nextInt();
		int i = 1 ;
		while( i <= t ){
			BigInteger a = cin.nextBigInteger();
			BigInteger b = cin.nextBigInteger();
			BigInteger c = a.mod(b) ;
			if( c == BigInteger.ZERO )
				System.out.println("Case "+i+": divisible");
			else
				System.out.println("Case "+i+": not divisible");
		}
		cin.close();
		

	}
}


```
## 数学公式测试

$$
\begin{array}{c|lcr}
n & \text{Left} & \text{Center} & \text{Right} \\
\hline
1 & 0.24 & 1 & 125 \\
2 & -1 & 189 & -8 \\
3 & -20 & 2000 & 1+10i \\
\end{array}
$$

$ x^2 + y^2 = z\_1 + z\_{a2} $

