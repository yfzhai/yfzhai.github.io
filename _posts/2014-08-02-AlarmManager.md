---
layout: posts
title: "Android中的定时器AlarmManager"
---    
## Android定时器AlarmManager
-------------------------------------------  
#### AlarmManager定义
AlarmManager是Android中常用的一种系统级别的提示服务，在特定时刻为我们广播一个指定的Intent。简单的说就是我们设定一个时间，然后在该时间到来时，AlarmManager为我们广播一个我们设定的Intent，通常我们使用PendingIntent，PendingIntent可以理解为Intent的封装包，简单的说就是在Intent上再加一个指定的动作。在使用Intent的时候，我们还需要在执行startActivity、startService或sendBroadcast才能使Intent有用，而PendingIntent的话就是将这个动作包含在内了。   
要得到一个PendingIntent对象，可以使用静态方法：getActivity(Context,int,Intent,int)、getBroadcast(Context,int,Intent,int)、getService(Context,int,Intent,int)分别对应着Intent的3个行为，跳转到一个activity组件、打开一个广播组件和打开一个服务组件。参数有4个，比较重要的是第一个和第三个。可以看到，要得到这个对象，必须传入Intent作为参数，必须有context参数。   
PendingIntent是一种特殊的Intent。主要区别在于Intent的执行是立刻的，而PendingIntent的执行不是立即的。PendingIntent的执行实质上是传入的参数Intent的操作，但是使用PendingIntent的目的在于它所包含的Intent的操作的执行是需要满足某些条件的。主要使用的地方和例子：通知Notification的发送；短消息SmsManager的发送和定时器AlarmManger的执行等等。   
#### AlarmManger的常用方法：
<font color="green">1、public void set(int type, long triggerAtMillis, PendingIntent operation)</font>    
该方法用于设置一次性闹钟，第一个参数表示闹钟类型，第二个参数表示闹钟执行时间，第三个参数表示闹钟响应动作。    
<font color="green">2、public void setRepeating(int type, long triggerAtMillis,long intervalMillis, PendingIntent operation)</font>    
该方法用于设置重复闹钟，第一个参数表示闹钟类型，第二个参数表示闹钟首次执行时间，第三个参数表示闹钟两次执行时间间隔，第四个参数表示闹钟响应动作。    
<font color="green">3、public void setInexactRepeating(int type, long triggerAtMillis,long intervalMillis, PendingIntent operation)</font>    
该方法也用于设置重复闹钟，与第二个方法相似，不过使用该方法，闹钟执行的时间间隔不是固定而已。相对而言，该方法更节能一些，因为系统可能会将几个差不多的闹钟合并为一个来执行，减少设备的唤醒次数。    
<font color="green">4、public void cancel(PendingIntent operation)</font>    
取消一个设置的闹钟    
<font color="green">5、public void setTimeZone(String timeZone)</font>    
设置系统的默认时区，需要android.permission.SET_TIME_ZONE权限    
#### 方法参数说明：
* int type    
AlarmManager.ELAPSED_REALTIME:表示闹钟在手机睡眠状态下不可用，该状态下闹钟使用相对时间(相对于系统启动开始)，状态值为3     
AlarmManager.ELAPSED_REALTIME_WAKEUP:表示闹钟在睡眠状态下会唤醒系统并执行提示功能，该状态下闹钟也使用相对时间，状态值为2     
AlarmManager.RTC:表示闹钟在睡眠状态下不可用，该状态下闹钟使用绝对时间，即当前系统时间，状态值为1     
AlarmManager.RTC_WAKEUP:表示闹钟在睡眠状态下会唤醒系统并执行提示功能，该状态下闹钟使用绝对时间，状态值为0     
AlarmManager.POWER_OFF_WAKEUP:表示闹钟在手机关机状态下也能正常进行提示功能，所以是5个状态中用的最多的状态之一，该状态下闹钟也是用绝对时间，状态值为4；不过本状态好像受SDK版本影响，某些版本并不支持     
* long startTime     
闹钟的第一次执行时间，以毫秒为单位，可以自定义时间，不过一般使用当前时间。需要注意的是，本属性与第一个属性（type）密切相关，如果第一个参数对应的闹钟使用的是相对时间（ELAPSED_REALTIME和ELAPSED_REALTIME_WAKEUP），那么本属性就得使用相对时间（相对于系统启动时间来说），比如当前时间就表示为：SystemClock.elapsedRealtime()；如果第一个参数对应的闹钟使用的是绝对时间（RTC、RTC_WAKEUP、POWER_OFF_WAKEUP），那么本属性就得使用绝对时间，比如当前时间就表示为：System.currentTimeMillis()。     
* long intervalTime     
表示两次闹钟执行的间隔时间，也是以毫秒为单位。     
* PendingIntent pi     
是闹钟的执行动作，比如发送一个广播、给出提示等等。PendingIntent是Intent的封装类。需要注意的是，如果是通过启动服务来实现闹钟提示的话，PendingIntent对象的获取就应该采用Pending.getService(Context c,int i,Intent intent,int j)方法；如果是通过广播来实现闹钟提示的话，PendingIntent对象的获取就应该采用PendingIntent.getBroadcast(Context c,int i,Intent intent,int j)方法；如果是采用Activity的方式来实现闹钟提示的话，PendingIntent对象的获取就应该采用PendingIntent.getActivity(Context c,int i,Intent intent,int j)方法。如果这三种方法错用了的话，虽然不会报错，但是看不到闹钟提示效果。     
#### AlarmManager应用案例
本案例使用用户自定广播实现闹钟功能，从当前时间开始，每隔一定时间(比如30秒)提示一次信息，实现步骤如下：    
1、在SendActivity.java中定义一个AlarmManger对象，指定该对象从当前时间开始，每隔30秒向名为"MYALARMRECEIVER"的广播接收器发出一条广播
<font size=4px>
<xmp class="prettyprint linenums">
public class SendActivity extends Activity {
	private Intent intent = null;
	private PendingIntent pendingIntent = null;
	private AlarmManager alarmManager = null;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);
		// 创建Intent对象，action指向广播接收类并附加字符串
		intent = new Intent("MYALARMRECEIVER");
		intent.putExtra("msg", "你该打酱油了");
		// 创建PendingIntent对象封装Intent，由于是使用广播，因此需要使用getBroadcast方法
		//该方法本身就包含了sendBroadcast的动作
		pendingIntent = PendingIntent.getBroadcast(this, 0, intent, 0);
		alarmManager = (AlarmManager) getSystemService(Context.ALARM_SERVICE);
		// 设置闹钟从当前时间开始，每隔30秒执行一次PendingIntent对象，注意第一个参数与第二个参数的关系
		alarmManager.setRepeating(AlarmManager.RTC_WAKEUP,
				System.currentTimeMillis(), 30 * 1000, pendingIntent);
	}
}
</xmp>
</font>
创建广播接收器类MyReceiver.java，在其onReceive方法中获取Intent对象传来的值并用Toast显示
<font size=4px>
<xmp class="prettyprint linenums">
public class MyReceiver extends BroadcastReceiver {
	public MyReceiver() {
	}
	@Override
	public void onReceive(Context context, Intent intent) {
		String msg = intent.getStringExtra("msg");
		Toast.makeText(context, msg, Toast.LENGTH_SHORT).show();
	}
}
</xmp>
</font>
在AndroidManifest.xml中注册广播接收类MyReceiver.java，并设置其action值为"MYALARMRECEIVER"
<font size=4px>
<xmp class="prettyprint linenums">
<receiver
            android:name=".MyReceiver"
            android:exported="false">
            <intent-filter>
                <action android:name="MYALARMRECEIVER"/>
            </intent-filter>
        </receiver>
</xmp>
</font>
[源码下载](http://pan.baidu.com/s/1gdIKBrd)