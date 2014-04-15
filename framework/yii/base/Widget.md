Widget类是yii 或者你的app中的所有widget的基类
=======================

该类扩展了组件类并实现了ViewContextInterface
----------

##属性
-----
- 静态的counter 用来计数 为生id时会用此做后缀
- autoIdPrefix 自动生成id的前缀是'w'
- stack 静态堆栈数组 widget可以嵌套 渲染时遵循先进后出 后进先出的堆栈特点所以需要堆栈功能


