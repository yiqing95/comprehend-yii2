Transaction
========

类说明：
----------
    表示数据库一个事务（注意如果应用使用多个库就是多个connection 那么就可能有多个事务啦！）
    用法：
      ~~~
      $transaction = $connection->beginTransaction();
      try {
          $connection->createCommand($sql1)->execute();
          $connection->createCommand($sql2)->execute();
          //.... other SQL executions
          $transaction->commit();
      } catch(Exception $e) {
          $transaction->rollback();
      }
      ~~~
    上例try段 还可以是ar 操作

协作类
--------------
    Connection