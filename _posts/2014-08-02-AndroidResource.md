---
layout: posts
title: "Android资源汇总"
---    
## Android各种资源大合集，持续更新……
-------------------------------------------  
### Android自定义字体
在Android中，用户可以自定义应用程序中的字体，只需要从网上下载所需字体(ttf格式)，放到assets/fonts文件夹中，之后可以通过Java代码进行设置所显示字体。     
首先获得组件对象：TextView tv = (TextView)findViewById(R.id.textView);      
然后调用Typeface类的静态方法createFromAsset()从assets文件夹中获得自定义字体：Typeface custom_font = Typeface.createFromAsset(getAssets(), "fonts/stx.ttf");     
最后一步就是设置所显示字体到TextView组件上：tv.setTypeface(custom_font);     
除了使用createFromAsset()方法，一下方法也可以用于有效的设置字体：    
![设置字体](/images/android/setfont.jpg)     
以上方法使用：     
<font size=4px>
<xmp class="prettyprint linenums">
Typeface custom_font1 = Typeface.create("宋体", Typeface.ITALIC);
Typeface custom_font2 = Typeface.create(custom_font1, Typeface.ITALIC);对中文无效
Typeface custom_font3 = Typeface.defaultFromStyle(Typeface.BOLD_ITALIC);对中文无效
Typeface custom_font4 = Typeface.createFromFile("/mnt/sdcard/Font/SIMLI.ttf");
</xmp>
</font>
### Android主题
主题(Theme)是一系列样式的组合(字体、背景颜色、文字颜色等)。在AndroidManifest.xml中定义的<application>和<activity>元素中加入属性android:theme，可以将一套样式添加到整个程序或某个Activity中。但是主题是不能应用在某个单独的View里。设置主题的语句比如：`android:theme="@android:style/Theme.Light"`，将程序的主题设置为"开灯"，意思是将原来"黑底白字"的界面设置为"白底黑字"，就好像漆黑一片的程序开了灯一样。     
### Android防止控件被连续点击
在开发中经常会遇到这样的情况，一个按钮点击后会弹出Toast或者Dialog，如果快速重复地点击，则Toast会重复地出现。而我们想要的是一定时间内点击只生效一次，或者说这种快速且重复的点击为无效点击。那么解决思路如下：     
1、需要定义一个全局变量`lastClickTime`，用于记录最后点击的时间。     
2、每次点击前需要进行判断，用lastClickTime和当前时间相比较，并且更新最后点击时间，若小于临界值，则算无效点击，不触发事件。具体实现请看如下工具类：     
<font size=4px>
<xmp class="prettyprint linenums">
class CommonUtils{
	private static long lastClickTime;
	public static boolean isFastDoubleClick(){
		long time = System.currentTimeMillis();
		long timeD = time - lastClickTime;
		if(0 < timeD && timeD < 800)
			return true;
		lastClickTime = time;
		return false;
	}
}
</xmp>
</font>
此后，在按钮的点击事件中可以判断`CommonUtils.isFastDoubleClick()`是否为真，进而做相应处理。     
### Animation之transition
transition是一种简单的动画显示。这种动画可以平滑的从一种图片变换为另一张图片，不是那种突兀的切换;transition可以简单的在两帧之间切换，常用于选择与被选择之间切换;XML文件包含切换的帧图片。transition标签作为容器，item为切换用的帧，android:drawable为图片id。TransitionDrawable获取transition中的资源，之后调用startTransition进行切换，该函数将第一帧切换到第二帧。reverseTransition是另一种切换方式，将反复切换两帧，会根据上一次切换的第二帧作为下一次切换的第一帧。下面布局文件可以作为ListView条目被选中时的背景色的渐变效果；使用`android:listSelector="@drawable/list_selector_background_transition"`      
<font size=4px>
<xmp class="prettyprint linenums">
<?xml version="1.0" encoding="utf-8"?>
<!-- ListView选项被按下时的背景色渐变 -->
<transition xmlns:android="http://schemas.android.com/apk/res/android" >

    <item android:drawable="@drawable/playlist_tile_pressed"/>
    <item android:drawable="@drawable/playlist_tile_longpress"/>

</transition>
</xmp>
</font>
例子：
<font size=4px>
<xmp class="prettyprint linenums">
<!-- list_selector.xml -->
<?xml version="1.0" encoding="utf-8"?>
<transition xmlns:android="http://schemas.android.com/apk/res/android" >

    <item android:drawable="@drawable/left"/>
    <item android:drawable="@drawable/right"/>

</transition>
</xmp>
</font>
<font size=4px>
<xmp class="prettyprint linenums">
imageView = (ImageView) findViewById(R.id.arrow);
// 需要设置ImageView组件的src属性为android:src="@drawable/list_selector"
// TransitionDrawable arrowDrawable = (TransitionDrawable)imageView.getDrawable();
// arrowDrawable.reverseTransition(7000);
TransitionDrawable arrowDrawable = (TransitionDrawable) getResources()
		.getDrawable(R.drawable.list_selector);
// 此时不需要设置ImageView的src属性
imageView.setImageDrawable(arrowDrawable);
arrowDrawable.startTransition(5000);
</xmp>
</font>
### Activity启动及退出时的动画显示
在开发过程中，为了增加效果感，有时需要设计Activity启动及推出时的动画，这时可以使用方法`overridePendingTransition()`来达到这种效果。`overridePendingTransition()`用于实现两个Activity切换时的动画，在Activity中使用(需要设置动画效果的Activity中或者调用startActivity()方法的Activity中设置都可以)，有两个参数，分别是进入动画和退出动画。     
**注意：**必须在startActivity()或finish()之后立即调用；而且在2.1版本以上有效；手机设置-显示-动画，要开启状态     
//实现淡入淡出效果
<font size=4px>
<xmp class="prettyprint linenums">
startActivity(new Intent(MainActivity.this,SecondActivity.class));
overridePendingTransition(android.R.anim.fade_in,android.R.anim.fade_out);
</xmp>
</font>
//由左向右滑入效果
<font size=4px>
<xmp class="prettyprint linenums">
startActivity(new Intent(MainActivity.this,SecondActivity.class));
overridePendingTransition(android.R.anim.slide_in_left,android.R.anim.slide_out_right);
</xmp>
</font>
//实现自定义动画效果，在SecondActivity中设置如下
<font size=4px>
<xmp class="prettyprint linenums">
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.activity_second);
	overridePendingTransition(R.anim.push_right_in, R.anim.hold);
}
@Override
protected void onPause() {
	overridePendingTransition(R.anim.hold, R.anim.push_right_out);
	super.onPause();
}
</xmp>
</font>
这样，SecondActivity启动和退出都会显示出效果，其中：
//hold.xml
<font size=4px>
<xmp class="prettyprint linenums">
<set xmlns:android="http://schemas.android.com/apk/res/android" >

    <translate
        android:duration="500"
        android:fromXDelta="0"
        android:toXDelta="0" />

</set>
</xmp>
</font>
//push_right_out.xml
<font size=4px>
<xmp class="prettyprint linenums">
<set xmlns:android="http://schemas.android.com/apk/res/android" >

    <translate
        android:duration="500"
        android:fromXDelta="0"
        android:fromYDelta="0"
        android:toXDelta="100%p"
        android:toYDelta="100%p" />

    <rotate
        android:duration="500"
        android:fromDegrees="0"
        android:pivotX="100%p"
        android:pivotY="100%p"
        android:toDegrees="90" />

    <alpha
        android:duration="500"
        android:fromAlpha="1.0"
        android:toAlpha="0.0" />

</set>
</xmp>
</font>
//push_right_in.xml
<font size=4px>
<xmp class="prettyprint linenums">
<set xmlns:android="http://schemas.android.com/apk/res/android" >

    <translate
        android:duration="500"
        android:fromXDelta="100%p"
        android:fromYDelta="100%p"
        android:toXDelta="0"
        android:toYDelta="0" />

    <rotate
        android:duration="500"
        android:fromDegrees="90"
        android:pivotX="100%p"
        android:pivotY="100%p"
        android:toDegrees="0" />

    <alpha
        android:duration="500"
        android:fromAlpha="0.0"
        android:toAlpha="1.0" />

</set>
</xmp>
</font>
//极具特色的动画效果 zoomin.xml
<font size=4px>
<xmp class="prettyprint linenums">
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:interpolator="@android:anim/decelerate_interpolator" >
  <scale
        android:duration="@android:integer/config_mediumAnimTime"
        android:fromXScale="0.1"
        android:fromYScale="0.1"
        android:pivotX="50%p"
        android:pivotY="50%p"
        android:toXScale="1.0"
        android:toYScale="1.0" />
  <alpha
        android:duration="@android:integer/config_mediumAnimTime"
        android:fromAlpha="0"
        android:toAlpha="1.0" />
</set>
</xmp>
</font>
//zoomout.xml
<font size=4px>
<xmp class="prettyprint linenums">
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:zAdjustment="top" >
    <scale
        android:duration="@android:integer/config_mediumAnimTime"
        android:fromXScale="1.0"
        android:fromYScale="1.0"
        android:pivotX="50%p"
        android:pivotY="50%p"
        android:toXScale="0.1"
        android:toYScale="0.1" />
    <alpha
        android:duration="@android:integer/config_mediumAnimTime"
        android:fromAlpha="1.0"
        android:toAlpha="0" />
</set>
</xmp>
</font>