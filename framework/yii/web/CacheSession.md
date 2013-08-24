
类说明
------------------
CacheSession 继承自标准session  这意味着session组件的配置处可以替换成这个类名
该类会使用底层的cache组件来作为session数据的持久化存储。

默认使用系统cache作为依赖（如果你不配置cache组件的话！） 但可以置换成其他cache


类职责
------------------
    作为一个session 那么就要完成php session生命周期的回调方法 可以看出read write destroy
    等方法都被实现了 底层顺利的委托给了 cache组件。


协作类
------------------
    cache 组件    yii的组件名字作为组件的key  其后可以配置任意实现了那个类的子类（里氏替换原则（Liskov Substitution principle））
    子类可以无条件替换父类出现的地方。


注意点：
    底层依赖cache组件 决定了session持久的不确定性 这是由于cache天生的脆弱不可靠导致的。你必须承受使用chache的后果（失效，被踢出局）

亮点：该类的方法：
```
protected function calculateKey($id)
	{
		return array(__CLASS__, $id);
	}

```
缓存的键值 竟然可以使用array  这样看来 缓存的键如果用对象也是可能的！！
