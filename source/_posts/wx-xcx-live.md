---
title: 微信小程序直播
tags: 
    - 微信小程序
categories: 
    - 微信小程序
---

#   开发事项
##  1. 在微信公众平台开通微信直播功能
``` 
开通要求：
1. 需要开通并连接微信商户号,并完成一笔支付订单
2. 小程序基本信息 服务类目 必须是直播所要求的类目

3.主播需实名认证
4.需在公众平台配置主播信息
```
##  2.开发使用
### 引入直播组件 live-player-plugin
> 在app.json引入主包跟分包 【直播组件】 live-player-plugin

- 主包引入
```
"plugins": {
    "live-player-plugin": {
        "version": "1.2.8", // 注意填写该直播组件最新版本号，微信开发者工具调试时可获取最新版本号（复制时请去掉注释）
        "provider": "wx2b03c6e691cd7370" // 必须填该直播组件appid，该示例值即为直播组件appid（复制时请去掉注释）
    }
}
```
- 分包引入
```
"subpackages": [
    {
        "plugins": {
            "live-player-plugin": {
                "version": "1.2.8", // 注意该直播组件最新版本号，微信开发者工具调试时可获取最新版本号（复制时请去掉注释）
                "provider": "wx2b03c6e691cd7370" // 必须填该直播组件appid，该示例值即为直播组件appid（复制时请去掉注释）
            }
        }
    }
]
```
### 使用直播组件
> 注意版本限制，低版本进入会提示用户升级，无法使用微信直播间功能

- 使用navigator组件跳转进入直播间
```
<!--index.wxml-->
<navigator url="plugin-private://wx2b03c6e691cd7370/pages/live-player-plugin?room_id={{roomId}}&custom_params={{customParams}}"></navigator>
```
```
// index.js
let roomId = [直播房间id] // 填写具体的房间号，可通过下面【获取直播房间列表】 API 获取
let customParams = encodeURIComponent(JSON.stringify({ path: 'pages/index/index', pid: 1 })) // 开发者在直播间页面路径上携带自定义参数（如示例中的path和pid参数），后续可以在分享卡片链接和跳转至商详页时获取，详见【获取自定义参数】、【直播间到商详页面携带参数】章节（上限600个字符，超过部分会被截断）
this.setData({
    roomId,
    customParams
})
```

- 使用navigateTo方法跳转进入直播间
```
// index.js
let roomId = [直播房间id] // 填写具体的房间号，可通过下面【获取直播房间列表】 API 获取
let customParams = encodeURIComponent(JSON.stringify({ path: 'pages/index/index', pid: 1 })) // 开发者在直播间页面路径上携带自定义参数（如示例中的path和pid参数），后续可以在分享卡片链接和跳转至商详页时获取，详见【获取自定义参数】、【直播间到商详页面携带参数】章节（上限600个字符，超过部分会被截断）
wx.navigateTo({
    url: `plugin-private://wx2b03c6e691cd7370/pages/live-player-plugin?room_id=${roomId}&custom_params=${customParams}`
})
```
- 效果图

![image](https://note.youdao.com/yws/res/40942/4A8086E8F35A40C581FBAE961B6AE69D)

##  3.组件接口
### 订阅组件subscribe
> 可以配置到小程序页面中，进行直播订阅

1. 在所需要用到该组件的页面 json 文件中进行引用
```
{
    "usingComponents": {
        "subscribe": "plugin-private://wx2b03c6e691cd7370/components/subscribe/subscribe"
    }
}
```
2. subscribe 参数说明
```
room-id：表示房间号，类型为 Number（默认为0）
width：表示宽度，类型为 Number（默认为120）
height：表示高度，类型为 Number（默认为41）
font-size：表示字号，类型为 Number（默认为17）
color：表示字体颜色，类型为 String（默认为'FFFFFF'）
background-color：表示背景色，类型为 String（默认为'#6467F0'）
custom-params：表示自定义参数，类型为 String（默认为空）
stop-propagation：表示阻止事件冒泡，类型为 Boolean（默认为false，不阻止事件冒泡）
```
```
<subscribe
    room-id="{{roomId}}"
    width="{{width}}"
    height="{{height}}"
    font-size="{{fontSize}}"
    color="{{color}}"
    background-color="{{backgroundColor}}"
    custom-params="{{customParams}}"
    bindsubscribe="onSubscribe">
</subscribe>
```
### 获取直播间单次订阅状态 getSubscribeStatus
```
let livePlayer = requirePlugin('live-player-plugin')

// 获取直播间单次订阅状态
const roomId = xxx // 房间 id
livePlayer.getSubscribeStatus({ room_id: roomId })
    .then(res => {
        console.log('房间号：', res.room_id)
        console.log('订阅用户openid', res.openid)
        console.log('是否订阅', res.is_subscribe)
    }).catch(err => {
        console.log('get subscribe status', err)
    })
```
### 获取直播状态getLiveStatus
1. 直播状态码
```
101 直播中：表示主播正常开播，直播正常的状态

102 未开始：表示主播还未开播

103 已结束：表示在直播端点击【结束】按钮正常关闭的直播，或直播异常 15 分钟后系统强制结束的直播

104 禁播：表示因违规受到运营处罚被禁播

105 暂停中：表示在 MP 小程序后台-控制台内操作暂停了直播

106 异常：表示主播离开、切后台、断网等情况，该直播被判定为异常状态，15 分钟内恢复即可回到正常直播中的状态；如果 15 分钟后还未恢复，直播间会被系统强制结束直播

107 已过期：表示直播间一直未开播，且已达到在 MP 小程序后台创建直播间时填写的直播计划结束时间，则该直播被判定为过期不能再开播
```
2. 示例代码
```
let livePlayer = requirePlugin('live-player-plugin')

// 首次获取立马返回直播状态
const roomId = xxx // 房间 id
livePlayer.getLiveStatus({ room_id: roomId })
    .then(res => {
        // 101: 直播中, 102: 未开始, 103: 已结束, 104: 禁播, 105: 暂停中, 106: 异常，107：已过期 
        const liveStatus = res.liveStatus
        console.log('get live status', liveStatus)
    })
    .catch(err => {
        console.log('get live status', err)
    })
// 往后间隔1分钟或更慢的频率去轮询获取直播状态
setInterval(() => {
    livePlayer.getLiveStatus({ room_id: roomId })
        .then(res => {
            // 101: 直播中, 102: 未开始, 103: 已结束, 104: 禁播, 105: 暂停中, 106: 异常，107：已过期 
            const liveStatus = res.liveStatus
            console.log('get live status', liveStatus)
        })
        .catch(err => {
            console.log('get live status', err)
        })
}, 60000)
```
### 获取用户openid参数 geiOpenid
```
let livePlayer = requirePlugin('live-player-plugin')

App({
    onShow(options) {
        livePlayer.getOpenid({ room_id: [直播房间id] }) // 该接口传入参数为房间号
            .then(res => {
                console.log('get openid', res.openid) // 用户openid
            }).catch(err => {
                console.log('get openid', err)
            })
    }
})
```
### 获取分享卡片链接参数

> 分享卡片进入直播间：房间号 room_id + 进入者 openid + 分享者 share_openid + 开发者自定义参数 custom_params

```
let livePlayer = requirePlugin('live-player-plugin')

App({
    onShow(options) {
        // 分享卡片入口场景才调用getShareParams接口获取以下参数
        if (options.scene == 1007 ||
            options.scene == 1008 ||
            options.scene == 1044 ||
            options.scene === 1154 ||
            options.scene === 1155) {
            livePlayer.getShareParams()
                .then(res => {
                    // 房间号
                    console.log('get room id', res.room_id) 
                    // 用户openid
                    console.log('get openid', res.openid) 
                    // 分享者openid，分享卡片进入场景才有
                    console.log('get share openid', res.share_openid) 
                    // 开发者在跳转进入直播间页面时，页面路径上携带的自定义参数，这里传回给开发者
                    console.log('get custom params', res.custom_params) 
                }).catch(err => {
                    console.log('get share params', err)
                })
        }
    }
})
```
## 其他
### 直播小窗
> close_picture_in_picture_mode：默认支持直播小窗，可通过close_picture_in_picture_mode=1关闭小窗功能


---


#   客户端
##  组件
### live-player
- 实时音视频播放

####    属性值
#####   src
#####   mode
#####   autoplay
#####   muted

#####   orientation
#####   object-fit
#####   background-mute
#####   min-cache

#####   max-cache
#####   sound-mode
#####   auto-pause-if-navigate
#####   auto-pause-if-open-native

#####   picture-in-picture-mode
#####   bindstatechange
#####   bindfullscreenchange
#####   bindnetstatus

#####   bindaudiovolumenotify
#####   bindenterpictureinpicture
#####   bindleavepictureinpicture
