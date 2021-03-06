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

+ 私有的id属性 表示该widget实例的id
+ getId方法用来生成该实例的id
+ setId方法设置它的id

+ 私有view 表示该widget渲染时的视图对象
+ getView getter方法 如果没有手动设置那么返回系统的视图组件
+ setView 是私有view属性的setter方法

+ run方法是留给资料复写的 主要用来执行渲染 该方法可以使用render来渲染指定的视图文件
+ render方法可以用一个数组作为参数来渲染指定的视图文件 在视图文件中数组会被展平 其key可以直接被使用
  该方法一般会在run方法中被调用 
+ renderFile 方法用指定的参数渲染一个文件 区别于render方法的是它使用文件路径而不是逻辑名渲染
+ getViewPath 返回本类实例的视图的基本路径 内部使用反射 路径是本类所在目录下的views目录
