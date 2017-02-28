## 开发示例

在这里我们拿一个demo来演示如何开发K线图，并在demo中显示k线的同时显示折线和柱状线，k线主要是针对开、关、高、低数据进行绘制的；而折现是针对MA5和DIF和DEA指标参数进行绘制的，MACD指标参数是利用柱状图进行绘制的。

### 效果

![](/assets/K_demo.png)

### 主要过程

```js
<!--从当前页面，引入模块加载器esl.js-->
<script src="esl.js"></script>
```

```js
<!--为ECharts准备一个具备大小（宽高）的Dom -->
<div id="main" style="height:600px; width:1000px;"></div>
```

```js
// 为模块加载器配置echarts的路径，从当前页面链接到echarts.js，定义所需的图表路径
require.config({
    paths:{ 
        'echarts' : './echarts',
        'echarts/chart/bar' : './echarts',//引入柱状图
        'echarts/chart/line' : './echarts',//引入折线图
        'echarts/chart/candlestick' : './echarts',//引入K线图
    }
});
```

```js
// 使用
require(
    [
        'echarts',
        'echarts/chart/bar', // 使用柱状图就加载bar模块，按需加载
        'echarts/chart/line',
        'echarts/chart/candlestick'
    ],
    function (ec) {

    //数据模型 time0 open1 close2 min3 max4 vol5 tag6 macd7 dif8 dea9
    //['2015-10-19',18.56,18.25,18.19,18.56,55.00,0,-0.00,0.08,0.09] 
    var data = splitData([
        ['2015-10-16',18.4,18.58,18.33,18.79,67.00,1,0.04,0.11,0.09],
        ['2015-10-19',18.56,18.25,18.19,18.56,55.00,0,-0.00,0.08,0.09],
        ['2015-10-20',18.3,18.22,18.05,18.41,37.00,0,0.01,0.09,0.09],
        ['2015-10-21',18.18,18.69,18.02,18.98,89.00,0,0.03,0.10,0.08],
        ['2015-10-22',18.42,18.29,18.22,18.48,43.00,0,-0.06,0.05,0.08],
        ['2015-10-23',18.26,18.19,18.08,18.36,46.00,0,-0.10,0.03,0.09],
        ['2015-10-26',18.33,18.07,17.98,18.35,65.00,0,-0.15,0.03,0.10],
        ['2015-10-27',18.08,18.04,17.88,18.13,37.00,0,-0.19,0.03,0.12],
        ['2015-10-28',17.96,17.86,17.82,17.99,35.00,0,-0.24,0.03,0.15],
        ['2015-10-29',17.85,17.81,17.8,17.93,27.00,0,-0.24,0.06,0.18],
        ['2015-10-30',17.79,17.93,17.78,18.08,43.00,0,-0.22,0.11,0.22],
        ['2015-11-02',17.78,17.83,17.78,18.04,27.00,0,-0.20,0.15,0.25],
        ['2015-11-03',17.84,17.9,17.84,18.06,34.00,0,-0.12,0.22,0.28],
        ['2015-11-04',17.97,18.36,17.85,18.39,62.00,0,-0.00,0.30,0.30],
        ['2015-11-05',18.3,18.57,18.18,19.08,177.00,0,0.07,0.33,0.30],
        ['2015-11-06',18.53,18.68,18.3,18.71,95.00,0,0.12,0.35,0.29],
        ['2015-11-09',18.75,19.08,18.75,19.98,202.00,1,0.16,0.35,0.27],
        ['2015-11-10',18.85,18.64,18.56,18.99,85.00,0,0.09,0.29,0.25],
        ['2015-11-11',18.64,18.44,18.31,18.64,50.00,0,0.06,0.27,0.23],
        ['2015-11-12',18.55,18.27,18.17,18.57,43.00,0,0.05,0.25,0.23],
        ['2015-11-13',18.13,18.14,18.09,18.34,35.00,0,0.05,0.24,0.22],
        ['2015-11-16',18.01,18.1,17.93,18.17,34.00,0,0.07,0.25,0.21],
        ['2015-11-17',18.2,18.14,18.08,18.45,58.00,0,0.11,0.25,0.20],
        ['2015-11-18',18.23,18.16,18.0,18.45,47.00,0,0.13,0.25,0.19],
        ['2015-11-19',18.08,18.2,18.05,18.25,32.00,0,0.15,0.24,0.17],
        ['2015-11-20',18.15,18.15,18.11,18.29,36.00,0,0.13,0.21,0.15],
        ['2015-11-23',18.16,18.19,18.12,18.34,47.00,0,0.11,0.18,0.13],
        ['2015-11-24',18.23,17.88,17.7,18.23,62.00,0,0.03,0.13,0.11],
        ['2015-11-25',17.85,17.73,17.56,17.85,66.00,0,-0.03,0.09,0.11],
        ['2015-11-26',17.79,17.53,17.5,17.92,63.00,0,-0.10,0.06,0.11],
        ['2015-11-27',17.51,17.04,16.9,17.51,67.00,0,-0.16,0.05,0.13],
        ['2015-11-30',17.07,17.2,16.98,17.32,55.00,0,-0.12,0.09,0.15]
    ]);
    //数组处理
    function splitData(rawData) {
      var datas = [];
      var times = [];
      var vols = [];
      var macds = []; var difs = []; var deas = [];
      for (var i = 0; i < rawData.length; i++) {
          datas.push(rawData[i]);
          times.push(rawData[i].splice(0, 1)[0]);
          vols.push(rawData[i][4]);
          macds.push(rawData[i][6]);
          difs.push(rawData[i][7]);
          deas.push(rawData[i][8]);
      }
      return {
          datas: datas,
          times: times,
          vols: vols,
          macds: macds,
          difs: difs,
          deas: deas
      };
    }

    //分段计算
    function fenduans(){
      var markLineData = [];
      var idx = 0; var tag = 0; var vols = 0;
      for (var i = 0; i < data.times.length; i++) {
          //初始化数据
          if(data.datas[i][5] != 0 && tag == 0){
              idx = i; vols = data.datas[i][4]; tag = 1;
          }
          if(tag == 1){ vols += data.datas[i][4]; }
          if(data.datas[i][5] != 0 && tag == 1){
              markLineData.push([{
                  xAxis: idx,
                  yAxis: data.datas[idx][1]>data.datas[idx][0]?(data.datas[idx][3]).toFixed(2):(data.datas[idx][2]).toFixed(2),
                  value: vols
              }, {
                  xAxis: i,
                  yAxis: data.datas[i][1]>data.datas[i][0]?(data.datas[i][3]).toFixed(2):(data.datas[i][2]).toFixed(2)
              }]);
              idx = i; vols = data.datas[i][4]; tag = 2;
          }

          //更替数据
          if(tag == 2){ vols += data.datas[i][4]; }
          if(data.datas[i][5] != 0 && tag == 2){
              markLineData.push([{
                  xAxis: idx,
                  yAxis: data.datas[idx][1]>data.datas[idx][0]?(data.datas[idx][3]).toFixed(2):(data.datas[idx][2]).toFixed(2),
                  value: (vols/(i-idx+1)).toFixed(2)+' M'
              }, {
                  xAxis: i,
                  yAxis: data.datas[i][1]>data.datas[i][0]?(data.datas[i][3]).toFixed(2):(data.datas[i][2]).toFixed(2)
              }]);
              idx = i; vols = data.datas[i][4];
          }
      }
      return markLineData;
    }

    //MA计算公式
    function calculateMA(dayCount) {
      var result = [];
      for (var i = 0, len = data.times.length; i < len; i++) {
          if (i < dayCount) {
              result.push('-');
              continue;
          }
          var sum = 0;
          for (var j = 0; j < dayCount; j++) {
              sum += data.datas[i - j][1];
          }
          result.push((sum / dayCount).toFixed(2));
      }
      return result;
    }

    var option = {
      title: {
          text: 'K线周期图表',
          left: 0
      },
      tooltip: {
          trigger: 'axis',
          axisPointer: {
              type: 'line'
          }
      },
      legend: {
          data: ['KLine', 'MA5']
      },
      grid: [           {
          left: '3%',
          right: '1%',
          height: '60%'
      },{
          left: '3%',
          right: '1%',
          top: '71%',
          height: '10%'
      },{
          left: '3%',
          right: '1%',
          top: '82%',
          height: '14%'
      }],
      xAxis: [{
          type: 'category',
          data: data.times,
          scale: true,
          boundaryGap: false,
          axisLine: { onZero: false },
          splitLine: { show: false },
          splitNumber: 20,
          min: 'dataMin',
          max: 'dataMax'
      },{
          type: 'category',
          gridIndex: 1,
          data: data.times,
          axisLabel: {show: false}
      },{
          type: 'category',
          gridIndex: 2,
          data: data.times,
          axisLabel: {show: false}
      }],
      yAxis: [{
          scale: true,
          splitArea: {
              show: false
          }
      },{
          gridIndex: 1,
          splitNumber: 3,
          axisLine: {onZero: false},
          axisTick: {show: false},
          splitLine: {show: false},
          axisLabel: {show: true}
      },{
          gridIndex: 2,
          splitNumber: 4,
          axisLine: {onZero: false},
          axisTick: {show: false},
          splitLine: {show: false},
          axisLabel: {show: true}
      }],
      dataZoom: [{
              type: 'inside',
              xAxisIndex: [0, 0],
              start: 20,
              end: 100
        },{
              show: true,
              xAxisIndex: [0, 1],
              type: 'slider',
              top: '97%',
              start: 20,
              end: 100
        },{
          show: false,
          xAxisIndex: [0, 2],
          type: 'slider',
          start: 20,
          end: 100
      }],
      series: [{
              name: 'K线周期图表',
              type: 'candlestick',
              data: data.datas,
              itemStyle: {
                  normal: {
                      color: '#ef232a',
                      color0: '#14b143',
                      borderColor: '#ef232a',
                      borderColor0: '#14b143'
                  }
              },
              markArea: {
                  silent: true,
                  itemStyle: {
                      normal: {
                          color: 'Honeydew'                  
                      }
                  },
                  data: fenduans()
              }
          }, {
              name: 'MA5',
              type: 'line',
              data: calculateMA(5),
              smooth: true,
              lineStyle: {
                  normal: {
                      opacity: 0.5
                  }
              }
          },{
              name: 'Volumn',
              type: 'bar',
              xAxisIndex: 1,
              yAxisIndex: 1,
              data: data.vols,
              itemStyle: {
                  normal: {
                      color: function(params) {
                          var colorList;
                          if (data.datas[params.dataIndex][1]>data.datas[params.dataIndex][0]) {
                              colorList = '#ef232a';
                          } else {
                              colorList = '#14b143';
                          }
                          return colorList;
                      },
                  }
              }
          },{
              name: 'MACD',
              type: 'bar',
              xAxisIndex: 2,
              yAxisIndex: 2,
              data: data.macds,
              itemStyle: {
                  normal: {
                      color: function(params) {
                          var colorList;
                          if (params.data >= 0) {
                              colorList = '#ef232a';
                          } else {
                              colorList = '#14b143';
                          }
                          return colorList;
                      },
                  }
              }
          },{
              name: 'DIF',
              type: 'line',
              xAxisIndex: 2,
              yAxisIndex: 2,
              data: data.difs
          },{
              name: 'DEA',
              type: 'line',
              xAxisIndex: 2,
              yAxisIndex: 2,
              data: data.deas
          }
      ]
    };    

    // 基于准备好的dom，初始化echarts实例
    var myChart = ec.init(document.getElementById('main'));

    myChart.setOption(option);
});
```



