---
title: kaikeba_0629
date:
tags:
---
kaikeba
<!-- more -->

### novalidate与required
```
Q:如果form设置了 novalidate, 但是在input中又设置了required, 哪个权重大?
A:我的测试结果是:novalidate生效
```

### box-sizing:border-box
```css
.tabs {
    width: 400px;
    margin: 30px auto;
    background-color: #FFF;
    border: 1px solid #C0DCC0;
    box-sizing: border-box; //------------->
}
```

### display:flex
```css
.tabs nav {
    height: 40px;
    text-align: center;
    line-height: 40px;
    overflow: hidden;
    background-color: #C0DCC0;
    display: flex; //------------->
}
```

### text-decoration:none
```css
nav a {
    display: block;
    width: 100px;
    border-right: 1px solid #FFF;
    color: #000;
    text-decoration: none; //------------->
}
```

### 自定义属性
```js
自定义属性--13自定义属性案例
(function (key) {
    //获取所有的a标签,判断有没有a标签应用了类样式
    var aObjs = document.querySelectorAll("nav a");
    for (var i = 0; i < aObjs.length; i++) {
        //判断a标签是否应用了类样式
        if (!aObjs[i].classList.contains("active")) {
            //设置默认的第一个a标签应用类样式,同时找到a对应的section显示出来
            aObjs[key].classList.add("active");
            //获取第一个a标签的自定义属性的值
            var attr = aObjs[key].dataset["cont"];
            //attr作为id属性值获取对应的section元素
            document.getElementById(attr).style.display = "block";
        }

        //为每个a注册点击事件
        aObjs[i].onclick = function () {
            //找到应用了active的a,干掉这个类样式,干掉这个a对应的section的样式
            var active = document.querySelector(".active");
            active.classList.remove("active");
            document.getElementById(active.dataset["cont"]).style.display = "none";
            //当前被点击的a应用类样式.同时设置对应section显示
            this.classList.add("active");
            document.getElementById(this.dataset.cont).style.display = "block";
        };
    }
})(0);
```

### 读取本地图片
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta http-equiv = "X-UA-Compatible" content = "IE=edge,chrome=1" />
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
<title>html5camera</title>
</head>
<body>
<div id="main">
  <div class="demo">
    <p>
      <label>请选择一个图像文件(请使用高级浏览器浏览：如Chrome，Firefox)：</label><br>
      <input type="file" id="file_input" />
    </p>
    <div id="result">
      <!-- 这里用来显示读取结果 -->
    </div>
  </div>
  <br/>

  <br/>
</div>
<script type="text/javascript">
    var result = document.getElementById("result");
    var input = document.getElementById("file_input");

    if(typeof FileReader === 'undefined'){
        result.innerHTML = "抱歉，你的浏览器不支持 FileReader";
        input.setAttribute('disabled','disabled');
    }else{
        input.addEventListener('change',readFile,false);
    }


    function readFile(){
        var file = this.files[0];
        if(!/image\/\w+/.test(file.type)){
            alert("请确保文件为图像类型");
            return false;
        }
        var reader = new FileReader();
        reader.readAsDataURL(file);

        reader.onload = function(e){
            //alert(3333)
            //alert(this.result);
            result.innerHTML = '<img src="'+this.result+'" alt=""/>'
        }
    }
</script>
</body>
</html>
```
