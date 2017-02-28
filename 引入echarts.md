## 获取 ECharts {#-echarts}

你可以通过以下几种方式获取 ECharts。

1. 从[官网下载界面](http://echarts.baidu.com/download.html)选择你需要的版本下载，根据开发者功能和体积上的需求，我们提供了不同打包的下载，如果你在体积上没有要求，可以直接下载[完整版本](http://echarts.baidu.com/dist/echarts.min.js)。开发环境建议下载[源代码版本](http://echarts.baidu.com/dist/echarts.js)，包含了常见的错误提示和警告。

2. 在 ECharts 的[GitHub](https://github.com/echarts)上下载最新的`release`版本，解压出来的文件夹里的`dist`目录里可以找到最新版本的 echarts 库。

3. 通过 npm 获取 echarts，`npm install echarts --save`，详见“[在 webpack 中使用 echarts](http://echarts.baidu.com/tutorial.html#%E5%9C%A8%20webpack%20%E4%B8%AD%E4%BD%BF%E7%94%A8%20ECharts)”

4. cdn 引入，你可以在[cdnjs](https://cdnjs.com/libraries/echarts)，[npmcdn](https://npmcdn.com/echarts@latest/dist/)或者国内的[bootcdn](http://www.bootcdn.cn/echarts/)上找到 ECharts 的最新版本。

## 模块化单文件引入（**推荐**）

```js
<!--从当前页面，引入模块加载器esl.js-->
 <script src="esl.js"></script>
```

```js
<!-- 为ECharts准备一个具备大小（宽高）的Dom -->
<div id="main" style="height:400px"></div>
```

```js
// 为模块加载器配置echarts的路径，从当前页面链接到echarts.js，定义所需的图表路径
require.config({
    paths:{ 
        'echarts' : './echarts',
        'echarts/chart/bar' : './echarts'
    }
});

// 使用
require(
[
        'echarts',
        'echarts/chart/bar' // 使用柱状图就加载bar模块，按需加载
    ],
    function (ec) {
        // 基于准备好的dom，初始化echarts图表
        var myChart = ec.init(document.getElementById('main')); 

        var option = {
            tooltip: {
                show: true
            },
            legend: {
                data:['销量']
            },
            xAxis : [
                {
                    type : 'category',
                    data : ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
                }
            ],
            yAxis : [
                {
                    type : 'value'
                }
            ],
            series : [
                {
                    "name":"销量",
                    "type":"bar",
                    "data":[5, 20, 40, 10, 10, 20]
                }
            ]
        };

        // 为echarts对象加载数据 
        myChart.setOption(option); 
    }
);
```

### 标签式单文件引入

```js
<!-- ECharts单文件引入 -->
<script src="echarts.js"></script>
```

```
<!-- 为ECharts准备一个具备大小（宽高）的Dom -->
<div id="main" style="height:400px"></div>
```

```js
// 基于准备好的dom，初始化echarts图表
var myChart = echarts.init(document.getElementById('main')); 

var option = {
    tooltip: {
        show: true
    },
    legend: {
        data:['销量']
    },
    xAxis : [
        {
            type : 'category',
            data : ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
        }
    ],
    yAxis : [
        {
            type : 'value'
        }
    ],
    series : [
        {
            "name":"销量",
            "type":"bar",
            "data":[5, 20, 40, 10, 10, 20]
        }
    ]
};

// 为echarts对象加载数据 
myChart.setOption(option);
```

### 自定义构建echarts单文件





