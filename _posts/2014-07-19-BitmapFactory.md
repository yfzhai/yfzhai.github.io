---
layout: posts
title: "图像操作"
---    
## 改变图像的大小    
-------------------------------------------
在通过BitmapFactory.decodeFile(String path)方法将图片转成Bitmap时，遇到大一些的图片，经常会遇到OOM(Out Of Memory)的问题。怎么避免它呢？BitmapFactory.Options这个类，有一个字段叫做`inJustDecodeBounds`，SDK中对该字段的说明"If set to true, the decoder will return null (no bitmap), but the out... fields will still be set"。也就是说，如果我们把它设为true，那么BitmapFactory.decodeFile(String path, Options opt)并不会真的返回一个Bitmap，它仅仅把它的宽和高取回来，这样就不会占用太多内存了。   
如果想改变图片的大小，还需要用到BitmapFactory.Options这个类里的inSampleSize成员，我们可以利用该成员实现图像缩放。如果设置为>1的值，要求解码器解码出原始图片的一个子样本，返回一个较小的bitmap，以节省空间。   
例如，inSampleSize==2，则取出的缩略图的宽和高都是原始图像的1/2，图片的大小就是原始图片 大小的1/4。SDK中说明，这个值必须设置为2的指数，因为即使不设置为2的指数，解码时也会被替换为离你设置值最近的2的指数值。   
### 属性inJustDecodeBounds的作用     
<font size=4px>
<xmp class="prettyprint linenums">
/**
	 * 解码图片 
	 */
	private Bitmap decodeBitmap(Context context, int imageId) {
		BitmapFactory.Options options = new BitmapFactory.Options();
		/*
		 * 如果设置为true，那么将不会返回实际的bitmap对象， 也不会被分配存储空间，这就避免了内存溢出
		 */
		options.inJustDecodeBounds = true;
		Bitmap bitmap = BitmapFactory.decodeResource(context.getResources(),
				imageId, options);
		if (bitmap == null) {
			Toast.makeText(context, "bitmap为空", Toast.LENGTH_SHORT).show();
		}
		int realWidth = options.outWidth;
		int realHeight = options.outHeight;
		Toast.makeText(context, "图片实际高度：" + realHeight + "\n实际宽度：" + realWidth,
				Toast.LENGTH_SHORT).show();
		int scal = (int) ((realHeight > realWidth ? realWidth : realHeight) / 100);
		if (scal <= 0)
			scal = 1;
		options.inSampleSize = scal;
		options.inJustDecodeBounds = false;
		bitmap = BitmapFactory.decodeResource(context.getResources(), imageId,
				options);
		// 缩放后的高度和宽度
		int w = bitmap.getWidth();
		int h = bitmap.getHeight();
		if (bitmap != null) {
			Toast.makeText(context, "bitmap不为空", Toast.LENGTH_SHORT).show();
		}
		Toast.makeText(context, "缩放后图片高度：" + h + "\n宽度：" + h,
				Toast.LENGTH_SHORT).show();
		return bitmap;
	}
</xmp>
</font>














<font size=4px>
<xmp class="prettyprint linenums">
/**
	 * 改变快速滑块图像及页面限制
	 * 
	 * @param list 创建的ListView对象
	 */
	private void changeFastScrollerDrawableAndSize(ListView list) {
		//FastScroller.mThumbDrawable变量保存了快速滑块图像
		//首先要通过AbsListView.mFastScroller变量获取FastScroller对象
		try {
			Field f = AbsListView.class.getDeclaredField("mFastScroller");
			/*由于mFastScroller是私有成员，因此要设置访问类型检查，
			 * 只有这样，才能获取和设置该成员对应的值*/
			f.setAccessible(true);
			Object obj = f.get(list);
			//获取FastScroller.mThumbDrawable变量的Field对象
			f = f.getType().getDeclaredField("mThumbDrawable");
			f.setAccessible(true);
			//获取FastScroller.mThumbDrawable变量的值
			Drawable drawable = (Drawable) f.get(obj);
			//装载新的快速滑块图像
			drawable = getResources().getDrawable(R.drawable.c);
			f.set(obj, drawable);//重新设置快速滑块图像
			//改变属性MIN_PAGES的值
			Field minPages = obj.getClass().getDeclaredField("MIN_PAGES");
			minPages.setAccessible(true);
			minPages.set(obj, 1);
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}
</xmp>
</font>     
### 根据屏幕分辨率调整图片大小   
<font size=4px>
<xmp class="prettyprint linenums">
/**
	 * 根据屏幕大小，缩放显示图片
	 */
	private Bitmap adjustPicture(Context context, int imageId) {
		WindowManager wm = (WindowManager) context
				.getSystemService(Context.WINDOW_SERVICE);
		DisplayMetrics dm = new DisplayMetrics();
		wm.getDefaultDisplay().getMetrics(dm);
		int screenWidth = dm.widthPixels;// 获得屏幕宽度
		int screenHeight = dm.heightPixels;// 获得屏幕高度
		BitmapFactory.Options options = new Options();
		// 不去真的解析图片，只是获取图片的头部信息：宽 高
		options.inJustDecodeBounds = true;
		BitmapFactory.decodeResource(context.getResources(), imageId, options);
		int imageWidth = options.outWidth;// 图片宽度
		int imageHeight = options.outHeight;// 图片高度
		// 计算缩放比例(按照大的值进行缩放)
		int scaleX = imageWidth / screenWidth;
		int scaleY = imageHeight / screenHeight;
		int scale = 1;
		// 图片比窗体宽高都宽
		if (scaleX > scaleY && scaleY >= 1)
			scale = scaleX;
		if (scaleY > scaleX && scaleX >= 1)
			scale = scaleY;
		// 现在需要解析图片了，该参数设置为false
		options.inJustDecodeBounds = false;
		options.inSampleSize = scale;
		Bitmap bitmap = BitmapFactory.decodeResource(context.getResources(),
				imageId, options);
		return bitmap;
	}
</xmp>
</font>   
### 根据指定大小缩放图片   
<font size=4px>
<xmp class="prettyprint linenums">
private Bitmap decodeBitmap(Context context, int resourceId, int size) {
		BitmapFactory.Options options = new BitmapFactory.Options();
		options.inJustDecodeBounds = true;
		BitmapFactory.decodeResource(context.getResources(), resourceId,
				options);
		final int REQUIRED_SIZE = size;
		int scale = 1;
		int width = options.outWidth;
		int height = options.outHeight;
		 while (true) {
		 if (width / 2 < REQUIRED_SIZE || height / 2 < REQUIRED_SIZE)
		 break;
		 width /= 2;
		 height /= 2;
		 scale *= 2;
		 }
		options.inJustDecodeBounds = false;
		options.inSampleSize = scale;
		Bitmap bitmap = BitmapFactory.decodeResource(context.getResources(),
				resourceId, options);
		return bitmap;
	}
</xmp>
</font>    
**注意：**当使用decodeFile(pathName, opts)方法解码图片时，pathName必须是SD卡中的路径，而不能是其他目录，比如decodeFile("/sdcard/a.jpg", null);
