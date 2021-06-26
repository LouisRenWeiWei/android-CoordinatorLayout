# android-CoordinatorLayout
1.前言
CoordinatorLayout是在Google I/O 15上，谷歌发布了一个新的 support library中的控件，它是support library中最重要的控件之一，所以大家要掌握它！
Coordinator在英文中是“协调者”的意思，所以我把CoordinatorLayout叫做“协调者布局“。

2 本章内容
本章主要讲解如下内容：

 1. 基本使用
    1.1 CoordinatorLayout+FloatingActionButton
    1.2 CoordinatorLayout+AppBarLayout
        1.2.1 AppBarLayout嵌套TabLayout
        1.2.2 AppBarLayout嵌套CollapsingToolbarLayout
        1.2.3 AppBarLayout与FloatingActionButton
 2.自定义Behavior
     2.1自定义Behavior
     2.2滚动监听
————————————————
CoordinatorLayout+FloatingActionButton

<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/activity_floatingaction"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >
    <android.support.design.widget.FloatingActionButton
        android:id="@+id/faBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|right"
        android:layout_margin="10dp"
        />
</android.support.design.widget.CoordinatorLayout>
————————————————
CoordinatorLayout+AppBarLayout
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:layout_scrollFlags="scroll|enterAlways"
            app:title="ToolBar" />
    </android.support.design.widget.AppBarLayout>

    <android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:padding="30dp"
                android:text="测试数据1"
                android:textSize="20sp" />

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:padding="30dp"
                android:text="测试数据2"
                android:textSize="20sp" />


        </LinearLayout>
    </android.support.v4.widget.NestedScrollView>
</android.support.design.widget.CoordinatorLayout>
————————————————


我们在CoordinatorLayout加入了AppBarLayout，AppBarLayout中有一个toolbar，这里我们对AppBarLayou做一下讲解：

1.AppbarLayout：继承了LinearLayout(默认的AppBarLayout是垂直方向)，它是为了Material Design设计的App Bar，它的作用是把AppBarLayout包裹的内容都作为AppBar。说白了它的出现就是为了和CoordinatorLayout搭配使用，实现一些炫酷的效果的。没有CoordinatorLayout，它和Linearlayout没区别。

2.app:layout_scrollFlags=”scroll|enterAlways”
AppBarLayout的直接子控件可以设置的属性:layout_scrollFlags

scroll|exitUntilCollapsed 如果AppBarLayout的直接子控件设置该属性,该子控件可以滚动,向上滚动NestedScrollView出父布局(一般为CoordinatorLayout)时,会折叠到顶端,向下滚动时NestedScrollView必须滚动到最上面的时候才能拉出该布局
scroll|enterAlways:只要向下滚动该布局就会显示出来,只要向上滑动该布局就会向上收缩
scroll|enterAlwaysCollapsed:向下滚动NestedScrollView到最底端时该布局才会显示出来
如果不设置改属性,则改布局不能滑动
3.细心的朋友可能在NestedScrollView中发现这么一条属性,这是干什么用的呢？

app:layout_behavior="@string/appbar_scrolling_view_behavior"
1
@string/appbar_scrolling_view_behavior是一个系统字符串，值为：android.support.design.widget.AppBarLayout$ScrollingViewBehavior。
唯一的作用是把自己（使用者）放到AppBarLayout的下面。（不能理解为什么叫ScrollingViewBehavior）所有View都能使用这个Behavior。大家可以把这句话去掉再试试效果。

为了节省篇幅，NestedScrollView中的TextView我删除了很多，大家测试的时候，拷贝上几个。

注意：要注意扩展，我们可以在AppBarLayout包含Toolbar和TableLayout.然后只给 Toolbar设置app:layout_scrollFlags=”scroll|enterAlways”属性，TableLayout不设置。这个大家自己试试效果吧，这里就不给代码了
————————————————
AppBarLayout+CollapsingToolbarLayout
接着3.2讲解的内容，我们继续往下深入。我们在AppBarLayout中添加CollapsingToolbarLayout。

Collapsing是折叠的意思。见名知意，CollapsingToolbarLayout的作用是提供了一个可以折叠的Toolbar，它继承至FrameLayout
————————————————
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context="mystudy.czh.com.mystudy.CollapsingToolBarActivity">

    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/AppTheme.NoActionBar.AppBarOverlay">

        <android.support.design.widget.CollapsingToolbarLayout
            android:layout_width="match_parent"
            android:layout_height="170dp"
            app:contentScrim="@color/colorAccent"
            app:layout_scrollFlags="scroll|exitUntilCollapsed"
            app:title="@string/app_name">

            <ImageView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:background="@drawable/toolbar_bg" />

            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:layout_collapseMode="pin"
                app:popupTheme="@style/AppTheme.NoActionBar.PopupOverlay"
                app:title="toolBar" />
        </android.support.design.widget.CollapsingToolbarLayout>
    </android.support.design.widget.AppBarLayout>

    <android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical"
            >

            <TextView
                android:layout_width="match_parent"
                android:layout_height="200dp"
                android:background="@color/colorAccent" />
        </LinearLayout>
    </android.support.v4.widget.NestedScrollView>

</android.support.design.widget.CoordinatorLayout>
————————————————
1.大家一定要弄清楚，它们之间的包含关系：
AppBarLayout >CollapsingToolbarLayout>Toolbar
2.CollapsingToolbarLayout作为AppBarLayout的直接子控件，也要设置app:layout_scrollFlags属性
3.app:layout_collapseMode属性是折叠模式，它是CollapsingToolbarLayout的直接子控件需要设置的,它的取值如下：

pin:在滑动过程中,此自布局会固定在它所在的位置不动,直到CollapsingToolbarLayout全部折叠或者全部展开
parallax:视察效果,在滑动过程中,不管上滑还是下滑都会有视察效果,不知道什么事视察效果自己看gif图(layout_collapseParallaxMultiplier视差因子 0~1之间取值,当设置了parallax时可以配合这个属性使用,调节自己想要的视差效果)
不设置:跟随NestedScrollView的滑动一起滑动,NestedScrollView滑动多少距离他就会跟着走多少距离
上面这几个属性大家如果看不懂，自己设置一下看看就明白了。
————————————————



最后说一下使用注意事项:
CoordinatorLayout继承自viewgroup,但是使用类似于framLayout,有层次结构,后面的布局会覆盖在前面的布局之上,但跟behavior属性也有很大关系,的app:layout_behavior属性,只有CoordinatorLayout的直接子布局才能响应,所以不要做徒劳无功的事

CoordinatorLayout+AppBarLayout+CollapsingToolbarLayout结合起来才能产生这么神奇的效果,不要想当然的光拿着CoordinatorLayout就要玩出来(刚接触的时候我也有这种想法),累死你

AppBarLayout:继承自lineLayout,使用时其他属性随lineLayou,已经响应了CoordinatorLayout的layout_behavior属性,所以他能搞出那么多特效来

AppBarLayout的直接子控件可以设置的属性:layout_scrollFlags ：

1.scroll|exitUntilCollapsed如果AppBarLayout的直接子控件设置该属性,该子控件可以滚动,向上滚动NestedScrollView出父布局(一般为CoordinatorLayout)时,会折叠到顶端,向下滚动时NestedScrollView必须滚动到最上面的时候才能拉出该布局 
2.scroll|enterAlways:只要向下滚动该布局就会显示出来,只要向上滑动该布局就会向上收缩 
3.scroll|enterAlwaysCollapsed:向下滚动NestedScrollView到最底端时该布局才会显示出来 
4.如果不设置改属性,则改布局不能滑动
1
2
3
4
CollapsingToolbarLayout,字面意思是折叠的toolbar,它确实是起到折叠作用的,可以把自己的自布局折叠 继承自framLayout,所以它的直接子类可以设置layout_gravity来控制显示的位置,它的直接子布局可以使用的属性:app:layout_collapseMode(折叠模式):可取的值如下:

1.pin:在滑动过程中,此自布局会固定在它所在的位置不动,直到CollapsingToolbarLayout全部折叠或者全部展开 
2.parallax:视察效果,在滑动过程中,不管上滑还是下滑都会有视察效果,不知道什么事视察效果自己看gif图(layout_collapseParallaxMultiplier视差因子 0~1之间取值,当设置了parallax时可以配合这个属性使用,调节自己想要的视差效果) 
3.不设置:跟随NestedScrollView的滑动一起滑动,NestedScrollView滑动多少距离他就会跟着走多少距离
1
2
3
toobar最好是放在CollapsingToolbarLayout,也不是没有其他用法,但是在这套系统中一般只能放在CollapsingToolbarLayout里面,才能达到好的效果,这里toolbar同时设置layout_gravity和app:layout_collapseMode时有一些比较复杂的情况.不一一作介绍,因为一般我们只会把toolbar放在最上面(不用设置layout_gravity属性,默认的),并且设置app:layout_collapseMode为pin,让他固定在最顶端,有兴趣的自己试试其他情况,

告你一个惊天大幂幂:只要CollapsingToolbarLayout里面包含有toolbar那么CollapsingToolbarLayout的折叠后高度就是toolbar的高度,相当于CollapsingToolbarLayout设置了minHeight属性,

再告诉你一个惊天大咪咪:CollapsingToolbarLayout折叠到最顶端时,它就是老大,会处于最上层,包括toolbar在内,所有的布局都会被他盖住,显示不出来.

CollapsingToolbarLayout 自己的属性说明（不是直接子布局可用的,是自己可以用的属性 ）：

contentScrim折叠后的颜色也是展开时的渐变颜色,效果超赞. 
title标题,如果设置在折叠时会动画的显示到折叠后的部分上面,拉开时会展开,很好玩的 
expandedTitleMargin当title文字展开时文字的margin,当然还有marginTop等属性,脑补吧 
app:collapsedTitleTextAppearance=”@style/Text”折叠时title的样式里面可以定义字体大小颜色等 
app:collapsedTitleTextAppearance=”@style/Text1”折叠时title的样式里面可以定义字体大小颜色等 
当然还有一些,自己试试吧,现在的这些已经完全够用了
1
2
3
4
5
6
还有最后一个问题:怎么实现固定表头,这个也简单,写一个布局放在CollapsingToolbarLayout之后,AppBarLayout之内即可,xml文件中自己找找看吧.你要是问如果放在CollapsingToolbarLayout之前,AppBarLayout之内会怎么样?这样折叠布局就不起作用了.不会折叠了


viewpager 下的recycleview不要设置android:nestedScrollingEnabled="false" ，要设置为true，否则就不能滑动控制了



eg:
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/rl_content"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:fitsSystemWindows="true">

        <com.google.android.material.appbar.CollapsingToolbarLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@color/white"
            app:layout_scrollFlags="scroll|exitUntilCollapsed"
            app:titleEnabled="false">

            <FrameLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:paddingTop="52dp"
                >
                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="vertical">

                    <LinearLayout
                        android:layout_width="match_parent"
                        android:layout_height="200dp"
                        android:background="@color/colorAccent" />

                    <ImageView
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:background="@mipmap/icon_card_bottom" />
                </LinearLayout>

                <RelativeLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_marginLeft="8dp"
                    android:layout_marginRight="8dp"
                    android:paddingBottom="16dp">

                    <LinearLayout
                        android:id="@+id/ll_top_btns"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:gravity="center"
                        android:orientation="horizontal">

                        <Button
                            android:id="@+id/btn_ai_statistics"
                            android:layout_width="0dp"
                            android:layout_height="wrap_content"
                            android:layout_weight="1"
                            android:background="@color/transparent"
                            android:drawableTop="@mipmap/icon_ai_statistics"
                            android:drawablePadding="8dp"
                            android:gravity="center"
                            android:paddingTop="18dp"
                            android:paddingBottom="8dp"
                            android:text="AI报表"
                            android:textColor="@color/white"
                            android:textSize="12sp" />

                        <Button
                            android:id="@+id/btn_score_statistics"
                            android:layout_width="0dp"
                            android:layout_height="wrap_content"
                            android:layout_marginLeft="15dp"
                            android:layout_weight="1"
                            android:background="@color/transparent"
                            android:drawableTop="@mipmap/icon_score_rank"
                            android:drawablePadding="8dp"
                            android:gravity="center"
                            android:paddingTop="18dp"
                            android:paddingBottom="8dp"
                            android:text="积分排名"
                            android:textColor="@color/white"
                            android:textSize="12sp" />

                        <Button
                            android:id="@+id/btn_store"
                            android:layout_width="0dp"
                            android:layout_height="wrap_content"
                            android:layout_marginLeft="15dp"
                            android:layout_weight="1"
                            android:background="@color/transparent"
                            android:drawableTop="@mipmap/icon_score_store"
                            android:drawablePadding="8dp"
                            android:gravity="center"
                            android:paddingTop="18dp"
                            android:paddingBottom="8dp"
                            android:text="兑换商品"
                            android:textColor="@color/white"
                            android:textSize="12sp" />

                        <Button
                            android:id="@+id/btn_visit_log"
                            android:layout_width="0dp"
                            android:layout_height="wrap_content"
                            android:layout_marginLeft="15dp"
                            android:layout_weight="1"
                            android:background="@color/transparent"
                            android:drawableTop="@mipmap/icon_visit_log_m"
                            android:drawablePadding="8dp"
                            android:gravity="center"
                            android:paddingTop="18dp"
                            android:paddingBottom="8dp"
                            android:text="营销日志"
                            android:textColor="@color/white"
                            android:textSize="12sp" />
                    </LinearLayout>

                    <LinearLayout
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:layout_below="@+id/ll_top_btns"
                        android:layout_marginTop="10dp"
                        android:background="@drawable/bg_customer_top_grid"
                        android:orientation="vertical"
                        android:paddingTop="5dp"
                        android:paddingBottom="15dp">

                        <LinearLayout
                            android:layout_width="match_parent"
                            android:layout_height="wrap_content"
                            android:gravity="center"
                            android:orientation="horizontal">

                            <Button
                                android:id="@+id/btn_send_msg"
                                android:layout_width="wrap_content"
                                android:layout_height="wrap_content"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_send_msg_2"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="发送短信"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />

                            <Button
                                android:id="@+id/btn_add_wx"
                                android:layout_width="wrap_content"
                                android:layout_height="wrap_content"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_add_wx"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="企微互联"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />

                            <Button
                                android:id="@+id/btn_brochure"
                                android:layout_width="wrap_content"
                                android:layout_height="wrap_content"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_brochure"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="宣传册"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />

                            <Button
                                android:id="@+id/btn_business_card"
                                android:layout_width="wrap_content"
                                android:layout_height="wrap_content"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_business_card"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="递名片"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />
                        </LinearLayout>

                        <View
                            android:layout_width="match_parent"
                            android:layout_height="1dp"
                            android:layout_marginLeft="22dp"
                            android:layout_marginTop="15dp"
                            android:layout_marginRight="22dp"
                            android:layout_marginBottom="15dp"
                            android:background="@color/divider" />

                        <LinearLayout
                            android:layout_width="match_parent"
                            android:layout_height="wrap_content"
                            android:gravity="center"
                            android:orientation="horizontal">

                            <Button
                                android:id="@+id/btn_visit_remind"
                                android:layout_width="0dp"
                                android:layout_height="wrap_content"
                                android:layout_weight="1"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_visit_remind_2"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="走访提醒"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />

                            <Button
                                android:id="@+id/btn_customer_nearby"
                                android:layout_width="0dp"
                                android:layout_height="wrap_content"
                                android:layout_weight="1"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_customer_nearby_2"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="附近客户"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />

                            <Button
                                android:id="@+id/btn_customer_industry_area"
                                android:layout_width="0dp"
                                android:layout_height="wrap_content"
                                android:layout_weight="1"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_customer_industry_area"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="园区客户"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />

                            <Button
                                android:id="@+id/btn_credit_village"
                                android:layout_width="0dp"
                                android:layout_height="wrap_content"
                                android:layout_weight="1"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_credit_village"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="信用村"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />

                            <Button
                                android:id="@+id/btn_blacklist"
                                android:layout_width="0dp"
                                android:layout_height="wrap_content"
                                android:layout_weight="1"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_blacklist"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="黑名单"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />
                        </LinearLayout>

                        <LinearLayout
                            android:layout_width="match_parent"
                            android:layout_height="wrap_content"
                            android:gravity="center"
                            android:orientation="horizontal">

                            <Button
                                android:id="@+id/btn_set_ai_recommend"
                                android:layout_width="0dp"
                                android:layout_height="wrap_content"
                                android:layout_weight="1"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_ai_recommend"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="智能推荐"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />


                            <Button
                                android:id="@+id/btn_overdue"
                                android:layout_width="0dp"
                                android:layout_height="wrap_content"
                                android:layout_weight="1"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_overdue_2"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="逾期催收"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />

                            <Button
                                android:id="@+id/btn_set_visit_plan"
                                android:layout_width="0dp"
                                android:layout_height="wrap_content"
                                android:layout_weight="1"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_set_visit_plan"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="拜访计划"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />

                            <Button
                                android:id="@+id/btn_product"
                                android:layout_width="0dp"
                                android:layout_height="wrap_content"
                                android:layout_weight="1"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_set_business_card"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="产品库"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />

                            <Button
                                android:id="@+id/btn_script_library"
                                android:layout_width="0dp"
                                android:layout_height="wrap_content"
                                android:layout_weight="1"
                                android:background="@color/transparent"
                                android:drawableTop="@mipmap/icon_script_library"
                                android:drawablePadding="8dp"
                                android:gravity="center"
                                android:paddingTop="18dp"
                                android:paddingBottom="8dp"
                                android:text="话术库"
                                android:textColor="#336CB0"
                                android:textSize="12sp" />
                        </LinearLayout>
                    </LinearLayout>
                </RelativeLayout>
            </FrameLayout>

            <androidx.appcompat.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="52dp"
                android:background="@color/app_color"
                app:layout_collapseMode="pin">

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="52dp"
                    android:background="@color/colorAccent"
                    android:orientation="horizontal"
                    app:layout_collapseMode="pin">

                    <TextView
                        android:id="@+id/search_et"
                        android:layout_width="0dp"
                        android:layout_height="28dp"
                        android:layout_gravity="center_vertical"
                        android:layout_weight="1"
                        android:background="@drawable/search_bg"
                        android:drawableLeft="@mipmap/icon_search_grey"
                        android:drawableRight="@mipmap/icon_voice_grey"
                        android:drawablePadding="8dp"
                        android:gravity="center_vertical"
                        android:hint="客户名称"
                        android:imeOptions="actionSearch"
                        android:maxLength="10"
                        android:maxLines="1"
                        android:paddingLeft="10dp"
                        android:paddingRight="10dp"
                        android:singleLine="true"
                        android:textColorHint="@color/app_bg"
                        android:textCursorDrawable="@drawable/cursor"
                        android:textSize="15sp" />


                    <TextView
                        android:id="@+id/tv_filter"
                        android:layout_width="wrap_content"
                        android:layout_height="match_parent"
                        android:layout_gravity="center_vertical"
                        android:gravity="center"
                        android:paddingLeft="10dp"
                        android:paddingRight="10dp"
                        android:text="筛选"
                        android:textColor="#ffffff"
                        android:textSize="15sp" />

                    <View
                        android:layout_width="1dp"
                        android:layout_height="15dp"
                        android:layout_gravity="center_vertical"
                        android:background="@color/white" />

                    <TextView
                        android:id="@+id/tv_sort"
                        android:layout_width="wrap_content"
                        android:layout_height="match_parent"
                        android:layout_gravity="center_vertical"
                        android:gravity="center"
                        android:paddingLeft="10dp"
                        android:paddingRight="10dp"
                        android:text="排序"
                        android:textColor="@color/white"
                        android:textSize="15sp" />
                </LinearLayout>
            </androidx.appcompat.widget.Toolbar>

        </com.google.android.material.appbar.CollapsingToolbarLayout>
    </com.google.android.material.appbar.AppBarLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <com.gxz.PagerSlidingTabStrip
            android:id="@+id/tabs"
            android:layout_width="match_parent"
            android:layout_height="40dp"
            android:background="@color/white"
            android:textColor="@color/black"
            android:textSize="15sp"
            app:pstsDividerColor="@android:color/transparent"
            app:pstsIndicatorColor="@color/app_color"
            app:pstsIndicatorHeight="0dp"
            app:pstsShouldExpand="false"
            app:pstsTabPaddingLeftRight="15dp"
            app:pstsTextSelectedColor="@color/app_color"
            app:pstsUnderlineColor="@android:color/transparent" />

        <androidx.viewpager.widget.ViewPager
            android:id="@+id/viewpager"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"/>
    </LinearLayout>





    <LinearLayout
        android:id="@+id/ll_sort"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@color/mask_color"
        android:orientation="vertical"
        android:visibility="gone"
        android:layout_marginTop="52dp">

        <ListView
            android:id="@+id/lv_sort"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" />
    </LinearLayout>
</androidx.coordinatorlayout.widget.CoordinatorLayout>




fragment的xml

<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <RelativeLayout
            android:layout_width="match_parent"
            android:layout_height="40dp"
            android:background="@color/app_bg"
            android:paddingLeft="15dp">

            <LinearLayout
                android:id="@+id/ll_total_count"
                android:layout_width="wrap_content"
                android:layout_height="match_parent"
                android:layout_alignParentLeft="true"
                android:orientation="horizontal">

                <TextView
                    android:id="@+id/tv_page_desc1"
                    android:layout_width="wrap_content"
                    android:layout_height="match_parent"
                    android:gravity="center_vertical"
                    android:text="筛选出 "
                    android:textColor="@color/color_66"
                    android:textSize="12sp" />

                <TextView
                    android:id="@+id/tv_page_desc"
                    android:layout_width="wrap_content"
                    android:layout_height="match_parent"
                    android:gravity="center_vertical"
                    android:text="0"
                    android:textColor="@color/app_color"
                    android:textSize="12sp" />

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="match_parent"
                    android:gravity="center_vertical"
                    android:text=" 家"
                    android:textColor="@color/color_66"
                    android:textSize="12sp" />
            </LinearLayout>


            <LinearLayout
                android:id="@+id/ll_score"
                android:layout_width="wrap_content"
                android:layout_height="match_parent"
                android:layout_toLeftOf="@+id/ll_score_rank"
                android:orientation="horizontal">

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="match_parent"
                    android:ems="1"
                    android:gravity="center_vertical"
                    android:text="积分"
                    android:textColor="@color/color_66"
                    android:textSize="8sp" />

                <TextView
                    android:id="@+id/tv_score"
                    android:layout_width="wrap_content"
                    android:layout_height="match_parent"
                    android:gravity="center_vertical"
                    android:paddingLeft="5dp"
                    android:paddingRight="8dp"
                    android:text="0"
                    android:textColor="@color/app_color"
                    android:textSize="12sp"
                    android:visibility="visible" />
            </LinearLayout>

            <LinearLayout
                android:id="@+id/ll_score_rank"
                android:layout_width="wrap_content"
                android:layout_height="match_parent"
                android:layout_alignParentRight="true"
                android:orientation="horizontal">

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="match_parent"
                    android:ems="1"
                    android:gravity="center_vertical"
                    android:text="排名"
                    android:textColor="@color/color_66"
                    android:textSize="8sp" />

                <TextView
                    android:id="@+id/tv_score_rank"
                    android:layout_width="wrap_content"
                    android:layout_height="match_parent"
                    android:gravity="center_vertical"
                    android:paddingLeft="5dp"
                    android:paddingRight="15dp"
                    android:text="0"
                    android:textColor="@color/app_color"
                    android:textSize="12sp"
                    android:visibility="visible" />
            </LinearLayout>

        </RelativeLayout>

        <com.scwang.smartrefresh.layout.SmartRefreshLayout
            android:id="@+id/refreshLayout"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1">

            <androidx.recyclerview.widget.RecyclerView
                android:id="@+id/recyclerView"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:overScrollMode="never"
                android:scrollbars="none"
                />
        </com.scwang.smartrefresh.layout.SmartRefreshLayout>
    </LinearLayout>
</FrameLayout>

