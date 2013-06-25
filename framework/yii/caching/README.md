**本包提供了缓存功能的实现


---------------
Cache类是一个基类 用到了模板设计方法 ，里面所有抽象方法都需要推迟到子类来实现。
该类除了提供子类需要实现的模板方法子组件外，还提供了公用方法 比如构造key 底层其实用到了MD5或者ctype_alnum  将可读key变为比较不容易冲突的键。

从源码看该类一个明显特征是该类实现了ArrayAccess 就是说缓存类可以用数组方式来操作！
$cache['myKey'] = 'myValue';  这样到等价存储方法$cache->set('myKey','myValue');  $cache['myKey']  <==> $cache->get('myKey');

还有自定义的serializer  缓存的内容如果不是常规字符串类型会被序列化的 读取时被反序列化。
默认都是使用的php内置的序列化机制。 通过这个变量可以指定自定义的序列和放序列化器。这个数组可以给定两个元素 第一个是序列化的方法（匿名方法）第二个即是反序列化的方法。



使用示例
----------------------------
  典型的使用方式
 
  ~~~
  $key = 'demo';
  $data = $cache->get($key);
  if ($data === false) {
      // ...generate $data here...
      $cache->set($key, $data, $expire, $dependency);
  }
  ~~~
 
  因为实现了ArrayAccess接口（SPL标准库接口），所以可以使用数组方式来访问
 
  ~~~
  $cache['foo'] = 'some data';
  echo $cache['foo'];
  ~~~


支持的底层缓存引擎
--------------------
> zendDataCache
> XCache
> WinCache  微软还对php有这个支持 意想不到哦！ [WinCache PHP extension](http://www.iis.net/expand/wincacheforphp)
> MemCache  同时支持Memcache和Memcached 两种缓存类型 可配置多个服务器 
> APC 
> DummyCache 哑cache  模拟了cache的功能 可以在开发时用此哑实现（占位符） 部署时替换为真正的缓存实现
> DbCache  数据库缓存 这个看似没有必要 其实不然  缓存的基本思路就是把更慢的数据放到稍快的存储介质中。一般我们缓存的会是DB中的数据 但有些数据的获取可能比访问DB还慢。默认此缓存会用来存储session数据的！
> FileCache 文件缓存实现

以上实现 有些是对已有缓存函数/驱动 的简单封装（适配器设计模式 简单委托 一端符合Yii Cache 接口的要求 另一端调用底层已有的驱动函数）

缓存依赖
--------------------
缓存依赖 缓存的失效与否先要计算该依赖  依赖的数据也被存入缓存中 每次取时会实时比较缓存中的依赖数据跟当下计算的数据是否一致 如果不一致认为失效！
换言之就是：存入时的缓存依赖计算（set） 跟取出时的缓存依赖计算（get） 二者只要不一致认为缓存失效！
> DbDependency 计算sql语句的单条返回结果 
> ExpressionDependency 可以给一个php表达式 底层用到了eval！
> FileDependency 给定一个文件路径 依赖于文件的最后修改时间
> GroupDependency 静态归组 多个缓存可以依赖一个 组依赖 这个特征很像Yii官方扩展中的Flushable扩展。静态方法invalidate可以让依赖同一个“组依赖”的所有缓存失效！注意在1.x中ar的内置缓存实现 由于很难手动让其失效（你很难知道那个缓存键--缓存要失效或者过期或者手动用这个键让其消失 但前提是你知道那个键），此时用Flashable或者这里的GroupDependency 就可以了 。
> ChainedDependency 链式缓存依赖  可以传递多个不同类型的依赖形成一个链 属性dependOnAll 可以用来告诉最终决定缓存过期是否取决于所有成员 默认是true 即所有成员的计算结果为失效整个依赖算才算失效。 否则任何一个成员失效整体都算失效！