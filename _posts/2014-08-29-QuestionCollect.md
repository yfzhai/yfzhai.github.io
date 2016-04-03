---
layout: posts
title: "Android项目问题汇总"
---

## Android项目问题集
----------------------------------
### Activity动画演示【工程：AnimaProjectDemo】
1、Android4.1增强了通知功能，在Activity中添加属性`android:parentActivityName=".MainActivity"`，可实现点击通知打开ResultActivity时，再按返回键，回到MainActivity，而不是回到之前的Task。     
2、在Activity中添加属性`android:uiOptions="splitActionBarWhenNarrow"`来启动拆分操作栏；在窄屏的手持设备上，启用拆分操作栏模式可以让系统将操作栏拆分成多个独立的部分。如下图所示，带有品牌打造和导航部分的操作栏布局在屏幕顶部，操作部分则与屏幕底部对齐。      
![Navigation](/images/Android/example.jpg)
3、用meta-data配置参数      
在Android中，有四个地方可以使用meta-data来配置参数，分别有activity、service、application和receiver中，使用方法如下：      
A.在Activity的XML里配置如下：
<font size=4px>
<xmp class="prettyprint linenums">
<activity android:name=".MyActivity" android:label="@string/app_name">  
    <intent-filter>  
	<action android:name="android.intent.action.MAIN" />  
	<category android:name="android.intent.category.LAUNCHER" />  
    </intent-filter>  
    <meta-data android:name="activity_name" android:value="activity_value" />  
</activity>  
//Java代码如下
ActivityInfo actInfo = mContext.getPackageManager().getActivityInfo(  
	    getComponentName(), PackageManager.GET_META_DATA);  
    String msg = actInfo.metaData.getString("activity_name");  
</xmp>
</font>
B.在service中
<font size=4px>
<xmp class="prettyprint linenums">
<service android:name=".MyService">  
     <meta-data android:name="service_name" android:value="service_value" />  
</service>  
//Java代码如下
ComponentName cn = new ComponentName(this, MyService.class);  
        try {  
            ServiceInfo serInfo = this.getPackageManager().getServiceInfo(cn,  
                    PackageManager.GET_META_DATA);  
        } catch (NameNotFoundException e) {  
            e.printStackTrace();  
        }  
        String msg = serInfo.metaData.getString("service_name");  
</xmp>
</font>
C.在application中
<font size=4px>
<xmp class="prettyprint linenums">
<meta-data android:name="application_name" android:value="application_value" />
//Java代码如下
ApplicationInfo appInfo = this.getPackageManager().getApplicationInfo(  
                    getPackageName(), PackageManager.GET_META_DATA);  
            String msg = appInfo.metaData.getString("application_name");  
</xmp>
</font>
D.在receiver中
<font size=4px>
<xmp class="prettyprint linenums">
<receiver android:name=".MyReceiver">  
    <meta-data android:name="receiver_name" android:value="receiver_value" />  
    <intent-filter>  
	<action android:name="android.intent.action.PHONE_STATE" />  
    </intent-filter>  
</receiver>  
//在Java代码中
if (TextUtils.equals("android.intent.action.PHONE_STATE", intent  
	.getAction())) {  
    ComponentName cn = new ComponentName(context, MyReceiver.class);  
    try {  
	ActivityInfo info = context.getPackageManager().getReceiverInfo(cn,  
		PackageManager.GET_META_DATA);  
    } catch (NameNotFoundException e) {  
	e.printStackTrace();  
    }  
    String msg = info.metaData.getString("receiver_name");  
    // 打电话测试即可  
    Toast.makeText(context, msg, Toast.LENGTH_SHORT).show();  
}  
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