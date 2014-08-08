---
layout: posts
title: "进度条也可以这样玩"
---

## 自定义进度条
通常使用的进度条是在布局文件中定义组件ProgressBar，本文使用ProgressDialog来创建对话框。ProgressDialog继承自AlertDialog，AlertDialog继承自Dialog，因此，创建ProgressDialog既可以使用new Dialog的方式，也可以调用Dialog的静态方法Dialog.show()。<br>
## ProgressBar和ProgressDialog的不同之处
ProgressBar是一个用来指示任务已完成进度的组件，应用程序可以改变其进度值；而ProgressDialog是一个对话框形式的进度条，用户必须等待进度的完成。<br>
下面直接上代码<br>
<font size=4px>
<xmp class="prettyprint linenums">
private void initProgressBar(Context context) {
		ProgressDialog progressBar = new ProgressDialog(context); // 创建ProgressDialog对象
		progressBar.setMessage("下载文件...");
		progressBar.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL); // 设置水平进度条
		progressBar.setCancelable(false); // 设置是否可以通过点击Back键取消
		progressBar.setCanceledOnTouchOutside(false); // 设置点击Dialog外是否取消Dialog进度条
		progressBar.setProgress(0); // 设置初始进度
		progressBar.setMax(100); // 设置进度最大值
		progressBar.setProgressDrawable(context.getResources().getDrawable(
				R.drawable.customprogressbar)); // 设置进度条显示样式
		progressBar.show(); // 显示进度条
	}
</xmp>
</font>
<br>
在res/drawable目录中建立一个文件customprogress.xml，用来设置进度条的显示效果<br>
<font size=4px>
<xmp class="prettyprint linenums">
<layer-list xmlns:android="http://schemas.android.com/apk/res/android" >

    <!-- 定义进度条的背景属性 -->
    <item android:id="@android:id/background">
        <shape>
            <corners android:radius="10dp" />

            <gradient
                android:angle="270"
                android:centerColor="#FFFFFF"
                android:centerY="1.0"
                android:endColor="#FFFFFF"
                android:startColor="#FFFFFF" />
        </shape>
    </item>
    <!-- 定义第二进度的颜色 -->
    <item android:id="@android:id/secondaryProgress">
        <clip>
            <shape>
                <corners android:radius="10dp" />

                <gradient
                    android:angle="270"
                    android:centerColor="#FFDB00"
                    android:centerY="1.0"
                    android:endColor="#FFCB00"
                    android:startColor="#FFD300" />
            </shape>
        </clip>
    </item>
    <!-- 定义进度的颜色 -->
    <item android:id="@android:id/progress">
        <clip>
            <shape>
                <corners android:radius="10dp" />

                <gradient
                    android:angle="270"
                    android:centerColor="#00FF00"
                    android:centerY="1.0"
                    android:endColor="#06101D"
                    android:startColor="#00FF00" />
            </shape>
        </clip>
    </item>

</layer-list>
</xmp>
</font>
![执行结果](/images/android/progress.jpg)<br>
<b>说明：</b><br>
该设置方法同样适用于SeekBar组件，只是SeekBar组件只有第一进度条，第二级进度条将被忽略。<br>