---
layout: posts
title: "电池充电时的动态电量值图标原来是这么实现的"
---

## 实现电池充电时电量变动图标的效果——LevelListDrawable
XML定义的Drawable的一种，以<level-list>作为根元素，期间可以包含任意多个<item>节点，每一个<item>节点包含一个drawable对象和maxLevel与minLevel值，如:<br>
<font size=4px>
<xmp class="prettyprint linenums">
<?xml version="1.0" encoding="utf-8"?>  
<level-list xmlns:android="http://schemas.android.com/apk/res/android" >  
    <item android:minLevel="0" android:maxLevel="10" android:drawable="@drawable/b1" />  
    <item android:minLevel="11" android:maxLevel="20" android:drawable="@drawable/b2" />  
    <item android:minLevel="21" android:maxLevel="30" android:drawable="@drawable/b3" />      
    <item android:minLevel="31" android:maxLevel="40" android:drawable="@drawable/b4" />  
</level-list>  
</xmp>
</font>
<br>
当我们向LevelListDrawable对象提供一个Level值后，LevelListDrawable对象就会从前往后查看每一个<item>，当某个<item>节点的Level<b>范围</b>满足提供的那个Level值后，就会返回该<item>节点里指定的drawable，并且不会继续往后找。所以定义这个LevelListDrawable时要注意各个<item>的顺序，如：<br>
<font size=4px>
<xmp class="prettyprint linenums">
<?xml version="1.0" encoding="utf-8"?>  
<level-list xmlns:android="http://schemas.android.com/apk/res/android" >  
    <item android:maxLevel="40" android:drawable="@drawable/b4" />     
    <item android:maxLevel="10" android:drawable="@drawable/b1" />  
    <item android:maxLevel="20" android:drawable="@drawable/b2" />  
    <item android:maxLevel="30" android:drawable="@drawable/b3" />      
</level-list>  
</xmp>
</font>
<br>
那么无论提供什么样的Level值，都不会返回后面三个<item>里的drawable（这里如果提供的Level值超过40，将返回一个空对象）。<br>
可以通过Drawable对象的setLevel(int)方法来提供Level值。<br>
比如当我们将一个LevelListDrawable作为一个View的background后，可以通过View的getBackground()方法获取这个Drawable对象，然后调用这个Drawable对象的setLevel()方法，提供不同的Level值，就可以改变View的背景。这个可以用来制作诸如进度条、音量调节等效果。<br>
ImageView组件还提供了setImageLevel()方法来设置android:src指定的LevelListDrawable的Level值<br>
<b>实例:</b><br>
定义icon.xml文件<br>
<font size=4px>
<xmp class="prettyprint linenums">
<?xml version="1.0" encoding="utf-8"?>
<level-list xmlns:android="http://schemas.android.com/apk/res/android" >
	<item android:maxLevel="0" android:drawable="@drawable/black0"/>
	<item android:maxLevel="1" android:drawable="@drawable/black1"/>
	<item android:maxLevel="2" android:drawable="@drawable/black2"/>
	<item android:maxLevel="3" android:drawable="@drawable/black3"/>
	<item android:maxLevel="4" android:drawable="@drawable/black4"/>
</level-list>
</xmp>
</font>
<br>
在Activity类中简单的实现单机按钮显示对应Level值的图标<br>
<font size=4px>
<xmp class="prettyprint linenums">
btn.setOnClickListener(new View.OnClickListener() {
			@Override
			public void onClick(View v) {
				current++;
				if(current >= 5)
					current = 0;
				imageView.setImageLevel(current);
			}
		});
</xmp>
</font>
<br>