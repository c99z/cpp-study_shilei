# Linux的五种IO模型详解

这节课我们来看linux的五种lO模型，

## 同步 阻塞 IO

第一个阻塞IO，在这里边儿画的这张图，大家仔细跟我看一下。

## 画图分析

### read系统调用接口

这张图呢啊，左边这个大括号啊，括出来的范围，这里边加了注释，这相当于就是进程啊，阻塞于read。进程阻塞于read，你也可以说线程好吧，在linux里边儿呢啊，这个进程跟线程啊，实际上在内核态是不做区分的，对吧啊啊，

进程你也可以理解成有一个主线程是不是啊？

在进程阻塞于read或者线程阻塞于read都行，

你调用了read或者receive啊，默认啊，这个你创建的这个socket啊，是一个默认的阻塞模式了。

并没有通过set sock fd把它设置成non block组塞嘛，对吧啊？

进程阻塞你看read右边儿呢，画两个这个大括号，

这分表示等待数据就是数据是否就绪，就是我们之前说的啊，

这第二个就是数据就绪好了啊，数据从内核空间。

这里边儿所说内核空间就是我们网络接收到远端发送过来数据的TCP缓冲区。是不是我们内核啊？

针对于sock fd，对应的就是一个socket对应的一个TCP接收缓冲区，拷贝到用户的空间，

就是用户指定的一个buffer空间里边。是不是啊？

### 应用程序一直阻塞  花费应用程序的时间

你看在这里边儿应用进程调用这个read系统调用啊，

这是一个系统lO是不是，系统一个lO接口儿？

内核数据未准备好，就是数据未就绪，

那在这个过程中呢？大家看到啊，

在应用程序看来，是一直阻塞住了啊，应用程序的这个进程或者线程是一直阻塞住。

对不对啊？数据准备好，也就是对端有数据过来TCP缓冲区上有数据可读了，是不是在这里边呢？receive。

啊，receive我们这个接口啊，就是read这个接口呢，我们在这里边。

这个接口儿会帮助我们把TCP内核缓冲区里边儿的数据拷贝到我们指定的这个buffer里边儿，

这整个数据是否就绪以及数据就绪好了以后，数据的这个从内核到用户空间的一个拷贝这整个过程呢，

应用程序这个时间都要在read上花费完，就是一直阻塞在这里。





okay吧，内核就是我们应用程序调用这个read的这个lO接口以后呢，

内核把这些东西呢做完以后。然后再唤醒当前的这个应用的这个线程啊，唤醒这个线程。

好吧啊，唤醒这个线程，你可以起来处理数据了。

唉，这就是一个同步的，一个阻塞lO。同步的阻塞lO。

就是我这个左边儿相当于就是a右边儿是不是相当于就是b哎

a就是应用程序b就是叫做系统内核对吧啊，

我调用这个read。你看这里边儿，除了传入这个一个buffer，

### 没有协商通知的方式

这里边儿有没有说是协商一种通知方式啊？没有吧啊，没有通知方式对不对？

应用进程一直把心思花在read上，一直就在等待read的返回结果。

OK吧，这就是一个同步的阻塞的IO模型，这种模型效率是不高，

但是编程呢，是非常简单的。

好吧，这是呢，阻塞的这么一个模式啊，那再来看一下这个非阻塞。



## 同步 非阻塞 IO

跟我们阻塞区别一下。

那非阻塞就是什么情况呢？

就是当我调用read之前呢？

### 设置非阻塞 set sockopt

我用这个set sockopt啊，这么一个系统的这个方法把我用socket函数创建了这个sock fd，给它设置成non blocking了。

啊，这个大家如果写过这些基本的网络编程代码的话呢，都知道是不是啊？

那么这个相当于跟上边儿来比较的话，

就是在数据未就绪状态下，它是不断的返回。

那不是说数据未准备好呢啊，就像阻塞一样啊，进程或者线程就一直阻塞住了，

对吧啊，这个是不断返回的，我们说我们可以通过返回值的判断。

啊，返回read的返回值size=0，而且errnum=eagain的话就表示这是一个正常的返回。就是内核的这个数据未就绪，

对不对啊？而如果数据准备好以后呢？

这个数据从内核TCP缓冲区拷贝到我应用程序的buffer这个缓冲区里边。

这个整个还是要花费谁的时间啊，花费应用程序的是不是时间啊？

哎，所以这个过程呢，它也是一个同步的一个操作。

好吧啊，这都是同步非阻塞模型。



## IO复用

再来看第三个是lO复用。

在这里边儿，大家看左边儿啊。还要复用左边儿这一块儿，

对应右边儿就是数据就绪啊，数据是否就绪，事件是否发生这么一个阶段。

进程阻塞与select PE等等待套接字变为可读。啊，

然后默认呢cli的proe pro都是它的参数都是可以设置time out超时间的啊，

你如果不设置它是默认工作在阻塞模式下的。

就是数据如果未准备好，在他们那边表示事件如果未发生的话啊，这个调用IO复用接口的这个进程或线程也是阻塞住的，对吧？

当然你可以给这些接口最后一个参数time out，是不是设置这个超时时间啊？

设置超时时间的话，这个相当于就跟上边一样。

它也可以公布在这个非阻塞模式下，对不对？

一秒超时，我们同样的去检测返回值。

以及lnumber=eagain表示它是错误返回呢，还是一个正常的啊？

非阻塞的是不是一个返回啊？这也发起系统这个调用了啊哦，默认是阻塞状态，

你看数据未准备好。这个应用进程或者应用线程是不是一直也是阻塞的状态啊？哎，如果数据准备好了，以后数据准备好了，以后你看这返回可读的fd。

相当于就是我们拿epoll来说吧，就是返回所有的发生事件的，是不是这些event啦？

唉，我们呢？接下来。在对发生事件的疑问，他啊，调用accept呀，调用read呀，

那就是看是有新连接到来呢，还是已建立连接的读写事件到来了？是不是啊？

那假如说在这里边举举了一个已建立连接的socket的读写事件，读事件发起系统调用。整个的这个数据从内核TCP缓冲区拷贝到应用程序的buffer缓冲区里边儿，这是还是要耗费应用程序的时间的。是不是？

所以它也是一个同步的过程。OK吧啊诶，我们lO复用接口调用的时候，它也是一个同步的过程。是不是哎，

整个的数据未就绪到数据就绪的整个的过程都是要花费这些lO复用机构的时间的嘛。

所以呢，epoll它是同步IO接口select poll epoll都是同步IO接口，记住了，不要把它说成异步IO。



啊，异步有一个非常重要的特点，就是开始操作的时候呢，会绑会协商一个通知方式。

事件发生以后，会用事先约定好的通知方式进行通知的，这里边儿并没有。

OK了吧啊，大家注意一下，

这是lO复用。

## IO复用与上述同步阻塞和非阻塞IO的区别

那实际上，如果在面试中啊，你在解释这个五种l模型的时候，你能在当场用笔呢，在纸上能大概画出来这种模型，我觉得效果呢是相当不错的啊。

那这个lO复用呢？跟上边儿的区别是什么区别？

就是它把左边儿应用程序的这一大段儿，我们就只调用了一个read。换成了啊，

数据从未就绪到就绪，我们调用了什么？

调用了lO复用接口啊，数据就绪以后呢，我们根据具体的发生事件的fd进行相应的lO接口的是不是调用啊？

那它的好处就是==一个线程里边儿，一个线程调用一个lO复用接口，它可以监听很多很多的一个socket的节字啊==，

不像我们上边的阻塞跟非阻塞，你看这应用进程或者应用线程，一个应用进程，一个应用线程。是不是一次只能去处理一个socket呀？

这太浪费进程或者线程了，



当我们去设计的高性能服务器的时候呢？

我们如果说一个线程只能去处理一个socket。

那我们作为服务器，在处理高并发的这个客户端连接的时候呢，那相当于就是来一个客户端，我服务器就要分配一个线程是不来处理这个客户端了，

那如果我的这个服务器的这个吞吐量要求每秒处理十万个。

那也就是说我这服务器一秒钟要开十万个线程，是不是来处理十万个并发客户的这个请求啊？

那这个服务器的压力太大了。啊，



线程数量不是越多越好，对不对？唉，线程数量的这个创建呢？

对操作系统的压力呢？是非常大的，所以我们不会创建那么多线程，所以lO复用为什么是我们在设计高性能服务器的时候呢？是必不可少的系统接口。

对吧？因为它最大的特点就是它调用的时候呢，在监听我们这些socket是否发生事件，是否有数据可操作可读或者可写？

是不是它一个lO复用可以监听多个套件字？

唉，==当多个套件字呢，有数据操作的话就是数据可读或者可写的话啊，lO复用呢，会给我们应用程序返回可读或者可写的这些socket的一个列表==。

OK吧唉，==我们接下来再根据人家lO复用返回的可读写事件的，这些fd呢，进行相应的一个读写操作==。

好吧啊，这都是一个同步的操作好不好？这都是同步的操作，电动l复用接口的过程是一个同步的。

是不是啊？那么这个读写数据的过程呢？

更是一个同步的啊，这都是同步的处理。



## 信号驱动

信号驱动。信号驱动在这里边儿，大家看一下signal drive。

这是我们Linux系统特有的。

大家看在这个等待数据就数据未就绪到数据就绪的这个过程中。

我们应用这个应用进程或者应用线程是在干什么？

阻塞呢，还是在继续执行呢诶，它继续时间。他是继续执行，他完全被放飞自我了。

啊，他可以看他任何想干的事情。

而不用像前边一样。是不是在这死等内核呢？

呃，数据从未就绪到就绪，因为没人，因为如果数据就绪没人通知他呀。

没人通知应用程序啊，他们也没有事先协商通知方式啊，

所以他就得在这里边儿，要么是阻塞在这里等待数据就去好，要么是不断的扭头看数据，准备好没数据，



我说的这两种情况刚好是对应于它的阻塞模式和非阻塞模式啊啊，

当然整个过程都是同步的。好不好？没人给他通知啊，他得自己去操心数据就绪好了没？



但是呢，信号驱动，你看唉它调用相应的这个方法。注册c个l星号处理程序。哎，这个就相当于就是干嘛呢？

协商了一个通知的方式啊，系统调用调用这个c个action啊，

注册了一下c个l星号的信号。它的这个处理程序相当于就是回调操作嘛，对不对啊？

然后呢？你看a向b注册以后。唉b，就直接给a返回了好了，我知道了啊，就是说呢，你对什么感事件感兴趣是不是啊？然后呢？这个事件发生以后呢？我该以什么样的方式来通知你这两件事情，

你都告诉内核了，内核给你回，我知道了，

你现在可以放飞自我了，你想做什么事情你？自己去做吧啊，

到时候呢，你感兴趣的事件发生了，

我以我们协商好的通知的方式来通知你就行了，你到时候再继续处理事情吧。



诶，这个过程你看这个现在等待数据的这个过程。

在信号驱动这个模型下，就是一个什么的过程啊，同步还是异步的过程，

==各位。那就是一个异步的过程了。==



应用进程或者应用线程没在这里边儿死等内核的数据呢？

是否从未就绪到就绪？

这个应用这个进程应用线程在这里边儿做完，我们刚才描述的事情以后，它完全就可以释放在这个释放这里边的逻辑啊，

然后呢，去做任何其他可以做的事情。然后内核呢，这里块儿的数据从未就绪到就绪的话呢，

它会通过信号啊，也就是通知，也就是调用我们事先注册好的一个信号的回调函数，信号的处理程序，那我这个应用程序就知道了有事件发生了，是不是啊？

有事件发生了，就是数据可读了，我这有socket可读了，或者socket可写了。

啊，但是这个读的过程或者写的过程。在读的过程，写的过程，

这个应用程序能不能放飞自我啊？放飞不了，它还是得调用read来去读这个数据。

==也就是说呢，数据从内核的TCP缓冲区里边儿拷贝到用户的buffer缓冲区里边儿，==

==这个过程呢？要花费应用程序的时间，应用程序必须自己操心，这件事情数据搬完了以后呢？应用程序才能继续往下执行。==

==那么，这个过程呢？是一个什么的过程啊？哎，是一个同步的过程。==



内核在第一个阶段是什么啊？是异步。很明显，在这里边儿有一个注册感兴趣的事件。

啊ch在这里边呢，要注册一个对应的fd以及fd的一个。信号儿处理程序对吧啊？

在第二个阶段是同步的。与非阻塞lO的区别。这个是不是鱼啊？是雨。与非阻塞l的区别在于，它提供了消息通知机制，不需要用户进程啊。

这个不断的，这个轮巡检查啊，这里边说的不断的轮巡检查就是。

像这里边的这个非阻塞模式。

或者说是像我们bol复用这里边儿设置timer的参数，让它不断的是不是超时返回，超时返回呀？

就跟这里边儿的。啊，不用用户进程不断的轮询检查，减少了系统API调用次数，

这什么意思呢？就是如果你是非阻塞，或者说是设置了time out的f复用的话。啊，

当你检查返回值等于0 lnumber=1 again的时候，

你是不是会再次调用read或者是lO复用接口来检测来查看内核的数据。

是否从未就绪到就绪了？对了吧啊，那你就得不断的去调用这些l接口嘛，调用一次lO接口发起一次lO这个系统的API调用。这都是要耗时间的。

是不是而这里边儿减少了系统调用的次数，所以它提高了效率，这是信号驱动。

## 信号驱动总结 异步与同步结合

啊，信号驱动。那在第一阶段呢，它完全是一个异步的过程。

啊，因为应用程序在这里边儿。彻底被释放了，它不用死等。

这个状态也不用不断的这个轮巡这个状态，

唉，这两个我刚才说的那两个过程就是对应于阻塞跟非阻塞嘛，是不是啊？

但是呢，如果说数据socket的上数据可读写可读或者可写的话呢，那么具体的。数据的读过程或者写过程啊，那还是我们应用程序自己做的，这是一同步的过程。



## 异步IO

OK，最后再说一个，我们的这个异步的IO啊，异步的IO，

我们说异步非阻塞IO是最典型的啊，

而之前我们都已经说了linux也给我们提供了aio read跟aio right这两个内核的这个API来让我们实现真真正正的基于操作系统级别的一个异步非阻塞的一个网络编程对吧啊？

### 异步 不耗费应用程序的时间

那么这你看整个过程，你看跟信号驱动不同的是跟上面的不同的是整个的过程啊，应用程序诶，从这个数据就绪，还有数据的这个拷贝，整个过程耗不耗费应用程序的时间啊？

不耗费应用程序和应用进程或者应用线程呢，是继续执行的，就是它完全可以去做其他事情，它只要事先通过aio read来告诉内核。

呃。我应用程序的一个缓冲区。啊，你通知我的一个信号。对不对啊？

以及呢？我对哪个socket感兴趣？

你告诉内核，然后你就可以啊，内核给你返回一下好的，我知道了啊，我到时候就按我按你告诉我的这个事件我去监听，相应的事件发生，并通知你是不是，然后应用程序就可以自己去做其他事情了，非常高效嘛，对不对啊？

当他所感兴趣的这些事件没有发生数据，没有读写完成的时候呢？

应用程序是完全可以自己去处理其他事情，对吧？

更好利用了CPU的这个时间片嘛。对吧啊。



你要不要让CPU闲下来嘛？

你看内核，如果在这里边儿就是我们调用aio read啊，在这里边儿内核看数据就绪啦。数据就绪，别打扰应用程序啊啊，因为咱们是工作在异步这个模式下的啊，

内核呢，再把这个数据呢拷贝到最开始，通过alv的指定的这个用户空间的这个buffer缓冲区里边。拷贝完了以后怎么样？

内核通过事先约定好的方式信号啊，注册的信号通知通知应用程序。

好了，你别忙活其他的了，你该处理这个了啊，你最开始告诉你对这个socket。

是不是它的读写事件感兴趣呀啊？然后呢？它现在读写事件已经发生了啊。该读该写的数据我已经给你放到这里边儿了啊，现在我用这个信号儿把你通知起来了，你就去做。相应的处理吧。



也就是说，在整个过程中啊。啊，根本不耗应用程序，任何的时间应用程序完全可以处理其他逻辑，当应用程序得到通知以后呢？这里边儿数据都已经读写完成了。而像这个这数据的读写是不靠应用程序自己来做的。这都是自己来做的，数据的读写都是应用程序，自己来做的需要耗费应用程序的时间。



唉，这就是异步IO最大不同。

哎，它的效率也是非常不错的好吧，当然它的编程也是最复杂的，出问题也是最难排除的。

鱼与熊掌不可兼得嘛，是不是啊？你要得到更高的效率，那就意味着更复杂的一些设计，对吧？出问题不好排查。啊，设计简单啊，往往来说呢啊，往往来说它在性能上呢，就会有一些欠缺啊。

这个事物都是平衡的啊，你有一长肯定有一短，是不是啊？

大家也不必太过于纠结，也不是说呢，那我有这个高铁的异步，我就不用同步了，是不是？

那并不是这样的啊，并不是这样的依据，特有的场景选择不同的，这个工作方式。

好吧啊。啊，典型的异步非阻塞状态node gs采用的。

这个网络IO模型就是异步非阻塞IO。

那么，在这里边儿呢？我们给大家就把linux上的五种l模型就讲解完了啊，在这五种模型里边儿，我们给大家着重的讲解了一下，左边代表我们应用程序，它的一个状态啊，是阻塞呢还是非阻塞呢是？死等内核给返回呢，还是不断的轮巡内核，现在当前的处理情况呢，还是完全的在这里边儿放飞自我去处理其他逻辑，等待内核通知呢？

是不是啊？那么这是我们左边画的大括号的意义那么这画成两段儿啊，

为什么画成两段儿这两段儿分别就代表我之前给大家说的网络l的两个阶段数据就绪跟数据读？撇那么在这里边儿，我觉得我们写成数据就绪。

啊，那就写成准备吧啊。我不改了，大家呃注意我们在说的时候，这都是一个意思好不好啊？这都是一个意思，我课件儿就不不做太大的改动了啊。

要不然大家到时候看这个视频学习的时候呢，我们视频上看的跟自己课件上总有出入不太好啊。

## 结尾

好了，那么这一节课的主要内容就说到这里了啊，

希望大家呢，通过我这一节课的讲解，跟我们前边儿所学的这些阻塞非阻塞，同步异步的内容。能够真真正正的把linux的五种l模型掌握清楚了。

好的吧啊，那你学完这个能不能说是合上我们的这个视频啊？合上我们的课件。

自己呢，用笔在纸上画一画，这五种阻塞同步阻塞同步非阻塞还要复用啊。这是lO复用以及信号驱动以及异步的它们的这么一个应用程序跟内核的这么一个交互的流程呢？

啊，如果简单能画出来并能。唉，叙述一下他们的这个处理过程啊，

并合理的在叙述过程中去。描述阻塞非阻塞，同步异步这些状态词语，

那么我觉得你对于这一部分的掌握。那就相当不错。

好，那我们这节课的内容就给大家说到这里。