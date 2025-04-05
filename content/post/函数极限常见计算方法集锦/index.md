+++
date = '2025-04-05T16:04:40+08:00'
draft = true
title = '函数极限常见计算方法集锦'
+++

本文非常直接，如标题所见就是一个常见的计算方式极限方法的集锦。

所以内在逻辑性确实不强，主要通过例题的形式阐述。

### 添项减项

当题目出现了交错的形式便可以考虑添项减项。

一般而言我们会加一项交错项，减一项交错项。

例如出现

$ AB+A'B'=AB+AB'-AB'+A'B'=A(B-B')+(A'-A)B' $

而如果出现

$ A^B+A'^{B'}=A^B+A^{B'}-A^{B'}+A'^{B'}=A^B(1+A^{B'-B})+A'^{B'}(1-(\frac{A}{A'})^{B'}) $

例如：

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2} $

注意到分母是幂函数已经比较简单了，所以只需要考虑分子的化简。

而分子刚好是$ A^B+A'^{B'}
 $型的，所以可以按上面说的方法添项减项。但是你会发现这样做并不容易。

所以我们还得再换个思路，因为分子是幂指函数所以可以考虑取e再取对数。

并且我们这样做正好统一了减号前后的结构。因此：

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=\lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-e^{\frac{e\ln(1+x)}{x}}}{x^2} $

此处有两种选择，一种是注意到两边的结构被统一了，可以用拉格朗日中值定理。

还有一种选择是注意到，现在的形式和$ A^B+A'^{B'}
 $型的处理类似，出现了相同的底数，所以可以提一项出去。

我们这一节在讲添项减项，因此首选第二个方案。

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=\lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-e^{\frac{e\ln(1+x)}{x}}}{x^2}=\lim_{x\to 0}e^{(1+x)^{\frac{1}{x}}}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2} $

接下来，我们先处理乘积前面的幂指函数。

方法其实已经用过了。

这里我们更聚焦一些，考虑$ \lim_{x\to 0}e^{(1+x)^{\frac{1}{x}}} $，由于e的x次方是连续的因此我们可以交换极限和函数的次序。

$ \lim_{x\to 0}e^{(1+x)^{\frac{1}{x}}}=e^{\lim_{x\to 0}(1+x)^{\frac{1}{x}}} $

接着你会发现极限内又是幂指函数，我们再聚焦一下：

$ \lim_{x\to 0}(1+x)^{\frac{1}{x}} $

### 利用第一第二重要极限

$ \lim_{x\to 0}(1+x)^{\frac{1}{x}} $

注意到这是第二重要极限，结果为e，那就算完了。

这个第二重要极限特别容易忽略，而第一重要极限则往往表现为等价无穷小，所以不会被遗忘。

再次我也专门为他开一个小标题。

但是针对这个问题，我希望舍近求远，

### 幂指函数：取e再取对数

因为这是幂指函数的极限，所以可以取e再取对数。

$ \lim_{x\to 0}(1+x)^{\frac{1}{x}}=\lim_{x\to 0}e^{\frac{\ln(1+x)}{x}} $

本质上这只是一个化简技巧而已，所以其他化简技巧也可以使用。

例如当分母出现根号的差或和可以考虑使用分母有理化。

### 利用函数的连续性

接下来，我们可以利用函数连续性来计算。

最简单的极限计算方法是利用函数的连续性。

根据函数f(x)在$ x=x_0 $处连续的定义：

$ \forall \epsilon > 0,\exists \delta>0,s.t. |x-x_0|<\delta,|f(x)-f(x_0)|<\epsilon
 $

我们有

$ \lim_{x\to x_0} f(x)=f(x_0)
 $

当其实函数连续不单单意味着以上这一个简简单单的公式。

一般的，如果我们有f(x)和g(x)在$ x=x_0 $处连续，那么就有

$ \lim_{x\to x_0} f(g(x))=f(\lim_{x\to x_0}g(x)
) $

![](https://cdn.nlark.com/yuque/0/2025/png/48758001/1743835392467-b8bdcc64-51b1-4b46-99db-b984132a6358.png)

也就是，如果复合函数的外层函数连续，内层函数也连续，那么我们就可以交换极限和函数的次序。

这个方法在幂指函数时非常常用。例如，

$ \lim_{x \to 0} (1+px)^{\frac{1}{x}}=\lim_{x \to 0} [(1+px)^{\frac{p}{x}}]^\frac{1}{p}=[\lim_{x \to 0} (1+px)^{\frac{p}{x}}]^\frac{1}{p}=e^\frac{1}{p} $

接着再交换极限和函数的次序

$ \lim_{x\to 0}(1+x)^{\frac{1}{x}}=\lim_{x\to 0}e^{\frac{\ln(1+x)}{x}}=e^{\lim_{x\to 0}\frac{\ln(1+x)}{x}} $

再次聚焦

$ \lim_{x\to 0}\frac{\ln(1+x)}{x} $

这已经可以用等价无穷小替换了，再次我们舍近求远使用洛必达法则。

### 使用洛必达法则

嗯，只是为了把洛必达法则串进去而已。

洛必达法则专门解决0比0型和什么比无穷型未定式的极限。

![](https://cdn.nlark.com/yuque/0/2025/png/48758001/1743835463588-98701a5c-72a9-4bb0-85dc-827c47b8fd86.png)

![](https://cdn.nlark.com/yuque/0/2025/png/48758001/1743835612296-8c0cd417-627b-4885-9542-914ba0e583b3.png)

注意第二个定理，我们并不要求分子也趋于无穷大。

所以其实不是无穷比无穷型，而是什么比无穷型，分子是啥我不管。

总之，我们可以用洛必达法则求

$ \lim_{x\to 0}\frac{\ln(1+x)}{x}=\lim_{x\to 0}\frac{\frac{1}{1+x}}{1}=1 $

现在再倒回去。

$ \lim_{x\to 0}(1+x)^{\frac{1}{x}}=\lim_{x\to 0}e^{\frac{\ln(1+x)}{x}}=e^{\lim_{x\to 0}\frac{\ln(1+x)}{x}}=e $

$ \lim_{x\to 0}e^{(1+x)^{\frac{1}{x}}}=e^{\lim_{x\to 0}(1+x)^{\frac{1}{x}}}=e^e $

### 利用极限的四则运算法则

接着再倒回去我们到了

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=\lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-e^{\frac{e\ln(1+x)}{x}}}{x^2}=\lim_{x\to 0}e^{(1+x)^{\frac{1}{x}}}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2} $

注意到乘号左边的极限已经求出来了，是非0常数，因此可以提前算出来。

所以

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=\lim_{x\to 0}e^{(1+x)^{\frac{1}{x}}}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2}=e^e\lim_{x\to 0}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2} $

### 利用等价无穷小

接下来，可以注意到分子是e的x次方-1型的，所以应当利用等价无穷小替换。

常用的等价无穷小替换如下：

(以下都是x趋于0时的）

> $ x \sim \sin x \sim \tan x \sim \arcsin x \sim \arctan x \sim e^x-1 \sim \ln(x+\sqrt{1+x^2})
>  $
>
> $ (1+x)^\alpha \sim 1+\alpha x $(注意小心根号！)
>
> $ 1-\cos x \sim \frac{x^2}{2} $
>
> $ x-1 \sim \ln x

 $这个是最常见的逆用等价无穷小的情况！！！

>

注意等价无穷小的逆用，例如

我们有：

$ 1-\cos^p x \sim -\ln \cos^p x=-p\ln \cos x\sim-p(\cos x -1)=p(1-\cos x)\sim\frac{px^2}{2} $

在这里我们使用

$ e^x-1 \sim x $

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=e^e\lim_{x\to 0}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2}=e^e\lim_{x\to 0}\frac{(1+x)^{\frac{1}{x}}-\frac{e\ln(1+x)}{x}}{x^2} $

### 利用泰勒公式

接下来我们终于可以使用泰勒公式计算了。

对于函数极限的计算，我们常用的是带皮亚诺余项的麦克劳林展开式。

在此给出常用的麦克劳林展开式。

> $ e^x=1+x+\frac{x^2}{2!}+...+\frac{x^n}{n!}+o(x^{n}) $ 不交错的n的阶乘分之x的n次方型
>
> $ \sin x=x-\frac{x^3}{3!}+\frac{x^5}{5!}+(-1)^n\frac{x^{2n+1}}{(2n+1)!}+o(x^{2n+2})
>  $ 交错的奇数的阶乘分之x的奇数次方型
>
> $ \sinh x=\frac{e^x-e^{-x}}{2}=x+\frac{x^3}{3!}+...+\frac{x^{2n+1}}{(2n+1)!}+o(x^{2n+2}) $ 不交错的奇数的阶乘分之x的奇数次方型
>
> $ \cosh x=\frac{e^x+e^{-x}}{2}=1+\frac{x^2}{2!}+...+\frac{x^{2n}}{(2n)!}+o(x^{2n+1}) $ 交错的奇数的阶乘分之x的奇数次方型
>
> $ \cos x=1-\frac{x^2}{2!}+\frac{x^4}{4!}+(-1)^n\frac{x^{2n}}{(2n)!}+o(x^{2n+1})
>  $不交错的偶数的阶乘分之x的偶数次方型
>
> $ \frac{1}{1-x}=1+x+x^2+...+x^n+o(x^{n}) $不交错的等比数列型
>
> $ \frac{1}{1+x}=1-x+x^2+...+(-x)^n+o(x^{n}) $交错的等比数列型
>
> $ \ln (1+x)=x-\frac{x^2}{2}+\frac{x^3}{3}+...+(-1)^{n-1}\frac{x^n}{n}+o(x^{n}) $交错的n分之x的n次方型
>
> $ -\ln (1-x)=x+\frac{x^2}{2}+\frac{x^3}{3}+...+\frac{x^n}{n}+o(x^{n}) $不交错的n分之x的n次方型
>
> $ arctanh (x)=\frac{1}{2}\ln \frac{1+x}{1-x}=x+\frac{x^3}{3}+\frac{x^5}{5}+...+\frac{x^{2n+1}}{2n+1}+o(x^{2n+2}) $不交错的奇数分之x的奇数次方型
>
> $ \arctan x=x-\frac{x^3}{3}+\frac{x^5}{5}+...+(-1)^n\frac{x^{2n+1}}{2n+1}+o(x^{2n+2}) $不交错的奇数分之x的奇数次方型
>
> $ (1+x)^\alpha=1+\alpha x+\frac{\alpha (\alpha-1)}{2!}x^2+...+\frac{\alpha(\alpha-1)(\alpha-2)...(\alpha-n+1)}{n!}x^n+o(x^n) $当指数$ \alpha
>  $是正整数的时候，该公式退化为二项式展开。特别注意$ \alpha
>  $是0.5，也就是$ \sqrt{1+x} $的情形！
>
> 我没有给出通项公式型：
>
> $ \tan x=x+\frac{x^3}{3}+o(x^4) $
>
> $ \arcsin x=x+\frac{x^3}{6}+o(x^4) $
>
> $ arcsinh(x)=\ln(x+\sqrt{1+x^2})=x-\frac{x^3}{6}+o(x^4) $
>
> $ (1+x)^\frac{1}{x}=e(1-\frac{1}{2}x+\frac{11}{24}x^2)+o(x^2) $

这里，注意到本题涉及的函数的泰勒展开已在上文给出，所以直接使用即可。

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=e^e\lim_{x\to 0}\frac{(1+x)^{\frac{1}{x}}-\frac{e\ln(1+x)}{x}}{x^2}=e^e\lim_{x\to 0}\frac{e(1-\frac{1}{2}x+\frac{11}{24}x^2)-\frac{e(x-\frac{x^2}{2}+\frac{x^3}{3})}{x}+o(x^2)}{x^2}=\frac{1}{8}e^{e+1} $

终于算出来了！！！

恭喜！！！

### 利用泰勒公式构造等价无穷小

在此提供一种只使用等价无穷小解决这题的方法。

也是解这题比较快的方法。

前面是一样的，利用幂指函数的技巧，然后再利用同底指数的技巧。

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=\lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-e^{\frac{e\ln(1+x)}{x}}}{x^2}=\lim_{x\to 0}e^{(1+x)^{\frac{1}{x}}}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2} $

然后利用第二重要极限，得到乘积前面的结果可以提出。

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=\lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-e^{\frac{e\ln(1+x)}{x}}}{x^2}=\lim_{x\to 0}e^{(1+x)^{\frac{1}{x}}}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2}=e^e\lim_{x\to 0}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2} $

然后和前面一样，利用等价无穷小替换。

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=\lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-e^{\frac{e\ln(1+x)}{x}}}{x^2}=\lim_{x\to 0}e^{(1+x)^{\frac{1}{x}}}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2}=e^e\lim_{x\to 0}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2}=e^e\lim_{x\to 0}\frac{(1+x)^{\frac{1}{x}}-\frac{e\ln(1+x)}{x}}{x^2} $

接着注意到减号前面还是一个幂指函数，再次利用幂指函数技巧。

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=\lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-e^{\frac{e\ln(1+x)}{x}}}{x^2}=\lim_{x\to 0}e^{(1+x)^{\frac{1}{x}}}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2}=e^e\lim_{x\to 0}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2}=e^e\lim_{x\to 0}\frac{(1+x)^{\frac{1}{x}}-\frac{e\ln(1+x)}{x}}{x^2}=e^e\lim_{x\to 0}\frac{e^{\frac{\ln(1+x)}{x}}-e\frac{\ln(1+x)}{x}}{x^2} $

根据$ \ln(1+x)\sim x $我们知道e指数上是趋于1的而不是趋于0。

然后我们利用泰勒展开构造等价无穷小。

由于$ e^x=1+x+\frac{x^2}{2!}+...+\frac{x^n}{n!}+o(x^{n}) $

故$ e^{x-1}=x+\frac{(x-1)^2}{2!}+...+\frac{(x-1)^n}{n!}+o((x-1)^{n}) $

于是$ e^{x}=ex+e\frac{(x-1)^2}{2!}+...+e\frac{(x-1)^n}{n!}+o((x-1)^{n}) $

从而

$ e^x-ex \sim e\frac{(x-1)^2}{2!} $

注意到，分子现在就是这个类型的等价无穷小，进行替换。

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=\lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-e^{\frac{e\ln(1+x)}{x}}}{x^2}=\lim_{x\to 0}e^{(1+x)^{\frac{1}{x}}}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2}=e^e\lim_{x\to 0}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2}=e^e\lim_{x\to 0}\frac{(1+x)^{\frac{1}{x}}-\frac{e\ln(1+x)}{x}}{x^2}=e^e\lim_{x\to 0}\frac{e^{\frac{\ln(1+x)}{x}}-e\frac{\ln(1+x)}{x}}{x^2}=e^e\lim_{x\to 0}\frac{(1+x)^{\frac{1}{x}}-\frac{e\ln(1+x)}{x}}{x^2}=\frac{e^{e+1}}{2}\lim_{x\to 0}\frac{(\frac{\ln(1+x)}{x}-1)^2}{x^2}=\frac{e^{e+1}}{2}\lim_{x\to 0}\frac{(\ln(1+x)-x)^2}{x^4} $

而我们有$ \ln (1+x)=x-\frac{x^2}{2}+\frac{x^3}{3}+...+(-1)^{n-1}\frac{x^n}{n}+o(x^{n}) $

所以就有$ \ln(1+x)-x \sim - \frac{x^2}{2} $，从而

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=\lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-e^{\frac{e\ln(1+x)}{x}}}{x^2}=\lim_{x\to 0}e^{(1+x)^{\frac{1}{x}}}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2}=e^e\lim_{x\to 0}\frac{1-e^{\frac{e\ln(1+x)}{x}-(1+x)^{\frac{1}{x}}}}{x^2}=e^e\lim_{x\to 0}\frac{(1+x)^{\frac{1}{x}}-\frac{e\ln(1+x)}{x}}{x^2}=e^e\lim_{x\to 0}\frac{e^{\frac{\ln(1+x)}{x}}-e\frac{\ln(1+x)}{x}}{x^2}=e^e\lim_{x\to 0}\frac{(1+x)^{\frac{1}{x}}-\frac{e\ln(1+x)}{x}}{x^2}=\frac{e^{e+1}}{2}\lim_{x\to 0}\frac{(\frac{\ln(1+x)}{x}-1)^2}{x^2}=\frac{e^{e+1}}{2}\lim_{x\to 0}\frac{(\ln(1+x)-x)^2}{x^4}=\frac{e^{e+1}}{2}\lim_{x\to 0}\frac{x^4}{4x^4}=\frac{e^{e+1}}{8} $

### 利用拉格朗日中值定理

前面我们说到这题还可以用拉格朗日中值定理，现在我们尝试一下。

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=\lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-e^{\frac{e\ln(1+x)}{x}}}{x^2}=\lim_{x\to 0}\frac{e^\theta((1+x)^{\frac{1}{x}}-\frac{e\ln(1+x)}{x})}{x^2} $

其中,$ \theta 介于(1+x)^{\frac{1}{x}},\frac{e\ln(1+x)}{x}之间 $

而当x趋于0时，两边都趋于e（理由前面已给出），因此$ \theta $也趋于e，从而

$ \lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-(1+x)^{\frac{e}{x}}}{x^2}=\lim_{x\to 0}\frac{e^{(1+x)^{\frac{1}{x}}}-e^{\frac{e\ln(1+x)}{x}}}{x^2}=\lim_{x\to 0}\frac{e^\theta((1+x)^{\frac{1}{x}}-\frac{e\ln(1+x)}{x})}{x^2}=e^e\lim_{x\to 0}\frac{(1+x)^{\frac{1}{x}}-\frac{e\ln(1+x)}{x}}{x^2} $

极限号里面的极限前文已给出两种计算方法。

拉格朗日中值定理还需要注意的是，当中值在两个等价的无穷小之间时，中值就等价与这两个无穷小。（按等价无穷小的定义和极限的迫敛性不难证明）

### 利用换元

对于函数极限还可以使用换元法求解，特别是当分母次数特别高的时候可以倒代换。

例如，

$ \lim_{x\to +\infty} x(\frac{\pi}{4}-\arctan \frac{x}{1+x}) $

首先，应该统一结构。

$ \lim_{x\to +\infty} x(\frac{\pi}{4}-\arctan \frac{x}{1+x})=\lim_{x\to +\infty} x(\arctan 1-\arctan \frac{x}{1+x}) $

然后仍然可以使用拉格朗日中值定理，但在此我们介绍倒代换的方法。

令$ t=\frac{1}{x} $

$ \lim_{x\to +\infty} x(\frac{\pi}{4}-\arctan \frac{x}{1+x})=\lim_{x\to +\infty} x(\arctan 1-\arctan \frac{x}{1+x})=\lim_{t\to0^+}\frac{\arctan 1-\arctan \frac{1}{1+t}}{t} $

然后再使用拉格朗日中值定理或者洛必达法则即可得到答案0.5。

### 利用泰勒展开求隐函数极限

对于隐函数，其极限问题往往结合隐函数求导法则再利用泰勒展开求解。

例如，

设y由$ x^3+y^3+xy=1 $确定，求$ \lim_{x\to 0}\frac{3y+x^2+x-3}{x^2} $

只需要按照隐函数求导法则求出y的1和2阶导数，分别为$ -\frac{1}{3},0 $再用泰勒展开求解即可，答案刚好是1。

本来都完成了，我也都发布了。结果刚发布就看到新的计算方法了。

### 主动计算等价无穷小进行替换

直接上例题，已知f(x)可导,f(0)=0,f'(0)不为0。求

$ \lim_{x\to0}\frac{\tan 2x \int_{0}^{x}tf(x-t)dt}{\int_0^xtf(x^2-t^2)dt} $

首先，注意到这是一个含变限积分的极限题，最常规的思路是通过洛必达法则消掉变限积分的变限。

但是这题分母可以用这个方法，分子就比较痛苦了。

分子上$ \tan 2x
 $可以等价为2x，但是这就变成了x乘以变限积分。

此时求一次导不能解决变限积分。

但是这其实不是最糟糕的。更糟糕的是，如果我们想给变限积分求导，那么被积函数里也不能有x，因此我们需要对积分号进行化简。(事实上，任何时候也都应该考虑给变限积分化简）

分子的化简非常简单，利用区间再现公式即可。

$ \int_0^xtf(x-t)dx=\int_0^x(x-t)f(t)dx=x\int_0^xf(t)dt-\int_0^xtf(t)dt $

分母则需要凑微分。

$ \int_0^xtf(x^2-t^2)dt=\frac{1}{2}\int_0^xf(x^2-t^2)dt^2=-\frac{1}{2}\int_0^xf(x^2-t^2)d(-t^2)=-\frac{1}{2}\int_0^xf(x^2-t^2)d(x-t^2)=-\frac{1}{2}\int_{x^2}^0f(u)du=\frac{1}{2}\int_0^{x^2}f(t)dt
 $因此

$ \lim_{x\to0}\frac{\tan 2x \int_{0}^{x}tf(x-t)dt}{\int_0^xtf(x^2-t^2)dt}=4\lim_{x\to0}\frac{x(x\int_0^xf(t)dt-\int_0^xtf(t)dt)}{\int_0^{x^2}f(t)dt} $

显然，情况比我们一开始预想得还要糟糕。（但是这题仍然可以用洛必达来做，之后你就会看到）

这时就可以尝试着主动计算分子或分母的等价无穷小，进行等价无穷小替换。

显然，分母比较简单，我们来尝试看看。

设分母是x的n次方阶的，那么只需计算

$ \lim_{x\to0}\frac{\int_0^{x^2}f(t)dt}{x^k}=\lim_{x\to0}\frac{2xf(x^2)}{kx^{k-1}}=\frac{2}{k}\lim_{x\to0}\frac{f(x^2)}{x^{k-2}}
 $

很简单，就是一次洛必达而已。

### 利用导数定义计算极限

注意到，这个形式和导数的定义相似。

如果k-2=2即k=4的话，就有

$ \lim_{x\to0}\frac{\int_0^{x^2}f(t)dt}{x^4}=\lim_{x\to0}\frac{2xf(x^2)}{4x^{4-1}}=\frac{1}{2}\lim_{x\to0}\frac{f(x^2)}{x^{2}}=\frac{1}{2}\lim_{x\to0}\frac{f(x^2)-f(0)}{x^{2}-0}=\frac{1}{2}f'(0) $

因此，$ \lim_{x\to0}\frac{\int_0^{x^2}f(t)dt}{\frac{x^4f'(0)}{2}}=1$

所以，$ \int_0^{x^2}f(t)dt \sim \frac{x^4f'(0)}{2} $

从而，

$ \lim_{x\to0}\frac{\tan 2x \int_{0}^{x}tf(x-t)dt}{\int_0^xtf(x^2-t^2)dt}=4\lim_{x\to0}\frac{x(x\int_0^xf(t)dt-\int_0^xtf(t)dt)}{\int_0^{x^2}f(t)dt}=4\lim_{x\to0}\frac{x(x\int_0^xf(t)dt-\int_0^xtf(t)dt)}{\frac{x^4f'(0)}{2}}=8\lim_{x\to0}\frac{x\int_0^xf(t)dt-\int_0^xtf(t)dt}{x^3f'(0)} $

接下来，似乎就没有什么办法了。

我们只能开洛了，所以其实前面就是用了一个技巧化简分母而已。

我们最终的做法和洛必达没有什么本质区别。

$ \lim_{x\to0}\frac{\tan 2x \int_{0}^{x}tf(x-t)dt}{\int_0^xtf(x^2-t^2)dt}=8\lim_{x\to0}\frac{x\int_0^xf(t)dt-\int_0^xtf(t)dt}{x^3f'(0)}=8\lim_{x\to0}\frac{\int_0^xf(t)dt}{3x^2f'(0)} $

然后继续洛必达。

$ \lim_{x\to0}\frac{\tan 2x \int_{0}^{x}tf(x-t)dt}{\int_0^xtf(x^2-t^2)dt}=8\lim_{x\to0}\frac{\int_0^xf(t)dt}{3x^2f'(0)}=\frac{4}{3f'(0)}\lim_{x\to0}\frac{f(x)}{x} $

这下你应该知道后面的极限怎么处理了。

$ \lim_{x\to0}\frac{\tan 2x \int_{0}^{x}tf(x-t)dt}{\int_0^xtf(x^2-t^2)dt}=\frac{4}{3f'(0)}\lim_{x\to0}\frac{f(x)-0}{x-0}=\frac{4}{3f'(0)}f'(0)=\frac{4}{3} $

很遗憾，最后我们的做法对比直接洛必达并没有什么本质上的变化，但是我觉得这样写确实过程上更清爽了。

而且有一个更重要的问题，就是我们这里是需要反复陪凑导数定义的，如果你不使用这个方法 而直接洛必达，我想几乎很少有人能想到配凑导数定义了。所以这个方法确实带来了思想上的简化。

