---
layout: post
title: 推开Canvas&&SVG的大门,一起走向EChars&&D3的世界
date: 2017-04-08 11:11:11.000000000 +09:00
categories: ECharts&&D3 Github
tags: ECharts&&D3&emsp;Github
---

&emsp;&emsp;最近遇到线条处理的问题,所以将之前使用过的Canvas,SVG(从实现画线的角度进行出发),进行总结;顺便将ECharts和D3两大库引入进来,进行阐述,这儿我从单一层面阐述ECharts和D3简单使用,至于它们更强大的光芒,期待自己后面内容不断更新~


**关键点**：

* 1.Canvas,SVG实现线条绘制
* 2.Canvas,SVG对比
* 3.Canvas,SVG实现常见效果画饼图
* 4.ECharts和D3库的简单使用


###一.Canvas
####1.canvas介绍
&emsp;&emsp;canvas标签定义图形，比如图表和其他图像。标记由 Apple 在 Safari 1.3 Web 浏览器中引入。对 HTML 的这一根本扩展的原因在于，HTML 在 Safari 中的绘图能力也为 Mac OS X 桌面的 Dashboard 组件所使用，并且 Apple 希望有一种方式在 Dashboard 中支持脚本化的图形。历史这儿就简单了解即可,知道Canvas是如何诞生的;

####2.canvas使用
&emsp;&emsp;使用Canvas(重要点),大多数 Canvas 绘图 API 都没有定义在canvas元素本身上，而是定义在通过画布的 getContext() 方法获得的一个“绘图环境”对象上。直白的说,通过丰富的Canvas API,在画布上画出我们想要的美眉(哈哈,你懂的~)
&emsp;&emsp;直接上DEMO,使用我们的强大的Canvas画起属于我们的线条;

```ojb
//1>.画线
HTML:
	/*注意:此处width,height写成属性赋值,不使用css,立flag,不是我们的重点*/
	<canvas id="c1" width="800" height="600"></canvas>;  
JS:
	var oC=document.getElementById('c1');
	var gd=oC.getContext('2d');
		
	gd.beginPath();         //beginPath() 方法开始一条路径，或重置当前的路径。(注意:当通过定时器不断重新绘制,让元素动起来,beginPath()能够清除之前的路径)
	gd.moveTo(100,100);     //起始点
	gd.lineTo(300,300);     //从起始点到某点画线
	gd.closePath();         //封闭空间
	
	gd.strokeStyle = 'red'; //设置边框样式
	gd.lineWidth=20;        //设置线条宽度
	gd.stroke();            //绘制路径
	
	gd.fillStyle = 'green'; //设置填充样式
	gd.fill();              //进行填充
	
//2>.画矩形
   var gd = document.getElementById('c1').getContext('2d');
   gd.fillStyle='red';    //设置填充颜色(注意顺序:先设置样式,然后根据样式进行填充;顺序错了没有效果)
   gd.fillRect(100, 100, 400, 300);  //绘制矩形

//3>.绘制圆
  //为了方便(弧度与角度的转换)计算和使用，可以将其封装成JavaScript函数(后面有推荐内容可看)
  function getRads (degrees) { return (Math.PI * degrees) / 180; }  //角度转弧度
  function getDegrees (rads) { return (rads * 180) / Math.PI; }  //弧度转角度
  
  var gd = document.getElementById('c1').getContext('2d');
  //语法:arc(圆心x, 圆心y, 半径, 起点角度(弧度), 终点角度(角度), 是否逆时针)
  gd.arc(300,300,100,getRads(0),getRads(360),false);  //绘圆
  gd.stroke();	   //描边
```


###二.SVG
####1.SVG介绍
&emsp;&emsp;可缩放矢量图形（Scalable Vector Graphics，SVG）是基于可扩展标记语言（XML），用于描述二维矢量图形的一种图形格式。SVG由W3C制定，是一个开放标准。

####2.SVG画线

```ojb
//1>.画直线(注意点:1.svg能够直接响应事件 2.x1,y1起点 , x2,y2终点)
    <svg id="svg1" width="800" height="600">
      <line x1="100" y1="100" x2="300" y2="300" style="stroke:red; stroke-width:20;" onmouseover="this.style.stroke='green'" onmouseout="this.style.stroke='red'" />
    </svg>
   
//2>.画矩形[style="stroke:red(外部线条) ; fill:green(填充)"]
    <svg id="svg1" width="800" height="600">
      <rect x="100" y="100" width="300" height="200" style="stroke:red;fill:green" />
    </svg>
  
//3>.画圆(cx,cy:圆心 , r:半径)
    <svg id="svg1" width="800" height="600">
      <circle cx="300" cy="300" r="200" />
    </svg>
  
 //4>.画椭圆(cx,cy:圆心 ; rx:水平半径,ry:垂直半径)
    <svg id="svg1" width="800" height="600">
      <ellipse cx="300" cy="200" rx="200" ry="100" />
    </svg>
 
 //5>.使用path能够实现上面的所有效果(line,rect,circle,ellipse)
 		*1.实现line效果(M 起点坐标 ; L 画线路径,可以多点路径) //注意点:数值之间也可以使用,连接.
        <svg id="svg1" width="800" height="600">
           <path d="M 100 100 L 300 300" style="stroke:black;fill:none;" />
        </svg>
      
      *2.实现rect效果(Z封闭空间)
        <svg id="svg1" width="800" height="600">
		      <path d="M 100,100 L 400,100 400,300 100,300 Z" style="stroke:black;fill:none;" />
		   </svg>
	  
	  	*3.实现circle,ellipse效果
	  		/*
       path中d属性的解析:
              M(x,y)  起始点
              L(x,y)  线条位置,可以有很多线条
              Z       封闭空间
              A       rx ry(半径)  x-axis-rotation(旋转角度)  large-arc-flag(大小弧标志0,1) sweep-flag(镜像标志:弧显示的位置 0左 1右)  x y(终点)
       */
	
	  	  <svg id="svg1" width="800" height="600">
		      <path d="
			      M 100,100
			      L 200,100
			      A 150,50,0,1,0,300,100
			      L 400,100
		      " style="stroke:black;fill:none;" />
    	 </svg>
```



###三.Canvas和SVG比美
&emsp;&emsp;没有完美的人,自然也没有完美的代码,有时候缺点可能是优点的触发点(又毒鸡汤了,不过确实如此)

<table>
	<tr>
		<td>Canvas</td>
		<td>位图(像素,放大失真)</td>
		<td>依赖js</td>
		<td>图形不能修改 </td>
		<td>没有事件 </td>
		<td>性能很高 </td>
		<td> 适合:大型图表,游戏 </td>
	</tr>
	<tr>
		<td>SVG</td>
		<td>矢量图(没单位,放大不失真)</td>
		<td>脱离js</td>
		<td>图形可以修改 </td>
		<td>有事件 </td>
		<td>性能一般 </td>
		<td> 适合:地图,交互频繁的图表 </td>

	</tr>
</table>


###四.常用案例通过Canvas和SVG分别实现饼图
####1.Canvas画饼
![repo1 icon](/assets/2017-04-08-images/pie.png)

```ojb
function getRads (degrees) { return (Math.PI * degrees) / 180; }  //角度转弧度
function getDegrees (rads) { return (rads * 180) / Math.PI; }  //弧度转角度

window.onload = function (){
      var oC = document.getElementById('c1');
      var oBtn = document.getElementById('btn1');

      var gd = oC.getContext('2d');

      function pie(startAng, endAng, color) {
        //#1.根据角度,画出线条
        var x = Math.cos(getRads(startAng))*150+300;
        var y = Math.sin(getRads(startAng))*150+300;

        gd.beginPath();
        gd.moveTo(300,300);
        gd.lineTo(x,y);
        gd.strokeStyle = 'cyan';
        gd.stroke();

        //#2.画弧
        gd.arc(300,300,150,getRads(startAng),getRads(endAng),false);

        //#3.闭合
        gd.closePath();

        gd.fillStyle = color;
        gd.fill();
      }

      //原始数据
      var arr = [1,1,1,1,1,1];   //每部分的值随便给
      var aColor = ['red', 'blue', 'green', 'yellow', 'pink', 'purple']; //颜色随便给

      //1.求和:(目的算出每个部分所占的等分,求得对应角度)
      var sum = 0;
      for(var i = 0;i<arr.length;i++){
        sum += arr[i];
      }

      //2.比例、角度(算出每份所占角度)
      var arrAng = [];
      for(var i = 0;i<arr.length;i++){
        arrAng[i] = 360*arr[i]/sum;
      }

      //3.画角
      //第n个pie      前一次~前一次+arrAng[n]
      var now = 0;
      for(var i = 0;i<arrAng.length;i++){
        pie(now, now+arrAng[i], aColor[i]);
        now += arrAng[i];
      }
    };
```

####2.SVG画饼
```ojb
    <svg id="svg1" width="800" height="600">
      <path d=
	      "
	      M 300,300
	      L 441,158
	      A 200,200,0,0,1,400,473
	      Z
	      " 
      style="stroke:black;fill:red;" />
    </svg>
```

###四.ECharts
####1.ECharts介绍
&emsp;&emsp;ECharts，缩写来自Enterprise Charts，商业级数据图表，一个纯Javascript的图表库，可以流畅的运行在PC和移动设备上，兼容当前绝大部分浏览器（IE6/7/8/9 /10/11，chrome，firefox，Safari等），底层依赖轻量级的Canvas类库ZRender，提供直观，生动，可交互，可高度个性化定制的数据可视化图表。创新的拖拽重计算、数据视图、值域漫游等特性大大增强了用户体验，赋予了用户对数据进行挖掘、整合的能力。
支持折线图（区域图）、柱状图（条状图）、散点图（气泡图）、K线图、饼图（环形图）、雷达图（填充雷达 图）、和弦图、力导向布局图、地图、仪表盘、漏斗图、事件河流图等12类图表，同时提供标题，详情气泡、图例、值域、数据区域、时间轴、工具箱等7个可交 互组件，支持多图表、组件的联动和混搭展现。
![image](/assets/2017-04-08-images/architecture.png)

####2.ECharts使用
#####1>.饼图
```ojb
		<html>
		  <head>
		    <meta charset="utf-8">
		    <title></title>
		    <style media="screen">
		    #div1 {width:500px;height:400px;border:1px solid black;}
		    </style>
		    <script src="echarts.js" charset="utf-8"></script>
		    <script>
		    window.onload=function (){
		      //1.初始化
		      var charts=echarts.init(document.getElementById('div1'));
		
		      //2.数据
		      var data={
		        title: {   //标题
		          text: '性别比例'
		        },
		        legend: {  //图例(按钮可点击,进行禁用)
		          data: ['男', '女', '其他']
		        },
		        series: [   //系列,可以实现嵌套饼图
		          {
		            name: '性别',
		            type: 'pie',   //图标类型
		            radius: ['60%', '70%'],   //内圆,外圆的半径大小
		            data: [    //name显示部分名字 ; value每部分对应值
		              {name: '男', value: 57.3},
		              {name: '女', value: 41.6},
		              {name: '其他', value: 1.1}
		            ]
		          }
		        ]
		      };
		
		      //3.放进去
		      charts.setOption(data);
		    };
		    </script>
		  </head>
		  <body>
		    <div id="div1"></div>
		  </body>
		</html>
```
![image](/assets/2017-04-08-images/pie.png)

#####1>.柱状图
```ojb
      var oDiv=document.getElementById('div1');

      //1.初始化echarts对象
      var charts=echarts.init(oDiv);

      //2.数据写好
      var data={
        //1>.标题
        title: {
          text: '2016年产量'
        },
        //2>.x轴
        xAxis: {
          data: ['第1季度', '第2季度', '第3季度', '第4季度']
        },
        //3>.y轴
        yAxis: {},
        //4>.图例
        legend: {},
        //5>.数据
        series: [
          {
            name: '产量',
            type: 'bar',
            data: [454334, 554433, 133221, 55666]
          }
        ]
      };

      //3.数据扔进去
      charts.setOption(data);
    };
```
![image](/assets/2017-04-08-images/ECharts_ histogram.png)


###五.D3
&emsp;&emsp;虽然D3是强大的绘图库,用来做图表,貌似不能突出他的强悍,但是这儿讲解依然使用D3进行图表绘制,就忽略逼格的问题了~
####1.D3介绍
&emsp;&emsp;D3可以将任意数据绑定到一个文档对象模型(DOM),然后应用数据驱动转换的文档。例如,您可以使用D3来生成一个HTML表从一个数字数组。或者,使用相同的数据创建一个交互式SVG条形图平滑过渡和交互。D3不是一个整体框架,旨在提供所有可能的功能。相反,D3解决了问题的症结所在:文档基于数据的有效处理。这避免了专有的表示和提供非凡的灵活性,暴露web标准的全部功能,如HTML、SVG和CSS。以最小的开销,D3非常快,支持大型数据集和动态行为交互和动画。D3的功能性风格允许代码重用通过收集不同的组件和插件。(牛逼吹得可以)

####2.D3画饼
//D3使用过程中抓住:生成器 -> 数据 -> 结果,形成整体脉络即可,一步一步完成;

```ojb
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
    <script src="d3.js" charset="utf-8"></script>
    <style media="screen">
    #svg1 {border:1px solid black;}
    </style>
    <script>
    window.onload=function (){
      /*
      注意点:提出g标签概念:在svg中提供了如g元素这样的将多个元素组织在一起的元素。由g元素编组在一起的可以设置相同的颜色，可以进行坐标变换;
      完成之后结构<svg><g><path></path></g></svg>,如果直接使用path位置默认为左上角,使用g标签更好控制图形位置
      */
      //1.准备工作
      var svg = d3.select('#svg1');
      var g = svg.append('g') 
      .attr('transform', 'translate(400,300)');  //添加g节点,移动位置

      var colors = ['#A0C', '#0AC', '#AC0', '#00C'];

      //2.饼图生成器
      var pieGen = d3.pie();

      //3.生成一堆arc数据
      var arrArc = pieGen([3000, 48000, 69000, 12000]);

      //4.arc生成器
      var arcGen = d3.arc();

      //5.arc生成器+arc数据  =>  一堆字符串(生成的字符串即path标签d属性需要的属性值)
      for(var i=0;i<arrArc.length;i++){
        var strArc=arcGen({
          innerRadius: 0,
          outerRadius: 150,
          startAngle: arrArc[i].startAngle,
          endAngle: arrArc[i].endAngle
        });

        //6.生成path元素
        g.append('path')
        .attr('d', strArc)
        .attr('fill', colors[i]);
      }
    };
    </script>
  </head>
  <body>
    <svg id="svg1" width="800" height="600"></svg>
  </body>
</html>

```




>推荐阅读: 
>[Canvas学习:绘制圆和圆弧](https://www.w3cplus.com/canvas/drawing-arc-and-circle.html)
>[ECharts官网](http://echarts.baidu.com/index.html)
>[D3中文手册](https://github.com/d3/d3/wiki/API--%E4%B8%AD%E6%96%87%E6%89%8B%E5%86%8C)

