---
title: UITabBar动画
date: 2016-10-28 11:20:13
tags: [iOS]
categories: iOS
---

#### UITabBar动画
　　说到UITabBar动画，不得不说一个比较有名的第三方RAMAnimatedTabBarController，可是RAMAnimatedTabBarController是基于swift语言来写的，对于在使用OC写项目的同学来说，是一件比较遗憾的事，不过没关系，我们也可以自己写，下面分享一下UITabBar的两种动画方式，一种是Image和Label都有动画，一种是仅Image有动画，大家可以根据需求选择哪种实现方式。<!--more-->
　　
	下面列出了主要代码，选择不同的方式可以将相应的代码注释掉或者去掉注释

```
@interface MainTabBarController ()

{
    NSInteger oldIndex;
}

@end

- (void)tabBar:(UITabBar *)tabBar didSelectItem:(UITabBarItem *)item {
    NSInteger selectIndex = [tabBar.items indexOfObject:item];
    if (selectIndex != oldIndex) {
        [self animationWithIndex:selectIndex];
    }
}

- (void)animationWithIndex:(NSInteger)index {
    // Image + Label动画
//    NSMutableArray *animationArrayM = [NSMutableArray array];
//    for (UIView *tabBarButton in self.tabBar.subviews) {
//        if ([tabBarButton isKindOfClass:NSClassFromString(@"UITabBarButton")]) {
//            [animationArrayM addObject:tabBarButton];
//        }
//    }
//    CAKeyframeAnimation *animation = [CAKeyframeAnimation animation];
//    animation.keyPath = @"transform.scale";
//    animation.values = @[@1.0,@1.3,@0.9,@1.15,@0.95,@1.02,@1.0];
//    animation.duration = 1;
//    animation.calculationMode = kCAAnimationCubic;
//    // 添加动画
//    [[animationArrayM[index] layer] addAnimation:animation forKey:nil];
//    
//    oldIndex = index;
    
    
    // Image动画
    NSMutableArray *animationArrayM = [NSMutableArray array];
    for (UIView *tabBarButton in self.tabBar.subviews) {
        if ([tabBarButton isKindOfClass:NSClassFromString(@"UITabBarButton")]) {
            for (UIImageView *imageV in tabBarButton.subviews) {
                if ([imageV isKindOfClass:NSClassFromString(@"UITabBarSwappableImageView")]) {
                    [animationArrayM addObject:imageV];
                }
            }
        }
    }
    CAKeyframeAnimation *animation = [CAKeyframeAnimation animation];
    animation.keyPath = @"transform.scale";
    animation.values = @[@1.0,@1.3,@0.9,@1.15,@0.95,@1.02,@1.0];
    animation.duration = 1;
    animation.calculationMode = kCAAnimationCubic;
    // 添加动画
    [[animationArrayM[index] layer] addAnimation:animation forKey:nil];
    
    oldIndex = index;
    
}
```
　　Image + Label动画如下图
　　
![](http://oetpv7kic.bkt.clouddn.com/UITabBarAnimation2.gif)
　　
　　Image动画如下图
　　
![](http://oetpv7kic.bkt.clouddn.com/UITabBarAnimation1.gif)

**欢迎吐槽，同时也欢迎转载，如转载请保留原文地址：
[https://walkstep.github.io/2016/10/28/UITabBarItemAnimation](https://walkstep.github.io/2016/10/28/UITabBarItemAnimation)**

**更新于2016年10月28日**
　　
　　
