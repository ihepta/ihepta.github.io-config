---
title: Centering in Css
thumbnailImagePosition: right
date: 2017-09-06 13:44:21
thumbnailImage: center.jpg
coverImage:
tags:
- css
categories:
---
css中的各种居中总结。
<!--excerpt-->

<!--toc-->

## Horizontally
### 行内元素
对于行内元素只需设置父级块级元素的text-align:center,
该方法适用于inline、inline-block、inline-table、inline-flex等。
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="parent">
		  	<span class="child">我是要居中的元素</span>
          </div>
      <!-- endtab -->
      <!-- tab css -->
          .parent{
		  	  text-align:center;
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
### 块级元素
块级元素水平居中只需设置其margin-left和margin-right为auto即可（通常该块级元素需要一个宽度，否则就会撑满父容器，无所谓居中不居中），常常缩写为margin:0 auto。
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="parent">
		  	<div class="child">我是要居中的元素</div>
          </div>
      <!-- endtab -->
      <!-- tab css -->
          .child{
		  	  width:200px;
			  margin:0 auto;
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
### 多个块级元素
如果有多个块级元素需要在一行中水平居中，则需要改变块级元素的display属性，可以将需要居中的元素设置成display:inline-block或者使用flexbox布局。
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="parent">
		  	<div class="inline-child">我是要居中的元素</div>
		  	<div class="inline-child">我是要居中的元素</div>
          </div>
		  // 或者
		  <div class="flex-parent">
			  <div class="child">我是要居中的元素</div>
			  <div class="child">我是要居中的元素</div>
		  </div>
      <!-- endtab -->
      <!-- tab css -->
		  .parent{
		  	  text-align:center;
		  }
		  .parent .inline-child{
			  display:inline-block;
		  }
		  .flex-parent{
			  display:flex;
			  justify-content:center;
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
## Vertically
### 行内元素（单行）
有时候一些行内的文字看上去是垂直居中的只是因为包含他们的parent有相同的上下padding。
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="parent">
		  	<a href="#0" class="child">我是要居中的元素</a>
		  	<a href="#0" class="child">我是要居中的元素</a>
          </div>
      <!-- endtab -->
      <!-- tab css -->
		  .parent{
			  padding: 30px 0;
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
如果某些情况下无法使用padding，又想垂直居中一些不会换行的文字或行内元素的时候可以使用line-height，将其值设置为和height相同。
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="parent">
		  	  我是要居中的文字
          </div>
      <!-- endtab -->
      <!-- tab css -->
		  .parent{
			  height:100px;
			  line-height:100px;
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
### 多行
对于多行的元素，使用上述的相同的padding-top和padding-bottom固然可行，但万一无法使用padding，则可以通过使用display:table配合vertical-align来实现垂直居中，因为td中的元素默认就是垂直居中。
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="center-table">
		  	  <p class="table-cell">我是多行要垂直居中的文字我是多行要垂直居中的文字我是多行要垂直居中的文字</p>
          </div>
      <!-- endtab -->
      <!-- tab css -->
		  .center-table{
			  display:table;
			  height:250px;
			  width:250px;
		  }
		  .center-table .table-cell{
			  display:table-cell;
			  vertical-align:middle;
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
垂直居中还可以使用如下flexbox的方法。(该方法必须确保父级容器有确定的高度)
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="flex-center">
		  	  <p>我是多行要垂直居中的文字我是多行要垂直居中的文字我是多行要垂直居中的文字</p>
          </div>
      <!-- endtab -->
      <!-- tab css -->
		  .flex-center{
			  display:flex;
			  flex-direction:column;
			  justify-content:center;
			  height:250px;
			  width:250px;
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
如果上述方法都不适用，则可以召唤出“幽灵元素”，就是用一个**满高**的伪元素放置在父级容器的内部，然后让内部的文字根据这个伪元素垂直居中。
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="ghost-center">
		  	  <p>我是多行要垂直居中的文字我是多行要垂直居中的文字我是多行要垂直居中的文字</p>
          </div>
      <!-- endtab -->
      <!-- tab css -->
		  .ghost-center{
			  position:relative;
		  }
		  .ghost-center::before{
			  content:" ";
			  display:inline-block;
			  height:100%;
			  vertical-align:middle;
		  }
		  .ghost-center p{
			  display:inline-block;
			  vertical-align:middle;
			  width:150px;
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
### 块级元素（块级的高度已知）
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="parent">
		  	  <p class="child">我是要垂直居中的块级元素</p>
          </div>
      <!-- endtab -->
      <!-- tab css -->
		  .parent{
			  position:relative;
		  }
		  .parent .child{
			  position:absolute;
			  top:50%;
			  height:100px;
			  margin-top:-50px;
    	  }
      <!-- endtab -->
{% endtabbed_codeblock %}
### 块级元素（块级的高度未知）
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="parent">
		  	  <p class="child">我是要垂直居中的块级元素</p>
          </div>
      <!-- endtab -->
      <!-- tab css -->
		  .parent{
			  position:relative;
		  }
		  .parent .child{
			  position:absolute;
			  top:50%;
			  transform:translateY(-50%);
    	  }
      <!-- endtab -->
{% endtabbed_codeblock %}
### 方便地使用flexbox
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="parent">
		  	  <p class="child">我是要垂直居中的块级元素</p>
          </div>
      <!-- endtab -->
      <!-- tab css -->
		  .parent{
			  display: flex;
  			  flex-direction: column;
  			  justify-content: center;
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
## Both Horizontally and Vertically
### 元素有固定的宽和高
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="parent">
		  	  <p class="child">我是要垂直居中的块级元素</p>
          </div>
      <!-- endtab -->
      <!-- tab css -->
		  .parent{
			  position:relative;
		  }
		  .parent .child{
			  width:100px;
			  height:100px;
			  position:absolute;
			  top:50%;
			  left:50%;
			  margin-top:-50px;
			  margin-left:-50px;
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
### 元素的宽和高未知
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="parent">
		  	  <p class="child">我是要垂直居中的块级元素</p>
          </div>
      <!-- endtab -->
      <!-- tab css -->
		  .parent{
			  position:relative;
		  }
		  .parent .child{
			  position:absolute;
			  top:50%;
			  left:50%;
			  transform:translate(-50%,-50%);
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
### 如果可以使用flexbox
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="parent">
		  	  <p class="child">我是要垂直居中的块级元素</p>
          </div>
      <!-- endtab -->
      <!-- tab css -->
		  .parent{
			  display:flex;
			  justify-content:center;
			  align-items:center;
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
### 如果可以使用grid
{% tabbed_codeblock %}
      <!-- tab html -->
          <div class="parent">
		  	  <span class="child">我居中啦！</span>
          </div>
      <!-- endtab -->
      <!-- tab css -->
		  .parent{
			  height:100px;
			  display:grid;
		  }
		  .parent .child{
			  margin:auto;
		  }
      <!-- endtab -->
{% endtabbed_codeblock %}
{% alert info no-icon %}
参考资料：https://css-tricks.com/centering-css-complete-guide/
{% endalert %}




