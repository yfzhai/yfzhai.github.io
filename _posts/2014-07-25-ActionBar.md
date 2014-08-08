---
layout: posts
title: "Android中的ActionBar"
---    
## ActionBar介绍与使用方法  
-------------------------------------------  
#### 什么是ActionBar
ActionBar位于Activity上方，用于显示Activity标题、图标、可以触发的动作、其它组件以及可以相互交互的选项，它也可以用于应用程序的导航。    
ActionBar在target SDK版本至少为11(Android 3.0)的设备上都会显示，通过设置Activity或Application的主题(Theme)可以取消显示ActionBar。默认的Android主题通常都显示ActionBar。
在Android 3.0版本以前(target SDK 版本API小于11)时使用的是options menu，当按下option按钮时就会显示option menu，由于ActionBar总是很容易看到，而option menu需要按下按钮才能显示，因此ActionBar的出现就是为了取代之前的option menu。   
#### 使用ActionBar
ActionBar中的实体通常称作动作(action)，虽然可以通过代码来创建ActionBar中的实体，但是通常的做法是将要创建的实体定义在XML资源文件中。每个菜单都单独的定义在res/menu目录中的文件中，Android系统在R文件中会自动创建该菜单资源文件的引用。下面以创建选项菜单为例来说明ActionBar的使用。   
*首先定义菜单资源文件menu.xml*   
<font size=4px>
<xmp class="prettyprint linenums">
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android" >
	<item android:id="@+id/action_refresh"
	    android:orderInCategory="100"
	    android:showAsAction="always"
	    android:icon="@drawable/black1"
	    android:title="Refresh"/>
	<item android:id="@+id/action_settings"
	    android:title="Settings"/>
</menu>
</xmp>
</font>
showAsAction属性
http://blog.csdn.net/stevenhu_223/article/details/8033153
http://blog.csdn.net/xyz_lmn/article/details/8132420
















<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>