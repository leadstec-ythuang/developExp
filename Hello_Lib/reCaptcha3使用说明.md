[TOC]

# reCAPTCHA v3
[官方网址](https://developers.google.com/recaptcha/)
[开发者文档](https://developers.google.com/recaptcha/intro?hl=zh-cn)
> 可在没有任何用户互动的情况下验证互动是否正当。是一个纯粹的 JavaScript API，会在后台对用户的行为进行监测，然后会返回一个分数（0-1）之间，可以以此自定义，小于 0.5 的就是机器人（ 缺省情况下，阈值设置为0.5，可以在控制台设置为其他值），他们就需要被验证，验证手机号等

### 特色
- 每月免费提供100万次调用
- 无需“我不是机器人”部件
- 不再需要用户点图。


## 使用
### 创建秘钥
 1. [谷歌账号登录并创建秘钥](https://www.google.com/recaptcha/admin/create)
  > 选项说明：
  > + 标签
  > + reCAPTCHA类型 一个网站秘钥只能与一种reCAPTCHA网站类型搭配使用
  > + 域名 注册使用范围仅限于在此输入的域名及其任何子域名,即注册了example.com也就同时注册了subdomain.example.com.不得包含任何路径,端口,查询或片段.
      可以使用公网IP和本地localhost,但不能用内网IP

2. API秘钥对
点击提交得到一个API秘钥对:**网站秘钥和秘钥**.
+ **网站秘钥** 用于在网站或移动程序上调用reCAPTCHA服务
+ **秘钥** 授权应用程序后端与reCAPTCHA服务器之间通信,一验证用户的响应

### 前端部署
#### 官方demo
```html
<button class="g-recaptcha" 
        data-sitekey="reCAPTCHA_site_key" 
        data-callback='onSubmit' 
        data-action='submit'>Submit</button>
```
##### 1. 加载reCAPTCHA, **reCAPTCHA_site_key填入注册申请拿到的网站秘钥**
```js
<script src="https://www.google.com/recaptcha/api.js?render=reCAPTCHA_site_key"></script>
```
##### 2. 针对要保护的每项操作调用grecpatcha.execute
核心代码就是通过执行 grecaptcha.execute(...) 方法，在回调中获取到 token值
```js
<script>
  function onClick(e) {
    e.preventDefault();
    grecaptcha.ready(function() {
      grecaptcha.execute('reCAPTCHA_site_key', {action: 'submit'}).then(function(token) {
          // Add your logic to submit to your backend server here.
      });
    });
  }
</script>
```

**action**参数为验证场景,可自定义数值,作用是可以在后台,对于不同的场景对用户得分进行特别的设置.
谷歌默认提供4种场景
+ homepage 一般场景，可在管理后台查看流量趋势
+ login	分数较低的用户需要进行二次验证
+ social 限制一些滥用的用户请求，一般用于评论
+ e-commerce	商品交易的时候

##### 3. 使用验证请求立即将拿到的token发送到后端
可以使用ajax提交，也可以把 token 插入到 HTML 的 input hidden 隐藏框，一起提交到后端进行校验

### 后端部署
每个 reCAPTCHA 用户响应令牌的有效期均为两分钟，并且只能进行一次验证，以防止重放攻击。如果您需要新令牌，可以重新运行 reCAPTCHA 验证。

+ 请求接口`https://www.google.com/recaptcha/api/siteverify`
+ 请求方法 POST
  参数:
  - secret 必须的，服务端的密钥
  - response	必须的，客户端验证的Token.
  - remoteip	可选的，客户端的ip地址
+ 响应结果
```json
{
  score: 0.9  // 评分0 到 1。1：确认为人类，0：确认为机器人。
  hostname: "localhost"  // 请求的地址
  success: true  // 是否验证成功，
  challenge_ts: "2020-02-27T05:26:05Z"
  action: "homepage"
}
```
### 国内的用户需要替换js文件的地址和服务端验证接口的地址
#### 替换客户端js地址
> https://www.google.com/recaptcha/api.js
  替换为
  https://www.recaptcha.net/recaptcha/api.js

#### 替换服务端接口地址
> https://www.google.com/recaptcha/api/siteverify
  替换为
  https://www.recaptcha.net/recaptcha/api/siteverify
## 完整例子
客户端
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>谷歌ReCaptcha</title>
	</head>
	<body>   
		<button>点击我执行验证</button>
	
	<script src="https://www.recaptcha.net/recaptcha/api.js?render=${captchaClientSecret}"></script>
	<script type="text/javascript">
		const CAPTCHA_CLIENT_SECRET = "${captchaClientSecret}";
		window.onload = () => {
			document.querySelector('button').addEventListener('click', () => {
				grecaptcha.execute(CAPTCHA_CLIENT_SECRET, {action: 'homepage'}).then(function(token) {
					console.log('客户端token:' + token);
					fetch('/validate?token=' + token, {
						method: 'GET'
					}).then(response => {
						if (response.ok){
							response.json().then(message => {
								console.log('服务端验证');
								console.log(message);
							});
						}
					});
				});
			});
		};
	</script>   
	</body>  
</html>
```
服务端
```java
import javax.servlet.http.HttpServletRequest;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.util.LinkedMultiValueMap;
import org.springframework.util.MultiValueMap;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import com.alibaba.fastjson.JSONObject;

@RestController
@RequestMapping("/validate")
public class ValidateController {
	
	@Value("${google.recaptcha.validate-api}")
	private String validateApi;
	
	@Value("${google.recaptcha.server-secret}")
	private String captchaServerSecret;
	
	@Autowired
	RestTemplate restTemplate;
	
	@GetMapping
	public Object validate (HttpServletRequest request,
						@RequestParam("token")String token) {
		
		HttpHeaders httpHeaders = new HttpHeaders();
		httpHeaders.set(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_FORM_URLENCODED_VALUE);

		MultiValueMap<String, Object> requestBody = new LinkedMultiValueMap<>();
		requestBody.add("secret", this.captchaServerSecret);
		requestBody.add("response", token);
		requestBody.add("remoteip", request.getRemoteAddr()); // 客户的ip地址，不是必须的参数。
		
		ResponseEntity<JSONObject> responseEntity = restTemplate.postForEntity(this.validateApi, new HttpEntity<>(requestBody,httpHeaders), JSONObject.class);
		
		if (!responseEntity.getStatusCode().is2xxSuccessful()) {
			// TODO 异常的HTTP状态码
		}
		
		return responseEntity.getBody();
	}
}
```

## 隐藏reCAPTCHA图标
使用 reCAPTCHA，会在网站上提示出一个图标.
如果需要隐藏，可以添加css
```css
.grecaptcha-badge { 
	display: none; 
} 
```