# Temporary Contribution


### 有关 UITableHeaderView 展开收缩动画的坑

#### 环境参数：

```
*
```

#### 问题分析：

iOS `UITableView` 控件中的 `TableHeaderView`，在实现展开文字和收缩文字动画时候可能会出现以下异常的情况。

1. 无法撑开 `UITableViewCell` ，即使改变了 `TableHeaderView` 的高度
2. 带 `UITableViewSectionView` 的情况下实现动画，会出现 `UITableViewSectionView` 与 `UITableViewCell` 分离的情况
3. 当 `TableHeaderView`的背景为透明的时候，展开的文字会很明显出现在下方 `UITableViewCell` 上面，然后动画才继续直到结束

#### 解决方法：

1. 这种情况是因为单纯改变 `TableHeaderView` 的高度并不会通知 `UITableView` 更新布局，需要在 `AnimationBlock` 中重新将 `TableHeaderView` 赋值给 `UITableView` 才能不断刷新布局。
2. 这种情况是因为使用了 `[UITableView beginUpdates]` 和 `[UITableView endUpdates]` 方法，这个方法会通知 `UITableViewCell` 跟随 `UITableView` 实现动画效果，但是带来的问题就是`UITableViewSectionView` 和 `UITableViewCell` 的分离，所以不用此方法比较合理
3. 该问题是因为 `TableHeaderView` 允许超出边缘的视图，但是修改为 `maskToBounds` 为 `YES` 之后发现并没有改进，进一步的解决办法是在 `TableheaderView` 上层再次添加一个底部空白的 `UIView`，然后修改此 `UIView` 的 `maskToBounds` 属性，即可解决



