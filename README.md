## 搭建项目目录
- 创建一个空文件夹，名字自定，老师叫做 bigevent
- 复制资料里面的 assets 和 home 到 bigevent 里面，备用即可。

## layui
- 官网地址：https://www.layui.com
- 用什么，去官网 -> 文档里面查找即可。

## 登录注册页面
- 创建好 login.html/css/js
- 页面中，引入css(layui.css、login.css)和js(jquery.js、layui.all.js、login.js)

## 登录注册页面布局
- 自己写样式（处理页面的高度、logo、盒子的位置等）
- 使用layui的表单（官网 -> 文档 -> 页面元素(左侧边栏) -> 表单 -> 小睹为快）
- 使用字体图标（官网 -> 文档 -> 页面元素(左侧边栏) -> 图标）
- 复制登录的盒子，为注册的盒子，分别给两个盒子 login、register类
- 默认让注册的盒子隐藏

## 切换两个盒子
- 自己调整思路完成即可。

## 完成注册功能
- 注册表单的submit事件 -> 阻止默认行为 -> 通过serialize收集值 -> ajax提交
- 一定要找到注册的表单，然后注册 submit 事件。
- 通过serialize收集值的时候，需要修改input的name属性
- 接口文档要求提交username和password
  - 所以修改用户名的name="username"
  - 所以修改密码框的name="password"
  - 所以删除确认密码的name属性
- ajax提交，res.status === 0 ，表示成功（大事件所有接口都是这样）
- 无论注册成功，还是失败，都给出提示
- 注册成功
  - 清空输入框
  - 切换到登录的盒子

## layui提供的表单验证
- 文档位置：官网 -> 文档 -> 内置模块（左侧边栏） -> 表单 -> 表单验证（右侧的目录）
- 使用方法：
  - 给 input 等表单元素，添加 lay-verify 属性，属性值就是验证规则
  - 内置的验证规则：required（必填）、email（邮箱）、number（数字）....
  - 比如：`<input type="text" lay-verify="required|email" />`

## 自定义验证规则

1. 加载form模块
   - 原则是用到什么模块，就加载什么模块
   - 语法： let 变量 = layui.模块名；
   - 比如：let form = layui.form;
2. 调用 form.verify() 方法，写自定义的验证规则
   - 要给 form.verify() 传递一个对象，对象的键就是验证规则，值可以写数组，也可以写函数
   - 验证的方法如果是数组
     - len: [正则表达式, '验证不通过时的提示']
   - 验证规则如果是函数
     - 函数的形参，表示使用该验证规则的输入框的值
     - return的是验证不通过时的提示
3. HTML页面中，使用自定义的验证规则
   - 和使用内置的验证规则一样用
   - 比如 `<input type="text" lay-verify="required|len" />`

## 表单验证的注意事项

使用layui的表单验证，有几个硬性要求：

1. 表单 必须 有 layui-form 类。`class="layui-form"`
2. 按钮必须是 submit 类型（type不填或者值是submit），并且必须得有 `lay-submit` 属性。

## 完成登录功能

和注册的实现步骤是一样的（表单提交事件 -> 阻止默认行为 -> 收集表单数据 -> Ajax提交）

如果登录成功，跳转到 index.html.



## 首页布局

- 在项目根目录，创建空白的 index.html .

- layui文档 -> 页面元素 -> 布局 -> 后台布局，复制全部的代码到 index.html 中
- 修改css的路径；
- 删除掉所有的js代码，连接自己的 layui.all.js

### 头部区域

- 更换logo图
- 去掉不要的导航（ul>li 的结构）
- 其他字，改一下。

### 侧边栏处理

- 自行调整顺序（首页、文章管理、个人中心）
- 让首页默认选中，给首页加 一个 layui-this 类
- 默认让全部的菜单隐藏，去掉 文章管理 的 layui-nav-itemed 类
- 给 ul 添加 lay-shrink="all" 属性，注意不是类，这样就有手风琴效果了。

### 图标处理

- 把图标标签 ，即i标签，放到 a标签里面
- 退出（layui-icon-layout）
- 首页（layui-icon-home）
- 文章管理（layui-icon-form）
- 个人中心（layui-icon-username）
- 所有子菜单（layui-icon-app）

自行调整间距。

### 头像处理

- 复制头部的 a 标签，放到侧边栏，并且改为div，给上高60、上下左右居中。
- 考虑到新注册的账户没有头像，所以我们再做一个“文字头像”
  - 在图片左边，弄一个span，自己调整成圆形，设置好对齐等等。
  - 默认做好之后，让两个头像都隐藏。

### 浏览器的本地存储

> 允许我们在浏览器端，保存一些数据。

主要有三种技术，可以实现在浏览器端保存数据

- cookie
- localStorage 和 sessionStorage
- indexedDB

### localStorage 和 sessionStorage

- 他俩允许我们使用键值对的方式，在浏览器中保存数据
- 保存的数据**只能是字符串**。如果要保存对象，需要把 JSON.stringify(对象) 转成字符串再保存。
- 保存的数据在 浏览器控制台 --> Application 面板中可以查看。
- localStorage和sessionStorage能够保存 5M+ 数据。
- 这两个对象具有相同的API方法
  - setItem(key, value) -- 存储数据
  - getItem(key) -- 获取数据
  - removeItem(key) -- 移除数据
  - clear() -- 清空数据
  - key(index) -- 获取键名
  - length -- 存储内容的长度

示例：

```js
// 存数据
localStorage.setItem('a', 'hello');

// 存对象的，需要把对象转成字符串
localStorage.setItem('b', JSON.stringify( { id: 1, name: 'zs' } ));

// 获取数据
localStorage.getItem('a'); // hello

// 移除一项数据
localStorage.removeItem('a');

```

### localStorage 和 sessionStorage特点

**共同点**：

一个源（http://www.itcbc.com:8080）的本地存储，其他源是不能操作（获取、添加、删除）的。

比如百度在本地存储中存了一些数据，其他的网址就不能操作了。

**不同点**：

localStorage存储的数据，永久保存，及时浏览器关闭了，获取重启电脑了，本地存储中的数据也不会丢失。可以跨标签页和浏览器窗口使用。

sessionStorage，在浏览器关闭之后，就会消失了。sessionStorage保存的数据只能在当前浏览器窗口中使用（可以在不同的标签页中使用），但是不能跨浏览器窗口使用（新建一个浏览器窗口就不行了）



