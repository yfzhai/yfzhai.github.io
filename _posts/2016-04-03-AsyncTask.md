---
layout: posts
title: "AsyncTask下载图片"
---

## Android AsyncTask下载图片
----------------------------------
<font size=4px>
<xmp class="prettyprint linenums">
class MyAsyncTask extends AsyncTask<String, Integer, String> {
		@Override
		protected void onPreExecute() {
			// 在主线程中执行，用于设置任务，例如显示进度条
			super.onPreExecute();
			progressBar = (ProgressBar) findViewById(R.id.progressBar);
			progressBar.setMax(100);
			progressBar.setVisibility(View.VISIBLE);
		}

		/**
		 * 唯一在非主线程中执行的步骤
		 * 在onPreExecute()方法执行结束后立即被后台线程调用，被用于执行较长时间的后台计算，
		 * 异步任务的参数将被传给该方法，计算的结果必须在本函数返回，并传给onPostExecute()方法，
		 * 在执行过程中可以调用publishProgress()来更新任务进度。 
		 */
		@Override
		protected String doInBackground(String... params) {
			String result = "";
			//getFilesDir()：/data/data/com.app.download/files
			String path = getFilesDir() +File.separator+ "downloadFile.png";
			HttpURLConnection conn = null;
			try {
				final URL url = new URL(params[0]);
				conn = (HttpURLConnection) url.openConnection();
				//由URL响应头centent-length指定的字节大小
				int length = conn.getContentLength();
				//返回URL所关联的资源内容的对象
				InputStream is = (InputStream) url.getContent();
				byte[] buff = new byte[length];
				int byteCount = (int) Math.ceil(length / 100);
				int download = 0;
				for (int i = 1; i < 100; i++) {
					//从输入流（is）中读取byteCount个字节数据，从download字节开始存储到buff中，
					//返回实际读取的字节数（当读取到流的末尾时返回-1）
					int read = is.read(buff, download, byteCount);
					download += read;
					Log.d(TAG, "文件大小："+length+"，已读取字节数："+download);
					//触发方法onProgressUpdate()更新进度条
					publishProgress(i);
				}
				BufferedInputStream in = new BufferedInputStream(
						conn.getInputStream());
				FileOutputStream fos = new FileOutputStream(path);
				int data = in.read();
				Log.d(TAG, "data="+data);
				while (data != -1) {
					fos.write(data);
					data = in.read();
				}
			} catch (IOException e) {
				Log.e("TAG", "error loading image: ", e);
				return null;
			} finally {
				if (conn != null) {
					conn.disconnect();
				}
				publishProgress(100);
			}
			return path;
		}
		
		//当publishProgress方法被调用时触发，接收传递来的参数更新进度
		@Override
		protected void onProgressUpdate(Integer... progress) {
			super.onProgressUpdate(progress);
			Log.d(TAG, "当前进度："+progress[0]);
			progressBar.setProgress(progress[0]);
		}
		/**
		 * 在UI线程中执行，接收由doInBackground()方法传递的文件保存路径参数
		 * 取消进度条显示
		 * 获取指定路径的图片bitmap
		 * 在ImageView组件上显示图片
		 */
		@Override
		protected void onPostExecute(String imagePath) {
			super.onPostExecute(imagePath);
			if (imagePath != null) {
				progressBar.setVisibility(View.INVISIBLE);
				Bitmap bitmap = BitmapFactory.decodeFile(imagePath);
				imageView.setImageBitmap(bitmap);
			}
		}
	}
</xmp>
</font>
**说明：调用方法，new MyAsyncTask().execute(Image_URL);