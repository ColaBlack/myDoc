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

![image](https://cdn.nlark.com/yuque/__latex/07c3c9564a753c54f2ab3b66678686cc.svg)

而如果出现

![image](https://cdn.nlark.com/yuque/__latex/e21f0414c3c8a5062b70c009e5382719.svg)

例如：

![image](https://cdn.nlark.com/yuque/__latex/1d8b9133880f51af78bd7dcb96f68ebe.svg)

注意到分母是幂函数已经比较简单了，所以只需要考虑分子的化简。

而分子刚好是![image](https://cdn.nlark.com/yuque/__latex/754106e416a98a0e35defa9e1c4cd695.svg)型的，所以可以按上面说的方法添项减项。但是你会发现这样做并不容易。

所以我们还得再换个思路，因为分子是幂指函数所以可以考虑取e再取对数。

并且我们这样做正好统一了减号前后的结构。因此：

![image](https://cdn.nlark.com/yuque/__latex/99fe984e9c49a74ad92bc69e0e9055ba.svg)

此处有两种选择，一种是注意到两边的结构被统一了，可以用拉格朗日中值定理。

还有一种选择是注意到，现在的形式和![image](https://cdn.nlark.com/yuque/__latex/754106e416a98a0e35defa9e1c4cd695.svg)型的处理类似，出现了相同的底数，所以可以提一项出去。

我们这一节在讲添项减项，因此首选第二个方案。

![image](https://cdn.nlark.com/yuque/__latex/477f1f3c78b32e20c429d7a3b4a04a9b.svg)

接下来，我们先处理乘积前面的幂指函数。

方法其实已经用过了。

这里我们更聚焦一些，考虑![image](https://cdn.nlark.com/yuque/__latex/11fbbd6703f1fa812f8f78e6b8057c30.svg)，由于e的x次方是连续的因此我们可以交换极限和函数的次序。

![image](https://cdn.nlark.com/yuque/__latex/d668f12c68f9ee1e4859eaffe7d70850.svg)

接着你会发现极限内又是幂指函数，我们再聚焦一下：

![image](https://cdn.nlark.com/yuque/__latex/db01926875142fb94fcd8b22d86d663a.svg)

### 利用第一第二重要极限

![image](https://cdn.nlark.com/yuque/__latex/db01926875142fb94fcd8b22d86d663a.svg)

注意到这是第二重要极限，结果为e，那就算完了。

这个第二重要极限特别容易忽略，而第一重要极限则往往表现为等价无穷小，所以不会被遗忘。

再次我也专门为他开一个小标题。

但是针对这个问题，我希望舍近求远，

### 幂指函数：取e再取对数

因为这是幂指函数的极限，所以可以取e再取对数。

![image](https://cdn.nlark.com/yuque/__latex/c46a2514c7876fbdd26d8601263b1de5.svg)

本质上这只是一个化简技巧而已，所以其他化简技巧也可以使用。

例如当分母出现根号的差或和可以考虑使用分母有理化。

### 利用函数的连续性

接下来，我们可以利用函数连续性来计算。

最简单的极限计算方法是利用函数的连续性。

根据函数f(x)在![image](https://cdn.nlark.com/yuque/__latex/e966678757bf1e42157e4502e9d417ee.svg)处连续的定义：

![image](https://cdn.nlark.com/yuque/__latex/81d3d586bcfe40bd564436502932f3dc.svg)

我们有

![image](https://cdn.nlark.com/yuque/__latex/8e091d968762569fd2531b3bb9b10ebb.svg)

当其实函数连续不单单意味着以上这一个简简单单的公式。

一般的，如果我们有f(x)和g(x)在![image](https://cdn.nlark.com/yuque/__latex/e966678757bf1e42157e4502e9d417ee.svg)处连续，那么就有

![image](https://cdn.nlark.com/yuque/__latex/4e43e40670a784c90716ba1973c27856.svg)

![](https://cdn.nlark.com/yuque/0/2025/png/48758001/1743835392467-b8bdcc64-51b1-4b46-99db-b984132a6358.png)

也就是，如果复合函数的外层函数连续，内层函数也连续，那么我们就可以交换极限和函数的次序。

这个方法在幂指函数时非常常用。例如，

![image](https://cdn.nlark.com/yuque/__latex/92beb040432392d91d23771532174578.svg)

接着再交换极限和函数的次序

![image](https://cdn.nlark.com/yuque/__latex/d6d0235983745894230cc5d5c0a7c6a8.svg)

再次聚焦

![image](https://cdn.nlark.com/yuque/__latex/e4304fa0fe38b9041d1925de73f03f22.svg)

这已经可以用等价无穷小替换了，再次我们舍近求远使用洛必达法则。

### 使用洛必达法则

嗯，只是为了把洛必达法则串进去而已。

洛必达法则专门解决0比0型和什么比无穷型未定式的极限。

![](https://cdn.nlark.com/yuque/0/2025/png/48758001/1743835463588-98701a5c-72a9-4bb0-85dc-827c47b8fd86.png)

![](https://cdn.nlark.com/yuque/0/2025/png/48758001/1743835612296-8c0cd417-627b-4885-9542-914ba0e583b3.png)

注意第二个定理，我们并不要求分子也趋于无穷大。

所以其实不是无穷比无穷型，而是什么比无穷型，分子是啥我不管。

总之，我们可以用洛必达法则求

![image](https://cdn.nlark.com/yuque/__latex/d28eabc92677a7a1cc780b696c9ba134.svg)

现在再倒回去。

![image](https://cdn.nlark.com/yuque/__latex/d90bdde6091939b5dad9225de2c5b4d4.svg)

![image](https://cdn.nlark.com/yuque/__latex/263bf021cf5d8f315bf7dee3af030083.svg)

### 利用极限的四则运算法则

接着再倒回去我们到了

![image](https://cdn.nlark.com/yuque/__latex/477f1f3c78b32e20c429d7a3b4a04a9b.svg)

注意到乘号左边的极限已经求出来了，是非0常数，因此可以提前算出来。

所以

![image](https://cdn.nlark.com/yuque/__latex/bca92a65bac2f76f74a0396330816790.svg)

### 利用等价无穷小

接下来，可以注意到分子是e的x次方-1型的，所以应当利用等价无穷小替换。

常用的等价无穷小替换如下：

(以下都是x趋于0时的）

> ![image](https://cdn.nlark.com/yuque/__latex/a43b36eb3a7e173b4ef326729bae2608.svg)
>
> ![image](https://cdn.nlark.com/yuque/__latex/78fe8053d852e82b70c4d2d563444429.svg)(注意小心根号！)
>
> ![image](https://cdn.nlark.com/yuque/__latex/208defe106acc65ce077c6901cad9aa7.svg)
>
> ![image](https://cdn.nlark.com/yuque/__latex/f13e5d22e49c8941338db7e9097ddffa.svg)这个是最常见的逆用等价无穷小的情况！！！

注意等价无穷小的逆用，例如

我们有：

![image](https://cdn.nlark.com/yuque/__latex/1fe01656a6ec28de07dda89d4b9bccbc.svg)

在这里我们使用

![image](https://cdn.nlark.com/yuque/__latex/4791feae87d5ef0c1977816962573505.svg)

![image](https://cdn.nlark.com/yuque/__latex/5c5c94a74071c642b4509cc9669097eb.svg)

### 利用泰勒公式

接下来我们终于可以使用泰勒公式计算了。

对于函数极限的计算，我们常用的是带皮亚诺余项的麦克劳林展开式。

在此给出常用的麦克劳林展开式。

> ![image](https://cdn.nlark.com/yuque/__latex/677d8b22f027fe5e9ab82c59166d0b90.svg) 不交错的n的阶乘分之x的n次方型
>
> ![image](https://cdn.nlark.com/yuque/__latex/9b8bdc46f5c5d3815a29e586636bfd36.svg) 交错的奇数的阶乘分之x的奇数次方型
>
> ![image](https://cdn.nlark.com/yuque/__latex/be130ca366b8d2fa13328ccda483fb70.svg) 不交错的奇数的阶乘分之x的奇数次方型
>
> ![image](https://cdn.nlark.com/yuque/__latex/2137b204d49003e098aa66cbfddaec14.svg) 交错的奇数的阶乘分之x的奇数次方型
>
> ![image](https://cdn.nlark.com/yuque/__latex/6c4434eedcf50a97c9ff96f85f427d87.svg)不交错的偶数的阶乘分之x的偶数次方型
>
> ![image](https://cdn.nlark.com/yuque/__latex/f11f22bea1e8c2cab488fa4b1023642c.svg)不交错的等比数列型
>
> ![image](https://cdn.nlark.com/yuque/__latex/f6c4a0316e20b6b7f42be95ae59c9f61.svg)交错的等比数列型
>
> ![image](https://cdn.nlark.com/yuque/__latex/cc736c1e0d912a0cf6f1ff6c0559a466.svg)交错的n分之x的n次方型
>
> ![image](https://cdn.nlark.com/yuque/__latex/38408105a75f2089370b5d825b4f69ac.svg)不交错的n分之x的n次方型
>
> ![image](https://cdn.nlark.com/yuque/__latex/c9315bd34cc93f43f504f305e5df917a.svg)不交错的奇数分之x的奇数次方型
>
> ![image](https://cdn.nlark.com/yuque/__latex/8823084f52549a6c0a2093282a249a01.svg)不交错的奇数分之x的奇数次方型
>
> ![image](https://cdn.nlark.com/yuque/__latex/109d924a48dd4167d3cbf41944650906.svg)当指数![image](https://cdn.nlark.com/yuque/__latex/b0364cde3cf7f2510337cce660a48c5f.svg)是正整数的时候，该公式退化为二项式展开。特别注意![image](https://cdn.nlark.com/yuque/__latex/b0364cde3cf7f2510337cce660a48c5f.svg)是0.5，也就是![image](https://cdn.nlark.com/yuque/__latex/7620d5635b9919dc85f05e459c182277.svg)的情形！
>
> 我没有给出通项公式型：
>
> ![image](https://cdn.nlark.com/yuque/__latex/f63fdb8d4d18e538daf877c90e243f17.svg)
>
> ![image](https://cdn.nlark.com/yuque/__latex/eddf337304577e07f3dc7c52ed6fcfae.svg)
>
> ![image](https://cdn.nlark.com/yuque/__latex/64bc0ce795067ef4419f6d7e12306361.svg)
>
> ![image](https://cdn.nlark.com/yuque/__latex/483f9ccb410143fafe795a069798c601.svg)

这里，注意到本题涉及的函数的泰勒展开已在上文给出，所以直接使用即可。

![image](https://cdn.nlark.com/yuque/__latex/4f21a8fc18550abb9882b42ab1c4b05d.svg)

终于算出来了！！！

恭喜！！！

### 利用泰勒公式构造等价无穷小

在此提供一种只使用等价无穷小解决这题的方法。

也是解这题比较快的方法。

前面是一样的，利用幂指函数的技巧，然后再利用同底指数的技巧。

![image](https://cdn.nlark.com/yuque/__latex/477f1f3c78b32e20c429d7a3b4a04a9b.svg)

然后利用第二重要极限，得到乘积前面的结果可以提出。

![image](https://cdn.nlark.com/yuque/__latex/e4743bb32d37b6d81cd0244b1e9b5b73.svg)

然后和前面一样，利用等价无穷小替换。

![image](https://cdn.nlark.com/yuque/__latex/1c94ce0ecb6219637dfcbcc4449040c7.svg)

接着注意到减号前面还是一个幂指函数，再次利用幂指函数技巧。

![image](https://cdn.nlark.com/yuque/__latex/20cefc9d28d9d9c4088f81cb94f3c282.svg)

根据![image](https://cdn.nlark.com/yuque/__latex/c868f28fe6b6e9e4634483eb0a778155.svg)我们知道e指数上是趋于1的而不是趋于0。

然后我们利用泰勒展开构造等价无穷小。

由于![image](https://cdn.nlark.com/yuque/__latex/677d8b22f027fe5e9ab82c59166d0b90.svg)

故![image](https://cdn.nlark.com/yuque/__latex/388c7d0007eb44ca712d9bd1c8573852.svg)

于是![image](https://cdn.nlark.com/yuque/__latex/3e44702fc7afac3e2842d07c741cad8c.svg)

从而

![image](https://cdn.nlark.com/yuque/__latex/5ffe761af6ff2442cc961cdb263c2a15.svg)

注意到，分子现在就是这个类型的等价无穷小，进行替换。

![image](https://cdn.nlark.com/yuque/__latex/c48ab2881893df26fbe7e01883dc4ac1.svg)

而我们有![image](https://cdn.nlark.com/yuque/__latex/cc736c1e0d912a0cf6f1ff6c0559a466.svg)

所以就有![image](https://cdn.nlark.com/yuque/__latex/1c6b83aa90cd59b6b3edbf45716b0418.svg)，从而

![image](https://cdn.nlark.com/yuque/__latex/4e5a7c85b8e86d9ddf6035f9863f9d79.svg)

### 利用拉格朗日中值定理

前面我们说到这题还可以用拉格朗日中值定理，现在我们尝试一下。

![image](https://cdn.nlark.com/yuque/__latex/130f9de657d4a11e3fd3b79b7267415a.svg)

其中,![image](https://cdn.nlark.com/yuque/__latex/025c9eeede7dedd813a953bd3aca40ec.svg)

而当x趋于0时，两边都趋于e（理由前面已给出），因此![image](https://cdn.nlark.com/yuque/__latex/ed5a4aa5e092e303a69c608582c70db9.svg)也趋于e，从而

![image](https://cdn.nlark.com/yuque/__latex/b8ebc7964b2d02cfd38ec2fe03432538.svg)

极限号里面的极限前文已给出两种计算方法。

拉格朗日中值定理还需要注意的是，当中值在两个等价的无穷小之间时，中值就等价与这两个无穷小。（按等价无穷小的定义和极限的迫敛性不难证明）

### 利用换元

对于函数极限还可以使用换元法求解，特别是当分母次数特别高的时候可以倒代换。

例如，

![image](https://cdn.nlark.com/yuque/__latex/1d2dd18be0f4a02cdbf769e13864e8b0.svg)

首先，应该统一结构。

![image](https://cdn.nlark.com/yuque/__latex/349cb215e91c1f939867b2b50243c9ff.svg)

然后仍然可以使用拉格朗日中值定理，但在此我们介绍倒代换的方法。

令![image](https://cdn.nlark.com/yuque/__latex/e503baa0ff638efb276aef1f28cc4459.svg)

![image](https://cdn.nlark.com/yuque/__latex/4b12feb2ea17e27f773e54ef845f23db.svg)

然后再使用拉格朗日中值定理或者洛必达法则即可得到答案0.5。

### 利用泰勒展开求隐函数极限

对于隐函数，其极限问题往往结合隐函数求导法则再利用泰勒展开求解。

例如，

设y由![image](https://cdn.nlark.com/yuque/__latex/5a052becae04022771c21aff37bd5cb4.svg)确定，求![image](https://cdn.nlark.com/yuque/__latex/f4637ea04b8ce369177b7024d53b082c.svg)

只需要按照隐函数求导法则求出y的1和2阶导数，分别为![image](https://cdn.nlark.com/yuque/__latex/74fae690d0bf7bdbcc868fe88069c53b.svg)再用泰勒展开求解即可，答案刚好是1。

