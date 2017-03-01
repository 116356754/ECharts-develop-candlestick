## 动态加载数据

在这里我们拿一个demo来演示如何动态加载数据来显示K线图，在demo中通过websocket来接收服务端发送过来的数据，并在demo中显示k线的同时显示折线，k线主要是针对开、关、高、低数据进行绘制的；而折现是针对MA5、MA10、MA20和MA30指标参数进行绘制。

### 效果

![](/assets/dynamic-K.png)

### 主要过程

```js
<!--加载echarts库-->
<script src="echarts.min.js"></script>
```

```js
<!--为ECharts准备一个具备大小（宽高）的Dom -->
<div id="main" style="height:600px; width:1000px;"></div>
```

```js
<!-- websocket客户端连接-->
var wsUri = "ws://127.0.0.1:7777";

function testWebSocket()
{
    var websocket = new WebSocket(wsUri);
    websocket.onopen = function(evt) { onOpen(evt) };
    websocket.onclose = function(evt) { onClose(evt) };
    websocket.onmessage = function(evt) { onMessage(evt) };
    websocket.onerror = function(evt) { onError(evt) };
}

function onOpen(evt)
{
    console.log("CONNECTED");
}

function onClose(evt)
{
    console.log("DISCONNECTED");
}

function calculateMA(id,dayCount) {
    var data0= option.series[0].data;
    if(data0.length >= dayCount)
    {
        var sum=0;
        for (var i = 0; i < dayCount; i++) {
            sum += parseFloat(data0[data0.length -1 - i][1]);
        }
        option.series[id].data.push((sum / dayCount).toFixed(2));                
    }
    else
    {
        option.series[id].data.push('-');
    }
}

function onMessage(evt)
{
    var rawData = evt.data.split(',');

    option.xAxis.data.push(rawData.splice(0, 1)[0]);
    option.series[0].data.push(rawData);
    calculateMA(1,5);
    calculateMA(2,10);
    calculateMA(3,20);
    calculateMA(4,30);
    myChart.setOption(option);
}            

function onError(evt)
{
    console.log('ERROR:' + evt.data);
}

window.addEventListener("load", testWebSocket, false);
```

```
<!-- echarts去绘制图-->
```



