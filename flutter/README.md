# Flutter 学习笔记

- Flutter 中一切皆Widget （部件），以下内容按照前端习惯称为： 组件

## Flutter 应用结构。

1. 一个 Flutter 应用如下：

   ```dart
    // 导入Material UI组件库(可选)
    import 'package:flutter/material.dart';

    // 程序入口调用 runApp() 方法，作用是启动Flutter应用，接受一个 Widget 参数，参数代表Flutter应用
    void main() => runApp(new MyApp());

    // 自己的Flutter应用（继承自无状态组件StatelessWidget）
    class MyApp extends StatelessWidget {
      @override
      // 覆写build方法 （组件的 UI 实现），方法名称与参数是固定写法
      Widget build(BuildContext context) {
        // 使用 MaterialApp  组件为根组件
        return MaterialApp(
            // 应用名称
            title: 'Flutter Demo',
            // 主题设置
            theme: ThemeData(primarySwatch: Colors.blue),
            // 主页路由，Scaffold是页面脚手架
            home: Scaffold(
              // 导航栏
              appBar: AppBar(
                title : Text('home page')
              ),
              // 页面主体
              body: Center(
                child: Text('home page')
              )
            )
        );
      }
    }
   ```

   - Flutter 在构建页面时，会调用组件的 build 方法，widget 的主要工作是提供一个 build()方法来描述如何构建 UI 界面
   - MaterialApp 是 Material 库中提供的 Flutter APP 框架，通过它可以设置应用的名称、主题、语言、首页及路由列表等。MaterialApp 也是一个 widget。
   - home 为 Flutter 应用的首页，它也是一个 widget。
   - StatelessWidget 类是一个无状态组件。



## 无状态组件与有状态组件

  - 在Flutter中，事件流是“向上”传递的，而状态流是“向下”传递的（与react和vue框架类似）

  - 无状态组件继承自 StatelessWidget 类，使用final 定义成员变量来接收传进来的参数，并在构建 UI 时使用
    ```dart
    import 'package:flutter/material.dart';

    void main() => runApp( MaterialApp(
      title: 'Demo',
      home: App('StatelessWidget'),
    ) );

    class App extends StatelessWidget {
        final String content;

        App(this.content);
        
        @override
        Widget build(BuildContext context){
          return Scaffold(
            appBar: AppBar(
              title: Text('无状态组件'),
            ),
            body: Center(
              child: Text(content),
            )
          );
        }
    }
    ```

    - 有状态组件，要创建一个自定义有状态widget，需创建两个类：StatefulWidget和State，状态对象包含widget的状态和build() 方法。当widget的状态改变时，状态对象调用setState()，告诉框架重绘widget
    ```dart
    import 'package:flutter/material.dart';

    void main() => runApp(Counter());

    // 固定写法
    class Counter extends StatefulWidget {
      @override
      _CounterState createState() => _CounterState();
    }

    // 有状态组件
    class _CounterState extends State<Counter> {
      // 状态变量
      int count = 0;
      // 按钮点击回调函数
      _inrement() {
        setState(() {
          count++;
        });
      }

      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          title: 'demo',
          home: Scaffold(
            appBar: AppBar(
              title: Text('StatefulWidget'),
            ),
            body: Center(
              child: Text(
                '$count',
                style: TextStyle(fontSize: 60.0, color: Colors.lightBlue),
              ),
            ),

            // 右下角的一个按钮(Scaffold脚手架提供)
            floatingActionButton: FloatingActionButton(
              child: Text(
                '+',
                style: TextStyle(fontSize: 30.0),
              ),
              // 指定点击回调函数
              onPressed: _inrement,
            ),
          ),
        );
      }
    }
    ```

## Flutter 常用 Widget

---------
  - 传参皆使用命名参数
  - 布局类都有 child | children 参数接收自己的子组件
---------

### 文本类

  - **Text** 创建一个带格式的文本 
  - **DefaultTextStyle** 该组件style属性可设置一个默认文本样式，会被其所有子组件继承

### 布局类
  - **Container** 类似html中div、可设置样式。

  - **Center** 内容为完全居中效果

  - **GridView** 可滚动网格布局

  - **ListView** 可滚动列表布局

  - **Stack** 允许子组件使用 **Positioned**类 相对于 **Stack** 上下左右进行定位，基于Web的Position absolute

  - flexbox 布局系统(基于web的flexbox)

    - **Row** 具有弹性空间的水平布局类，基于Web的Flexbox
    - **Column** 具有弹性空间的垂直布局类，基于Web的Flexbox

    - 共同属性：
      - MainAxisAlignment 主轴对齐方式
      - CrossAxisAlignment 副轴对齐方式
      - mainAxisSize 设置主轴方向组件是占最大宽度还是最小宽度（默认最大）
    
    - **Expanded** 类，具有flex属性(默认为1)控制自己所占主轴长度的份数
   
### 手势处理类

  - **GestureDetector** 不具有显示效果，检测由用户做出的手势并绑定回调函数



### Material 组件

  - **Card** 带圆角和阴影的盒子

  - **ListTile** 列表项目类，图标与文字的一行排列 


## 动画

  - 基本概念

    - **Animation** 对象是Flutter动画库中的一个核心类，生成指导动画的值
    - **AnimationController** 管理 **Animation** 
    - **CurvedAnimation** 将过程抽象为一个非线性曲线.
    - **Tween** 在正在执行动画的对象所使用的数据范围之间生成值。例如，Tween可能会生成从红到蓝之间的色值，或者从0到255。
    - 使用 **Listeners** 和 **StatusListeners** 监听动画状态改变。
    
    - Flutter中的动画系统基于Animation对象的，widget可以在build函数中读取Animation对象的当前值， 并且可以监听动画的状态改变。

    - 与web不同，Flutter 中的 Animation 对象与 UI界面 没有关系，它提供的只是当前状态与值。

    - **Animation** 是一个在一段时间内依次生成一个区间之间值的类

    - **AnimationController** 