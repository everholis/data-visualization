# 001 D3.js study

[TOC]



## 기초

- D3는 UTF-8 문자 사용

  ```javascript
  <DOCTYPE html> <meta charset="utf-8">
  ```

  or

  ```javascript
  <script charset="utf-8" src="d3.js"></script>
  ```

  or

  ```javascript
  <!-- UTF-8 문자를 포함하지 않는 압축된 스크립트 사용 -->
  <script src="d3.min.js"></script>
  ```

  

- 데이터 표준

  | 타입 | 함수           | 설명                                                         |
  | ---- | -------------- | ------------------------------------------------------------ |
  | csv  | d3.csv()       | 쉼표(,)를 구분자로 사용하는 csv 데이터 로드                  |
  | tsv  | d3.tsv()       | 탭(\t)을 구분자로 사용하는 tsv 데이터 로드                   |
  | json | d3.json()      | json 데이터 로드                                             |
  | dsv  | d3.dsvFormat() | 사용자 정의 구분자를 사용하는 데이터 로드<br />ex) var psv = d3.dsvFormat("\|");  <br />       psv.parse("foo\|bar\n1\|2")); |
  | xml  | d3.xml()       |                                                              |
  | text | d3.text()      |                                                              |
  | html | d3.html()      |                                                              |

  

## 데이터 로드

- csv 데이터 로드 예

  ```javascript
  d3.csv("mydata.csv", function(error, d){
      //mydata.csv 의 모든 data가 d 로 전달됨
      
      if(error) throw error;
      
      var dataSet = [ ];
      for(var i=0; i<d.length; i++){
         dataSet.push(d[i].item1);
      }
    
      d3.select("div") 	// div 요소 지정
        .data(dataSet)  	// 데이터를 요소에 연결
        .enter()  		// 데이터 개수만큼 반복
});
  ```
  
  

## 주요 함수

#### select(), selectAll()

선택된 요소의 selection 객체를 리턴, 만일 요소가 없는 경우 빈 selection 객체를 리턴



#### data()

select()로 선택된 DOM 요소와 인자로 입력된 데이터셋을 연결한다.

```javascript
// 이 경우 연결된 <p> 태그가 없는 상황이라면, data()호출로 연결된 selection이 
// 없기 때문에, text()호출시 아무일도 일어나지 않는다.
// 만이 <p> 한개 있다면 <p>2</p>  DOM 요소를 보게 될 것이다.
var dataset = [ 2, 4, 6, 8, 10 ];
var selection = d3.select("body") 
	selection = selection.selectAll("p")
	selection = selection.data(dataset) 
	selection = selection.text(function(d){return d;});

```



#### datum()

select()로 선택된 단일 DOM 요소와 인자로 입력된 단일데이터를 연결한다.

```javascript
<body>
    <p>D3 exammple</p>
    <script>

    d3.select("body")
        .select("p")
        .datum(100)
        .text(function (d, i) {
            return d;
        });
    </script>
</body>
```





#### enter() 

select()로 선택된 DOM 요소와 데이터셋이 많은 경우, 그 차이 만큼 빈공간을 확보하여 selection 객체를 리턴한다.



#### append() 

selection 객체의 빈공간에  DOM 요소를 추가한다.



#### exit()

선택된 DOM 요소 보다 데이터셋의 개수 적은 경우, 남은 DOM 요소의 selection 객체를 리턴한다.



#### remove()

남은 DOM 요소를 제거한다.



#### attr()

선택된 DOM element의 정적 속성을 적용한다. 

```javascript
d3.select("p").attr("id","myid");
```

*정적속성 : 예를 들어 전구?? 정도*



#### property()

선택된 DOM element의 동적 속성을 적용한다.

```javascript
 d3.select("input").property("checked",true);
```

*동적속성 : 예를 들어 전기?? 정도*



#### style()

선택된 DOM element의 스타일을 적용한다.

```javascript
 d3.select("p").style("color", "red")
```



#### classed()

선택된 DOM element의 class attribute를 적용하거나, 스타일을 적용한다.

- 클래스 추가하기

  ```javascript
  d3.select(".myclass").classed("yourclass", true);
  ```

- 클래스 제거하기

  ```javascript
  d3.select(".myclass").classed("yourclass", false);
  ```

- 클래스 검사하기

  ```javascript
  //d3.select(".myclass").classed("yourclass");
  d3.select("p").classed("yourclass");
  ```

  선택된 DOM element가 yourclass 클래스라면 true, 아니면 false
  
  

## D3 초기화 단계 구조화

D3 초기화 단계를 나누는 이유는 정확히 파악은 안되었으나, 함수에서 리턴되는 되는 객체와 this에서 차이가 있는 
것 같다. 
이렇게 하여야 element가 데이터셋 보다 많은 경우라도, 둘이 동일하게 맞춰진다.

```javascript
<script src="../data/d3/d3.js"></script>
<script>
    
// 요소 와 데이터셋 바인드 단계
var s = d3.select("body").selectAll("p").data(dataset);

// 필요한 요소 추가 단계
	s.enter().append("p");

// 남는 요소 제거 단계
	s.exit().remove();

// Data Visualization 처리
	s.text(function(d){ return d;});

</script>
```


​	

## 함수 인자

append(), style(), text() 와 같은 함수는 인자로 함수를 사용할 수 있다.

```javascript
// 예제 1) 
.text(function(d) {
    return d;
});

// 예제 2) 
.text(function (d, i) {
    console.log(d); // the data element
    console.log(i); // the index element
    console.log(this); // the current DOM object

    return d;
});
```



## Event Handling

#### selection.on()

이벤트 리스너를 추가 / 삭제한다.  

```javascript
d3.selectAll("div")
    .on("mouseover", function(){
    // Get current event info
    console.log(d3.event);
});

```



#### selection.dispatch()

선택한 element에 이벤트를 발생시킨다.

```javasc
<script>
var elem = d3.select("div")
    .on("mouseleave", function(){
    // Get current event info
    console.log(d3.event);
});

d3.select("#btn")
  .on("click", function() {
    elem.dispatch("mouseleave");
  });
</script>
<button id="btn">Dispatch mouseleave to all div</button>  
```



#### d3.event

발생한 DOM event 객체

```javascript
d3.select("div")
	.on('click', function(data,index){
        d3.event; // => Original DOM Event
    	console.log(d3.event.x, d3.event.y)
    });

```



#### d3.mouse(container)

현재 마우스 위치 좌표 (x, y) 정보

```javascript
d3.selectAll("div")
    .on("mouseover", function(){

        // Get x & y co-ordinates
        console.log(d3.mouse(this));
	})
```



#### d3.touch()

Next







## Animation



#### selection.transition()

객체에 대하여 변환 객체를 생성

#### transition.duration()

변환 객체에 대해 몇 밀리초 내에 진행되는지 지정함.

#### transition.ease()

변환시 움직임에 효과를 지정한다.

easeElastic , easeBounce , easeLinear , easeSin , easeQuad
easeCubic , easePoly , easeCircle , easeExp , easeBack

#### transition.delay()

변환이 몇 초 뒤에 시작되는지 지정함.



#### transition example

```javascript
// 파란색 원을 4초동안, x축으로 40px 에서 500px 까지 이동
var svg = d3.select("body") // body 요소 선택
    .append("svg")          // 선택된 body 요소에 SVG 요소 추가
    .attr("width", 600)     // 가로는 600 px
    .attr("height", 500)    // 세로는 500 px
    .append("circle")       // SVG에 원을 하나 추가함
    .attr("fill", "blue")   // 파란색 원
    .attr("r", 20)          // 반지름은 20px
    .attr('cx', 40)         // x축 좌표
    .attr('cy', 20)        // y축 좌표
    .transition()           // 변환객체 생성
	.ease(d3.easeBounce)	// 통통튀는 효과를 지정함
    .duration(4000)         // 변환 진행 시간은 4초(4000밀리초)
    .attr('cx', 500);       // 변환시킬 속성은 cx (x축으로 920까지 변환할 것임)

</script>
```

```javascript
// 파란색 원을 4초동안, x축으로 40px 에서 500px 까지 이동,
// 이후, 4초동안, y축으로 20px 에서 400px 까지 이동
var svg = d3.select("body") // body 요소 선택
    .append("svg")          // 선택된 body 요소에 SVG 요소 추가
    .attr("width", 600)     // 가로는 600 px
    .attr("height", 500)    // 세로는 500 px
    .append("circle")       // SVG에 원을 하나 추가함
    .attr("fill", "blue")   // 파란색 원
    .attr("r", 20)          // 반지름은 20px
    .attr('cx', 40)         // x축 좌표
    .attr('cy', 20)        // y축 좌표
	// 1차 변환
    .transition()           // 변환 객체 생성
    .duration(4000)         // 변환 진행 시간은 4초(4000밀리초)
    .attr('cx', 500);       // 변환시킬 속성은 cx (x축으로 920까지 변환할 것임)
    // 2차 변환
	.transition()           // 변환 객체 생성
    .duration(4000)         // 변환 진행 시간은 4초(4000밀리초)
    .attr('cy', 400);  		// 변환시킬 속성은 cy (y축으로 400까지 변환할 것임)
```



## Data Loading

csv, json, tsv, xml 파일로 부터, 데이터를 로드하여 데이터셋을 적용한다.



| data.csv                                |
| --------------------------------------- |
| Name, Age<br />David, 32<br />Juila, 20 |

```javascript
<script>
d3.csv("./data.csv", function(data) {
    for (var i = 0; i < data.length; i++) {
        console.log(data[i].Name);
        console.log(data[i].Age);
    }
});
</script>
```



#### SVG

- Scalable Vector Graphics
- 선, 사각형, 원 등의 모양을 표현할 수 있다.
- text 기반의 이미지를 그린다.
- 좌측 상단 모서리(0,0)에서 부터, 상대 좌표로 위치를 표현한다.

#### Rect

```html
<!DOCTYPE html>
<meta charset="utf-8">


<body>
<script src="https://d3js.org/d3.v4.min.js"></script>

<script>
    var width = 500;
    var height = 500;

    //Create SVG element
    var svg = d3.select("body")
            .append("svg")
            .attr("width", width)
            .attr("height", height);

    //Create and append rectangle element
    svg.append("rect")
            .attr("x", 0)
            .attr("y", 0)
            .attr("width", 200)
            .attr("height", 100)
</script>
    
<br>    
<svg width="500" height="500">
    <rect x="100" y="100" width="200" height="200"></rect>
</svg>

</body>
</html>
```

#### Line

```html
<body>
<script>
    var width = 500;
    var height = 500;

    //Create SVG element
    var svg = d3.select("body")
    .append("svg")
    .attr("width", width)
    .attr("height", height);

    //Create line element inside SVG
    svg.append("line")
       .attr("x1", 100)
       .attr("x2", 500)
       .attr("y1", 50)
       .attr("y2", 50)
       .attr("stroke", "black")
</script>
    
<br>    
<svg width="500" height="500">
    <line x1="100" y1="50" x2="500" y2="250" stroke="black" />
</svg>
    
</body>
```

#### Circle

```html
<body>
    
</body>
<script>
    var width = 500;
    var height = 500;

    //Create SVG element
    var svg = d3.select("body")
                .append("svg")
                .attr("width", width)
                .attr("height", height);

    //Append circle 
    svg.append("circle")
       .attr("cx", 250)
       .attr("cy", 50)
       .attr("r", 50)
</script>
<br>
<svg width="500" height="500">
    <ellipse cx="250" cy="25" rx="100" ry="25"/>
</svg>

</body>

```

#### ellipse

```html
<!DOCTYPE html>
<meta charset="utf-8">

<body>
<script src="https://d3js.org/d3.v4.min.js"></script>

<script>
    var width = 500;
    var height = 500;

    var svg = d3.select("body")
                .append("svg")
                .attr("width", width)
                .attr("height", height);

    svg.append("ellipse")
       .attr("cx", 250)
       .attr("cy", 50)
       .attr("rx", 150)
       .attr("ry", 50)
</script>
    
<br>    
<svg width="500" height="500">
    <ellipse cx="250" cy="25" rx="100" ry="25"/>
</svg>

</body>
</html>
```

Text

```html
<!DOCTYPE html>
<meta charset="utf-8">

<body>
<script src="https://d3js.org/d3.v4.min.js"></script>

<script>
    var width = 500;
    var height = 500;

    //Create SVG element
    var svg = d3.select("body")
                .append("svg")
                .attr("width", width)
                .attr("height", height);
    
    //Create group element
    var g = svg.append("g")
  
    //Create and append ellipse element into group
    var ellipse = g.append("ellipse")
                   .attr("cx", 250)
                   .attr("cy", 50)
                   .attr("rx", 150)
                   .attr("ry", 50)
                   .append("text")

    //Create and append text element into group
    g.append("text")
     .attr("x", 150)
     .attr("y", 50)
     .attr("stroke", "#fff")
     .text("This is an ellipse!");
</script>
    
<br>    
<svg width="500" height="500">
    <text x="250" y="25">Your text here</text>
</svg>

</body>
</html>
```

