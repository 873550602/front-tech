### 简介
相较于flex的一维的弹性布局，那么grid就可以说是二维的弹性布局了，所以grid拥有flex布局特点，而且较之更强大
### 概念
- **网格容器**
  
  display属性为grid的html元素，它是所有网格项的直接父元素
- **网格项**
  
  网格容器的直接子元素
- **网格轨道**

  两条行网格线或两条列网格线之间的部分
- **网格线**
  用于辅助描述网格结构的虚拟线，在实际web页面中并不存在
- **网格区域**
  有多个相同区域名称的网格单元组成的布局空间

### 创建一个网格不局容器
```css
.container{
    display: grid;
}
```
### 网格容器属性

1. **grid-template-columns**
   
   定义网格线的名称和网格轨道上的网格项的尺寸大小

```css
// 定义一个拥有三列，且每列的宽度为100px的网格布局
.container{
    display:grid;
    width: 600px;
    height: 600px;
    grid-template-columns:100px 100px 100px;
}
```
![](./img/1.jpg)

```css
// 定义一个带有网格线名称的布局
.container{
    display:grid;
    grid-template-columns:[line1] 100px [line2] 100px [line3] 100px [line4];
}

// 定义一个拥有三列，且每列的宽度为20%的网格布局
.container{
    display:grid;
    grid-template-columns:20% 20% 20%;
}
```