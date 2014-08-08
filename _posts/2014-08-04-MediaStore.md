---
layout: posts
title: "Android多媒体数据库之MediaStore"
---    
## 多媒体数据库MediaStore
-------------------------------------------  
MediaStore类是Android系统提供的一个多媒体数据库，Android中的多媒体信息都可以从这里提取。这个MediaStore包括了多媒体数据库的所有信息，包括音频、视频和图像，Android把所有的多媒体数据库接口进行了封装，所有的数据库不用自己进行创建。MediaStore包含三个内部类，分别为；    
MediaStore.Audio:存放音频信息    
MediaStore.Image:存放图片信息    
MediaStore.Video:存放视频信息    
当手机开机、插拔USB连接时，Android系统会启动MediaScanner，扫描SD卡和内存里的文件。扫描的结果保存在/data/data/com.android.media/providers/databases/external下。通过DDMS的File Explorer即可查看这个文件，文件包含了图片、音乐、视频等文件的信息。因此，如果程序中要获得这些信息，不需要逐个遍历文件，只要使用ContentProvider就可以获取SD卡中的相关信息了。下面将分别介绍。    
### 获取音乐文件
<font size=4px>
<xmp class="prettyprint linenums">
//初始化音乐列表，包括音乐集和更新显示列表
	private void initMusicList() {
		// 首先获取ContentResolver实例
		ContentResolver resolver = getContentResolver();
		// 选择音乐文件数据，并存储到Cursor对象中
		Cursor cursor = resolver.query(
				MediaStore.Audio.Media.EXTERNAL_CONTENT_URI, null, null, null,
				null);
		//Google强烈建议在加载数据时，使用LoaderManager及其相关的机制。
		SimpleCursorAdapter adapter = new SimpleCursorAdapter(this,
				android.R.layout.simple_list_item_2, cursor, new String[] {
						MediaStore.Audio.AudioColumns.TITLE,
						MediaStore.Audio.AudioColumns.ARTIST }, new int[] {
						android.R.id.text1, android.R.id.text2 });
		listView.setAdapter(adapter);
	}
</xmp>
</font>
对上面代码的解析：    
ContentResolver：数据选择器，用于筛选需要的数据；     
Cursor：可随机访问的结果集，用于保存数据库查询结果；     
MediaStore：基于SQLite的多媒体数据库，它包含了音频、视频、图片等所有多媒体文件的信息；    
MediaStore.Audio.Media.EXTERNAL_CONTENT_URI：URI类的静态对象，它存储了歌曲的路径；     
MediaStore.Audio.AudioColumns.TITLE：歌曲标题在Cursor对象中的列名；     
MediaStore.Audio.AudioColumns.ARTIST：歌曲艺术家在Cursor对象中的列名。     
从数据库中查询歌曲信息并保存在数据库中：     
<font size=4px>
<xmp class="prettyprint linenums">
public List<Mp3Info> getMp3Infos() {  
    Cursor cursor = getContentResolver().query(  
        MediaStore.Audio.Media.EXTERNAL_CONTENT_URI, null, null, null,  
        MediaStore.Audio.Media.DEFAULT_SORT_ORDER);  
    List<Mp3Info> mp3Infos = new ArrayList<Mp3Info>();  
    for (int i = 0; i < cursor.getCount(); i++) {  
        Mp3Info mp3Info = new Mp3Info();  
        cursor.moveToNext();  
        long id = cursor.getLong(cursor  
            .getColumnIndex(MediaStore.Audio.Media._ID));   //音乐id  
        String title = cursor.getString((cursor   
            .getColumnIndex(MediaStore.Audio.Media.TITLE)));//音乐标题  
        String artist = cursor.getString(cursor  
            .getColumnIndex(MediaStore.Audio.Media.ARTIST));//艺术家  
        long duration = cursor.getLong(cursor  
            .getColumnIndex(MediaStore.Audio.Media.DURATION));//时长  
        long size = cursor.getLong(cursor  
            .getColumnIndex(MediaStore.Audio.Media.SIZE));  //文件大小  
        String url = cursor.getString(cursor  
            .getColumnIndex(MediaStore.Audio.Media.DATA));              //文件路径  
    int isMusic = cursor.getInt(cursor  
        .getColumnIndex(MediaStore.Audio.Media.IS_MUSIC));//是否为音乐  
    if (isMusic != 0) {     //只把音乐添加到集合当中  
        mp3Info.setId(id);  
        mp3Info.setTitle(title);  
        mp3Info.setArtist(artist);  
        mp3Info.setDuration(duration);  
        mp3Info.setSize(size);  
        mp3Info.setUrl(url);  
        mp3Infos.add(mp3Info);  
        }  
    }  
return mp3Infos;  
}  
</xmp>
</font>
### 获取图像
MediaStore.Images.Media.EXTERNAL_CONTENT_URI存储了图像的路径，下面代码可以获取SD卡上的图像信息    
<font size=4px>
<xmp class="prettyprint linenums">
private List<String> getImageSrcs(Context context) {
		List<String> lists = new ArrayList<String>();
		ContentResolver resolver = context.getContentResolver();
		// 获取外部存储卡上的图片
		Cursor cursor = resolver.query(
				MediaStore.Images.Media.EXTERNAL_CONTENT_URI, null, null, null,
				null);
		int totalCount = cursor.getCount();
		cursor.moveToFirst();// cursor移动到第一行
		for (int i = 0; i < totalCount; i++) {
			String src = cursor.getString(cursor
					.getColumnIndex(MediaStore.Images.Media.DATA));
			lists.add(src);
			cursor.moveToNext();
		}
		cursor.close();// 关闭游标
		return lists;
	}
</xmp>
</font>
当要生成缩略图时，可以使用：`Bitmap bitmap = MediaStore.Images.Thumbnails.getThumbnail(MainActivity.this.getContentResolver(), Integer.parseInt(id), Thumbnails.MICRO_KIND, null);`     
<font size=4px>
<xmp class="prettyprint linenums">
protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);
		btn = (Button) findViewById(R.id.button1);
		imageView = (ImageView) findViewById(R.id.imageView);
		imageView2 = (ImageView) findViewById(R.id.imageView2);
		imageSrcs = new ArrayList<Map<String, String>>();
		imageSrcs = getImageSrcs(this);
		if (imageSrcs.size() == 0) {
			Toast.makeText(this, "没有发现图像", Toast.LENGTH_SHORT).show();
			return;
		}
		btn.setOnClickListener(new View.OnClickListener() {
			@Override
			public void onClick(View v) {
				Map map = imageSrcs.get(num);
				String src = (String) map.get("src");
				String id = (String) map.get("id");
				// 将指定图像(由ID指定)生成缩略图
				Bitmap bitmap = MediaStore.Images.Thumbnails.getThumbnail(
						MainActivity.this.getContentResolver(),
						Integer.parseInt(id), Thumbnails.MICRO_KIND, null);
				imageView.setImageURI(Uri.parse(src));
				imageView2.setImageBitmap(bitmap);
				num++;
				if (num == imageSrcs.size())
					num = 0;
			}
		});
	}
	private List<Map<String, String>> getImageSrcs(Context context) {
		List<Map<String, String>> lists = new ArrayList<Map<String, String>>();
		ContentResolver resolver = context.getContentResolver();
		// 获取外部存储卡上的图片
		Cursor cursor = resolver.query(
				MediaStore.Images.Media.EXTERNAL_CONTENT_URI, null, null, null,
				null);
		int totalCount = cursor.getCount();
		cursor.moveToFirst();// cursor移动到第一行
		for (int i = 0; i < totalCount; i++) {
			Map<String, String> map = new HashMap<String, String>();
			String src = cursor.getString(cursor
					.getColumnIndex(MediaStore.Images.Media.DATA));
			int id = cursor.getInt(cursor
					.getColumnIndex(MediaStore.Images.Media._ID));
			map.put("src", src); // 保存图像路径
			map.put("id", String.valueOf(id));// 保存图像ID
			lists.add(map);
			cursor.moveToNext();
		}
		cursor.close();// 关闭游标
		return lists;
	}
</xmp>
</font>
`MediaStore.Images.Thumbnails.EXTERNAL_CONTENT_URI`存储了图像对应的缩略图的路径，因此，可以使用下面代码获取缩略图：     
<font size=4px>
<xmp class="prettyprint linenums">
private List<String> getThumbnails(Context context) {
	List<String> lists = new ArrayList<String>();
	ContentResolver resolver = context.getContentResolver();
	Cursor cursor = resolver.query(
			MediaStore.Images.Thumbnails.EXTERNAL_CONTENT_URI, null, null,
			null, null);
	cursor.moveToFirst();
	for (int i = 0; i < cursor.getCount(); i++) {
		String src = cursor.getString(cursor
				.getColumnIndex(MediaStore.Images.Thumbnails.DATA));
		lists.add(src);
		cursor.moveToNext();
	}
	cursor.close();
	return lists;
}
</xmp>
</font>