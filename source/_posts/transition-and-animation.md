---
title: Transition and Animation
thumbnailImagePosition: right
date: 2017-06-22 10:02:13
thumbnailImage: css.jpg
coverImage:
tags:
- css
categories:
---
css3过渡和动画学习总结。
<!--excerpt-->

<!--toc-->

很早之前就想着总结一下css3的transition和animation，可是一直没有系统地整理过，趁着近来空闲的时间稍作整理记录。

## Transition

### 过渡属性
- transition:简写属性，用于在一个属性中设置四个过渡属性。
- transition-property:规定应用过渡的CSS属性的名称，默认是all。
- transition-duration:定义过渡效果花费的时间，默认是0，即不会有过渡的效果。
- transition-timing-function:规定过渡效果的时间曲线，默认是"ease"。
- transition-delay:规定过渡效果何时开始，默认是0，触发即发生过渡。

以下两者A和B代码效果完全相同：
```
A:
div{
	transition-property:width;
	transition-duration:1s;
	transition-timing-function:linear;
	transition-delay:2s;
}
```
```
B(简写形式):
div{
	transition:width 1s linear 2s;
}
```

### 实例演示
{% tabbed_codeblock %}
      <!-- tab css -->
          .div-1{
				width:50px;
				height:50px;
				background-color:yellow;
				text-align:center;
				line-height:50px;
				border-radius:10px;
				transition:all 2s;/*所有可过渡的属性均在2s内完成*/
			}
			.div-1:hover{
				width:70px;
				height:70px;
				background-color:greenyellow;
				line-height:70px;
				border-radius:35px;
				transform:rotate(360deg);
			}
      <!-- endtab -->
      <!-- tab html -->
          <div class="div-1">hover me</div>
      <!-- endtab -->
{% endtabbed_codeblock %}
{% raw %}
<style>
	.div-1{
		width:50px;
		height:50px;
		background-color:yellow;
		text-align:center;
		line-height:50px;
		border-radius:10px;
		transition:all 2s;
	}
	.div-1:hover{
		width:70px;
		height:70px;
		background-color:greenyellow;
		line-height:70px;
		border-radius:35px;
		transform:rotate(360deg);
	}
    .animation{
        position: relative;
        width: 50px;
        height:50px;
        border-radius: 10px;
        background-color: deepskyblue;
        animation: myAnimation 4s infinite;
        animation-fill-mode: none;
    }
    @keyframes myAnimation {
        0%{
            left:0;
            background-color: deepskyblue;
        }
        50%{
            left:300px;
            background-color: lightcoral;
            transform: rotate(30deg);
        }
        100%{
            left:0;
            background-color: deepskyblue;
            transform: rotate(-360deg);
        }
    }
</style>
{% endraw %}

<div class="div-1">hover</div>

## Animation

### 动画属性
- @keyframes:规定动画，以百分比来规定改变发生的时间，或者通过关键词"from"和"to"，等价于0%和100%，尽量采用百分比的形式。
- animation:所有动画属性的简写形式，除了animation-play-state属性。
- animation-name:规定@keyframes动画的名称，默认none。
- animation-duration:规定动画完成一个周期所花费的秒或毫秒，默认是0。
- animation-timing-function:规定动画的速度曲线，默认是"ease"。
- animation-delay:规定动画何时开始，默认是0。
- animation-iteration-count:规定动画播放的次数，可以设置为n或infinite，默认是1。
- animation-direction:规定动画是否在下一个周期逆向地播放，可以设置为"alternate"即奇数次正常播放，偶数次逆向播放，默认是"normal"。
- animation-play-state:规定动画是否正在运行或暂停，可以设置为"paused"让动画暂停，大多在js中动态控制动画运行或暂停，默认是"running"。
- animation-fill-mode:规定对象动画时间之外的状态，可选forwards(当动画完成之后保持最后一个属性值，即在最后一个关键帧中定义的属性)、backwards(在animation-delay所指定的一段时间内，在动画显示之前，应用开始属性值，即在第一个关键帧中定义的属性)、both(向前和向后填充模式都被应用)，默认none。

以下两者A和B代码效果完全相同：
```
A:
div{
	animation-name: myAnimation;
	animation-duration: 5s;
	animation-timing-function: linear;
	animation-delay: 2s;
	animation-iteration-count: infinite;
	animation-direction: alternate;
	animation-play-state: running;
}
```
```
B(简写形式):
div{
	animation: myAnimation 5s linear 2s infinite alternate;
}
```
### 实例演示
{% tabbed_codeblock %}
    <!-- tab css -->
        .animation{
	        position: relative;
	        width: 50px;
	        height:50px;
	        border-radius: 10px;
	        background-color: deepskyblue;
	        animation: myAnimation 4s infinite;
	        animation-fill-mode: none;
	    }
	    @keyframes myAnimation {
	        0%{
	            left:0;
	            background-color: deepskyblue;
	        }
	        50%{
	            left:300px;
	            background-color: lightcoral;
	            transform: rotate(30deg);
	        }
	        100%{
	            left:0;
	            background-color: deepskyblue;
	            transform: rotate(-360deg);
	        }
	    }    
    <!-- endtab -->
    <!-- tab html -->
        <div class="animation"></div>
    <!-- endtab -->
{% endtabbed_codeblock %}
<div class="animation"></div>

## 两者的区别和联系
经常碰到两个相似的东西的时候，我们常常会想，这两个东西到底有什么关系，又有什么联系呢？下面引用一段知乎上看到的回答来阐述这个疑问。

{% alert info no-icon %}
transition关注的是CSS property的变化，property值和时间的关系是一个三次贝塞尔曲线。
animation作用于元素本身而不是样式属性，可以使用关键帧的概念，应该说可以实现更自由的动画效果。至于实现动画效果用哪一种，我的感觉是要看应用场景，但很多情况下transition更简单实用些。

作者：施伟伟
链接：https://www.zhihu.com/question/19749045/answer/13016286
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
{% endalert %}
以下结论摘自《HTML5与CSS3设计模式》:
{% alert info no-icon %}
CSS过渡规范允许CSS属性值在指定时间间隔中平滑地变化。通常，当CSS属性值改变时，渲染结果会立刻更新，如果使用CSS过渡，我们就能够从旧状态慢慢平滑地变化到新状态。
CSS动画与过渡类似，也是逐渐修改CSS属性的表现值。
它们的主要区别在于，过渡是在属性值发生变化时自动触发，而动画则需要在动画属性生效时显式地执行。因此，动画需要显式设置动画属性的值。这些值由关键帧指定。
----《HTML5与CSS3设计模式》
{% endalert %}
