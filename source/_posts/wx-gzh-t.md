---
title: 微信公众号开发知识
tags: 
    - 微信公众号
categories: 
    - 微信公众号
---

###  获得微信权限
```
https://open.weixin.qq.com/connect/oauth2/authorize?appid=appid&redirect_uri=http://xxxx.com&response_type=code&scope=snsapi_userinfo&state=STATE#wechat_redirect
//得到code,前端获取code方式
```

### 判断是否在微信环境
```
    //判断是否在微信
	var isWeixin = false;
    function is_weixin() { 
         var ua = window.navigator.userAgent.toLowerCase();
         var reg = /MicroMessenger/i
         if (ua.match(reg) == 'micromessenger') {
            isWeixin = true;
         } else { 
            isWeixin = false;
        } 
    }
```
###  微信公众号支付
```
//如果后台拿不到code,前端传code给后台
    <!-- 获取后台返回必要字段 -->
    function wxpay1(data){
		if (typeof window.parent.WeixinJSBridge == "undefined"){
		    if( document.addEventListener ){
		        document.addEventListener('WeixinJSBridgeReady', onBridgeReady, false);
		    }else if (document.attachEvent){
		        document.attachEvent('WeixinJSBridgeReady', onBridgeReady); 
		        document.attachEvent('onWeixinJSBridgeReady', onBridgeReady);
		    }
		}else{
		    onBridgeReady(data);
		}
	}
	
	function onBridgeReady(data){
	   window.parent.WeixinJSBridge.invoke(
	      'getBrandWCPayRequest', {
	         "appId":data.appid,     //公众号名称，由商户传入     
	         "timeStamp":data.timeStamp.toString(), 
	         "nonceStr":data.nonce_str, //随机串     
	         "package":"prepay_id="+data.prepay_id, 
	         "signType":"MD5",
	         "paySign":data.paySign //微信签名 
	      },
	      function(res){
	      	$('.pay_confirm').attr('pay',0)
	      	<!-- 可以打印res支付返回信息 -->
		    if(res.err_msg == "get_brand_wcpay_request:ok"){
				<!-- 支付成功后 -->
		    } 
	   }); 
	}
```
### 微信h5支付直接跳转
```
//如果后台拿不到code,前端传code给后台
//直接跳转回调链接（前端无需太多操作）
```
### 微信公众号自定义分享
```
function wxshare(data,obj){
		wx.config({
			debug: false, 
			appId: data.appId,
			timestamp: data.timestamp,
			nonceStr: data.nonceStr, 
			signature:data.signature,
			jsApiList: [
	            	'updateAppMessageShareData',
	            	'updateTimelineShareData',
	            	//防止兼容性，用以下接口
	            	'onMenuShareAppMessage',
	            	'onMenuShareTimeline',
	            	'onMenuShareQQ',
	            	'onMenuShareQZone',
	            	'onMenuShareWeibo'
			]
		});
		// 设置分享内容
		// 注意图片链接错误，链接错误
		wx.ready(function(){
			wx.checkJsApi({
			    jsApiList: [
			        'updateAppMessageShareData',
			        'updateTimelineShareData',
			        'onMenuShareAppMessage',
			        'onMenuShareTimeline',
			        'onMenuShareQQ',
			        'onMenuShareQZone',
			        'onMenuShareWeibo'
			    ]
			});
			/*分享到微博*/
			wx.onMenuShareWeibo({
				title: obj.title, // 分享标题
				desc: obj.content, // 分享描述
			    link: data.url, // 分享链接
			    imgUrl: obj.img, // 分享图标
			    success: function () { 
			    	console.log('分享成功')
			    },
			    cancel: function () { 
					console.log('分享失败')
			    }
			}),
			/*分享到qq空间*/
			wx.onMenuShareQZone({
				title: obj.title, // 分享标题
				desc: obj.content, // 分享描述
			    link: data.url, // 分享链接
			    imgUrl: obj.img, // 分享图标
			    success: function () { 
			    	console.log('分享成功')
			    },
			    cancel: function () { 
					console.log('分享失败')
			    }
			}),
			/*分享到qq*/
			wx.onMenuShareQQ({
				title: obj.title, // 分享标题
				desc: obj.content, // 分享描述
			    link: data.url, // 分享链接
			    imgUrl: obj.img, // 分享图标
			    success: function () { 
			    	console.log('分享成功')
			    },
			    cancel: function () { 
					console.log('分享失败')
			    }
			}),
			/*分享到朋友圈*/
			wx.onMenuShareTimeline({
				title: obj.title, // 分享标题
				desc: obj.content, // 分享描述
			    link: data.url, // 分享链接
			    imgUrl: obj.img, // 分享图标
			    success: function () { 
			    	console.log('分享成功')
			    },
			    cancel: function () { 
					console.log('分享失败')
			    }
			}),
			/*分享给朋友*/
			wx.onMenuShareAppMessage({
				title: obj.title, // 分享标题
				desc: obj.content, // 分享描述
			    link: data.url, // 分享链接
			    imgUrl: obj.img, // 分享图标
			    success: function () { 
			    	console.log('分享成功')
			    },
			    cancel: function () { 
					console.log('分享失败')
			    }
			}),
			wx.error(function(res){
			    alert(JSON.stringify(res))
			});
		})
```
### 微信第三方登录
```
//后台自行处理,如果后台拿不到code,前端传code给后台
```