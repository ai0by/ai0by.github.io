---
title: api
date: 2019-04-04 15:27:21
urlname: api_sinaImg
---

## 微博图床-远程图片上传api

### 介绍
我们在使用爬虫相关内容的时候，存放图片时往往会遇到图片尺寸过大，存储不方便等问题，这时候，存放在一个永久存储的云上面就很有必要，微博是一个不限流量，全球CDN的图床~
微博也是有缺点的，他并不是一个易于管理的图床，仅限于存放图片但不能管理图片，如果希望使用可以管理的图床，可以参考使用自建图床，参考我的:[FuliCOSIMG](https://img.0161.org)

### 参数说明

以GET的方式提交到：[https://api.0161.org/sinaimg/sinaImg.php](https://api.0161.org/sinaimg/sinaImg.php)

传递参数类型：GET POST

参数 | 数值类型 | 示例 | 是否必传 
:-: | :-: | :-: | :-: 
url | String | https://sbcoder.cn/img/avatar.jpg | 是

返回参数类型 ： JSON

### 演示

示例:
```json
{"large":"http://ww2.sinaimg.cn/bmiddle/0062WdSely1g1p8mlciyrg30390120jl.gif"}
```
PHP DEMO
```PHP
$data = array(
	'url' => "https://sbcoder.cn/img/avatar.jpg", 
);
echo curlPost("https://api.0161.org/sinaimg/sinaImg.php",$data);
function curlPost($url,$res){
	$curl = curl_init();
	curl_setopt($curl, CURLOPT_URL, $url);
	curl_setopt($curl, CURLOPT_SSL_VERIFYPEER, 0); 
	curl_setopt($curl, CURLOPT_FOLLOWLOCATION, 1); 
	curl_setopt($curl, CURLOPT_AUTOREFERER, 1);
	curl_setopt($curl, CURLOPT_POST, 1); 
	curl_setopt($curl, CURLOPT_POSTFIELDS, http_build_query($res));
	curl_setopt($curl, CURLOPT_TIMEOUT, 30); 
	curl_setopt($curl, CURLOPT_HEADER, 0); 
	curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1); 
	$result = curl_exec($curl);
	curl_close($curl);
    return $result;
}
```
