---
layout: posts
title: "垂直进度条"
---

## 自定义垂直进度条
Android SDK没有提供垂直进度条组件，因此可以自己绘制垂直进度条。绘制进度条有多种方法，最简单的方法是使用图像剪切资源，下面是图像剪切资源的代码：cut.xml<br>

<font size=4px>
<xmp class="prettyprint linenums">
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item>
        <bitmap android:src="@drawable/topbar_bg_clicked"/> <!-- 背景颜色 -->
    </item>
    <item>                                            <!-- 被剪切(进度改变后)的背景颜色 -->
      <clip android:clipOrientation="vertical"
        android:drawable="@drawable/c"
        android:gravity="top">
     </clip>
    </item>
</layer-list>
</xmp>
</font>
<br>
在Activity类中只需要按照以下操作即可:其中ClipDrawable.setLevel方法设置截取的百分比，系统预定义了10000为100%<br>
<font size=4px>
<xmp class="prettyprint linenums">
protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);
		imageView = (ImageView) findViewById(R.id.imageview);
		imageView.setImageResource(R.drawable.cut);<br>
		// 也可以设置ImageView的属性android:src="@drawable/cut"，此时可以按照下面进行图像截取
		//imageView.getDrawable().setLevel(8000);
		imageView.setImageLevel(8000); //从图像顶端截取图像的80%
	}
</xmp>
</font>
![执行结果](/images/android/cut.jpg)<br>