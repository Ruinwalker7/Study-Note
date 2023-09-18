# HTML



<img src="https://www.runoob.com/wp-content/uploads/2013/06/02A7DD95-22B4-4FB9-B994-DDB5393F7F03.jpg" alt="img" style="zoom: 50%;" />

## 基础

```html
<h1>标题</h1>
<p>段落</p>
<a href="www.baidu.com">链接</a>
<img decoding="async" src="/images/logo.png" width="258" height="39" />  <!-- 图片 -->
<hr><!--水平线-->
<br> <!--换行-->
```

### HTML 格式化标签

HTML 使用标签 <b>("bold") 与 <i>("italic") 对输出的文本进行格式, 如：**粗体** or *斜体*

**通常标签 <strong> 替换加粗标签 <b> 来使用, <em> 替换 <i>标签使用。**

```html
<big>这个文本字体放大</big>
<em>这个文本是斜体的</em>
<i>这个文本是斜体的</i>
<small>这个文本是缩小的</small>
这个文本包含
<sub>下标</sub>
这个文本包含
<sup>上标</sup>
<del>删除字</del>
```



### 计算机输出标签

```html
<code>代码</code>
<var>变量</var>
<pre>预格式文本</pre>   <!-- 文本啥样就啥样-->
```



### 引文, 引用, 及标签定义

```html
<p><bdo dir="rtl">该段落文字从右到左显示。</bdo></p>  
```

<p>该段落文字从左到右显示。</p>  
<p><bdo dir="rtl">该段落文字从右到左显示。</bdo></p>  



#### 地址

```html
<address>地址</address>
```

<address>
Written by <a href="mailto:webmaster@example.com">Jon Doe</a>.<br> 
Visit us at:<br>
Example.com<br>
Box 564, Disneyland<br>
USA
</address>



### 链接

```html
<a href="https://www.runoob.com/" target="_blank" rel="noopener noreferrer">访问菜鸟教程!</a>
<p>如果你将 target 属性设置为 &quot;_blank&quot;, 链接将在新窗口打开。</p>


```



### 头部

可以添加在头部区域的元素标签为: <title>, <style>, <meta>, <link>, <script>, <noscript> 和 <base>。

#### HTML <link> 元素

<link> 标签定义了文档与外部资源之间的关系。

<link> 标签通常用于链接到样式表:

```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```





### 表格

HTML 表格由 **<table>** 标签来定义。

- **tr**：tr 是 table row 的缩写，表示表格的一行。
- **td**：td 是 table data 的缩写，表示表格的数据单元格。
- **th**：th 是 table header的缩写，表示表格的表头单元格。



### 列表

```html
<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>
```

<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>

```html
<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>
```

<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>



### 区块

HTML 可以通过 <div> 和 <span>将元素组合起来。



### 表单

HTML 表单用于收集用户的输入信息。

HTML 表单表示文档中的一个区域，此区域包含交互控件，将用户收集到的信息发送到 Web 服务器。



表单元素是允许用户在表单中输入内容，比如：文本域（textarea）、下拉列表（select）、单选框（radio-buttons）、复选框（checkbox） 等等。

表单元素的类型时使用`type`指定的





div button input label img 