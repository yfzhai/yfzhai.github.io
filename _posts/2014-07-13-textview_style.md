---
layout: posts
title: "改变文本显示样式"
---

## 更改文本显示样式(超链接、前景色和背景色)：
BackgroundColorSpan只能设置文字的背景色，为了更加通用，可以自定义一个类，使其同时可以设置文字颜色和背景色，本文为了同时设置超链接的样式，自定义一个继承ClickableSpan的类NoLineClickSpan,并重写updateDrawState方法<br><br>
<font size=4px>
<xmp class="prettyprint linenums">
   private class NoLineClickSpan extends ClickableSpan {
		private int mTextColor;
		private int mBackgroundColor;

		public NoLineClickSpan(int textColor, int backgroundColor) {
			this.mTextColor = textColor;
			this.mBackgroundColor = backgroundColor;
		}

		// setColor和linkColor均用于设置文本颜色，同时使用时优先使用setColor设置的颜色
		@Override
		public void updateDrawState(TextPaint ds) {
			ds.bgColor = mBackgroundColor; // 超链接文本背景色
			ds.setColor(mTextColor); // 设置超链接文本色
			// ds.linkColor = Color.BLACK;
			ds.setUnderlineText(false); // false:去掉下划线;true:添加下划线
		}

		@Override
		public void onClick(View widget) {
			// 点击超链接时触发
			Toast.makeText(MainActivity.this, "您点击了我", Toast.LENGTH_SHORT).show();
		}
	}
</xmp>
</font>
<br><br>
在Activity类的onCreate方法中，为了使得单击链接时有要执行的动作，必须设置MovementMethod对象，即：<br>
<font size=4px>textView.setMovementMethod(LinkMovementMethod.getInstance());</font><br><br>