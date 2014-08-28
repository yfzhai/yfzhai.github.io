---
layout: posts
title: "Android资源汇总"
---    
## Android各种资源大合集，持续更新……
-------------------------------------------  
### 主题和样式
在讨论这一部分之前，先来讨论一个大问题，主题和样式的区别。      
**主题：**Theme是针对窗体级别的，改变窗体样式。在application和activity标签下使用。     
**样式：**Style是针对窗体元素级别的，改变指定控件或者Layout的样式。在具体控件下使用。     
那么怎么自定义主题和样式呢？按以下步骤：     
1、在res/values目录下新建一个名叫style.xml的文件      
2、对于每一个主题和样式，给&lt;style&gt;元素增加一个全局唯一的名字，和一个可选的父类属性      
3、在&lt;style&gt;元素内部，申明一个或者多个&lt;item&gt;,每一个&lt;item&gt;定义了一个名字属性，并且在元素内部定义了这个风格的值      
4、然后可以在其他XML资源，manifest或应用程序代码中引用这些自定义资源      
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
### Android字符串
1、Android系统提供了对简单的HTML标签的支持，方便开发者设置格式化的文本内容，比如斜体、粗体等。 通过`android.text.Html.fromHtml(String source)`，函数可以获取一个格式化后的文本显示对象。如果您的字符串在 strings.xml 文件中，则需要转义HTML标签，不然的话 经过Android处理后 所有的HTML标签都给过滤掉了。需要把所有的“<”用“&lt;”替换，例如:`<string name=”htmlText”>&lt;strong>粗体&lt;/strong></string>`，如果文本内容比较长， 则转义起来比较麻烦，并且阅读也不太方便，这种情况下可以使用XML的CDATA标记， 如下所示：   
<font size=4px>
<xmp class="prettyprint linenums">
<string name="htmlText"> <![CDATA[ <p>HTML标记的文本内容 
<strong>粗体</strong><em>斜体</em></p> 
<p> <tt>teletype-style 字体</tt></p> ]]> 
</string>
</xmp>
</font>
2、字体大小     
对于能够显示文字的控件（如TextView、EditText、RadioButton、Button、CheckBox、Chronometer等等），有时需要控制字体的大小。Android平台定义了三种字体大小。
<font size=4px>
<xmp class="prettyprint linenums">
"?android:attr/textAppearanceLarge" 
"?android:attr/textAppearanceMedium" 
"?android:attr/textAppearanceSmall" 
//使用方法为
android:textAppearance="?android:attr/textAppearanceLarge"   
android:textAppearance="?android:attr/textAppearanceMedium"     
android:textAppearance="?android:attr/textAppearanceSmall" 
//或
style="?android:attr/textAppearanceLarge"   
style="?android:attr/textAppearanceMedium"     
style="?android:attr/textAppearanceSmall" 
</xmp>
</font>
例如，`android:textAppearance`用于设置文字外观，如`?android:attr/textAppearanceLargeInverse`，这里引用的是系统自带的一个外观，？表示系统是否有这种外观，否则使用默认的外观。     
3、字体颜色
<font size=4px>
<xmp class="prettyprint linenums">
//Android平台字体颜色
android:textColor="?android:attr/textColorPrimary"
android:textColor="?android:attr/textColorSecondary"
android:textColor="?android:attr/textColorTertiary"
android:textColor="?android:attr/textColorPrimaryInverse"
android:textColor="?android:attr/textColorSecondaryInverse"
</xmp>
</font>
4、Android的ProgressBar样式
<font size=4px>
<xmp class="prettyprint linenums">
//水平进度条
style="?android:attr/progressBarStyleHorizontal"
//超大号圆形进度条
style="?android:attr/progressBarStyleLarge"
//小号圆形进度条
style="?android:attr/progressBarStyleSmall"
//标题性进度条
style="?android:attr/progressBarStyleSmallTitle"
</xmp>
</font>
5、分隔符
<font size=4px>
<xmp class="prettyprint linenums">
//横向
<View
android:layout_width="fill_parent"
android:layout_height="1dip"
android:background="?android:attr/listDivider" />
//纵向
<View android:layout_width="1dip"
android:layout_height="fill_parent"
android:background="?android:attr/listDivider" />
</xmp>
</font>
6、CheckBox样式
<font size=4px>
<xmp class="prettyprint linenums">
style="?android:attr/starStyle"
</xmp>
</font>
7、类似标题栏效果的TextView
<font size=4px>
<xmp class="prettyprint linenums">
style="?android:attr/listSeparatorTextViewStyle"
</xmp>
</font>
8、其它有用的样式
<font size=4px>
<xmp class="prettyprint linenums">
android:layout_height="?android:attr/listPreferredItemHeight"
android:paddingRight="?android:attr/scrollbarSize"
style="?android:attr/windowTitleBackgroundStyle"
style="?android:attr/windowTitleStyle"
android:layout_height="?android:attr/windowTitleSize"
android:background="?android:attr/windowBackground"
</xmp>
</font>
9、修改Activity的标题栏样式
<font size=4px>
<xmp class="prettyprint linenums">
//如在styles.xml中增加
<resources> 
    <style name="AutoWindowTitleBackground"> 
        <item name="android:background">#778899</item> 
    </style> 
    <style name="autoWindowTitlebar" parent="android:Theme"> 
        <item name="android:windowTitleSize">32dp</item>
        <item name="android:windowTitleBackgroundStyle">@style/AutoWindowTitleBackground
	</item>
    </style> 
</resources>
//接着再修改AndroidManifest.xml文件，找到要自定义标题栏的Activity，
//添加上android:theme值，比如：
<activity android:name=".MainActivity" android:theme="@style/autoWindowTitlebar">
</xmp>
</font>
10、去掉所有Activity界面的标题栏     
修改AndroidManifest.xml，在application 标签中添加`android:theme=”@android:style/Theme.NoTitleBar”`。
<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>

<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>

<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>

<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>

<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>

<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>

<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>

<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>