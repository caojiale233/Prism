# Prism
    关键词：网页；在线仿真；三棱镜的折射；最小偏向角的测量；全反射；极坐标

## 仿真原理
三棱镜的折射现象，可以由电磁场的边值关系解释，边值关系在几何光学中的体现即为折射定律，因此仿真的关键点在于把握角度的变化以及光线-棱镜交点的坐标。
前端中实现指定长度、指定角度、指定位置的线条，可以生成一个无意义的块元素div来实现，线段长度可以用样式"width:length;"指定，
角度、位置可通过"transform:rotate(angle) translate(x,y);"设置。
在网页中，仅需设置线段两端点的极坐标即生成一条任意线段，在完成线段生成的逻辑后，三棱镜折射的问题就成为了一个“简单的”几何题。
构建整个系统在棱镜旋转θ度后的状态，即可动态观察三棱镜入射角与出射角的关系，能观察到的物理现象有最小偏向角和全反射。
除了对三棱镜仿真，代码中的许多片段也能分离出来作为一个模块单独使用。

## 技术细节
### 极坐标系下的线段在网页上生成
Javascript部分：
```javascript
function Line(){
  this.dom=document.createElement('div');
  
  /*设定位置*/
  this.set=function(r1,cta1,r2,cta2){
    let l=Math.sqrt(r1*r1+r2*r2-2*r1*r2*Math.cos(cta1-cta2));
    this.dom.style.width=l+'px';
    let cta=Math.atan((r1*Math.sin(cta1)-r2*Math.sin(cta2))/(r1*Math.cos(cta1)-r2*Math.cos(cta2)));
    this.dom.style.transform=`translate(${(r1*Math.cos(cta1)+r2*Math.cos(cta2))/2-l/2}px,${-(r1*Math.sin(cta1)+r2*Math.sin(cta2))/2-1}px) rotate(${-cta}rad)`;
  }
  
  /*插入位置(dom.appendChild)*/
  this.AC=$$_=>{document.querySelector($$_).appendChild(this.dom);return this}
  
  if(arguments.length)this.set(...arguments);
  this.dom.className='lines';
}
```
CSS部分：
```css
.lines{
    color:white;
	width:100px;
	height:2px;
	background:white;
	transform:translate(-150px,0px) rotate(0.75rad);
	position:absolute;
	top:0;
	left:0;
	transition:width 0.2s,transform 0.2s;
}
.lines_arrow::before{
	width:inherit;
	height:0;
	display:block;
	content:'▶';
	line-height:2px;
	font-size:15px;
	text-align:center;
	font-family: cursive;
	font-weight: bold;
}
```
说明：JS中所有单位均为像素(px)。

    