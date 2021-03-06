# Banner
###### 基于ViewPager封装的轮播图，支持无限轮播、支持自定义动画、支持自定义时长。随意布局。
* * *

## 简介
基于ViewPage的的封装，UI样式完全由自己控制。可自定义各种动画。可简单实现各种轮播图效果，支持对轮播图的各种事件监听，例如：点击、长按、页面选中等。

修复了ViewPage配合RecyclerView使用时的两大bug（1.Banner重新出现在屏幕上后第一次轮播没有动画，动画执行一半时上拉再下拉动画会卡在那里）。

配合RecyclerView使用时当BannerView所在的ViewHolder被移除屏幕后轮播会自动停止，重新出现后轮播会自动开始，无需用代码进行任何操作。

## 下载
###### 第一步：添加 JitPack 仓库到你项目根目录的 gradle 文件中。
```
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```
###### 第二步：添加这个依赖。
```
dependencies {
    compile 'com.github.kelinZhou:Banner:2.1.0'
}
```

## 效果图
#### 样式一：标题等信息跟随页面滚动。
![Banner](materials/gif_banner_one.gif)
#### 样式二：标题固定，圆点指示器。
指示器可以在XML中配置，翻页过程中无需做任何处理。支持配置点的颜色点的大小和选中时点的颜色和大小以及点与点之间的间距。

![Banner](materials/gif_banner_two.gif)
#### 样式三：标题固定，数字指示器。
指示器可以在XML中配置，翻页过程中无需做任何处理。支持配置当前页字体颜色总页数字体颜色分隔符以及分割符字体颜色。

![Banner](materials/gif_banner_three.gif)

## 使用
#### 数据模型。
Banner中每一页的数据模型都必须实现```BannerEntry```接口，以下是接口中的所有方法：

| 方法名称 | 说明 | 返回值 |
|---------|-----|--------|
|```View onCreateView(ViewGroup parent);```|创建当前页面的布局视图。改方法只有第一加载视图进入页面的时候才会调用。也就是说视图有复用机制。|返回创建好的View对象。|
|```CharSequence getTitle();```|获取标题。|返回当前页面的标题内容，也可以返回空，如果你当前页面没有标题的话。|
|```CharSequence getSubTitle();```|获取子标题。|返回当前页面的子标题内容，也可以返回空，如果你当前页面没有子标题的话。|
|```VALUE getValue();```|获取当前页面的数据。该方法为辅助方法，是为了方便开发者调用而提供的，Api本身并没有任何调用。如果你不需要该方法可以空实现。|返回你所需要的任何对象|
|```boolean same(BannerEntry newEntry);```|比较两个模型是否相同（帮助文档中写的比较详细，这里就不多说了。注释比较长，具体可以参考帮助文档。）|该返回值决定了参数中的对象是否与this中所有需要展示在UI视图上的字段一致，如果一直返回true，否则返回false。|
#### XML中使用。
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:app="http://schemas.android.com/apk/res-auto"
                xmlns:tools="http://schemas.android.com/tools"
                android:layout_width="match_parent"
                android:layout_height="175dp"
                android:orientation="vertical"
                android:paddingBottom="6dp"
                android:paddingTop="6dp">

    <com.kelin.banner.view.BannerView
        android:id="@+id/vp_view_pager"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        <!--当Banner中的图片只有一张时的处理方式-->
        app:singlePageMode="canNotPaging|noIndicator"
        <!--为BannerView指定指示器，只要是实现了Pageable接口的Veiw都可以-->
        app:bannerIndicator="@+id/biv_indicator"
        <!--为BannerView指定用来显示标题的控件-->
        app:titleView="@+id/tv_title"
        <!--为BannerView指定用来显示副标题的控件-->
        <!--app:subTitleView=""-->
        <!--为BannerView设置翻页间隔-->
        app:pagingIntervalTime="3000"
        <!--为BannerView设置翻页时长减速倍数（是ViewPager时长的几倍）-->
        app:decelerateMultiple="4"
        <!--为BannerView指定动画差值器-->
        <!--app:interpolator=""-->
        android:background="#FFF"/>

    <LinearLayout android:layout_width="match_parent"
                  android:layout_height="wrap_content"
                  android:layout_alignParentBottom="true"
                  android:background="#8000"
                  android:gravity="center_vertical"
                  android:orientation="horizontal"
                  android:padding="6dp">

        <!--用来显示标题的控件-->
        <TextView
            android:id="@+id/tv_title"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_gravity="bottom"
            android:layout_weight="1"
            android:maxWidth="300dp"
            android:paddingBottom="6dp"
            android:paddingLeft="12dp"
            android:paddingRight="12dp"
            android:paddingTop="6dp"
            android:textColor="@android:color/white"
            android:textSize="15sp"
            android:textStyle="bold"
            tools:text="我是标题！"/>

        <!--Banner的圆点型指示器-->
        <com.kelin.banner.view.PointIndicatorView
            android:id="@+id/biv_indicator"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            <!--设置总页数，这个参数设置了也是没有意义的，最总会以BannerView的总页数为准。配置
            自定义属性只是为了能再布局文件中看到效果-->
            app:totalCount="4"
            <!--圆点的半径-->
            app:pointRadius="3dp"
            <!--选中时(也就是当前页)圆点的半径-->
            app:selectedPointRadius="4dp"
            <!--圆点与圆点之间的间距-->
            app:pointSpacing="4dp"
            <!--圆点的颜色-->
            app:pointColor="#5fff"
            <!--选中时(也就是当前页)圆点的颜色-->
            app:selectedPointColor="@android:color/white"/>
    </LinearLayout>
</RelativeLayout>
```
上面的BannerView的自定义属性```app:singlePageMode="canNotPaging|noIndicator"```需要做下说明，因为支持无限轮播，那么只有一张图片的时候是否还需要无限轮播？这个属性就是用来配置当Banner中的图片只有一张时的处理方式的。

###### 该属性是一个flag属性，一共有以下两个值：
1. ```noIndicator``` 表示如果只有一张图片则没有指示器。也就是说无论你是否设置了指示器，如果只有一张图片的话那么指示器都是不显示的。但是依然是支持无限轮播的。
2. ```canNotPaging``` 表示如果只有一张图片则不可以轮播。但是如你设置了指示器的话，指示器依然会显示。

    *上面两个属性可以同时配置，中间用"|"符号链接，例如上面代码中的配置。这样的话如果只有一张图片则既不会轮播而且无论你是否设置了指示器则都不会显示。*

    **以上所说的只有一张图片是指通过```BannerView```的```setEntries```方法设置数据源时数据源集合的```size()```等于1。所说的设置只是器是指在XML代码中为BannerView配置```app:bannerIndicator```属性或者通过代码```BannerView.setIndicatorView(@NonNull BannerIndicator indicatorView)```为BannerView设置指示器。**

#### 代码中使用。
```
    //找到BannerView控件。
    BannerView bannerView = findViewById(R.id.vp_view_pager);
    //设置自定义翻页动画改变器，也可以不设置。如果没有设置，则使用ViewPager默认的翻页动画。
    bannerView.setPageTransformer(true, new DepthPageTransformer());
    //getData()方法是从网络上获取数据。这里只是伪代码。
    List<TitleImageBannerEntry> bannerEntries = getData();
    //设置数据源并开始轮播。如果不希望启动轮播则调用两个参数的方法。
    bannerView.setEntries(bannerEntries);
```
#### 设置监听。
**页面点击监听**
```
bannerView.setOnPageClickListener(new BannerView.OnPageClickListener() {
    @Override
    protected void onPageClick(BannerEntry entry, int index) {
        //某个页面被单击后执行，entry就是这个页面的数据模型。index是页面索引，从0开始。
    }
});
```
**页面长按监听**
```
bannerView.setOnPageLongClickListener(new BannerView.OnPageLongClickListener() {
    @Override
    public void onPageLongClick(BannerEntry entry, int index) {
        //某个页面被长按后执行，entry就是这个页面的数据模型。index是页面索引，从0开始。
    }
});
```
**页面改变监听**
```
bannerView.setOnPageChangedListener(new BannerView.OnPageChangeListener() {
    @Override
    public void onPageSelected(BannerEntry entry, int index) {
        //某个页面被选中后执行，entry就是这个页面的数据模型。index是页面索引，从0开始。
    }

    @Override
    public void onPageScrolled(int index, float positionOffset, int positionOffsetPixels) {
        //页面滑动中执行，这个与ViewPage的回调一致。
    }

    @Override
    public void onPageScrollStateChanged(int state) {
        //页面滑动的状态被改变时执行，也是与ViewPager的回调一致。
    }
});
```

* * *
### License
```
Copyright 2016 kelin410@163.com

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```