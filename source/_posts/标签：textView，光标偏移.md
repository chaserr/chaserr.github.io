---
title: textView使用小结
date: 2016-06-15 12:08:29
tags: 
	- textView
	- 光标偏移量
categories: iOS知识小结

---


# textView使用小结：（仿QQ空间留言输入）

效果图如下：

![textView从中间慢慢往上输出](http://7xuupy.com1.z0.glb.clouddn.com/textview.gif)

&#160;&#160;这里主要使用的是textView的一个属性值：`textContainerInset`

&#160;&#160;代码如下：

``` objectivec

- (void)awakeFromNib {
    [super awakeFromNib];
    // Initialization code
    _textView.textContainerInset = UIEdgeInsetsMake(CGRectGetHeight(_textView.frame) * 0.5 +8, 0, 0, 0);

}

```
<!-- more -->
&#160;&#160;&#160;&#160;刚开始的时候设置textView的光标，也即是系统内部的`UITextContainerView`的偏移量为`_textView`的中间。
接着，我们在输入的过程中，需要将已经输入的文字网上移动，这里还是要改变`UITextContainerView`的偏移量就行了。操作属性`textContainerInset`，这里注意需要在textView的代理方法里实现

```objectivec
#pragma mark -- UITextViewDelegate

- (void)textViewDidChange:(UITextView *)textView{
    GBFillJobStateCell *cell = CELL_SUBVIEW_TABLEVIEW(textView, self.tableview);
    
    // 设置textView默认显示的文字
    if (textView.text.length == 0) {
        
        cell.placeholdText.hidden = NO;
    }else{
        
        cell.placeholdText.hidden = YES;
    }
    
    CGSize  tH     = [self textSizeWithTextView:(UITextView *)textView Font:textView.font.pointSize text:nil];
    CGFloat offset = (textView.frame.size.height - tH.height)/2.f;
    
    // 设置textContainerInset属性
    textView.textContainerInset = UIEdgeInsetsMake(offset, 0, offset, 0);

}

- (CGSize)textSizeWithTextView:(UITextView *)textView Font:(CGFloat)font text:(NSString *)string
{
    NSString *text = string ? string : textView.text;
    
    CGFloat tMargen = textView.textContainer.lineFragmentPadding * 2;
    CGFloat tW = textView.frame.size.width - tMargen;
    CGSize tSize = [text boundingRectWithSize:CGSizeMake(tW, MAXFLOAT) options:NSStringDrawingUsesLineFragmentOrigin attributes:@{NSFontAttributeName:[UIFont systemFontOfSize:14]} context:nil].size;
    return  tSize;
}
```