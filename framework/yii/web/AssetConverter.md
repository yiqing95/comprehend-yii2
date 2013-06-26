可以转换将几种流行的脚本格式转换为   css js

         'less' => array('css', 'lessc {from} {to}'),
   		'scss' => array('css', 'sass {from} {to}'),
 		'sass' => array('css', 'sass {from} {to}'),
 		'styl' => array('js', 'stylus < {from} > {to}'),

对不同的脚本后缀使用不同的命令 ！

exec 是执行系统命令的函数 可以根据脚本后缀选择执行某个命令