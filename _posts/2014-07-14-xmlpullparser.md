---
layout: posts
title: "XML解析"
---

## 使用XmlPullParser解析XML文档：
XML文档的解析方式一般有三种：DOM、SAX和XmlPullParser。DOM和SAX是Java平台提供的XML解析方式，也同样使用与Android平台，而XmlPullParser是专门用于Android解析XML的类，也是Android开发中首推的XML解析方式，本文讲解了如何通过不同解析方式来解析XML文档<br>
###工程描述<br>
<font color="blue">1.本实例用于解析一个包含employee信息的XML文档，并把结果显示在ListView组件上</font><br>
<font color="blue">2.本实例把XML文件放在res/xml目录中，命名为employees.xml</font><br>
<font color="blue">3.Activity加载后，将把解析结果显示在ListView组件上</font><br>
First Step:先创建XML文件employees.xml并存放在res/xml目录中，内容如下：<br>
<font size=4px>
<xmp class="prettyprint linenums">
<?xml version="1.0" encoding="UTF-8"?>
<employees>
    <employee>
        <id>2163</id>
        <name>Kumar</name>
        <department>Development</department>
        <type>Permanent</type>
        <email>kumar@tot.com</email>
    </employee>
    <employee>
        <id>6752</id>
        <name>Siva</name>
        <department>DB</department>
        <type>Contract</type>
        <email>siva@tot.com</email>
    </employee>
    <employee>
        <id>6753</id>
        <name>Timmy</name>
        <department>Testing</department>
        <type>Permanent</type>
        <email>timmy@tot.com</email>
    </employee>
</employees>
</xmp>
</font>
<br>
Second Step:定义布局文件main.xml，主布局文件只包含一个ListView组件，存放在res/layout目录中，内容如下：<br>
<font size=4px>
<xmp class="prettyprint linenums">
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >
	<ListView android:id="@+id/listview"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"/>
</LinearLayout>
</xmp>
</font>
<br>
Third Step:定义ListView的布局文件list_item.xml<br>
<font size=4px>
<xmp class="prettyprint linenums">
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp"
    android:textSize="16sp" />
</xmp>
</font>
<br>
Fourth Step:创建JavaBean，用于存放信息，Employee.java<br>
<font size=4px>
<xmp class="prettyprint linenums">
package com.zyf.pull;

public class Employee {
	private String name;
	private int id;
	private String department;
	private String type;
	private String email;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getDepartment() {
		return department;
	}

	public void setDepartment(String department) {
		this.department = department;
	}

	public String getType() {
		return type;
	}

	public void setType(String type) {
		this.type = type;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String toString() {
		return id + ":" + name + "\n" + department + "\n" + type + "\n" + email;
	}
}
</xmp>
</font>
<br>
Fifth Step:创建XMLPullParserHandler类用于解析XML文档<br>
<font size=4px>
<xmp class="prettyprint linenums">
package com.zyf.pull;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import org.xmlpull.v1.XmlPullParser;
import org.xmlpull.v1.XmlPullParserException;

import android.content.res.XmlResourceParser;

public class XMLPullParserHandler {
	List<Employee> listEmployee; // 保存全部解析数据，每一个数据都是一个Employee类的对象
	private Employee employee;// 定义Employee对象
	private String text;

	public XMLPullParserHandler() {
		listEmployee = new ArrayList<Employee>();
	}

	public List<Employee> getEmployees() {
		return listEmployee;
	}

	/**
	 * 通过 XmlResourceParser来解析XML文件，
	 * XmlResourceParser的对象通过getResources().getXML() ;获得
	 */
	public List<Employee> parse(XmlResourceParser xmlResourceParser) {
		try {
			int eventType = xmlResourceParser.getEventType(); // 取得操作事件
			while (eventType != XmlPullParser.END_DOCUMENT) { // 如果文档没有结束
				String tagname = xmlResourceParser.getName();
				switch (eventType) {
				case XmlPullParser.START_TAG:
					if (tagname.equalsIgnoreCase("employee")) // 如果是employee节点
						employee = new Employee(); // 创建Employee实例
					break;
				case XmlPullParser.TEXT: // 元素内容
					text = xmlResourceParser.getText(); // 取出元素内容
					break;
				case XmlPullParser.END_TAG: // 结束标记
					if (tagname.equalsIgnoreCase("employee")) // 如果是employee节点
						listEmployee.add(employee); // 添加employee对象到list集合中
					else if (tagname.equalsIgnoreCase("name"))
						employee.setName(text);
					else if (tagname.equalsIgnoreCase("id"))
						employee.setId(Integer.parseInt(text));
					else if (tagname.equalsIgnoreCase("department"))
						employee.setDepartment(text);
					else if (tagname.equalsIgnoreCase("email"))
						employee.setEmail(text);
					else if (tagname.equalsIgnoreCase("type"))
						employee.setType(text);
					break;
				default:
					break;
				}
				eventType = xmlResourceParser.next(); // 下一个事件码
			}
		} catch (XmlPullParserException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return listEmployee;
	}
}
</xmp>
</font>
<br>
Sixth Step:创建Activity类XmlPullParserDemo.java <br>
<font size=4px>
<xmp class="prettyprint linenums">
package com.zyf.pull;

import java.util.List;

import android.app.Activity;
import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.ListView;

public class XmlPullParserDemo extends Activity {
	private ListView listView = null;
	private List<Employee> listEmployee = null;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);
		listView = (ListView) findViewById(R.id.listview);
		XMLPullParserHandler parser = new XMLPullParserHandler();
		listEmployee = parser.parse(getResources().getXml(R.xml.employees));
		ArrayAdapter<Employee> adapter = new ArrayAdapter<Employee>(this,
				R.layout.list_itm, listEmployee);
		listView.setAdapter(adapter);
	}
}
</xmp>
</font>
<br><br>
<b>注意:</b><br>
1、getResources().getXml(R.xml.employees);可以获得路径res/xml下employees.xml文件的XmlResourceParser对象，通过该对象可以很容易的额进行XML的解析操作<br>
2、如果employees.xml文件放在assets目录下，则可以通过getAssets().open("employees.xml");来获得InputStream对象，此时，XMLPullParserHandler.java中的parsse函数需要改为<br>
<font size=4px>
<xmp class="prettyprint linenums">
public List<Employee> parse(InputStream is) {
		XmlPullParserFactory xmlpullparserFactory = null;
		XmlPullParser xmlpullParser = null;
		try {
			xmlpullparserFactory = XmlPullParserFactory.newInstance();
			/*
			 * 解析XML的时候，如果将Namespace打开，则在解析生成document的时候，会查询里面是否有schema，查询xmln。
			 * 因此不要设置setNamespaceAware除非Schema存在
			 */
			xmlpullparserFactory.setNamespaceAware(true);
			xmlpullParser = xmlpullparserFactory.newPullParser();
			xmlpullParser.setInput(is, "UTF-8"); // 设置输入流
			int eventType = xmlpullParser.getEventType();
			while (eventType != XmlPullParser.END_DOCUMENT) {
				String tagname = xmlpullParser.getName();
				switch (eventType) {
				case XmlPullParser.START_TAG:
					if (tagname.equalsIgnoreCase("employee"))
						employee = new Employee(); // 创建Employee实例
					break;
				case XmlPullParser.TEXT:
					text = xmlpullParser.getText();
					break;
				case XmlPullParser.END_TAG:
					if (tagname.equalsIgnoreCase("employee"))
						listEmployee.add(employee); // 添加employee对象到list集合中
					else if (tagname.equalsIgnoreCase("name"))
						employee.setName(text);
					else if (tagname.equalsIgnoreCase("id"))
						employee.setId(Integer.parseInt(text));
					else if (tagname.equalsIgnoreCase("department"))
						employee.setDepartment(text);
					else if (tagname.equalsIgnoreCase("email"))
						employee.setEmail(text);
					else if (tagname.equalsIgnoreCase("type"))
						employee.setType(text);
					break;
				default:
					break;
				}
				eventType = xmlpullParser.next();
			}
		} catch (XmlPullParserException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return listEmployee;
	}
</xmp>
</font>
<br>
3、如果employees.xml文件存放在SD卡的某个目录中(如XML)，则可以从该目录中读取文件输入流<br>
<font size=4px>
<xmp class="prettyprint linenums">
if (!Environment.getExternalStorageState().equals(
				Environment.MEDIA_MOUNTED)) { // 如果sdcard不存在
			return;
		}
		File file = new File(Environment.getExternalStorageDirectory()
				.toString()
				+ File.separator
				+ "XML"
				+ File.separator
				+ "employees.xml");
		if(!file.exists())
			return ;
		try{
			InputStream is = new FileInputStream(file);
			listEmployee = parser.parse(is);
		}catch(IOException e){
			e.printStackTrace();
		}
</xmp>
</font>   
## 使用SAX解析XML文档：   
使用SAX解析方式，需要先定义一个集成DefaultHandler类的解析器MySAXParser.java:   
<font size=4px>
<xmp class="prettyprint linenums">   
package com.zyf.pull;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
/**
 *  使用SAX解析，要定义一个继承DefaultHandler类的解析器类
 *  @author Administrator
 */
import org.xml.sax.helpers.DefaultHandler;

public class MySAXParser extends DefaultHandler {
	private List<Employee> SaxListInfo = null; // 保存全部元素
	private Employee employee; // 定义封装对象
	private String elementName; // 保存元素名称

	@Override
	public void startDocument() throws SAXException { // 文档开始
		SaxListInfo = new ArrayList<Employee>(); // 实例化集合
	}

	@Override
	public void startElement(String uri, String localName, String qName,
			Attributes attributes) throws SAXException {
		if ("employee".equals(localName))
			employee = new Employee();
		this.elementName = localName;
	}

	@Override
	public void endElement(String uri, String localName, String qName)
			throws SAXException { // 元素结束
		if ("employee".equals(localName)) {
			SaxListInfo.add(employee);
			employee = null;
		}
		this.elementName = null;
		super.endElement(uri, localName, qName);
	}

	@Override
	public void characters(char[] ch, int start, int length)
			throws SAXException { // 取得元素内容
		if (this.elementName != null) { // 表示有元素
			String data = new String(ch, start, length); // 取得文字新信息
			if ("id".equals(elementName))
				employee.setId(Integer.parseInt(data)); // 设置id属性
			else if ("name".equals(elementName))
				employee.setName(data); // 设置name属性
			else if ("department".equals(elementName))
				employee.setDepartment(data); // 设置department属性
			else if ("type".equals(elementName))
				employee.setType(data); // 设置type属性
			else if ("email".equals(elementName))
				employee.setEmail(data); // 设置email属性

		}
		super.characters(ch, start, length);
	}

	// 取得全部集合
	public List<Employee> getAll() {
		return this.SaxListInfo;
	}

	//
	public List<Map<String, Object>> getData() {
		List<Map<String, Object>> data = new ArrayList<Map<String, Object>>();
		int size = SaxListInfo.size();
		for (int i = 0; i < size; i++) {
			Map<String, Object> map = new HashMap<String, Object>();
			map.put("id", SaxListInfo.get(i).getId());
			map.put("name", SaxListInfo.get(i).getName());
			map.put("department", SaxListInfo.get(i).getDepartment());
			map.put("type", SaxListInfo.get(i).getType());
			map.put("email", SaxListInfo.get(i).getEmail());
			data.add(map);
		}
		return data;
	}
}
</xmp>
</font> 
解析代码： 
<font size=4px>
<xmp class="prettyprint linenums">
SAXParserFactory factory = SAXParserFactory.newInstance(); //创建工厂类
		SAXParser parser = null;
		MySAXParser sax = new MySAXParser();
		try {
			parser = factory.newSAXParser();  //创建SAX的解析类
			parser.parse(getAssets().open("employees.xml"), sax);
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		List<Employee>listEmployee = sax.getAll();
</xmp>
</font>   
[XML三大解析方式汇总](http://pan.baidu.com/s/1gdGcyCr)   
