AutoTimestamp
====================


类说明
-------------
    该类是一个行为。可以自动填充AR的创建时间（create_time）和更新时间(update_time)
    可通过如下途径附加该行为到AR上：

     ~~~
      public function behaviors()
      {
          return array(
              'timestamp' => array(
                  'class' => 'yii\behaviors\AutoTimestamp',
              ),
          );
      }
      ~~~

      改行为监听AR主体的两个事件：ActiveRecord::EVENT_BEFORE_INSERT ,ActiveRecord::EVENT_BEFORE_UPDATE ,
      并在两个事件发生前更新属性 指定的字段：
      public $attributes = array(
      		ActiveRecord::EVENT_BEFORE_INSERT => 'create_time',
      		ActiveRecord::EVENT_BEFORE_UPDATE => 'update_time',
      	);


      协作类
----------------
AR  : 该行为对象附加的主体（attach to owner）
