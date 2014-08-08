---
layout: posts
title: "Android数据存储"
---    
## Android中的数据存储
-------------------------------------------  
在Android系统中提供了多种存储技术，这些存储技术可以将数据保存在各种存储介质上。例如，SharedPreferences可以将数据保存在应用软件的私有存储区，这些存储区中的数据只能被写入这些数据的软件读取。除此之外，Android系统还支持文件存储、SQLite数据库、OBB文件、云存储等。    
### 使用Properties保存程序配置
Properties对象是一个具有键值对的Hashtable，其键值对都是String类型。在程序开发中，有时需要改变应用的主题，并且在下次启动时显示的是已保存的主题，这时候，需要对设置的主题进行保存，这可以通过Properties类来实现。下面代码是一个保存并获取主题的工具类：     


<font size=4px>
<xmp class="prettyprint linenums">
//保存主题的工具类，只保存主题的名字
public class PropertyBean {
	public static String[] THEMES;
	private static String DEFAULT_THEME;
	//应用上下文
	private Context context = null;
	private String theme;
	public PropertyBean(Context context){
		this.context = context;
		//获取array.xml中的主题名称
		THEMES = context.getResources().getStringArray(R.array.theme);
		DEFAULT_THEME = THEMES[0];
		loadTheme();
	}
	//读取主题，保存在文件"configuration.cfg"中
	private void loadTheme(){
		Properties properties = new Properties();
		try {
			FileInputStream stream = context.openFileInput("configuration.cfg");
			properties.load(stream);
			theme = properties.getProperty("theme").toString();
		} catch (Exception e) {
			saveTheme(DEFAULT_THEME);
		}
	}
	private boolean saveTheme(String theme){
		Properties properties = new Properties();
		properties.put("theme", theme);
		try{
			FileOutputStream stream = context.openFileOutput("configuration.cfg", Context.MODE_PRIVATE);
			properties.store(stream, "");
			return true;
		}catch(Exception e){
			return false;
		}
	}
	public String getTheme(){
		return theme;
	}
	public void setAndSaveTheme(String theme){
		this.theme = theme;
		saveTheme(theme);
	}
}
</xmp>
</font>
生成的configuration.cfg文件位于/data/data/包路径/files/下    
[工程下载](http://pan.baidu.com/s/1jGA7eOe)
<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>
