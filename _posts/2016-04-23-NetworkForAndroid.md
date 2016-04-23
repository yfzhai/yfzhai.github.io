---
layout: posts
title: "Android网络编程"
---

## Android网络编程
----------------------------------      
**1、检查网络是否可用**             
<font size=4px>
<xmp class="prettyprint linenums">
private boolean isNetworkAvailable() {
	ConnectivityManager cm = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
	NetworkInfo networkInfo = cm.getActiveNetworkInfo();
	//如果没有网络可用，networkInfo为null
	//否则检查是否连上网络
	if (networkInfo != null && networkInfo.isConnected()) {
		return true;
	}
	return false;
}
</xmp>
</font>
需要权限：android.permission.ACCESS_NETWORK_STATE            
