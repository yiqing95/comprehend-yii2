Widget类是yii 或者你的app中的所有widget的基类
=======================

该类扩展了组件类并实现了ViewContextInterface
----------

##属性
-----
- 静态的counter 用来计数 为生id时会用此做后缀
- autoIdPrefix 自动生成id的前缀是'w'
- stack 静态堆栈数组 widget可以嵌套 渲染时遵循先进后出 后进先出的堆栈特点所以需要堆栈功能

## 方法
-----
-  静态begin方法用来开始本类widget实例的创建 参数是widget的配置属性，这个方法
   等价yii1.x 中的$this->beginWidget 但可获得IDE的智能提示的有点。
  
- 静态方法end 用来结束widget 其输出直接渲染。
- begin和end之间经常输出纯的html代码。
_ 静态widget方法使用配置数组来创建本类实例 并返回渲染的字符串；该类是Yii1.x中控制器(BaseController::)widget方法的替代出现
  拥有IDE智能提示的优点。

