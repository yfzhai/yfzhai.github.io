---
layout: posts
title: "WebService网址"
---

## WebService网址
----------------------------------      
**1、快递查询接口**           
http://webservice.36wu.com/ExpressService.asmx         
**2、ip查询接口**     
http://webservice.36wu.com/ipService.asmx       
**3、天气预报接口**     
http://webservice.36wu.com/weatherService.asmx        
**4、身份证查询接口**     
http://webservice.36wu.com/IdCardService.asmx       
**5、手机归属地接口**     
http://webservice.36wu.com/MobilePhoneService.asmx      
**6、翻译接口**     
http://webservice.36wu.com/TranslationService.asmx      
**7、火车时刻表接口**      
http://webservice.36wu.com/TrainService.asmx      
**8、股票查询接口**     
http://webservice.36wu.com/StockService.asmx       
**9、邮编接口**     
http://webservice.36wu.com/ZipcodeService.asmx       
**10、二维码**     
http://webservice.36wu.com/DimensionalCodeService.asmx            
**11、公交查询**     
http://webservice.36wu.com/mapService.asmx      
**12、ISBN查询**     
http://webservice.36wu.com/ISBNService.asmx      
**13、ICP备案查询**      
http://webservice.36wu.com/ICPServic.asmx      
基于SOAP协议的远程调用，天气预报帮助类
<font size=4px>
<xmp class="prettyprint linenums">
package com.app.utils;

import java.io.IOException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.ksoap2.SoapEnvelope;
import org.ksoap2.serialization.SoapObject;
import org.ksoap2.serialization.SoapSerializationEnvelope;
import org.ksoap2.transport.HttpTransportSE;
import org.xmlpull.v1.XmlPullParserException;

import com.app.bean.WeatherBean;

public class WeatherServiceHelper {
	// 命名空间
	private static final String mNameSpace = "http://WebXml.com.cn/";
	// 接口地址
	private static final String mURL = "http://www.webxml.com.cn/WebServices/WeatherWebService.asmx";
	// 需要调用的方法名（获得本天气预报Web Services支持的洲、国内外省份和城市信息）
	private static final String mGetSupportProvince = "getSupportProvince";
	// 需要调用的方法名（获得本天气预报Web Services支持的城市信息,根据省份查询城市集合）
	private static final String mGetSupportCity = "getSupportCity";
	// 根据城市或地区名称查询获得未来三天内天气情况、现在的天气实况、天气和生活指数
	private static final String mGetWeatherbyCityName = "getWeatherbyCityName";

	/**
	 * 获取州、国内外省份和城市信息
	 */
	public List<String> getProvince() {
		List<String> provinces = new ArrayList<String>();
		//第一步： 创建SoapObject对象，指定WebService的命名空间及调用方法名称
		//第二步：假设方法有参数的话，设置调用方法参数 soapObject.addProperty("参数名称", "参数值");
		SoapObject soapObject = new SoapObject(mNameSpace, mGetSupportProvince);
		//第三步：设置SOAP请求信息(参数部分为SOAP协议版本号，与你要调用的WebService中版本号一致)
		SoapSerializationEnvelope envelope = new SoapSerializationEnvelope(
				SoapEnvelope.VER11);
		// 指定WebService的类型为dotNet
		envelope.dotNet = true;
		envelope.setOutputSoapObject(soapObject);
		HttpTransportSE httpTranstation = new HttpTransportSE(mURL);
		try {
			httpTranstation.call(mNameSpace + mGetSupportProvince, envelope);
			SoapObject result = (SoapObject) envelope.getResponse();
			int count = result.getPropertyCount();
			for (int i = 0; i < count; i++) {
				provinces.add(result.getProperty(i).toString());
			}
		} catch (IOException e) {
			e.printStackTrace();
		} catch (XmlPullParserException xpe) {
			xpe.printStackTrace();
		}
		return provinces;
	}

	/**
	 * 根据省份或者直辖市获取天气预报所支持的城市集合
	 * 
	 * @param province
	 * @return 省份不能带有“省”字
	 */
	public List<String> getCitys(String province) {
		List<String> citys = new ArrayList<String>();
		SoapObject soapObject = new SoapObject(mNameSpace, mGetSupportCity);
		// 给SoapObject对象添加属性，指定的洲或国内的省份，若为ALL或空则表示返回全部城市；
		// 返回数据：一个一维字符串数组 String()，结构为：城市名称(城市代码)
		soapObject.addProperty("byProvinceName", province);
		SoapSerializationEnvelope envelope = new SoapSerializationEnvelope(
				SoapEnvelope.VER11);
		envelope.dotNet = true;
		envelope.setOutputSoapObject(soapObject);
		HttpTransportSE httpTranstation = new HttpTransportSE(mURL);
		try {
			httpTranstation.call(mNameSpace + mGetSupportCity, envelope);
			SoapObject result = (SoapObject) envelope.getResponse();
			int count = result.getPropertyCount();
			for (int i = 0; i < count; i++)
				citys.add(result.getProperty(i).toString());
		} catch (IOException e) {
			e.printStackTrace();
		} catch (XmlPullParserException xpe) {
			xpe.printStackTrace();
		}
		return citys;
	}

	/**
	 * 根据城市名称获取天气预报信息
	 * 
	 * @param city
	 * @return
	 */
	public WeatherBean getCityWeather(String city) {
		WeatherBean bean = new WeatherBean();
		SoapObject soapObject = new SoapObject(mNameSpace,
				mGetWeatherbyCityName);
		soapObject.addProperty("theCityName", "50745");
		SoapSerializationEnvelope envelope = new SoapSerializationEnvelope(
				SoapEnvelope.VER11);
		envelope.dotNet = true;
		envelope.setOutputSoapObject(soapObject);
		HttpTransportSE httpTranstation = new HttpTransportSE(mURL);
		try {
			httpTranstation.call(mNameSpace + mGetWeatherbyCityName, envelope);
			SoapObject result = (SoapObject) envelope.getResponse();
			// 解析结果
			bean = parseWeather(result);
		} catch (IOException e) {
			e.printStackTrace();
		} catch (XmlPullParserException xpe) {
			xpe.printStackTrace();
		}
		return bean;
	}

	/**
	 * 解析返回的结果
	 * 
	 * @param soapObject
	 */
	public WeatherBean parseWeather(SoapObject soapObject) {
		WeatherBean bean = new WeatherBean();
		List<Map<String, Object>> list = new ArrayList<Map<String, Object>>();
		Map<String, Object> map = null;
		List<Integer> icons     = null;
		// 城市名称
		bean.setCityName(soapObject.getProperty(1).toString());
		// 城市简介
		bean.setCityDescription(soapObject.getProperty(
				soapObject.getPropertyCount() - 1).toString());
		// 天气实况+建议
		bean.setLiveWeather(soapObject.getProperty(10).toString() + "\n"
				+ soapObject.getProperty(11).toString());
		//日期
		String date = soapObject.getProperty(6).toString();
		//=============================================================
		map = new HashMap<String, Object>();
		String weatherToday = "今天："+date.split(" ")[0];
		weatherToday += "\n天气："+date.split(" ")[1];
		weatherToday += "\n气温："+soapObject.getProperty(5).toString();
		weatherToday += "\n风力："+soapObject.getProperty(7).toString();
		weatherToday += "\n";
		icons = new ArrayList<Integer>();
		//天气图)
		icons.add(parseIcon(soapObject.getProperty(8).toString()));
		icons.add(parseIcon(soapObject.getProperty(9).toString()));
		map.put("weatherDay", weatherToday);
		map.put("icons", icons);
		list.add(map);
		//=============================================================
		map = new HashMap<String, Object>();
		date = soapObject.getProperty(13).toString();
		String weatherTomorrow = "明天："+date.split(" ")[0];
		weatherTomorrow += "\n天气："+date.split(" ")[1];
		weatherTomorrow += "\n气温："+soapObject.getProperty(12).toString();
		weatherTomorrow += "\n风力："+soapObject.getProperty(14).toString();
		weatherTomorrow += "\n";
		icons = new ArrayList<Integer>();
		icons.add(parseIcon(soapObject.getProperty(15).toString()));
		icons.add(parseIcon(soapObject.getProperty(16).toString()));
		map.put("weatherDay", weatherTomorrow);
		map.put("icons", icons);
		list.add(map);
		//=============================================================
		map = new HashMap<String, Object>();
		date = soapObject.getProperty(13).toString();
		String weatherAfterTomorrow = "后天："+date.split(" ")[0];
		weatherAfterTomorrow += "\n天气："+date.split(" ")[1];
		weatherAfterTomorrow += "\n气温："+soapObject.getProperty(17).toString();
		weatherAfterTomorrow += "\n风力："+soapObject.getProperty(19).toString();
		weatherAfterTomorrow += "\n";
		icons = new ArrayList<Integer>();
		icons.add(parseIcon(soapObject.getProperty(20).toString()));
		icons.add(parseIcon(soapObject.getProperty(21).toString()));
		map.put("weatherDay", weatherAfterTomorrow);
		map.put("icons", icons);
		list.add(map);
		bean.setList(list);
		return bean;
	}
	/**
	 * 解析天气图标
	 */
	private int parseIcon(String data){
		int resID = 32;
		String result = data.substring(0, data.length() - 4).trim();
		if(!result.equals("nothing"))
			resID = Integer.parseInt(result.trim());
		return resID;
	}
}
</xmp>
</font>
