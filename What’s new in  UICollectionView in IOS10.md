                                      What’s new in  UICollectionView in IOS10

##  第一部分 Smooth scrolling 

### 1.circle of life of cell
    
```
    prepareForReuse: This gives the cell an opportunity to reset itself to its default state, ready to receive new data from your app. 
```

```
    cellForItemAtIndexPath: You’ll access your  data models, set them on the cell, 
```
  


###  ios10以前 ：
```
     reuse queue —> prepareForReuse —>  cellForItemAtIndexPath —> willDisplayCell —> didEndDisplayingCell —> 进入复用池
```

- - - -

###  ios10:         
```
    reuse queue —> prepareForReuse —>  cellForItemAtIndexPath —>        ..hesitant ..    —>  willDisplayCell —>   didEndDisplayingCell —> hold a little bit  longer —> 进入复用池
```

### prepareForReuse / cellForItemAtIndexPath :
         之前是cell的上边缘马上进去屏幕的时候就会调用该方法,而iOS 10 提前到cell还在屏幕外面的时候就调用; 
![](Resource/屏幕快照%202016-12-14%2019.43.52.png)
![](Resource/屏幕快照%202016-12-14%2019.44.07.png)
![](Resource/屏幕快照%202016-12-14%2019.44.19.png)
![](Resource/屏幕快照%202016-12-14%2019.44.32.png)

- - - -

![](Resource/屏幕快照%202016-12-14%2019.44.44.png)
![](Resource/屏幕快照%202016-12-14%2019.44.53.png)
![](Resource/屏幕快照%202016-12-14%2019.45.06.png)
![](Resource/屏幕快照%202016-12-14%2019.45.17.png)

###  hold a little bit  longer :
          在iOS 10 之前,cell只能从重用队列里面取出,再走一遍生命周期,并调用cellForItemAtIndexPath创建或者生成一个cell.在iOS 10 中,系统会cell保存一段时间,也就是说当用户把cell滑出屏幕以后,如果又滑动回来,cell不用再走一遍生命周期了,只需要调用willDisplayCell方法就可以重新出现在屏幕中了.

> The first thing we want to talk about is we want to do all the heavy lifting in cellForItemAtIndexPath. All the, all the content creation for your cell. Everything should be centered in cellForItemAtIndexPath.  

> Additionally, we want to make sure we do minimal work willDisplayCell in didEndDisplayCell.  


*2. 10以前如果要加载更多，比如加载一行cell，会同时获取这一行所有cell，现在每次只获取一个，依次获取，将要显示在屏幕时会同时给所有cell发送willDisplayCell Message。*

*3.如果需要使用iOS10以前cell的生命周期，可以设置UICollectionView的属性prefetchingEnabled* 


### model pre-fetching (可选)

>  新增协议UICollectionViewDataSourcePrefetching  

```
    prefetchItemsAtIndexPaths  异步预加载数据
```
  
   
```
    cancelPrefetcingForItemsAtIndexPaths可以用来处理在滑动中取消或者降低提前加载数据的优先级.滑动变向时call cancel
```

      把model读取放在后台队列
      快速滑动时不执行
     

UITableView也适用

```
    prefetchRowsAtIndexPaths

    cancelPrefetchingForRowsAtIndexPaths
```



##  第二部分 Improvements to self-sizing cells 



### 1.cell size

```
    autolayout

    sizeThatFits

    preferredLayoutAttributesFittingAttributes
```


###  CollectionViewFlowLayout

```
    layoutEstimatedItemSize 
    UICollectionViewFlowLayoutAutomaticSize,
```


##  第三部分 interactive reordering

### interactive reordering，iOS9

```
    beginInteractiveMovementForItemAtIndexPath
```

```
    updateInteractiveMovementTargetPosition:
     手势在UICollectionView坐标系内的位置
```

```
    endInteractiveMovement，call datasource，设置model
```

```
    cancelInteractiveMovement ,不会call datasource
```


###    
###  UICollectionViewController对interactive reordering也添加了支持，只要设置interactive movement属性为true，会自动添加gesture调用 以上方法，只需实现data source部分


### paging support
         
          UICollectionView和UITableView添加了对isPagingeEnable 的支持。


###  UIRefreshControl
   
          refreshControl作为UIScrollView的一个属性，支持UIScrollView、 UICollectionView、UITableView，创建UIRefreshControl，添加target和action，设置成scrollview的属性。



##总结：

       ##  cell生命周期
       ##  model pre-fetching 
       ##  UICollectionViewFlowLayoutAutomaticSize
      
##参考：

      [UICollectionView in ios10](https://developer.apple.com/videos/play/wwdc2016/219/)
 
 
