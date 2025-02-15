+++
date = '2024-11-23T16:28:26+08:00'
draft = true
title = '浏览器是如何渲染页面的'

+++

初次发布于[我的个人文档](https://colablack.github.io)。（每次都是个人文档优先发布哦）

>  在上次我们以浏览器的事件循环为例简要介绍了如何调度异步资源，这一次要来填个坑，介绍一下浏览器是如何渲染页面的。没看过上一期的话就先看一下[上一期](https://colablack.github.io/p/%E5%BC%82%E6%AD%A5%E4%B8%8E%E8%B5%84%E6%BA%90%E8%B0%83%E5%BA%A6-%E4%BB%A5%E6%B5%8F%E8%A7%88%E5%99%A8%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E4%B8%BA%E4%BE%8B/)的开头，了解一下浏览器的多进程图景。

### 1.解析HTML

在上一期我们讲到，浏览器的页面由渲染进程完成，在这一期你将看到渲染进程是一个包含了渲染主线程等多个线程的进程。

当浏览器通过网络进程向服务器发送请求得到响应的HTML文本后，他要做的第一个工作就是解析收到的HTML文本。

对于大部分标签，建立对应的DOM树。对于CSS，建立对应的CSSOM树。

但是当解析工作遇到link的时候，按道理就需要去下载对应的css，但是我们知道网络IO是一个耗时比较长的过程，所以这个步骤也会阻塞渲染主线程。

因此，浏览器并不是以这个方式解析的。

而是先启动一个预解析线程，预先快速"浏览"一下HTML标签，下载HTML文本引用的外部CSS文件和JS文件。

当渲染主线程遇到一般的标签那就解析成DOM，但是当渲染主线程遇到link引用CSS的时候，渲染主线程将不会等待，直接解析下面的其他内容。（异步）

那么外部的CSS由谁解析呢？

由和刚刚说的预解析线程解析。

那外部JS呢？

JS比较特殊，现在来讲。

由于在JS中存在改变DOM树和CSSOM树的可能，所以JS的解析完全由渲染主线程操刀，并且当渲染主线程遇到script标签的时候，它会暂停现有的对HTML的解析（阻塞），等待预解析线程把对应的JS代码下载好，并且需要等到全局代码都被解析执行完成之后才继续解析HTML文本。

总之，解析过程就是得到DOM和CSSOM的过程，此时的CSSOM已经是一个包含浏览器默认样式、内部样式、外部样式、行内样式的比较完整的树结构了。

### 2.样式计算

之所以说是“比较完成的树”是因为此时CSSOM里还保留着em等相对值和计算属性，这一步就是将这些相对值和计算属性计算出来。

计算之后的就是最终样式computed style了。

非常简单，不需要多说什么其他话了。

### 3.布局

第三步是根据前面的信息对页面进行布局，生成布局树。

在这一步渲染主线程要遍历全部的DOM，计算他们的位置信息和几何信息。

但是注意，布局树和DOM树并不完全一致。

对于CSS为`display:none`的元素，由于他们不显示，所以不会在布局树中。DOM中不存在伪元素节点，但我们知道伪元素节点都有几何信息所以他们存在于布局树中。

由于存在规定

> 内容必须在行盒中
>
> 行盒和块盒不能相邻

所以，在布局这一步还会生成一些匿名的行盒或者块盒来使得页面满足这一规则。

### 4.分层

接下来，渲染主线程会对布局树的元素进行分层。这是一个优化的方法，当将来某一层改变后就只需要对这一层的元素进行对应改变就可以了，这将提高浏览器的性能。

### 5.绘制

接下来，绘制这一步就是为每一层生成对应的绘制指令，你可以理解为canvas之类的东西。

啊，什么把画笔移动到哪里哪里，然后向哪里哪里花一条直线什么的。

到生成绘制指令这一步，渲染主线程的任务才算结束。

### 6.分块

接下来压力给到了合成线程。啊，其实也不全是。

分块的工作要交给很多个被称为分块器的线程协作完成。

所谓分块就是字面意思，将每一层的元素分为若干小块，以便后续步骤的分工。

### 7.光栅化

一看到光栅化这个词已经有人在瑟瑟发抖了吧，其实很简单的。

就是把前面分的每一块变成位图。

或者说，合成线程会将每一个块的信息交给显卡（GPU）进程来绘制我们真正能看到的图像。

当然GPU进程会开多个线程来同时绘制图像，并且它将优先计算靠近视口的块，因为用户正在看那里嘛。

### 8.绘制

这一步是最后一步了。

合成线程在拿到GPU渲染的一张张位图后，将生成指引（quad）信息，标注出每个位图应该画在屏幕的哪个位置，要不要进行选择和缩放等。

CSS中的`transform`正是也只是作用于这一阶段，这也是其运行效率高的本质原因。

当生成指引信息后，合成线程会将指引信息传递给GPU进程，然后GPU进程会产生**系统调用**操作显卡硬件绘制用户真正能看到的页面图像。

### 9.后记

这就是浏览器渲染一个页面的全部步骤了。

了解这个或许没有什么用，额可能吧。

不过可以让你小心一点。

有一些操作会影响布局树，这时浏览器会从第三步重新开始后面的全部过程了，这被称为**回流/重排(reflow)**，如果可以的话应该尽量避免这样的操作。

当然，当你连续多次进行修改布局树的操作，浏览器会进行优化，自动合并这些操作，等对应的JavaScript代码全部完成后才统一进行一次reflow。

但是这也意味着，这时JavaScript获取的布局属性可能是不准确的，为了避免这种情况，当JavaScript代码尝试获取布局属性的时候浏览器会立刻进行reflow操作而不是等待代码全部运行完成。所以这种情况也应该规避。

而当你改变了DOM的几何信息的时候，浏览器就需要从绘制这一步重新开始，这一过程被称为**重绘(repaint)**。

这个效率会比reflow高一些，因为reflow是从第三步布局开始，也会经历完repaint经历的全部过程。（也有说法称reflow会引起repaint，从结果上看两种说法没有任何区别，你怎么理解都可以）

但是还得是transform高效啊，只影响了最后一步绘制，只有合成线程受到影响了而已，这效率高的不是一点半点啊。
