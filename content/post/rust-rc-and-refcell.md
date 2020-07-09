+++
title = "Rust中的Rc和RefCell"
lastmod = 2019-12-14T01:12:54+08:00
tags = ["rust"]
categories = ["rust"]
draft = false
author = "Sen Na"
+++

最近看了一点[too-many-lists](https://rust-unofficial.github.io/too-many-lists/)，感觉Rust好难啊。。

其中在实现双向链表时用到了Rc<RefCell<T>>这样的结构，经过大量的搜索，终于知道了为什么这样的结构是必要的。

在Rust中，规定了一个object只能有一个owner，可以有多个对这个object的不可变借用或者只有一个可变借用，同时这些借用也要满足Rust的生命周期规则，防止dangling
reference的出现。

这样的约束固然是不错的，但是有些时候还是显得束手束脚。而Rc<T>可以提供共享的
ownership，这样可以让多个变量同时指向一块内存（但是和引用不同，引用并不给予
ownership），当没有任何变量指向这块内存时，把相应的资源释放即可。这其实就是“引用计数”了，也是objective-c和swift默认的内存管理方式，当然这种内存管理方式也是有一定问题的，比如“环引用”。

虽然Rc<T>提供了共享资源的方式，但是根据Rust的引用规则，如果一个资源有多个引用，这些引用是不能改变里面的T的。

{{< highlight rust >}}
let x = Rc::new(5i32);
let y = x.clone();
{{< /highlight >}}

在上面的代码中，x和y都是没有任何办法改变那个5i32的。

那只好把Rc<T>中的T再用RefCell包裹一下。

{{< highlight rust >}}
let x = Rc::new(RefCell::new(5i32));
let y = x.clone();

let val = x.borrow_mut();
*val = 50;
{{< /highlight >}}

这样，Rc共享的东西实际上变成了RefCell<T>，而RefCell可以对外面提供类似于&T和&mut
T的Ref<T>和RefMut<T>，RefCell并不要求自己是mut就可以改变内部的值（这是通过内部的unsafe实现的），这就是所谓的"interior mutability"。
