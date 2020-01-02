## 什么是`SPU`和`SKU`
`SPU`：`Standard Product Unit`标准化产品单元
`SKU`：`Stock Keeping Unit`库存量单位

简单来说`SPU`可以理解为商品，`SKU`可以理解为单品

举例来说`iPhone 11`是一个商品，与之配套的就是一系列规格，包括颜色、内存等。

通过规格能够确定唯一的手机，也就是说商品时模糊的，单品是唯一的，必须是一组规格确定的唯一的商品。

## 编程技巧

### 变量的命名

在编写复杂的业务逻辑时，要借助一些形象化或符号化的东西来借助我们思考，所以在变量的命名上面要下功夫，甚至是组件和页面的命名上要下功夫。

变量的命名要具备一些形象化和符号化的特点。

可以帮你准确的去描述一些现实中具体的事物，每一个变量都有具体的意义的话，对于我们辅助思考是非常有意义的。

### 把核心的问题抽象出来

在商品详情页，核心的问题是`SKU`。

找到问题的核心点之后，可以写一个最简单的自定义组件，然后把核心问题在这个自定义的组件上体现出来。

自定义组件在面对复杂的问题是一种很好的辅助思考的工具

## 变量命名的技巧

一般都喜欢用`sku-ccontroller`的方式来命名。

这种带有特定领域的对象的前缀命名，适合处理简单的业务逻辑。

在复杂的业务里面，当你阅读代码或者在思考的时候，`sku-`命名很容易干扰到你。

在这里用的是`realm`来命名组件，它有领域或范围的意思；规格那块用的是`fence`，它有栅栏的意思。

在复杂业务逻辑上，能有一个名词的就用一个名词，大量出现比较长的变量名，阅读起来是非常痛苦的，找一些比较鲜明的事物来命名是比较好的。

## 新建业务对象

把业务对象写在`models`中，页面或者组件中是用来绑定数据的。

这里新建两个业务对象`fence`和`fence-group`

## 提取规格

从服务端返回的数据可以看到的，是一个个单品，所以前端需要自己处理出各种规格。

```js
金属灰 七龙珠 小号S
青芒色 灌篮高手 中号M
青芒色 圣斗士 大号L
橘黄色 七龙珠 小号S
```

如果能把它顺时针旋转`90`度就满足我们的需求，这个在数学上是一个矩阵，可以用矩阵的思想来完成。

这里用矩阵的转置就能实现了，矩阵的转置也就是行列号互换。

## 代码实现1（借助矩阵思维，没有用矩阵特性）

```js
//fence-group.js
//
initFences() {
  const matrix = this._createMatrix(this.skuList)
  const fences = []
  let currentJ = -1
  matrix.each((element, i, j) => {
    if(currentJ !== j){
      // 开启一个新列，需要创建一个新的 Fence
      currentJ = j
      fences[j] = this._createFence(element)
    }
    fences[currentJ].pushValueTitle(element.value)
  })
  console.log(fences)
}

/*
  创建 Matrix，把单品传入 Matrix，返回 Matrix 类
*/
_createMatrix(skuList) {
  const m = []
  skuList.forEach(sku => {
    m.push(sku.specs)
  })
  return new Matrix(m)
}

_createFence(element){
  const fence = new Fence()
  return fence
}

//fence.js
pushValueTitle(title) {
  this.valueTitles.push(title)
}

//matrix.js
/*
  规格是一个二维数组，把遍历的逻辑封装到 Matrix 内部
  each 接收一个回调函数，给回调函数传入 element，列号，行号
*/
each(cb) {
  for (let j = 0; j < this.colsNum; j++) {
    for (let i = 0; i < this.rowsNum; i++) {
      const element = this.m[i][j]
      cb(element, i, j)
    }
  }
}
```

## 代码实现2（利用矩阵实现）

```js
//fence-gropu.js
initFences() {
  const matrix = this._createMatrix(this.skuList)
  const fences = []
  const AT = matrix.transpose()
  AT.forEach(r => {
    const fence = new Fence(r)
    fence.init()
    fences.push(fence)
  })
  console.log(fences)
}

//matrix.js
/*
  第一层遍历应该遍历 列
  第二层遍历再遍历 行
*/
transpose(){
  const desArr = []
  for(let j = 0; j < this.colsNum; j++){
    desArr[j] = []
    for(let i = 0; i < this.rowsNum; i++){
      desArr[j][i] = this.m[i][j]
    }
  }
  return desArr
}
```
```````````````````````````````````````````````````````````````````````````````````
