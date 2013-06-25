类说明
============
该类用来移除html文本中的标记间的空字符 如：
> <a>xx</a>   <x.. 变为<a>xx</a><x

实现说明
-----------
底层用到了php缓存来获取输出  然后用正则替换掉内容中的空格：
实现核心:
> public function run()
>  	{
>  		echo trim(preg_replace('/>\s+</', '><', ob_get_clean()));
>  	}

举一反三
-----------
本质是对字符串的查找和替换  试想如果run方法的实现变为可以指定任意查找替换模式
用一个类照抄此类 但暴露查找替换的元素 这样很容易实现内容的脏字过滤替换功能！


##使用示例##
 ```php
  <body>
      <?php Spaceless::begin(); ?>
          <div class="nav-bar">
              <!-- tags -->
          </div>
          <div class="content">
              <!-- tags -->
          </div>
      <?php Spaceless::end(); ?>
  </body>
  ```
