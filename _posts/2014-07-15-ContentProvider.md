---
layout: posts
title: "ContentProvider使用"
---

## ContentProvider获取联系人信息
--------------------------
Android SDK提供了很多Content Provider可以读写系统的信息，例如，读取系统联系人的信息，读取收信箱的短信内容，这些都需要通过query方法来查询，当然，通过与其对应的insert、delete和update方法也可以修改这些数据。    
### 先来看一下ContentProvider的工作流程，如图所示[来自这里](http://androidexample.com/Content_Provider_Basic/index.php?view=article_discription&aid=120&aaid=140)<br>
![ContentProvider工作流程](/images/android/Content_provider_basic_workflow.png)       
### 再看表结构    
![ContentProvider表结构](/images/android/Conentprovider_table_structure.png)    
调用其他程序的Content Provider与调用Activity类似，也需要一个字符串来描述。调用Activity叫Activity Action，调用Content Provider叫URL，而且这个URL必须以"content://"开头。<br>
通过Content Provider查询系统数据需要调用ContentResolver.query方法，该返回的定义如下：<br>
<font size=4px>
<xmp class="prettyprint linenums">
public final Cursor query(
				Uri uri, 
				String[] projection,
	            String selection, 
	            String[] selectionArgs, 
	            String sortOrder) {
	    }
</xmp>
</font>
##### 参数的详细说明：
* uri:表示ContentProvider的Uri，例如，Uri.parse("content://sms/inbox");
* projection:相当于SQL语句中select和from之间的部分，也就是查询结果返回的字段，例如，"name,salary"。
* selection:相当于SQL语句中where子句后面的部分，也就是查询条件。例如，"name=? and salary>?"。
* selectionArgs:如果selection参数的值包含了参数，也就是问号（？），selectionArgs则表示这些参数值。如果selection参数的值不包含任何参数，selectionArgs参数值为null，例如，new String[]{"bill", "1200"}
* sortOrder:相当于SQL语句中order by子句后面的部分，也就是要排序的字段，例如，"name, salary desc"。
<b>注意：</b>在设置projection、selection、sortOrder这几个参数时，不能加预期对应的关键字，例如，projection参数值不能设成"select name,salary"，而应设成"name,salary"。如果不想设置这些参数，可以将他们的值设为null。
(From《Android 开发权威指南》)      
以获取联系人信息为例对这些参数再说明:     
<b>CONTENT_URI:</b>一个内容URI是用于标识provider中的数据，类似SQL查询的表名    
![CONTENT_URI](/images/android/Content_uri_description.png)    
<b>projection:</b>该参数定义了你想从表中取的列数据，如    
<font size=4px>
<xmp class="prettyprint linenums">
String[] mProjection =
{
    // Contract class constant for the _ID column name
    ContactsContract.Contacts._ID, 
    // Contract class constant for the DISPLAY_NAME column name
    ContactsContract.Contacts.DISPLAY_NAME 
};
</xmp>
</font>
<b>selection:</b>查询条件，如：ContactsContract.CommonDataKinds.Phone.CONTACT_ID + " = ?"    
#### 读取联系人信息的权限要求：
* 读取联系人权限：&lt;uses-permission android:name="android.permission.READ_CONTACTS"/>
* 添加联系人权限：&lt;uses-permission android:name="android.permission.WRITE_CONTACTS"/>


#### 联系人相关的uri
* content://com.android.contacts/contacts 操作的数据是联系人信息Uri
* content://com.android.contacts/data/phones 联系人电话Uri
* content://com.android.contacts/data/emails 联系人Email Uri
###比较实用的封装方法<br>
1.查询联系人信息(姓名，电话号码，Email)<br>
<font size=4px>
<xmp class="prettyprint linenums">
/**
	 * 查询联系人信息，并返回集合对象
	 * ContactInfo类是JavaBean类，保存联系人的相关信息 
	 */
	private List<ContactInfo> getListInfo() {
		List<ContactInfo> listInfo = new ArrayList<ContactInfo>();
		// 查找联系人信息
		Cursor cursor = getContentResolver().query(
				ContactsContract.Contacts.CONTENT_URI, null, null, null, null);
		while (cursor.moveToNext())// 遍历查询结果，遍历每个联系人的详细信息【姓名，电话号码，Email等】
		{
			StringBuffer sbNumber = new StringBuffer();
			StringBuffer sbEmail = new StringBuffer();
			// 获得联系人ID
			String contactId = cursor.getString(cursor
					.getColumnIndex(ContactsContract.Contacts._ID));
			// 每个联系人可能有多个手机号码
			Cursor phones = getContentResolver().query(
					ContactsContract.CommonDataKinds.Phone.CONTENT_URI,
					null,
					ContactsContract.CommonDataKinds.Phone.CONTACT_ID + "="
							+ contactId, null, null);
			while (phones.moveToNext()) {
				String phone = phones
						.getString(phones
								.getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER));
				sbNumber.append(phone + "\n");
			}
			phones.close();
			// 查询Email
			Cursor emails = getContentResolver().query(
					ContactsContract.CommonDataKinds.Email.CONTENT_URI,
					null,
					ContactsContract.CommonDataKinds.Email.CONTACT_ID + "="
							+ contactId, null, null);
			while (emails.moveToNext()) {
				// 获得多个email
				String emailAddress = emails
						.getString(emails
								.getColumnIndex(ContactsContract.CommonDataKinds.Email.DATA));
				sbEmail.append(emailAddress + "\n");
			}
			emails.close();
			String name = cursor.getString(cursor
					.getColumnIndex(ContactsContract.Contacts.DISPLAY_NAME));
			ContactInfo contactInfo = new ContactInfo();
			contactInfo.setName(name);
			contactInfo.setNumber(sbNumber.toString());
			contactInfo.setEmail(sbEmail.toString());
			listInfo.add(contactInfo);
		}// while
		return listInfo;
	}
</xmp>
</font>
可以将联系人信息显示在自定义布局ListView上并添加响应事件操作<br>
<font size=4px>
<xmp class="prettyprint linenums">
protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.search);
		listView = (ListView) findViewById(R.id.listView);
		lists = new ArrayList<Map<String, Object>>();
		listInfo = getListInfo();
		for (int i = 0; i < listInfo.size(); i++) {
			contactInfo = listInfo.get(i);
			Map<String, Object> map = new HashMap<String, Object>();
			map.put("name", contactInfo.getName());
			map.put("email", contactInfo.getEmail());
			map.put("number", contactInfo.getNumber());
			lists.add(map);
		}
		SimpleAdapter adapter = new SimpleAdapter(this, lists,
				R.layout.list_view, new String[] { "name", "number", "email" },
				new int[] { R.id.textName, R.id.textNumber, R.id.textEmail });
		listView.setAdapter(adapter);
		listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
			@Override
			public void onItemClick(AdapterView<?> parent, View view,
					int position, long id) {
				TextView textView = (TextView) ((LinearLayout) view)
						.getChildAt(1);
				Toast.makeText(search.this, textView.getText(),
						Toast.LENGTH_SHORT).show();
			}
		});
	}
</xmp>
</font>
2.添加联系人信息<br>
<font size=4px>
<xmp class="prettyprint linenums">
/**
	 * 添加联系人信息
	 * 
	 * @param listInfo 联系人信息集合，封装联系人信息对象
	 */
	private void addContactInfo(List<ContactInfo> listInfo) {
		for (int i = 0; i < listInfo.size(); i++) {
			ContentValues values = new ContentValues();
			//首先向RawContacts.CONTENT_URI执行一个空值插入，目的是获取系统返回的rawContactId 
			Uri rawContactUri = getContentResolver().insert(
					RawContacts.CONTENT_URI, values);
			long rawContactId = ContentUris.parseId(rawContactUri);
			// 往data表插入姓名数据
			values.clear();
			values.put(Data.RAW_CONTACT_ID, rawContactId);
			values.put(Data.MIMETYPE, StructuredName.CONTENT_ITEM_TYPE);// 设置内容类型
			values.put(StructuredName.GIVEN_NAME, listInfo.get(i).getName());// 设置联系人名字
			getContentResolver().insert(
					android.provider.ContactsContract.Data.CONTENT_URI, values);
			// 往data表插入phone数据
			values.clear();
			values.put(Data.RAW_CONTACT_ID, rawContactId);
			values.put(Data.MIMETYPE, Phone.CONTENT_ITEM_TYPE);
			values.put(Phone.NUMBER, listInfo.get(i).getNumber());
			values.put(Phone.TYPE, Phone.TYPE_MOBILE);// 设置电话类型
			getContentResolver().insert(
					android.provider.ContactsContract.Data.CONTENT_URI, values);
			// 往data表插入Email数据
			values.clear();
			values.put(Data.RAW_CONTACT_ID, rawContactId);
			values.put(Data.MIMETYPE, Email.CONTENT_ITEM_TYPE);
			values.put(Email.DATA, listInfo.get(i).getEmail());
			values.put(Email.TYPE, Email.TYPE_WORK);// 设置电子邮件类型
			getContentResolver().insert(
					android.provider.ContactsContract.Data.CONTENT_URI, values);
			Toast.makeText(MainActivity.this, "添加成功", Toast.LENGTH_SHORT)
					.show();
		}
	}
</xmp>
</font>
3.删除联系人<br>
可以根据姓名求对应ID进而删除，核心代码如下：<br>
<font size=4px>
<xmp class="prettyprint linenums">
Uri uri = Uri.parse("content://com.android.contacts/raw_contacts");
			// 根据姓名求id
			cursor = contentResolver.query(uri, new String[] { Data._ID }, "display_name=?", 
					new String[] { name }, null);
			if (cursor.moveToFirst()) {// 根据id删除data表中相应数据
				int id = cursor.getInt(0);
				contentResolver.delete(uri, "display_name=?", new String[] { name });
				uri = Uri.parse("content://com.android.contacts/data");
				contentResolver.delete(uri, "raw_contact_id=?", new String[] { id + "" });
				Toast.makeText(MainActivity.this, "删除联系人成功", Toast.LENGTH_SHORT).show();
			}
			cursor.close();
</xmp>
</font>