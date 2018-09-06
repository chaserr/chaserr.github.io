---
title: 点击cell动态改变cell高度
date: 2016-06-16 19:43:36
tags: 
	- TableViewCell
	- 动态改变高度
	- cell伸缩
categories: iOS知识小结
---
应一个群朋友的要求，想实现如下图这种的效果：
![效果图1](http://7xuupy.com1.z0.glb.clouddn.com/E552427019C66D11EA34D2FE5FCBA4E3.gif)

问题：点击cell，然后根据cell内部出来的view的高度来动态改变当前的点击的cell的高度。
解决思路：
<!--more-->
1. 点击cell的时候会弹出一个contentView,根据数据，获取这个contentView的高度。为了实现刷新的动画效果，接下来的cell的高度变化都在 

``` objectivec
[tableView beginUpdates];
// code
[tableView endUpdates];
```
上面这个代码块之间执行。
2. 当contentView以动画的形式出现的时候，先展示contentView，然后再改变cell的高度
3. 当contentView消失的时候，先将contentView消失，然后再刷新这一行的高度
4. 在返回cell高度的方法中，根据contentView是否显示而显示不同的高度。

> 现在cell高度计算，都会先给一个预估高度，这样代理方法就不会一开始走`heightForRowAtIndexPath`这个方法，会先走`cellForRowAtIndexPath`这个方法，这样的话，当cell显示的时候才去调用`heightForRowAtIndexPath`方法，此时cell已经创建成功了。这样就可以在这个方法里通过`cellForRowAtIndexPath`来获取cell了,相比从前，计算cell高度简单多了。


最后简单的模拟了一下不同cell的高度，实现的效果如下图：
![效果图2](http://7xuupy.com1.z0.glb.clouddn.com/EC8C15D3-4180-4238-834D-8475B9FB597B.gif)

核心代码：

``` objectivec

    if (!self.isVisiable) {
        _index_arr = arc4random()%5;
        _indexPath_cell = indexPath;
        _self_adaption = YES;
        CGFloat f = [[_Harray objectAtIndex:_index_arr] floatValue];
        UIView *testView = [[UIView alloc]
                            initWithFrame:CGRectMake(0, 0, SCREEN_WIDTH, f - 10)];
        testView.backgroundColor = [UIColor colorWithRed:1.000 green:0.971 blue:0.138 alpha:1.000];
        testView.userInteractionEnabled = NO;
        UIImageView *imageView = [[UIImageView alloc] initWithFrame:testView.bounds];
        imageView.image = [UIImage imageNamed:@"ab97283425c2ce1c75ccac15a1fed5fd.jpg"];
        [testView addSubview:imageView];
        _testView= testView;
        [cell.contentView addSubview:testView];
        [tableView beginUpdates];
        testView.transform = CGAffineTransformMakeScale(1, 0.01);
        
        [UIView animateWithDuration:0.5
                         animations:^{
                             
                             testView.transform = CGAffineTransformMakeScale(1, 1);
                             CGRect oldFrame = cell.frame;
                             oldFrame.size.height = f;
                             cell.frame = oldFrame;

                         }completion:^(BOOL finish){
                             _visiable = YES;

                         }];
        [tableView endUpdates];
        
    }
    else{
    
        [UIView animateWithDuration:0.5
                         animations:^{
                             
                             _testView.transform = CGAffineTransformMakeScale(1, 0.01);
                             
                         }completion:^(BOOL finish){
                             [_testView removeFromSuperview];
                             _visiable = NO;
                             _self_adaption = NO;

                             [self.tableView reloadRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationAutomatic];
                         }];
    }
    
```

demo地址：[git传送门](https://github.com/chaserr)
