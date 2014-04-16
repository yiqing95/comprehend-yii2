#  所有WebApplication 类的基类
-----
  你可以自己继承该类 而且一般也应该有个自己的该类的子类 这样方便做单点控制

## 成员介绍
-----

- defaultRoute 默认控制器 每个应用(module)会有一个默认控制器 当url的route只有该模块的id时
  （如果不是application的话）会直接跳转到该route上
- catchAll 是一个数组配置 用来捕获所有的请求 给指定的route  常用在网站维护时

- controller 当前正在运行的控制器实例 控制流总是在不同的“域”中穿梭 但这个变量总会告诉你当前
  控制流经过的控制器是那个 这个不是总存在 在route解析期它是空的 ，所以要访问这个变量最好在某个
  action代码区间或者是view layout中进行访问。
  
+   bootstrap方法 用来启动应用 他是应用程序生命周期中的某个点 这个点可以挂一些钩子进来。
  这里主要设置了两个别名
  - @webroot 项目的根目录
  - @web 是当前项目的基url
  紧接着调用了父类的同名方法。
  
+ handleRequest方法  用来处理请求的
  主要解析url 返回内部路由 并设置到当前的requestedRoute上
  之后运行route对应的action 并返回一个Response对象
  
+ _homeUrl 是  项目的基、根url
- getHomeUrl 是_homeUrl的getter方法 获取当前app的根url 如果没有手动设置那么采用请求对象的脚本url
  这里还要根据urlManager组件的配置 决定用哪个 如果显示脚本名（经常是index.php）那么就显示带脚本路径
  如  www.yiiProject.com/[subName/]index.php  
  如果urlManager的showScriptName配置为false  则会显示项目的根url路径 同上面不同的是 没有入口脚本的路径
  ：www.yiiProject.com/[subName/]
- setHomeUrl  是_homeUrl 属性的setter方法  
  
+ getSession方法 获取应用组件session
+ getUser方法  获取应用组件user

+ coreComponents  返回该module(注意Application也是module 这里用的是组合设计模式)的核心组件：
  eg :  request response session user 
  
+ registerErrorHandler 注册错误处理器  传入的参数是app配置数组 可以看出是引用传值 用来修改配置数组的。
  里面是先判断配置文件（config/main.php）中是否配置了错误处理器  如果没配置那么使用yii自己的错误处理器
  所以 我们如果想对错误做些特殊处理那么可以考虑提供自己的错误处理器  比如发短信 发邮件功能！
