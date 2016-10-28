---
title: 说说通知(NSNotification)的使用
date: 2016-10-21 10:33:32
tags: [iOS, NSNotification]
categories: iOS
---
#### NSNotification
　　打开Xcode，进入NSNotification，你可以看到

```
@interface NSNotification : NSObject <NSCopying, NSCoding>

@property (readonly, copy) NSString *name;
@property (nullable, readonly, retain) id object;
@property (nullable, readonly, copy) NSDictionary *userInfo;

- (instancetype)initWithName:(NSString *)name object:(nullable id)object userInfo:(nullable NSDictionary *)userInfo NS_AVAILABLE(10_6, 4_0) NS_DESIGNATED_INITIALIZER;
- (nullable instancetype)initWithCoder:(NSCoder *)aDecoder NS_DESIGNATED_INITIALIZER;

@end

@interface NSNotification (NSNotificationCreation)

+ (instancetype)notificationWithName:(NSString *)aName object:(nullable id)anObject;
+ (instancetype)notificationWithName:(NSString *)aName object:(nullable id)anObject userInfo:(nullable NSDictionary *)aUserInfo;

- (instancetype)init /*NS_UNAVAILABLE*/;	/* do not invoke; not a valid initializer for this class */

@end
```
<!--more-->
　　name：通过name属性你可以查看在接收通知时要处理的通知类型，也就相当于是一个标识一样，这样你才能分辨你接收到的是哪个消息。
　　object：发布通知的对象，可以为空。
　　userInfo：用来存储发送通知时携带的参数，常用来传值
　　创建NSNotification的方法：
　　初始化方法：

```
- (instancetype)initWithName:(NSString *)aName
                      object:(id)object
                    userInfo:(NSDictionary *)userInfo
```
　　类方法：

```
// 不传值
+ (instancetype)notificationWithName:(NSString *)aName
                              object:(id)anObject

// 传值               
+ (instancetype)notificationWithName:(NSString *)aName
                              object:(id)anObject
                            userInfo:(NSDictionary *)userInfo
```
#### NSNotificationCenter
```
@interface NSNotificationCenter : NSObject {
    @package
    void * __strong _impl;
    void * __strong _callback;
    void *_pad[11];
}

+ (NSNotificationCenter *)defaultCenter;

- (void)addObserver:(id)observer selector:(SEL)aSelector name:(nullable NSString *)aName object:(nullable id)anObject;

- (void)postNotification:(NSNotification *)notification;
- (void)postNotificationName:(NSString *)aName object:(nullable id)anObject;
- (void)postNotificationName:(NSString *)aName object:(nullable id)anObject userInfo:(nullable NSDictionary *)aUserInfo;

- (void)removeObserver:(id)observer;
- (void)removeObserver:(id)observer name:(nullable NSString *)aName object:(nullable id)anObject;

- (id <NSObject>)addObserverForName:(nullable NSString *)name object:(nullable id)obj queue:(nullable NSOperationQueue *)queue usingBlock:(void (^)(NSNotification *note))block NS_AVAILABLE(10_6, 4_0);
    // The return value is retained by the system, and should be held onto by the caller in
    // order to remove the observer with removeObserver: later, to stop observation.

@end
```
　　其实当我们使用通知时，更多的还是使用NSNotificationCenter里的方法
　　添加通知观察

```
- (void)addObserver:(id)notificationObserver
           selector:(SEL)notificationSelector
               name:(NSString *)notificationName
             object:(id)notificationSender
```
　发通知

```
- (void)postNotification:(NSNotification *)notification

- (void)postNotificationName:(NSString *)notificationName
                      object:(id)notificationSender
                      
- (void)postNotificationName:(NSString *)notificationName
                      object:(id)notificationSender
                    userInfo:(NSDictionary *)userInfo
```

　移除通知

```
- (void)removeObserver:(id)notificationObserver

- (void)removeObserver:(id)notificationObserver
                  name:(NSString *)notificationName
                object:(id)notificationSender
```

代码示例：

```
- (void)viewDidLoad {
    [super viewDidLoad];

    UITextField tf = [[UITextField alloc] initWithFrame:CGRectMake(10, [UIScreen mainScreen].bounds.size.height-40, [UIScreen mainScreen].bounds.size.width-20, 30)];
    tf.borderStyle = UITextBorderStyleRoundedRect;
    [self.view addSubview:tf];

    // 接收键盘将要显示时的通知
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(keyboardWillShow:) name:UIKeyboardWillShowNotification object:nil];

    // 接收键盘将要隐藏时的通知
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(keyboardWillHide:) name:UIKeyboardWillHideNotification object:nil];
}

- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
    [self.view endEditing:YES];
}

/**
 *  接收到键盘将要显示时的通知
 *
 *  @param noti <#noti description#>
 */
- (void)keyboardWillShow:(NSNotification *)noti
{
    // 获取键盘的frame
    CGRect keyboardRect = [[noti.userInfo objectForKey:UIKeyboardFrameEndUserInfoKey] CGRectValue];

//    NSLog(@"%@", NSStringFromCGRect(keyboardRect));

    // 获取键盘动画的持续时间
    CGFloat durTime = [noti.userInfo[UIKeyboardAnimationDurationUserInfoKey] floatValue];

    // 修改UITextField的y轴
//    [UIView animateWithDuration:durTime animations:^{

        CGRect tfRect = tf.frame;

        // 当前键盘的y轴 - 输入框的高度 - 间隔
        tfRect.origin.y = keyboardRect.origin.y - tfRect.size.height - 10;

        tf.frame = tfRect;
//    }];
}

/**
 *  接收键盘将要隐藏时的通知
 *
 *  @param noti <#noti description#>
 */
- (void)keyboardWillHide:(NSNotification *)noti
{
    // 还原原始位置
    CGRect tfRect = tf.frame;
    tfRect.origin.y = [UIScreen mainScreen].bounds.size.height - 40;
    tf.frame = tfRect;
}

- (void)dealloc
{
    // 移除通知
    [[NSNotificationCenter defaultCenter] removeObserver:self name:UIKeyboardWillShowNotification object:nil];

    [[NSNotificationCenter defaultCenter] removeObserver:self name:UIKeyboardWillHideNotification object:nil];
}
```
　　使用通知的场景：
　　1.控制器与一个或多个任意的对象进行通信(监控) 2.UIDevice通知 3.键盘通知 4.传值

　　关于通知移除：
　　可以在dealloc方法里移除，也可以在添加通知前，调用[[NSNotificationCenter defaultCenter] removeObserver:self]方法

**欢迎吐槽，同时也欢迎转载，如转载请保留原文地址：
[https://walkstep.github.io/2016/10/21/NSNotification](https://walkstep.github.io/2016/10/21/NSNotification)**
　　
**更新于2016年10月21日**

　　
