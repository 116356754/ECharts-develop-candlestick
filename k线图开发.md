## 开发示例

在这里我们拿一个demo来演示如何开发K线图，并在demo中显示k线的同时显示折线和柱状线，k线主要是针对开、关、高、低数据进行绘制的；而折现是针对MA5和DIF和DEA指标参数进行绘制的，MACD指标参数是利用柱状图进行绘制的。

### 效果

![](/assets/candle_demo.png)

### 主要过程

```js
<!--从当前页面，引入模块加载器esl.js-->
<script src="esl.js"></script>
```

```
<!--为ECharts准备一个具备大小（宽高）的Dom -->
<div id="main" style="height:600px; width:1000px;"></div>
```



