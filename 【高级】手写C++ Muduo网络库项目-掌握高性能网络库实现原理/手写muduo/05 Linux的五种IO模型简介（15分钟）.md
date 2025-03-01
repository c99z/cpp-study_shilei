# Linux的五种IO模型



接下来的内容呢？我给大家讲一下啊unix或者说是linux上的这个五种IO模型。

非常典型，重要的一个知识啊。

那在这里边，所谓的IO模型呢？

实际上是我们操作系统，任何的IO，比如说是这个内存IO啊，磁盘IO啊，网络IO。

啊，都可以应用值啊。

## 主要考虑网络IO

当然，我们主要在这里边儿还是涉及到网络lO啊，我们主要考虑网络lO就行了，

包括里边儿提到的所有的一些接口啊。这里边的五种lO模型主要包含阻塞,非阻塞lO你记住啊，

### 重点是阻塞IO,非阻塞IO

五种l这就是重点阻塞lO，非阻塞lO啊，阻塞是blocking，非阻塞是non blocking，

### IO复用

还有lO复用。

lO multiplex。select poll epoll对吧啊？这都指的是lO复用接口。

### 信号驱动IO

信号驱动IO，这是我们linux里边儿特有的一种IO模型，signal driven。

信号驱动，注意那我们大家学的都是基于linux这个高性能网络服务器，相当于后台开发嘛，对吧啊？

所以呢，对于基于linux的这个系统的开发的话，有一种IO模型叫做信号驱动，不要忘了啊。

### 异步IO

再加上异步IO，异步IO啊asynchronous。

大家再去看一些这个有关这个多线程啊，并发编程的一些呃英文原版描述的时候啊，注意这里边儿常用的。

ss开头的就是同步IO , asynchronous就是异步的意思好吧啊，这是常见的一些词汇。

### 结合图片分析

那也就是这五种。好了吧啊，这五种呢，我也分别呢，给大家画了一个图，我们打算结合着这个图呢，给大家把linux上的这种五种l模型。

再给大家讲解一遍啊，

那么讲解的过程中就应用到了我们之前一起学习的这些阻塞,非阻塞IO，同步啊，异步这些概念。



好吧啊，再加深一下，对我们之前学习的这些内容的一个深刻的一个理解，

顺便啊。那么把这个linux上的五种lO模型的知识给它搞定以后，

我们如果被问题啊，你是怎么理解linux的这个lO模型呢啊？

呃，你都知道linux上的哪些lO模型对吧啊？

你说一下能不能分别进行一个解释？

那在这里边儿有阻塞，非阻塞lO，复用，信号驱动以及异步lO

对不对啊？在讲这些每一种lO的时候，再结合我们上面讲的阻塞，非阻塞同步，异步。

我相信这样的回答。应该是非常不错的。好吧啊，面试官也是能够信服。



那在讲这个内容之前呢？

我在这里边儿写了一段儿话，就是把我们之前讲的这些内容呢，又总结了一下。

刚好是作为给我们讲linux的五种lO模型的一个铺垫啊，

大家再来跟我回顾一下。就是之前呢，我们已经讲过了这四个词汇是吧啊？

这四个词汇的这个意思啊，

在这里边儿我写了这么一段儿话，我们总结一下，就是我们被问到阻塞非阻塞，同步异步解释一下这些东西的时候呢？

啊，大家可以把这个我列的这个东西呢，作为一个参考啊，

### 回顾上节课的4个词汇的参考回答

那一个典型的网络lO接口调用啊，、

分为两个阶段，分别是数据就绪和数据读写。

数据就绪，数据准备指的就是一个意思，就是说呢，远端的数据有没有来，对吧？

有没有数据可读或者可写数据准备好了没这个阶段。



还有一个阶段就是啊，数据准备好了，是不是啊？

那我现在数据准备好了，我是读呢，还是写呢？就是数据读写的这么一个操作啊，

那么数据就绪阶段分为了阻塞和非阻塞的这么一个状态啊，

表现的结果就是如果是工作在这个lO，工作在阻塞状态的话，那就是当前线程就变成一个阻塞状态了。是不是啊？

或是如果你工作在非阻塞状态的话呢？就是调用这些lO接口是直接返回的。

那直接返回呃，你不管是阻塞还是非阻塞返回值，如果是负一的话，你就拿这个receive或者read来说。

返回值如果是负一的话，在TCP的这个客户端服务器的这个连接过程中表示网络中断了，对端中断了，

对吧啊？

如果返回是零，而且error number是eagain的话。那这表示什么？这就表示呢，是一个非阻塞的IO正常，是不是返回啊？因为没有数据啊，数据没有准备好，

而正常的返回，那我们一般在应用程序上就是在于循环继续调用这个工作，在非阻塞模式下的这个IO接口。



OK，那么同步跟异步表示什么呢？

之前的内容里边我给大家说过了啊，我们同步跟异步啊。

我们有两种，

一种说的是lO同步跟异步，

一种说的是业务就纯粹，我们实现业务应用程序上业务的同步跟异步。

虽然呢，他们这个说的这个角度不同啊，但是呢，其核心思想原理都是一样啊。

同步呢，就表示a向b发起一个请求，就是a给我们b的一个接口，对不对啊？接口呃，

或者说是呢呃，调用某个业务逻辑的API接口，

那b就是某个业务逻辑了。是不是啊？

那在这里边儿呢？如果站在lO同步跟异步的角度来看呢？

就是a就是某一个应用程序，b是不是就是操作系统啊？唉，应用程序向操作系统请求一个网络IO接口啊。

数据的读写如果都是由请求方a自己来完成的。

不管是阻塞还是非阻塞，阻塞是什么呢？就是说呢，数据没有读完啊，这个方法呢，是不会返回的。

是不是唉？这个方法返回以后呢？

表示呢a自己把数据也读完了。

或者是非阻塞诶，一会儿过来看一下数据好了没，一会儿过来看数据搬好了没，数据搬好了没啊？是不是啊？



那这就是我们所说的同步阻塞，在这边给大家写了啊，或者同步非阻塞。

好吧啊，同步阻塞或者同步非阻塞在这里边儿呢，

你看看我是分别调用receive。

receive的第一个参数是不是就是这个fd啊？第二个参数是buffer，第三个参数是一个buffer的长度，第四个参数是flag啊。

那我们说receive它本身就是一个同步的IO接口啊，在处理IO的时候，阻塞跟非阻塞都是同步的IO，

### 同步阻塞

对吧啊？这都是同步的。啊，

同步阻塞是什么意思呢？就是说呢，在调用这个接口的时候啊。

如果数据没有读完。啊，这个receive呢，是会让当前线程阻塞的，

什么时候返回呢？就是数据就绪了，而且呢，我们这个应用程序也是花时间把内核的TCP缓冲区的数据拷贝到

当前应用程序的buf里边，这就叫同步组件。算完成了，是不是啊？

### 同步非阻塞

而这个同步非阻塞呢？在这里边儿就是数据如果未就绪，它还是直接返回，它还是直接返回，

我们判断size=0，而且number=eagain。

对吧啊，就表示呢，它是由于工作在非阻塞模式下，但是数据还没有准备好啊，数据如果准备好了。

它还是应用程序，自己花时间把数据从内核的TCP缓冲区，拷贝到我们应用程序的buffer。

这个缓冲区里边，然后返回的这个size，就是我们读取的数据的一个大小字节数。

### 总结：同步阻塞和非阻塞的表现形式

对吧啊，这就是同步阻塞跟同步非阻塞它的两种表现，不同的表现形式啊，

我们在讲lO模型的时候呢，前两种阻塞这个指的就是同步阻塞啊,非阻塞指的就是同步非阻塞好吧啊，

这种lO模型指的就是同步非阻塞。

### 异步IO

那么，异步表示，我给大家说了异步一个非常大的特点，就是通知，通知是不是啊？

刚开始呢，有一个协商通知方式，比如说通过信号啊，通过回调操作啊，回调函数啊，是不是啊？

通过绑定器绑定一个回调函数。

C++里边啊，后边还有一个通知的过程，异步表示a向b请求调用一个网络IO接口或者是调用业务，

某个业务逻辑的API接口时啊，

向b传入了啊，请求的事件就是a想调用b的哪一个方法啊，我们在这里边把这个方法的调用。

啊，说成是抢修的一个事件，对吧？

以及呢啊，这个事件发生时。呃，就是这个数据处理完了，你要通知a的，这么一个方式哎。



a就可以处理其他逻辑了。对不对？

因为你a调用b的某一个方法，那个方法还得吭斥吭斥自己是不去做一些事情啊啊，

做完以后才会通知a的。

当b监调事件处理完成以后呢，会用事先约定好的通知方式信号啊，或者是回调函数啊，来通知a来处理结果。

对不对？也就是说呢，整个在等待数据是否就绪就绪了以后，数据读完成或者写完成这个过程呢？

耗不耗a的时间啊？==根本就不耗a的时间，完全是由b来做==。

==b做完以后只是通知a啊，我给你把该做的全部做完了。==



接下来就该你了，你看你后续该要怎么处理？这就是一个什么呀，这就是一个异步的处理过程。



那有些同学说了，那你在这里边儿写个异步阻塞跟异步非阻塞是什么意思呀？

### 异步非阻塞

同学们肯定听过异步非阻塞，对不对？

==我们nodejs就是一个非常典型的基于单线程的一个异步非阻塞的网络IO模型啊，性能非常不俗==，

### 异步非阻塞 不合理

有没有听过异步阻塞啊？听肯定是听过对不对？

但是你看看有没有哪些第三方的优秀库，采用的是异步阻塞模型啊？==没人采用这个的，因为这个它不合理，==



为什么不合理呢？你看啊，

异步本来就是a要调用b的一个方法啊，向b发起一个请求。

对吧，a我不仅告诉b，我要调用你哪个方法，

而且呢，告诉了你，你到时候做完以后通知我的方式。

那你说我把这些事情做完啦，我a是不是就可以在这儿处理其他逻辑了？a没有事情可做了嘛？

a也不用担心，到时候没人通知他，已经告诉b，到时候唉，方法调用完成以后呢？通知a的方式了a就完全可以。去做其他事情。

### a没有必要等待b做完事情

==a有没有必要在这儿阻塞等待b？做完事情执行完方法，然后给a通知啊啊，那a就在整个过程就死死的等待b给我通知，有没有必要啊啊？没有必要，你这是完全是不是在浪费啊？==



啊，你这完完全全在浪费线程a的能力啊。

啊线程a完全是有时间可以去处理其他逻辑的，你却让a阻塞在这里，所以异步阻塞没有必要。

异步不阻塞，没有必要。好的吧啊，

### 举例说明 异步阻塞

说说直白点就是a就是我b就是你啊，那我现在要调用你的一个能力。我让你给我办一件事情。是不是啊？给我办一件事情。你给我你办事情需要花时间吧啊，就像我a调用b里边的一个方法，

需要花时间的是不是啊？然后呢？我告诉你了，你把这个事情做完，你或者打电打电话通知我，或者是发邮件通知我啊，或者说是整个微信的这个视频通知我，是不是啊？

我随便了，我通知方式告诉你。

那然后我就死死的坐到这儿，啥也不干，就等着你给我打电话啊，

发信息。或者跟我视频。啊，那我就是闲的没事做了，我在这段时间里边啊，我完全可以做我自己的事情。对不对诶？我跑跑步，锻炼身体都行啊。

对吧啊，所以呢，我a没有必要在这里边阻塞住，所以异步阻塞有没有这种模型啊，在理论上是有这种模型的，但是呢。

这完全是在浪费时间啊a，不可能什么也不做，所以一般呢，异步都是跟非阻塞IO在一块儿工作的。



## 总结

没问题了吧啊，这就是同步阻塞，同步非阻塞啊，异步阻塞，异步非阻塞的这些模型。

好不好啊？我希望大家还是从理论上啊，还是从IO的这个表现形式上，业务的调用关系上来去理解这些东西。



网上有很多的一些例子啊，

比如说用一个烧开水啊，这这个例子呢？来给你讲这些啊，模型的不同，

或者用买机票啊的例子给你讲这些不同。

实际上呢，如果说是你实在是不能够理解啊，你可以去看一看那些通俗的例子，但是我非常不建议你以那些非常通俗的生活的例子来理解这些词语。因为你不可能在面试官跟前再去解释这些词语的时候。你再说你要买机票，你烧开水去解释这些东西。



对不对？那些东西的上那些解释上不了台面儿。好吧，那是不可能在面试官跟前说出来那个东西，只能私下帮你去理解一下，最终我们留在脑子里边儿了，



还是以非常专业的术语。专业的一个描述啊，去理解去表述他们的。

好吧，在这里边儿呢，相当于我又以一段儿总结的话语呢，给大家来阐述了一下，同步阻塞，同步非阻塞，

异步阻塞，异步非阻塞。这些内容啊，对于我们之前讲的内容，再做一次总结啊，辅助的一个理解啊，也为我们讲linux的五种lO模型啊。

啊，再做一个铺垫，希望大家呢，也好好的去总结一下，把我这儿给大家总结的这个内容呢，好好的去理解一下。好吧啊.

那这节课我们的这个内容就先说到这里。