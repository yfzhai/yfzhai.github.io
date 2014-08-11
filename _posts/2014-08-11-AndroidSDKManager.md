---
layout: posts
title: "Android SDK Manager更新、下载速度慢解决办法"
---    
## Android SDK Manager更新、下载速度慢解决办法
-------------------------------------------  
今天安装Android SDK后进行更新，发现加载更新速度非常慢，甚至无法加载更新，于是在网上查找到一个解决办法并应用后感觉效果很好，于是记录下来以便日后方便使用。     
**方法步骤**     
1、首先用记事本打开C:\Windows\System32\drivers\etc目录下的hosts文件，将下面文字复制到hosts文件中(注意是复制而不要修改原来的内容)     
<font size=4px>
<xmp class="prettyprint linenums">
203.208.46.146 www.google.com
74.125.113.121 developer.android.com
203.208.46.146 dl.google.com
203.208.46.146 dl-ssl.google.com
</xmp>
</font>
说一下原理：为什么在hosts文件中加入这些内容就会加快下载速度呢？大家都知道每个网站都对应一个IP地址，那么打开域名时，比如www.baidu.com，会先到DNS服务器解析成IP地址，然后再去访问。那咱们在hosts里面加入了Android SDK获取更新链接和下载更新链接的网址以及对应的IP地址，目的就是省去了DNS解析这个步骤，于是节约了时间，也就加快了下载速度。    
2、修改一下http请求方式，改为强制http请求，而非https请求，原理是减少了数据量的传输。打开Android SDK Manager，在菜单Tools下的Options里面，有一项Force https://..sources to be fetched using http://...，将这一项勾选上，就可以了。之后再进行更新，会发现速度很快了。     
参考：[http://jingyan.baidu.com/article/b0b63dbfd0948c4a483070ea.html](http://jingyan.baidu.com/article/b0b63dbfd0948c4a483070ea.html)
