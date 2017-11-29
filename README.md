# 關於我

## 目前為穀保家商 資料處理科 的 學生

## 興趣是寫程式

## 當前寫過的<span style=color:#00FF00>語言</span>有以下：


* ## [Visual Basic](#visual-basic回頂部)
* ## [C#](#c回頂部)
* ## [C](#cc回頂部)
* ## [C++](#cc回頂部)
* ## [HTML](#html回頂部)
* ## [CSS](#css回頂部)
* ## [Scss](#scss回頂部)
* ## [JavaScript](#javascript回頂部)
* ## [PHP](#php回頂部)
* ## [R](#r回頂部)

## 網頁部分使用 <span style=color:#33CCFF>Visual Studio Code</font>做開發，
## C++使用<span style=color:#33CCFF>Dev C++</span>，
## R語言使用<span style=color:#33CCFF>RStudio</span>，
## 其餘則是使用 <span style=color:#33CCFF>Visual Studio</span>

## 使用過的<span style=color:#00FF00>函式庫</span>有以下：

* ## [jQuery](#jquery回頂部)
* ## [D3.JS](#d3js回頂部)
* ## [EJS](#ejs回頂部)
* ## [Bootstrap](#bootstrap回頂部)

## 使用過的<span style=color:#00FF00>資料庫</span>有：MySql 、 MongoDB 、 FireBase，但目前最主要使用的是MySql

## 在學校製作專題開發了一個系統，名稱為[續食你我他](http://food.kpvs.ntpc.edu.tw/recyclefood)

## 此系統有build成APP ， Android用戶可以在Google Play 商店 搜尋 續食你我他

## 自己也有架設網站伺服器，[點我進入網站](http://1.34.6.160)

---

# <font size="+3" style=color:yellow>部分程式碼</font>

## <span id=vb>Visual Basic</span>　[回頂部](#關於我)

```vb
Public Class Form1
    Class Tree
        Public data As Integer
        Public child As New List(Of Tree)
        Public subTree As New List(Of Integer)
        Public parent As Tree
        Sub New()
            parent = Nothing
        End Sub
    End Class
    Private Sub Form1_Load(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MyBase.Load
        FileOpen(1, "in1.txt", OpenMode.Input)
        FileOpen(2, "out.txt", OpenMode.Output)
        Dim m As Integer = LineInput(1)
        For w = 1 To m
            Dim data As List(Of String) = LineInput(1).Split.ToList
            Dim node As New List(Of Integer)
            For Each item In data
                Dim edge() As Integer = item.Split(",").Select(Function(X) CInt(X)).ToArray
                node.AddRange(edge.ToList)
            Next
            Dim root As Integer = node.Find(Function(x) node.FindAll(Function(y) y = x).Count = 1)
            Dim alreadyNode As New List(Of Integer) From {root}
            Dim Tree As New Tree With {.data = root}
            Dim centerNode As New List(Of Integer)
            Do Until data.Count = 0
                Dim index As Integer = 0
                Dim parent, child As Integer
                For i = 0 To data.Count - 1
                    Dim edge() As Integer = data(i).Split(",").Select(Function(x) CInt(x)).ToArray
                    If alreadyNode.Contains(edge.First) Or alreadyNode.Contains(edge.Last) Then
                        parent = Array.Find(edge, Function(x) alreadyNode.Contains(x))
                        child = Array.Find(edge, Function(x) alreadyNode.Contains(x) = False)
                        alreadyNode.AddRange(edge.ToList)
                        index = i : Exit For
                    End If
                Next
                createTree(Tree, child, parent)
                data.RemoveAt(index)
            Loop
            For Each current In alreadyNode.Distinct
                Dim startNode As Tree = findNode(Tree, current)
                Dim depth As Integer = depthSearch(startNode)
                Dim topHt As Integer = topSearch(startNode)
                If topHt = depth Then centerNode.Add(current)
            Next
            PrintLine(2, If(centerNode.Count > 0, Strings.Join(centerNode.Select(Function(X) X.ToString).ToArray, ","), "0"))
        Next
        FileClose()
        Close()
    End Sub
    Sub createTree(ByVal Tree As Tree, ByVal data As Integer, ByVal parent As Integer)
        Dim node As New Tree With {.data = data, .parent = Tree}
        If Tree.data = parent Then Tree.child.Add(node) : Tree.subTree.Add(data) : Return
        Dim index As Integer = Tree.child.FindIndex(Function(X) X.data = parent OrElse X.subTree.Contains(parent))
        createTree(Tree.child(index), data, parent)
        Tree.subTree.Add(data)
    End Sub
    Function depthSearch(ByVal Tree As Tree, Optional ByVal ht As Integer = 0, Optional ByRef maxHt As Integer = 0)
        If Tree.child.Count = 0 Then maxHt = Math.Max(maxHt, ht) : Return maxHt
        For Each node In Tree.child
            depthSearch(node, ht + 1, maxHt)
        Next
        Return maxHt
    End Function
    Function topSearch(ByVal Tree As Tree, Optional ByVal ht As Integer = 0, Optional ByRef maxHt As Integer = 0)
        If Tree.parent Is Nothing Then maxHt = Math.Max(ht, maxHt) : Return maxHt
        Return topSearch(Tree.parent, ht + 1, maxHt)
    End Function
    Function findNode(ByVal Tree As Tree, ByVal target As Integer)
        If Tree.data = target Then Return Tree
        Dim index As Integer = Tree.child.FindIndex(Function(X) X.data = target Or X.subTree.Contains(target))
        Return findNode(Tree.child(index), target)
    End Function
End Class
```

## <span id=C#>C#</span>　[回頂部](#關於我)

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace _Cs277_20170214
{
    public class WeatherData : EventArgs
    {
        public double Temperature { get; set; }
        public double Pressure { get; set; }
        public double Humidity { get; set; }
        public static Random random { get; set; }

        public WeatherData(double temperature, double pressure, double humidity)
        {
            this.Temperature = temperature;
            this.Pressure = pressure;
            this.Humidity = humidity;
        }

        public override bool Equals(object obj)
        {
            if (obj is WeatherData)
            {
                WeatherData other = obj as WeatherData;
                return
                    this.Temperature == other.Temperature
                    && this.Pressure == other.Pressure
                    && this.Humidity == other.Humidity;
            }
            else
            {
                return false;
            }
        }

        public static WeatherData Generate()
        {
            if (WeatherData.random == null)
                random = new Random();
            double temperature = random.NextDouble(16.0, 21.0);
            double pressure = random.NextDouble(0.95, 1.0);
            double humidity = random.NextDouble(98, 100);
            return new WeatherData(temperature, pressure, humidity);
        }
    }
}
```

## <span id=C++>C/C++</span>　[回頂部](#關於我)

```c
#include<iostream>
#include<algorithm>
#include<vector>
#include<list>

using namespace std;

template< class T>
bool cmp(T a, T b){
	return a>b;
}
int main()
{
	
	list<int> lst;
	list<int>::iterator it, it2;
	int value;
	while(1){
		cin >> value;
		if(value == -1){
			break;
		}
		lst.push_back(value);
	}
	
	//sort(lst.begin(), lst.end(), cmp<double>);
	lst.sort();
	//printPyList<int>(lst);
	it2 = lst.begin();
	cout << "[";
	for(it=lst.begin();it!=lst.end();it++){
		cout << *it ;
		it2++;
		if(it2!=lst.end()){
			cout << ", ";
		}
	}
	cout << "]"<< endl;
	
	it = lst.begin();
	for(int i=0;i<3;i++)
		it++;
	lst.insert(it,10);
	it2 = lst.begin();
	cout << "[";
	for(it=lst.begin();it!=lst.end();it++){
		cout << *it ;
		it2++;
		if(it2!=lst.end()){
			cout << ", ";
		}
	}
	cout << "]"<< endl;
	
	int ct = count(lst.begin(),lst.end(),10);
	cout << ct << endl;
	
	lst.sort();
	lst.reverse();
	it2 = lst.begin();
	cout << "[";
	for(it=lst.begin();it!=lst.end();it++){
		cout << *it ;
		it2++;
		if(it2!=lst.end()){
			cout << ", ";
		}
	}
	cout << "]"<< endl;
	
	return 0;
}
```

## <span id=html5>HTML</span>　[回頂部](#關於我)

```html
<center>
    <form action="buyCheck.php" method="POST">
        <table>
            <tr>
                <td>
                    日期<input type="text" name="date" id="date" / disabled="disabled">
                </td>
                <td>
                    時段<select name="select" id="select" disabled="disabled"></select><br />
                </td>
            </tr>
            <tr>
                <td>
                    訂餐數量
                    <select name="other" id="other">
                        <option value="1">1客</option>
                        <option value="2">2客</option>
                        <option value="3">3客</option>
                    </select>
                </td>
                <td>
                    套餐名稱
                    <select name="food" id="food">
                        <option value="1">Food01</option>
                        <option value="2">Food02</option>
                        <option value="3">Food03</option>
                        <option value="4">Food04</option>
                        <option value="5">Food05</option>
                        <option value="6">Food06</option>
                    </select>
                </td>
            </tr>
            <tr>
                <td>
                    訂餐桌數
                    <select name="OrderQ" id="OrderQ"></select>
                </td>
            </tr>
            <tr>
                <td>
                    桌號 <input type="text" name="OrderNumber" id="OrderNumber"  disabled>
                </td>
                <td>
                    <a href="javascript:void(0);" onclick="javascript:randTable()">自動產生桌號</a>
                </td>
                <td>
                    <a href="selectTable.php">選擇桌號</a>
                </td>
            </tr>
        </table>
    </form>
</center>
```

## <span id=css3>CSS</span>　[回頂部](#關於我)

```css
td#joinupdate {
    border-radius: 12%;
    border-color: blue;
    width: 90%;
    height: 45%;
}

#reply {
    display: none;
}

td#joinupdate:hover #reply {
    border-radius: 15%;
    border-color: cornflowerblue;
    width: 92%;
    height: 45%;
    display: block;
}

.peonum:hover {
    color: #00BBFF;
}

.relative {
    position: relative;
}

.absolute {
    position: absolute;
    top: 60%;
    left: 42%;
}

.inback {
    position: absolute;
    top: 12%;
    left: 27%;
}

.inBackSelect1 {
    position: absolute;
    top: 25%;
    left: 20%;
    filter: brightness(125%);
}

.inBackSelect1:hover {
    filter: brightness(.8);
    filter: drop-shadow(5px 5px 5px #333);
}

.inBackSelect2 {
    position: absolute;
    top: 25%;
    left: 60%;
    filter: brightness(125%);
}

.inBackSelect2:hover {
    filter: brightness(.8);
    filter: drop-shadow(5px 5px 5px #333);
}

```
## <span id=Scss>Scss</span>　[回頂部](#關於我)

```scss
$pixel : 200px;          
$percent: 70%;            
$int: 128;
$float: 1.5em;
$img :  "../images/hello.jpg";
$noRepeat :  no-repeat;
$hex : #808080;
$rgba : rgba($int,$int,$int,.2);
$list : 1px 1px 3px black,  3px 3px 5px $hex;
$color :  blue;
$mid: middle_child;
@mixin mixin1 {
     color: gray;
     background: $color;
     line-height: $float;
}
div {
  width: $pixel;
  line-height: $float;
  background: url($img) $noRepeat $rgba;
  box-shadow: $list;
}
body {
      @include mixin1();
}
#parent {
  background: green;
  #child-One{
        background: yellow;
        font-size:1em;
  }
  #child-Two{
        font-weight: 200;
        #inner-child .content{
              background:blue;
        }
  }
  &.funny{
    height: 100px;  
  }
}
```

## <span id=js>JavaScript</span>　[回頂部](#關於我)

```javascript
<script>
function checkForm(){
	mail=form.m_mail.value;
	tel=form.m_tel.value;
	var pass=form.m_pass.value.toLowerCase();
	var mailC=0;
	var d=0;
	var i = 0;
	var eng=0;
	var mat=0;
	for(i = 0;i<mail.length ; i++){
		if(mail.charAt(i)=='@'){
	mailC=1;			
		}else if (mail.charAt(i)=='.'){
		d=1;
		}
	}
	if(mailC==0 || d==0){
		location.href="addMsg.php?err=1";
	    return false;
	}
    d=0;
    for(i=0;i<tel.length;i++){
        if(tel.charAt(i)=='-'){
            d=1;
        }
    }
    if(d>1){
        location.href="addMsg.php?err=1";
        return false;
    }
    for(i=0;i<6;i++){
        if(pass.charAt(i)>= 'a' && pass.charAt(i)<= 'z'){
            eng+=1;
        }else if(pass.charAt(i)>=0 && pass.charAt(i)<=9){
            mat+=1;	
        }
    }
    if(eng!=3 && mat!=3){
        location.href="addMsg.php?err=1";
        return false;
    }
        return true;
}
</script>
```

## <span id=php7>PHP</span>　[回頂部](#關於我)

```php
<?php
        function callDB($selectDBName){
        $buttonColor=array('primary','success','info','warning','danger');
        $oneString = strtolower(substr($selectDBName,0,1));
        include_once('mysqlLink.php');
        mysql_select_db('resources_share');
        $codeSql="select * from `".$selectDBName."`";
        $code_query=mysql_query($codeSql);
        echo "<center id=body>";
        $id=1;
        ?>
        <table border="1" style="text-align:center;" class="table" > 
        <th  style="text-align:center" height="40px">No.</th>
        <th style="text-align:center">Source</th>
        <!--<th style="text-align:center">date</th>-->
        <?php
        while($code=mysql_fetch_assoc($code_query)){
            ?>
            <tr ><td style="padding:20px;" >
            <?php
                echo $id.".";
                $id+=1;
                $color=$id %5;
            ?>
                </td>
                <td width="80%" style="vertical-align:middle" ><a href="index.php?id=<?php echo $code[$oneString."_id"]?>"><button class="btn btn-<?php echo $buttonColor[$color]?>" style="width:100%;"><?php echo $code[$oneString."_title"]?></button></a></td>
                <!--<td  ><font size="-2" color="black">上傳時間：<?php echo $code[$oneString."_date"];?></font></td>-->
            <tr>
        <?php }  ?>
        </table>
        <?php
        echo "</center>";
        }
    ?>
```
## <span id=R>R</span>　[回頂部](#關於我)

```R
library(igraph)
g <- make_empty_graph(directed=TRUE)
g <- add_vertices(g,10, color='#3399ff')
g <- add_edges(g,c(
  2,6,
  9,5,
  5,8,
  5,7,
  7,3,
  2,10,
  10,4,
  3,1,
  1,8,
  4,6
))
V(g)$label <- LETTERS[1:10]
E(g)$weight <- c(8,9,4,6,5,1,10,5,7,6)
plot(g, 
     edge.label=E(g)$weight ,
     layout=layout_in_circle(g),
     edge.arrow.size = 0.2
)
```

## <span id=jq>jQuery</span>　[回頂部](#關於我)

```javascript
$(function(){
    var week=["一","二","三","四","五","六","日"];
    var date=['','','','','',1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30];
    var page = $("#hidd").val();
    for(var i=0 ;i<8;i++){
    $("#"+i).text(week[((page-1)*7+i-1)%7]+date[(page-1)*7+i-1]);
    }
    $tt=(function tt(rand){
        $("#OrderNumber").val(rand);	
    });
        $("td").click(function(){
            if($(this).text()>0){
                $("td").removeClass("yellow");
                var id=$(this).attr("id");
                $("#"+id).addClass("yellow");
                $("#date").val(id);	
            }
        });	
});
```

## <span id=d3.js>D3.JS</span>　[回頂部](#關於我)

```javascript
<script>
            // 1. 定義width, height, padding, letterList變數
            var w = 1000;
            var h = 600;
            var padding = 100;
            var letterList = ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "M", "N", "O", "P", "Q", "T", "U", "V",
              "W", "X", "Z"
            ];
            //2. 建立svg()畫布環境
            svg2();
            function mid2(d) {
              d.date = +d.date || 0;
              d.count = +d.count || 0;
              d.avg_age = +d.avg_age || 0;
              d.mid_age = +d.mid_age || 0;
              d.crude_rate = +d.crude_rate || 0;
              return d;
            }
            //3. 用d3讀取csv
            d3.csv("cancer_data.csv", mid2, function (dataSet) {
              bind2(dataSet);
              render2(dataSet);
              btnList2(dataSet);
            });
            function svg2() {
              d3.select("#hw4_2").append("svg").attr({
                width: w,
                height: h
              });
              d3.select("#hw4_2 svg").append("g").append("rect").attr({
                width: "100%",
                height: "100%",
                fill: "white"
              });
              d3.select('#hw4_2 svg')
                .append('g')
                .classed('axis', true)
                .attr('id', 'axisX');
              d3.select('#hw4_2 svg')
                .append('g')
                .classed('axis', true)
                .attr('id', 'axisY');
            }
            //4. 建立bind()
            function bind2(dataSet) {
              var selection = d3.select("#hw4_2 svg")
                .selectAll("circle")
                .data(dataSet);
              selection.enter().append("circle");
              selection.exit().remove();
            }
            function render2(dataSet) {
              //5. 定義xScale,yScale,rScale, fScale比例尺(range目的在決定在svg上位置)
              var xScale = d3.time.scale()
                .domain([
                  0,
                  d3.max(dataSet, function (d) {
                    return d.crude_rate;
                  })
                ])
                .range([padding, w - padding]);
              var yScale = d3.scale.linear()
                .domain([
                  0,
                  d3.max(dataSet, function (d) {
                    return d.avg_age;
                  })
                ])
                .range([h - padding, padding]);
              var rScale = d3.scale.linear()
                .domain([
                  d3.min(dataSet, function (d) {
                    return d.count;
                  }),
                  d3.max(dataSet, function (d) {
                    return d.count;
                  })
                ])
                .range([5, 10]);
              var fScale = d3.scale.category20();
              // Axis
              var xAxis = d3.svg.axis()
                .scale(xScale)
                .orient('bottom');
              var yAxis = d3.svg.axis()
                .scale(yScale)
                .orient('left');
              //6. 建立render()繪圖
              d3.select("#hw4_2").selectAll("circle")
                .transition()
                .attr({
                  cx: function (d) {
                    return xScale(d.crude_rate);
                  },
                  cy: function (d) {
                    return yScale(d.avg_age);
                  },
                  r: function (d) {
                    return rScale(d.count);
                  },
                  fill: function (d, i) {
                    return fScale(d.city);
                  },
                  stroke: '#666666',
                  'stroke-width': 1
                });
              d3.select("#hw4_2").selectAll("circle")
                .on('mouseover', function (d) {
                  var posX = d3.select(this).attr('cx');
                  var posY = d3.select(this).attr('cy');
                  var tooltip = d3.select('#tooltip2')
                    .style({
                      left: (+posX + 20) + 'px',
                      top: (+posY + 20) + 'px'
                    });
                  tooltip.select('#city2').text(d.city);
                  tooltip.select('#industry2').html(d.category + '<br>案例數：' + d.count);
                  tooltip.classed('hidden', false);
                })
                .on('mouseout', function (d) {
                  d3.select('#tooltip2').classed('hidden', true);
                });
              d3.select('#hw4_2 #axisX')
                .attr('transform', 'translate(0,' + (h - padding + 20) + ')')
                .call(xAxis);
              d3.select('#hw4_2 #axisY')
                .attr('transform', 'translate(' + (padding - 20) + ',0)')
                .call(yAxis);
            }
            function unique2(array) {
              var n = [];
              for (var i = 0; i < array.length; i++) {
                if (n.indexOf(array[i]) == -1) {
                  n.push(array[i]);
                }
              }
              return n;
            }
            function btnList2(dataSet) {
              var catArr = dataSet.map(function (d) {
                return d.category;
              });
              var uCatArr = unique2(catArr);
              var fCatArr = uCatArr.filter(function (d) {
                return d != '';
              });
              var selection = d3.select('#hw4_2')
                .append('select')
                .selectAll('option')
                .data(fCatArr);
              selection.enter().append('option')
                .attr({
                  value: function (d) {
                    return d;
                  }
                })
                .text(function (d) {
                  return d;
                });
              selection.exit().remove();
              d3.select('#hw4_2 select').on('change', function (d) {
                var category = d3.select('#hw4_2 select').property('value');
                var newDataSet = dataSet.filter(function (d) {
                  return d.category == category;
                });
                update2(newDataSet);
              });
              function update2(newDataSet) {
                // var newDataSet = dataSet.filter(function (d) {
                //   return d.category == category;
                // });
                bind2(newDataSet);
                render2(newDataSet);
              }
              d3.select('#timer').on('click', function (d) {
                var dateArr = unique2(dataSet.map(function (d) {
                  return d.date;
                }));
                var i = 0;
                d3.select('#hw4_2 svg').append('text')
                .attr({
                  id: 'showYear',
                  x: w-padding-400,
                  y: h-padding,
                  fill: 'black',
                  opacity: 0.2,
                  'font-size': 200
                });
                window.setInterval(function () {
                  var category = d3.select('#hw4_2 select').property('value');
                  var newDataSet = dataSet.filter(function (d) {
                    return d.category === category && d.date === dateArr[i];
                  });
                  update2(newDataSet);
                  d3.select('#showYear').text(dateArr[i]);
                  i++;
                  if (i >= dateArr.length) {
                    i = 0;
                  }
                }, 1000);
              });
            }
          </script>
```

## <span id="ejs">EJS</span>　[回頂部](#關於我)

```js
var express = require('express');
var app = express();
app.use(express.static(__dirname + '/public'));
app.set('views', __dirname + '/views');
app.set('view engine', 'ejs');


app.get('/user', function (req, res) {
    if (req.query.x != null && req.query.y != null) {
        var x = req.query.x;
        var y = req.query.y
        var data = {
            "x": x,
            "y": y,
            "result": parseInt(x) + parseInt(y)
        };
        res.render("result", data);
    } else {
        res.render("query");
    }

    res.end();
});

app.get('/add/:x/:y', function (req, res) {
    if (req.params.x != null && req.params.y != null) {
        var x = req.params.x;
        var y = req.params.y
        var data = {
            "x": x,
            "y": y,
            "result": parseInt(x) + parseInt(y)
        };
        res.render("result", data);
    } else {
        res.send("輸入錯誤");
    }

    res.end();
});

app.get('/home', function (req, res) {
    var pokemon = {
        name: "妙蛙種子",
        img: "001.png"
    };
    res.render('home', { "pokemon": pokemon });
});

app.listen(1234);
```

## <sapn id=bs>Bootstrap</span>　[回頂部](#關於我)

```html
<link rel="stylesheet" href="dist/css/bootstrap-theme.css">
<link rel="stylesheet" href="dist/css/bootstrap.css">
<link rel="stylesheet" href="dist/css/bootstrap-theme.css.map">
<link rel="stylesheet" href="main.css">
<body>
    <div style="padding:20px;width:100%;height:50%;">
        <h1>
            <center id="body">
                <p class="bg-warning" style="padding:15px;">upload Resources</p>
            </center>
        </h1>
        <form action="addData.php" method="post" enctype="multipart/form-data">
            <input type="text" name="title" class="form-contorl my-input" style="font-size:20px;" placeholder="Topic" required>
            <br>
            <textarea class="form-contorl uploadText" style="height:200px" name="content" placeholder="key in content.." required></textarea>
            <br>
            <!--<textarea class="form-contorl uploadText" name="code" placeholder="key in code.." required></textarea>-->
            <input type="text" name="dataName"  class="my-input"  required placeholder="上傳檔案的檔案名稱"><br>
            <input type="file" name="file" id="file" class="btn btn-warning" required><br>
            <center>
                <button type="submit" class="btn btn-success">upload</button>　
                <button type="reset" class="btn btn-danger">reset</button>　
                <a href="./">go back..</a>
            </center>
        </form>
    </div>
</body>
```
