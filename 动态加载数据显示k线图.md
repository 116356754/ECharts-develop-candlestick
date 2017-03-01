## 动态加载数据

在这里我们拿一个demo来演示如何动态加载数据来显示K线图，在demo中通过websocket来接收服务端发送过来的数据，并在demo中显示k线的同时显示折线，k线主要是针对开、关、高、低数据进行绘制的；而折现是针对MA5、MA10、MA20和MA30指标参数进行绘制。

### 效果

![](/assets/dynamic-K.png)

### 主要过程

```js
<!--加载echarts库-->
<script src="echarts.min.js"></script>
```



