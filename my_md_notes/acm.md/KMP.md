# KMP

> 引用作者:阮一峰 [[原文](https://www.ruanyifeng.com/blog/2013/05/Knuth–Morris–Pratt_algorithm.html)]
>
> 引用作者:孤~影[[原文](https://www.cnblogs.com/yjiyjige/p/3263858.html)]
>
> 字符串匹配算法
>
> 就是有两个字符串 A,B(length(A)>length(B)) 在A里找一个连续子串使其与B相同

一般的算法就是一个一个比,遇到一个不一样的就把B整体向后移动一位 再从头比较

![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\bg2013050103-16434652022082.png)

![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\bg2013050104-16434652146674.png)



这样做虽然可行，但是效率很差，因为你要把"搜索位置"移到已经比较过的位置，重比一遍。

但是如下图 我们已经知道ABCDAB这几个字母已经成功匹配 那么我们可以利用这个已知信息

KMP算法的想法是，设法利用这个已知信息，不要把"搜索位置"移回已经比较过的位置，继续把它向后移，这样就提高了效率。

![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\bg2013050107-16434653034416-16434653056767.png)

如下图KMP算法就会这么做

![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\bg2013050110-16434657655009-164346576645810-164346577178211.png)

观察B串 我们可以看到在CD没有匹配成功所以在D之前的ABCDAB可以看到有两个相同的前后最子串AB , 因为已经知道了B串第二个"AB"已经匹配上了而且在此之前也有一个"AB" 那么把前面一个AB移动到后面那个AB对应的位置不就可以减少比较量 不在重复比较了吗?

 但是如何判断移动到哪个位置呢?

这就用到了《部分匹配表》

![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\bg2013050109-164346615292413.png)

下面介绍《部分匹配表》是如何产生的。

首先，要了解两个概念："前缀"和"后缀"。 "前缀"指除了最后一个字符以外，一个字符串的全部头部组合；"后缀"指除了第一个字符以外，一个字符串的全部尾部组合。



以"ABCDABD"为例，

> －　"A"的前缀和后缀都为空集，共有元素的长度为0；
>
> 　　－　"AB"的前缀为[A]，后缀为[B]，共有元素的长度为0；
>
> 　　－　"ABC"的前缀为[A, AB]，后缀为[BC, C]，共有元素的长度0；
>
> 　　－　"ABCD"的前缀为[A, AB, ABC]，后缀为[BCD, CD, D]，共有元素的长度为0；
>
> 　　－　"ABCDA"的前缀为[A, AB, ABC, ABCD]，后缀为[BCDA, CDA, DA, A]，共有元素为"A"，长度为1；
>
> 　　－　"ABCDAB"的前缀为[A, AB, ABC, ABCD, ABCDA]，后缀为[BCDAB, CDAB, DAB, AB, B]，共有元素为"AB"，长度为2；
>
> 　　－　"ABCDABD"的前缀为[A, AB, ABC, ABCD, ABCDA, ABCDAB]，后缀为[BCDABD, CDABD, DABD, ABD, BD, D]，共有元素的长度为0。



"部分匹配"的实质是，有时候，字符串头部和尾部会有重复。比如，"ABCDAB"之中有两个"AB"，那么它的"部分匹配值"就是2（"AB"的长度）。搜索词移动的时候，第一个"AB"向后移动4位（字符串长度-部分匹配值）或者是J指针向前移动2个单位，就可以来到第二个"AB"的位置。





部分匹配表使用一个next数组存储

* next[j] = k，表示当T[i] ! = P [ j ] 时，j指针的下一个位置。

其在KMP中发挥的作用就是当在第J位发生了匹配不成功时使用J = next [ J ] 把 比较的指针转到合适的位置,来保证算法正确性和高效性 

##### 对于同一个next数组对于不同的A串的普遍适用性: next[i]表示在p[i]的位置发生了不匹配 j指针应该转移到哪里 那么就是说$[0,j-1]$都已经匹配成功与B串部分相同 所以不管A串如何 next只与B数组相关,并且对任何A数组都有普遍适用性, 

如图

![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\17083912-49365b7e67cd4877b2f501074dae68d2-164346657753215.png)



C和D不匹配了 因为不能重复比较所以 i 不变 那么 显然 j 应该移动到 p[1]的位置 因为已知A是匹配上的

![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\17083929-a9ccfb08833e4cf1a42c30f05608f8f5-164346675540917.png)

那么next数组的算法

``` c++
void getnext(){
	 int k=-1,j=0;
	 nex[0]=-1;
	while(j<lenp){
		if(k==-1||p[j]==p[k]){
			nex[++j]=++k;
		}
		else{
			k=nex[k];
		}
	}
	return;
}
```



![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\17084258-efd2e95d3644427ebc0304ed3d7adefb-164346715986119.png)

像上图这种情况，**j**已经在最左边了，不可能再移动了，这时候要应该是i指针后移。所以在代码中才会有next[0] = -1;这个初始化 代表第B [ 0 ] 不匹配的话 j 指针不移动 反而是 i 指针向下移动一位



如果是当j为1的时候呢？

  

​                                                ![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\17084310-29f9f8dbb6034151a383e7ccf6f5583e-164346719207021.png)  

显然，**j**指针一定是后移到**0**位置的。因为它前面也就只有这一个位置





###### 下面这个是最重要的，请看如下图：

​                                                 ![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\17084327-8a3cdfab03094bfa9e5cace26796cae5.png) 







​                                         ![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\17084342-616036472ab546c082aa991004bb0034.png)  





 

请仔细对比这两个图。

我们发现一个规律：

当P[k] == P[j]时，

有next[j+1] == next[j] + 1

其实这个是可以证明的：

因为在P[j]之前已经有P[0 ~ k-1] == p[j-k ~ j-1]。（next[j] == k）

这时候现有P[k] == P[j]，我们是不是可以得到P[0 ~ k-1] + P[k] == p[j-k ~ j-1] + P[j]。

即：P[0 ~ k] == P[j-k ~ j]，

###### **即next[j+1] == k + 1 == next[j] + 1**。

这里的公式不是很好懂，还是看图会容易理解些。

 

那如果P[k] != P[j]呢？比如下图所示：

![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\17122358-fd7e52dd382c4268a8ff52b85bff465d.png) 



像这种情况，如果你从代码上看应该是这一句：k = next[k];为什么是这样子？你看下面应该就明白了。



 ![img](C:\Users\86186\Desktop\笔记\我的md笔记\acm.md\Untitled.assets\17122439-e349fed25e974e7886a27d18871ae48a.png)



现在你应该知道为什么要k = next[k]了吧！像上边的例子，我们已经不可能找到[ A，B，A，B ]这个最长的后缀串了，但我们还是可能找到[ A，B ]、[ B ]这样的前缀串的。所以这个过程像不像在定位[ A，B，A，C ]这个串，当C和主串不一样了（也就是k位置不一样了），那当然是把指针移动到next[k]啦。

 

有了next数组之后就一切好办了，我们可以动手写KMP算法了

下一步就是通过"遍历"的方法找到是否有与B串相符合的A的子串 一般来说有那么就返回子串第一位的索引 没有就返回 -1 如果是要找所有与B串相同的子串在每一次匹配完成后都要重新







(洛谷板子题)[[P3375 【模板】KMP字符串匹配 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P3375)]

``` c++
#include <cstdio>
#include <iostream>
#include <string>
#include <cmath>
#include <queue>
#include <vector>
#include <deque>
#include <set>
#include <map>
#include <cstring>
#include <algorithm>
using namespace std;
typedef long long ll;
char s[1000006],p[1000006];
int nex[1000006];
int lens,lenp;
void getnext(){
	 int k=-1,j=0;
	 nex[0]=-1;
	while(j<lenp){
		if(k==-1||p[j]==p[k]){
			nex[++j]=++k;
		}
		else{
			k=nex[k];
		}
	}
	return;
}
int main() {
	// ios::sync_with_stdio(false)
	scanf("%s%s",s,p);
	lens=strlen(s);
	lenp=strlen(p);
	getnext();
	int i=0,j=0;
	while(i<lens&&j<lenp){
		if(j==-1||s[i]==p[j]){
			i++;
			j++;
		}
		else{
			j=nex[j];
		}
		if(j==lenp){//这一段是当要求求出所有的匹配子串时需要的 只求第一个的话把这给if注掉
			printf("%d\n",i-j+1);
			j=nex[j];
		}
	}
    // if(j==lenp){//当只求一第一个相同子串时 s
		// printf("%d",i-j+1);
	// }
	for(int i=1;i<=lenp;i++){
		printf("%d ",nex[i]);
	}
	return 0;
}
```

