#关于drawSequence

直接从[https://github.com/bramp/js-sequence-diagrams](https://github.com/bramp/js-sequence-diagrams) 拿下来的代码。官网上已经实现了生成`SVG`与`CANVAS`的两种功能了，当然官网也是用`canvg`来实现svg转canvas的。

不过，官网的总大小有点大。所以我只拿它画时序图的核心代码来实现我的目的。生成base 64的图片。

## 对canvg进行了微调

在使用canvg对marker进行绘制箭头时，发现，marker中的`refX`与`refY`属性失效。所以对它进行了修复。（目前2015-10-09 canvg是有这个问题，以后版本应该会被修复）

## How to use

在需要调用的页面的底部加入以下代码

```html
<script type="text/javascript" src="canvg/rgbcolor.js"></script> 
<script type="text/javascript" src="canvg/StackBlur.js"></script>
<script type="text/javascript" src="canvg/canvg.js"></script>
<script type="text/javascript" src="sequence/underscore-min.js"></script>
<script src="raphael-min.js"></script>
<script src="sequence/sequence-diagram-min.js"></script>
<script>
~function(){
    var _diagram=document.createElement("div"),canvas=document.createElement("canvas");
    _diagram.id="diagram";
    _diagram.style.cssText='position: absolute; width: 1024px; height: 1024px; left: -1024px; top: -1024px; visibility: hidden;';
    document.body.appendChild(_diagram);
    window.drawSequence=function(sequence,cb){
        if(!sequence)return ;
        var diagram = Diagram.parse(sequence);
        _diagram.innerHTML="";
        diagram.drawSVG('diagram', {theme: 'simple'});
        var svg=document.getElementById("diagram").innerHTML;
        canvg(canvas, svg);
        var base64=canvas.toDataURL();
        cb&&cb(base64);
    };
}();
</script>
```

可以按以下方式使用drawSequence:

```javascript
drawSequence(
    //sequence代码
    '\
#Note left of A: Note to the\\n left of A\
#Note right of A: Note to the\\n right of A\
#Note over A: Note over A\
Note over A,B: Note over A,B\
    ',
    function(base64){
        //todo
        console.log(base64);
    }
);
```

## DEMO

DEMO: [http://leeenx.github.io/drawSequence/](http://leeenx.github.io/drawSequence/)

## 适用浏览器

本项目是在chrome下开发的，其它浏览器没有测试过。估计firefox和ie9以上都是可以运行的

## 感谢

感谢`js-sequence-diagrams` 和 `canvg`

js-sequence-diagrams 的GigHub地址： [https://github.com/bramp/js-sequence-diagrams](https://github.com/bramp/js-sequence-diagrams)

canvg 的GigHub地址: [https://github.com/gabelerner/canvg](https://github.com/gabelerner/canvg)


