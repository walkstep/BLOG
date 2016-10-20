---
title: GCD常用用法
date: 2016-10-20 11:21:10
tags: [iOS, GCD]
categories: iOS
---
　　看苹果的官方文档就知道GCD的用法还是蛮多的，但我们常用的其实并不多，在这里，只列出我们常用的一些用法。
***
#### 什么是GCD
　　Grand Central Dispatch (GCD)是Apple开发的一个多核编程的较新的解决方法。它主要用于优化应用程序以支持多核处理器以及其他对称多处理系统。(更为详细的介绍可以google一下)<!--more-->
#### 任务和队列概念
　　任务：执行某段代码所做的事
　　队列：用来存放任务的线性表
　　
　　**任务分为：同步执行和异步执行**
　　同步执行：一个功能调用时，在没有得到结果之前，该调用就不会返回。也就是一件一件事做，做完上一件事才能做下一件事。
　　异步执行：一个功能异步调用时，该调用不会立即返回结果，会开启一个新的线程去执行，当得到结果时，会通过状态、通知或回调来通知调用者。在同一个时间可以做多件事情，并不需要等待。
　　
　　**队列分为：串行队列和并行队列**
　　串行队列：一个任务执行完成以后才能执行下一个任务。
　　并行队列：多个任务可以同时执行。
#### GCD队列、任务类型及创建
　　GCD队列分为三种：main queue(主队列)、global queue(全局队列)和create queue(用户队列)
　　
　　**main queue(串行队列)**

```
dispatch_queue_t mainQueue = dispatch_get_main_queue();
```
　　**global queue(并行队列)**

```
dispatch_queue_t globalQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);

// 参数1 优先级
DISPATCH_QUEUE_PRIORITY_DEFAULT    默认的
DISPATCH_QUEUE_PRIORITY_HIGH       优先级高
DISPATCH_QUEUE_PRIORITY_LOW        优先级低
// 参数2 系统保留的参数0
```
　　**create queue(可以创建串行队列或者并行队列)**

```
// 创建串行队列(两种方法)
dispatch_queue_t newQueue = dispatch_queue_create("com.dispatch.newQueue", NULL);
**or**
dispatch_queue_t newQueue = dispatch_queue_create("com.dispatch.newQueue", DISPATCH_QUEUE_SERIAL);

// 创建并行队列
dispatch_queue_t newQueue = dispatch_queue_create("com.dispatch.newQueue", DISPATCH_QUEUE_CONCURRENT);
```
　　任务分为两种：dispatch_sync(同步执行)、dispathc_async(异步执行）
　　**dispatch_sync(同步执行)**

```
// 创建同步执行任务
dispatch_sync(queue, ^{
	// 操作代码
});
```
　　**dispatch_async(异步执行)**

```
// 创建同步执行任务
dispatch_async(queue, ^{
	// 操作代码
});
```
#### 用法示例
##### 主队列 + 同步执行
```
- (void)mainSync {
    dispatch_queue_t mainQueue = dispatch_get_main_queue();
    
    NSLog(@"----begin");
    
    for (int i = 0; i < 10; i++) {
        dispatch_sync(mainQueue, ^{
            NSLog(@"%@-------%d", [NSThread currentThread], i);
        });
    }
    
    NSLog(@"----end");
}
```
运行结果：

```
2016-10-20 16:11:27.831 GCDDemo[20258:626929] ----begin
```
##### 主队列 + 异步执行
```
- (void)mainAsync {
    dispatch_queue_t mainQueue = dispatch_get_main_queue();
    
    NSLog(@"----begin");
    
    for (int i = 0; i < 10; i++) {
        dispatch_async(mainQueue, ^{
            NSLog(@"%@-------%d", [NSThread currentThread], i);
        });
    }
    
    NSLog(@"----end");
}

```
运行结果：

```
2016-10-20 16:12:03.295 GCDDemo[20283:627561] ----begin
2016-10-20 16:12:03.295 GCDDemo[20283:627561] ----end
2016-10-20 16:12:03.300 GCDDemo[20283:627561] <NSThread: 0x7ff9d1507de0>{number = 1, name = main}-------0
2016-10-20 16:12:03.301 GCDDemo[20283:627561] <NSThread: 0x7ff9d1507de0>{number = 1, name = main}-------1
2016-10-20 16:12:03.301 GCDDemo[20283:627561] <NSThread: 0x7ff9d1507de0>{number = 1, name = main}-------2
2016-10-20 16:12:03.302 GCDDemo[20283:627561] <NSThread: 0x7ff9d1507de0>{number = 1, name = main}-------3
2016-10-20 16:12:03.302 GCDDemo[20283:627561] <NSThread: 0x7ff9d1507de0>{number = 1, name = main}-------4
2016-10-20 16:12:03.303 GCDDemo[20283:627561] <NSThread: 0x7ff9d1507de0>{number = 1, name = main}-------5
2016-10-20 16:12:03.303 GCDDemo[20283:627561] <NSThread: 0x7ff9d1507de0>{number = 1, name = main}-------6
2016-10-20 16:12:03.303 GCDDemo[20283:627561] <NSThread: 0x7ff9d1507de0>{number = 1, name = main}-------7
2016-10-20 16:12:03.304 GCDDemo[20283:627561] <NSThread: 0x7ff9d1507de0>{number = 1, name = main}-------8
2016-10-20 16:12:03.304 GCDDemo[20283:627561] <NSThread: 0x7ff9d1507de0>{number = 1, name = main}-------9
```
##### 全局队列 + 同步执行
```
- (void)globalSync {
    dispatch_queue_t globalQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    
    NSLog(@"----begin");
    
    for (int i = 0; i < 10; i++) {
        dispatch_sync(globalQueue, ^{
            NSLog(@"%@-------%d", [NSThread currentThread], i);
        });
    }
    
    NSLog(@"----end");
}
```
运行结果：

```
2016-10-20 16:12:44.735 GCDDemo[20313:628185] ----begin
2016-10-20 16:12:44.736 GCDDemo[20313:628185] <NSThread: 0x7fa02ed039b0>{number = 1, name = main}-------0
2016-10-20 16:12:44.736 GCDDemo[20313:628185] <NSThread: 0x7fa02ed039b0>{number = 1, name = main}-------1
2016-10-20 16:12:44.736 GCDDemo[20313:628185] <NSThread: 0x7fa02ed039b0>{number = 1, name = main}-------2
2016-10-20 16:12:44.736 GCDDemo[20313:628185] <NSThread: 0x7fa02ed039b0>{number = 1, name = main}-------3
2016-10-20 16:12:44.737 GCDDemo[20313:628185] <NSThread: 0x7fa02ed039b0>{number = 1, name = main}-------4
2016-10-20 16:12:44.737 GCDDemo[20313:628185] <NSThread: 0x7fa02ed039b0>{number = 1, name = main}-------5
2016-10-20 16:12:44.737 GCDDemo[20313:628185] <NSThread: 0x7fa02ed039b0>{number = 1, name = main}-------6
2016-10-20 16:12:44.737 GCDDemo[20313:628185] <NSThread: 0x7fa02ed039b0>{number = 1, name = main}-------7
2016-10-20 16:12:44.737 GCDDemo[20313:628185] <NSThread: 0x7fa02ed039b0>{number = 1, name = main}-------8
2016-10-20 16:12:44.737 GCDDemo[20313:628185] <NSThread: 0x7fa02ed039b0>{number = 1, name = main}-------9
2016-10-20 16:12:44.737 GCDDemo[20313:628185] ----end
```
##### 全局队列 + 异步执行
```
- (void)globalAsync {
    dispatch_queue_t globalQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    
    NSLog(@"----begin");
    
    for (int i = 0; i < 10; i++) {
        dispatch_async(globalQueue, ^{
            NSLog(@"%@-------%d", [NSThread currentThread], i);
        });
    }
    
    NSLog(@"----end");
}
```
运行结果：

```
2016-10-20 16:13:06.518 GCDDemo[20330:628521] ----begin
2016-10-20 16:13:06.518 GCDDemo[20330:628521] ----end
2016-10-20 16:13:06.518 GCDDemo[20330:628552] <NSThread: 0x7fcd5ce1b970>{number = 2, name = (null)}-------0
2016-10-20 16:13:06.518 GCDDemo[20330:628553] <NSThread: 0x7fcd5cc33530>{number = 5, name = (null)}-------2
2016-10-20 16:13:06.518 GCDDemo[20330:628551] <NSThread: 0x7fcd5ce1b6f0>{number = 3, name = (null)}-------1
2016-10-20 16:13:06.519 GCDDemo[20330:628552] <NSThread: 0x7fcd5ce1b970>{number = 2, name = (null)}-------4
2016-10-20 16:13:06.519 GCDDemo[20330:628554] <NSThread: 0x7fcd5cd03e60>{number = 4, name = (null)}-------3
2016-10-20 16:13:06.519 GCDDemo[20330:628553] <NSThread: 0x7fcd5cc33530>{number = 5, name = (null)}-------5
2016-10-20 16:13:06.519 GCDDemo[20330:628555] <NSThread: 0x7fcd5cf0bd90>{number = 6, name = (null)}-------6
2016-10-20 16:13:06.519 GCDDemo[20330:628551] <NSThread: 0x7fcd5ce1b6f0>{number = 3, name = (null)}-------7
2016-10-20 16:13:06.519 GCDDemo[20330:628556] <NSThread: 0x7fcd5ce068e0>{number = 7, name = (null)}-------8
2016-10-20 16:13:06.519 GCDDemo[20330:628552] <NSThread: 0x7fcd5ce1b970>{number = 2, name = (null)}-------9
```
##### 用户队列(串行) + 同步执行
```
- (void)serialSync {
    dispatch_queue_t newQueue = dispatch_queue_create("com.dispatch.newQueue", DISPATCH_QUEUE_SERIAL);
    
    NSLog(@"----begin");
    
    for (int i = 0; i < 10; i++) {
        dispatch_sync(newQueue, ^{
            NSLog(@"%@-------%d", [NSThread currentThread], i);
        });
    }
    
    NSLog(@"----end");
}
```
运行结果：

```
2016-10-20 16:13:40.965 GCDDemo[20353:629062] ----begin
2016-10-20 16:13:40.966 GCDDemo[20353:629062] <NSThread: 0x7f83b9408500>{number = 1, name = main}-------0
2016-10-20 16:13:40.966 GCDDemo[20353:629062] <NSThread: 0x7f83b9408500>{number = 1, name = main}-------1
2016-10-20 16:13:40.966 GCDDemo[20353:629062] <NSThread: 0x7f83b9408500>{number = 1, name = main}-------2
2016-10-20 16:13:40.967 GCDDemo[20353:629062] <NSThread: 0x7f83b9408500>{number = 1, name = main}-------3
2016-10-20 16:13:40.967 GCDDemo[20353:629062] <NSThread: 0x7f83b9408500>{number = 1, name = main}-------4
2016-10-20 16:13:40.967 GCDDemo[20353:629062] <NSThread: 0x7f83b9408500>{number = 1, name = main}-------5
2016-10-20 16:13:40.967 GCDDemo[20353:629062] <NSThread: 0x7f83b9408500>{number = 1, name = main}-------6
2016-10-20 16:13:40.967 GCDDemo[20353:629062] <NSThread: 0x7f83b9408500>{number = 1, name = main}-------7
2016-10-20 16:13:40.967 GCDDemo[20353:629062] <NSThread: 0x7f83b9408500>{number = 1, name = main}-------8
2016-10-20 16:13:40.968 GCDDemo[20353:629062] <NSThread: 0x7f83b9408500>{number = 1, name = main}-------9
2016-10-20 16:13:40.968 GCDDemo[20353:629062] ----end
```
##### 用户队列(串行) + 异步执行
```
- (void)serialAsync {
    dispatch_queue_t newQueue = dispatch_queue_create("com.dispatch.newQueue", DISPATCH_QUEUE_SERIAL);
    
    NSLog(@"----begin");
    
    for (int i = 0; i < 10; i++) {
        dispatch_async(newQueue, ^{
            NSLog(@"%@-------%d", [NSThread currentThread], i);
        });
    }
    
    NSLog(@"----end");
}
```
运行结果：

```
2016-10-20 16:14:13.925 GCDDemo[20376:629631] ----begin
2016-10-20 16:14:13.926 GCDDemo[20376:629631] ----end
2016-10-20 16:14:13.926 GCDDemo[20376:629660] <NSThread: 0x7f9b5ff09e60>{number = 2, name = (null)}-------0
2016-10-20 16:14:13.926 GCDDemo[20376:629660] <NSThread: 0x7f9b5ff09e60>{number = 2, name = (null)}-------1
2016-10-20 16:14:13.927 GCDDemo[20376:629660] <NSThread: 0x7f9b5ff09e60>{number = 2, name = (null)}-------2
2016-10-20 16:14:13.927 GCDDemo[20376:629660] <NSThread: 0x7f9b5ff09e60>{number = 2, name = (null)}-------3
2016-10-20 16:14:13.927 GCDDemo[20376:629660] <NSThread: 0x7f9b5ff09e60>{number = 2, name = (null)}-------4
2016-10-20 16:14:13.927 GCDDemo[20376:629660] <NSThread: 0x7f9b5ff09e60>{number = 2, name = (null)}-------5
2016-10-20 16:14:13.927 GCDDemo[20376:629660] <NSThread: 0x7f9b5ff09e60>{number = 2, name = (null)}-------6
2016-10-20 16:14:13.927 GCDDemo[20376:629660] <NSThread: 0x7f9b5ff09e60>{number = 2, name = (null)}-------7
2016-10-20 16:14:13.928 GCDDemo[20376:629660] <NSThread: 0x7f9b5ff09e60>{number = 2, name = (null)}-------8
2016-10-20 16:14:13.928 GCDDemo[20376:629660] <NSThread: 0x7f9b5ff09e60>{number = 2, name = (null)}-------9
```
##### 用户队列(并行) + 同步执行
```
- (void)concurrentSync {
    dispatch_queue_t newQueue = dispatch_queue_create("com.dispatch.newQueue", DISPATCH_QUEUE_CONCURRENT);
    
    NSLog(@"----begin");
    
    for (int i = 0; i < 10; i++) {
        dispatch_sync(newQueue, ^{
            NSLog(@"%@-------%d", [NSThread currentThread], i);
        });
    }
    
    NSLog(@"----end");
}
```
运行结果：

```
2016-10-20 16:14:40.857 GCDDemo[20400:630101] ----begin
2016-10-20 16:14:40.857 GCDDemo[20400:630101] <NSThread: 0x7fbea8d04530>{number = 1, name = main}-------0
2016-10-20 16:14:40.857 GCDDemo[20400:630101] <NSThread: 0x7fbea8d04530>{number = 1, name = main}-------1
2016-10-20 16:14:40.857 GCDDemo[20400:630101] <NSThread: 0x7fbea8d04530>{number = 1, name = main}-------2
2016-10-20 16:14:40.858 GCDDemo[20400:630101] <NSThread: 0x7fbea8d04530>{number = 1, name = main}-------3
2016-10-20 16:14:40.858 GCDDemo[20400:630101] <NSThread: 0x7fbea8d04530>{number = 1, name = main}-------4
2016-10-20 16:14:40.858 GCDDemo[20400:630101] <NSThread: 0x7fbea8d04530>{number = 1, name = main}-------5
2016-10-20 16:14:40.858 GCDDemo[20400:630101] <NSThread: 0x7fbea8d04530>{number = 1, name = main}-------6
2016-10-20 16:14:40.858 GCDDemo[20400:630101] <NSThread: 0x7fbea8d04530>{number = 1, name = main}-------7
2016-10-20 16:14:40.858 GCDDemo[20400:630101] <NSThread: 0x7fbea8d04530>{number = 1, name = main}-------8
2016-10-20 16:14:40.858 GCDDemo[20400:630101] <NSThread: 0x7fbea8d04530>{number = 1, name = main}-------9
2016-10-20 16:14:40.913 GCDDemo[20400:630101] ----end
```
##### 用户队列(并行) + 异步执行
```
- (void)concurrentAsync {
    dispatch_queue_t newQueue = dispatch_queue_create("com.dispatch.newQueue", DISPATCH_QUEUE_CONCURRENT);
    
    NSLog(@"----begin");
    
    for (int i = 0; i < 10; i++) {
        dispatch_async(newQueue, ^{
            NSLog(@"%@-------%d", [NSThread currentThread], i);
        });
    }
    
    NSLog(@"----end");
}
```
运行结果：

```
2016-10-20 16:15:36.636 GCDDemo[20452:631061] ----begin
2016-10-20 16:15:36.636 GCDDemo[20452:631061] ----end
2016-10-20 16:15:36.637 GCDDemo[20452:631088] <NSThread: 0x7fa27e709460>{number = 2, name = (null)}-------0
2016-10-20 16:15:36.637 GCDDemo[20452:631089] <NSThread: 0x7fa27e508ce0>{number = 3, name = (null)}-------1
2016-10-20 16:15:36.637 GCDDemo[20452:631091] <NSThread: 0x7fa27e61d140>{number = 5, name = (null)}-------3
2016-10-20 16:15:36.637 GCDDemo[20452:631090] <NSThread: 0x7fa27e635340>{number = 4, name = (null)}-------2
2016-10-20 16:15:36.637 GCDDemo[20452:631088] <NSThread: 0x7fa27e709460>{number = 2, name = (null)}-------4
2016-10-20 16:15:36.637 GCDDemo[20452:631089] <NSThread: 0x7fa27e508ce0>{number = 3, name = (null)}-------5
2016-10-20 16:15:36.637 GCDDemo[20452:631091] <NSThread: 0x7fa27e61d140>{number = 5, name = (null)}-------6
2016-10-20 16:15:36.637 GCDDemo[20452:631092] <NSThread: 0x7fa27e70b7c0>{number = 6, name = (null)}-------8
2016-10-20 16:15:36.637 GCDDemo[20452:631090] <NSThread: 0x7fa27e635340>{number = 4, name = (null)}-------7
2016-10-20 16:15:36.638 GCDDemo[20452:631088] <NSThread: 0x7fa27e709460>{number = 2, name = (null)}-------9
```
##### dispatch_group
　　有时我们有多个任务需要执行，当所有的任务执行完成后，然后再去做其他的事，这是dispatch_group则显得格外的方便，我们可以在dispath_group里异步执行任务

```
- (void)group {
    // 创建一个组
    dispatch_group_t group = dispatch_group_create();
    
    // 创建一个队列，该队列决定组是并行还是串行
    dispatch_queue_t queue = dispatch_queue_create("com.queue.newQueue", DISPATCH_QUEUE_CONCURRENT);
    
    // 耗时的线程1
    dispatch_group_async(group, queue, ^{
        
        int k = 5;
        
        while (k--) {
            
            NSLog(@"k1: %d", k);
            [NSThread sleepForTimeInterval:1];
        }
    });
    
    // 耗时的线程2
    dispatch_group_async(group, queue, ^{
        
        int k = 5;
        
        while (k--) {
            
            NSLog(@"k2: %d", k);
            [NSThread sleepForTimeInterval:1];
        }
    });
    
    // 当组中所有的任务都执行完后得到一个通知
    dispatch_group_notify(group, queue, ^{
        
        NSLog(@"执行完成");
    });
}
```

运行结果：

```
2016-10-20 16:36:41.591 GCDDemo[20563:639355] k1: 4
2016-10-20 16:36:41.591 GCDDemo[20563:639353] k2: 4
2016-10-20 16:36:42.664 GCDDemo[20563:639353] k2: 3
2016-10-20 16:36:42.664 GCDDemo[20563:639355] k1: 3
2016-10-20 16:36:43.731 GCDDemo[20563:639355] k1: 2
2016-10-20 16:36:43.731 GCDDemo[20563:639353] k2: 2
2016-10-20 16:36:44.806 GCDDemo[20563:639353] k2: 1
2016-10-20 16:36:44.806 GCDDemo[20563:639355] k1: 1
2016-10-20 16:36:45.879 GCDDemo[20563:639353] k2: 0
2016-10-20 16:36:45.881 GCDDemo[20563:639355] k1: 0
2016-10-20 16:36:46.882 GCDDemo[20563:639355] 执行完成
```
##### dispatch_apply
　　多次执行一段代码，高效执行

```
- (void)apply {
    dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    
    dispatch_async(queue, ^{
        
        // 高效率的执行
        dispatch_apply(10, queue, ^(size_t index) {
            
            NSLog(@"%ld", index);
        });
    });
}
```
运行结果：

```
2016-10-20 16:40:19.713 GCDDemo[20589:640961] 1
2016-10-20 16:40:19.713 GCDDemo[20589:640963] 0
2016-10-20 16:40:19.713 GCDDemo[20589:640960] 2
2016-10-20 16:40:19.713 GCDDemo[20589:640965] 3
2016-10-20 16:40:19.713 GCDDemo[20589:640961] 4
2016-10-20 16:40:19.713 GCDDemo[20589:640963] 5
2016-10-20 16:40:19.714 GCDDemo[20589:640960] 6
2016-10-20 16:40:19.714 GCDDemo[20589:640965] 7
2016-10-20 16:40:19.714 GCDDemo[20589:640961] 8
2016-10-20 16:40:19.714 GCDDemo[20589:640963] 9
```
##### dispatch_after
　　延时提交任务

```
// 延迟两秒提交任务
dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
    // 回到主队列执行代码
});
```
##### dispathc_once
　　整个程序运行中只执行一次，可以用来定义单例

```
static dispatch_once_t onceToken;
dispatch_once(&onceToken, ^{
    // 此处代码只执行一次
});


// 使用dispatch_onece来定义单例
static Singleton *singleten = nil;
+ (instancetype)shared
{
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        // 确保block的代码只被执行一次
        singleten = [[self alloc] init];
    });

    return singleten;
}

```
**欢迎吐槽，同时也欢迎转载，如转载请保留原文地址：
[https://walkstep.github.io/2016/10/20/GCD](https://walkstep.github.io/2016/10/20/GCD)**

**更新于2016年10月20日**


　　
