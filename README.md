MOBAC-Ext
=========

Mobile Atlas Creator (MOBAC) extended version based on v1.9.16

## 扩展版新增/修改内容历史
2014-11-09
- 图源文件增加hiddenDefault定义图源默认是否隐藏
- 修正书签坐标异常bug

2014-11-07
-  localTileFiles图源增加mapSpaceType元素
-  localTileSQLite图源增加mapSpaceType元素
-  调整最大瓦片数

2014-11-06
- 修正MapSpace = msGeoLatlong图源输出的投影设置问题
- 修正ignoreErrors在bsh图源无效问题
- 修正不同投影图源切换时视窗中间点及选择区错位问题

2014-11-05
- 增加watermark水印图源
- 修正GeoLatlong无法直接输出地图册bug

2014-11-02
- MapSpace增加对Geo Lat/Long坐标系支持
- 多层图源增加unionAllZoom元素
- settings.xml增加swapDir元素
- 修正瓦片大小不是256时的bug

2014-10-30
- MapSpace增加对GCJ-02坐标系支持
- 多层图源支持不同类型的MapSpace，且允许强制统一为标准墨卡托坐标系
- 本地瓦片图源支持空瓦片


### 图源文件元素详述

#### BeanShell图源
- 增加mapSpaceType变量，地图空间类型，取值范围如下
	- msMercatorSpherical：球形墨卡托投影（默认）
	- msMercatorEllipsoidal：椭球墨卡托投影
	- msMercatorGCJ02：球形墨卡托投影叠加GCJ-02坐标偏移
	- msGeoLatlong：经纬度等间隔投影
- 增加hiddenDefault变量，默认为false，设为true时图源默认为disabled状态，即在图源下拉列表中不显示

##### 示例
	mapSpaceType = MapSpaceType.msMercatorGCJ02;
	hiddenDefault = false;

#### XML在线图源
- 增加mapSpaceType元素，地图空间类型；
- 增加httpHeadReferer元素，HTTP请求头Referer属性；
- 增加httpHeadUserAgent元素，HTTP请求头UserAgent属性；
- url元素模版增加{$-y}、{$sx}、{$sy}占位符，分别为坐标在左下角的y轴瓦片编号、x轴子编号、y轴子编号。

##### 示例
	<mapSpaceType>msMercatorGCJ02</mapSpaceType>
	<url><![CDATA[http://p{$part}.map.gtimg.com/maptilesv3/{$z}/{$sx}/{$sy}/{$x}_{$-y}.png]]></url>
	<httpHeadReferer><![CDATA[http://map.qq.com/]]></httpHeadReferer>
	<httpHeadUserAgent><![CDATA[Mozilla/5.0 (Windows NT 6.1; WOW64; rv:2.0) Gecko/20100101 Firefox/4.0 Opera 12.16]]></httpHeadUserAgent>

#### XML本地图源
- localTileFiles图源增加emptyTileFile元素，空瓦片图片文件，当请求的瓦片不存在时返回此元素配置的图片文件
- localTileFiles图源增加加mapSpaceType元素
- localTileSQLite图源增加加mapSpaceType元素

##### 示例
	<emptyTileFile><![CDATA[D:\map\empty.png]]></emptyTileFile>
	<mapSpaceType>msGeoLatlong</mapSpaceType>

#### 多层图源
- 增加forceMercator元素，是否强制转换为msMercatorSpherical，转换需要耗费更多的CPU时间，默认为false；
- 增加unionAllZoom元素，多层地图图层级别是否为所有图层的级别并集，设置为true时适用于不同级别访问不同的图源，默认为false。

##### 示例
	<forceMercator>true</forceMercator>
	<unionAllZoom>true</unionAllZoom>

#### 水印图源
- 图源标签：watermark
- name：图源名称
- minZoom：最小缩放级别，默认为0
- maxZoom：最大缩放级别，默认为22
- watermarkFile：水印文件名，必填
- probability：水印出现几率百分比，取值范围0-100，默认为100
- mosaic：8x8马赛克掩码，每行使用8个字符标识掩码位，为1时显示水印，为0时不显示，行间使用逗号分隔，默认为空，设置此值后probability将被忽略

##### 示例
	<watermark>
		<name>水印</name>
		<minZoom>14</minZoom>
		<maxZoom>17</maxZoom>
		<watermarkFile><![CDATA[D:\map\watermark.png]]></watermarkFile>
		<probability>50</probability>
		<mosaic>10001000,00010001,00100010,01000100,10001000,00010001,00100010,01000100</mosaic>
	</watermark>

#### settings.xml
- 增加swapDir元素，用于指定瓦片存档临时目录，不设置时使用系统默认临时目录

##### 示例
	<swapDir>F:\Temp</swapDir>

***

*MOBAC官方网站：http://mobac.sourceforge.net/*

*MOBAC Ext开源项目网站: https://github.com/rilyu/mobac-ext*

*MOBAC Ext交流Q群：209602056*
