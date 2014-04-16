# AssetManager 用来管理assetBundle 以及asset资源的发布
------------

该类是一个应用程序组件 可以用Yii::$app->assetManager 来访问它 可在配置文件中修改它的配置

## 类成员解读
-------------------

- bundles 是一个配置数组 键是类名 值是配置数组或者AssetBundle的实例对象
  我们可以在配置期或者运行期 修改某些bundle的配置哦：
  ~~~
  [
          'yii\bootstrap\BootstrapAsset' => [
              'css' => [],
          ],
      ]
  ~~~
  上面的配置表示css文件不需要发布  我们要使用自己的css文件啦
  

- basePath 存放发布后的资源的基路径是@webroot/assets 即项目根目录下的assets目录 当然你也可以自定义哦
  比如统一挪到public/assets 目录下 前提是public目录也是web可访问的目录。
  
- baseUrl assets 发布后的基url 默认是@web/assets 路径  注意如果你修改了上面的路径那么这里也要做相应的调整。

- linkAssets 是否使用系统链接 默认是不使用（fasle），意味着资源文件会被拷贝到[[basePath]]目录下面， 如果使用
  了系统链接那么不会发生拷贝 这样在开发模式下很方便
  因为系统链接只在*nix操作系统，windows的Vista/2008或者之上版本可用 所以要注意宿主环境哦。

- fileMode 文件模式 拷贝时使用的模式权限  如果不指定那么根据情况决定用什么权限
- dirMode 拷贝后的文件夹权限模式 默认是0755 对本组或者所有者是可写的 但对其他用户是只读权限。


+ init方法  是应用程序组件中的生命周期方法， 经常用来做属性合法性验证 如果某些属性未赋值那么给默认值或者抛出
  异常 这里主要检查基路径是否配置正确 基文件夹是否可写 把别名转换为真实路径
  以及防止别名右侧出现多余的 / 反斜杠
  
+  getBundle方法 根据名称获取bundle实例 如果第二个参数指定为true表示顺便发布资源到web可访问的目录。
  该方法内部主要从配置数组$this->bundles 中搜寻指定名称的value值  当其为数组时使用yii的帮助方法
  Yii::createObject 来从数组创建对象
  ~~~
  $bundle = Yii::createObject(array_merge(['class' => $name], $this->bundles[$name]));
  ~~~
  上面的这用法很经典 如果配置数组中不存在class时直接使用name 存在配置时那么会用配置的class 。
  注意yii的惯用法   数组经常等价一个类的实例！
   
