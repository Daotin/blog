官方文档：https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115

## 打开微信的拍照功能
![image](https://user-images.githubusercontent.com/23518990/70415994-f1a9a700-1a98-11ea-965b-e8a2decbd6dd.png)

## 录音/扫一扫/打开地图
```js
wx.ready(function(){
    let btn = document.getElementById("btn")
    let scan = document.getElementById("scan")
    let map = document.getElementById("map")

    let lyid = null

    // 录音/播放
    btn.onclick = function(){
        switch(status){
            case "pending":
                wx.startRecord()
                status = "recording"
                this.innerText = "停止录音"
                break;
            case "recording":
                wx.stopRecord({
                    success:(res)=>{
                        lyid = res.localId
                        status = "finish"
                        this.innerText = "播放录音"
                    }
                })
                break;
            case "finish":
                wx.playVoice({
                    localId:lyid
                })
                status = "pending"
                this.innerText = "开始录音"
                break;
        }
    }

    // 打开扫一扫
    scan.onclick = function() {
        wx.scanQRCode({
            needResult: 0, // 默认为0，扫描结果由微信处理，1则直接返回扫描结果，
            scanType: ["qrCode","barCode"], // 可以指定扫二维码还是一维码，默认二者都有
            success: function (res) {
                var result = res.resultStr; // 当needResult 为 1 时，扫码返回的结果
            }
        });
    }

    // 打开地图
    map.onclick = function() {
        wx.openLocation({
            latitude: 0, // 纬度，浮点数，范围为90 ~ -90
            longitude: 0, // 经度，浮点数，范围为180 ~ -180。
            name: '我的位置', // 位置名
            address: '我的详细位置', // 地址详情说明
            scale: 2, // 地图缩放级别,整形值,范围从1~28。默认为最大
            infoUrl: 'www.baidu.com' // 在查看位置界面底部显示的超链接,可点击跳转
        });
    }
});
```