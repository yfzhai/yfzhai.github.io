---
layout: posts
title: "改变ListView快速滑块"
---    
## 修改ListView组件上的快速滑块图像    
-------------------------------------------
使用过新浪微博Android客户端会知道当快速滑动微博列表时右侧会出现一个小方块，这就是列表的快速滑块。如果停止滑动列表，大约1秒左右，滑块就会消失。那么如果为ListView组件加上快速滑块呢？以及是否可以修改快速滑块显示图像呢？答案是肯定的，按照以下步骤操作即可。   

#### 显示快速滑块   
使用布局文件需要将android:fastScrollEnabled属性设置为true，如果使用Java代码需要调用ListView.setFastScrollEnabled(true)方法。   
#### 改变快速滑块的图像   
ListView组件没有提供修改快速滑块的API，因此，不能直接修改快速滑块图像。这时候查看ListView类的源代码（鼠标移动到ListView上，按下Ctrl键，单击就跳转到ListView类文件上），但是在ListView中找不到有用信息，再看ListView继承自AbsListView类，此时再去查看AbsListView类源码，发现有一个属性：_private FastScroller mFastScroller;_，再查找FastScroller类，发现无法找到，此时通过搜索android源码，发现FastScroller.java中有一个成员mThumbDrawable，而该变量保存了快速滑块图像，因此可以通过该变量可以达到修改快速滑块图像的目的，这需要通过反射机制来实现。还有一点需要说明的是，当滚动内容较小，不到当前ListView的4个屏幕高度时是不会出现这个快速滑动滑块的。这是因为FastScroller有一个成员_private static final int MIN_PAGES = 4;_而该成员指定了显示滑块需要的最小页面数是4。要想不受页面数限制，也需要改变这个属性的值。因此，下面的方法就同时改变了滑块图像及页面数限制。    
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