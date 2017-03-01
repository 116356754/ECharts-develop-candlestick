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

/*
*接收到的客户端数据，解析并计算，重新绘制k线图
*数据模型 time open close min max
*[2010/1/4,3289.75,3243.76,3243.319,3295.279]
*/
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

```js
<!-- echarts去绘制图-->
///WEBSOCKET RECEIVE
// 基于准备好的dom，初始化echarts实例
var myChart = echarts.init(document.getElementById('main'));

// 指定图表的配置项和数据
// 数据意义：开盘(open)，收盘(close)，最低(lowest)，最高(highest)
var option = {
	animation: false,
	title: {
		text: '上证指数',
		left: 0
	},
	tooltip: {
		trigger: 'axis',
		formatter: function(params) {
			var res = params[0].name
			res += '<br/>  开盘 : ' + params[0].data[0] + '  最高 : ' + params[0].data[3];
			res += '<br/>  收盘 : ' + params[0].data[1] + '  最低 : ' + params[0].data[2];
			return res;
		},
		axisPointer: {
			type: 'line',
			animation: false
		}

	},
	legend: {
		data: ['日K','MA5', 'MA10', 'MA20', 'MA30']
	},

	toolbox: {
		show : true,
		feature : {
			mark : {show: true},
			dataView : {
				show: true, 
				readOnly: true
			},
			restore : {show: true},
			saveAsImage : {show: true}
		}
	},
	grid: {
		left: '10%',
		right: '10%',
		bottom: '15%'
	},
	xAxis: {
		type: 'category',
		data: [],
		scale: true,
		boundaryGap : true,
		axisLine: { lineStyle: { color: '#8392A5' } ,onZero: false},
		splitLine: {show: false},
		splitNumber: 20,
		min: 'dataMin',
		max: 'dataMax'
	},
	yAxis: {
		scale: true,
		axisLine: { lineStyle: { color: '#8392A5' }},
		splitLine: { show: true }
	},
	dataZoom: [
		{
			type: 'inside',
		},
		{
			show: true,
			type: 'slider',
			y: '90%',
		}
	],
	series: [
		{
			name: '日K',
			type: 'candlestick',
			data: [],
			itemStyle: {
				normal: {
					color: '#FD1050',
					color0: '#0CF49B',
					borderColor: '#FD1050',
					borderColor0: '#0CF49B'
				}
			}
		 }
		 ,
		{
			name: 'MA5',
			type: 'line',
			data: [],
			smooth: true,
			lineStyle: {
				normal: {opacity: 0.5}
			}
		}
		,
		{
			name: 'MA10',
			type: 'line',
			data: [],
			smooth: true,
			lineStyle: {
				normal: {opacity: 0.5}
			}
		},
		{
			name: 'MA20',
			type: 'line',
			data: [],
			smooth: true,
			lineStyle: {
				normal: {opacity: 0.5}
			}
		},
		{
			name: 'MA30',
			type: 'line',
			data: [],
			smooth: true,
			lineStyle: {
				normal: {opacity: 0.5}
			}
		}
	]
};

	
myChart.showLoading();

// 使用刚指定的配置项和数据显示图表。
var start = new Date();

myChart.setOption(option);

console.log(new Date() -start + 'ms');

myChart.hideLoading();
```



