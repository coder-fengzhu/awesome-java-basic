### <font style="color:rgb(51, 51, 51);">语法</font>
find  [路径]  [匹配条件]  [动作]



**<font style="color:rgb(51, 51, 51);">expression</font>**<font style="color:rgb(51, 51, 51);"> 是可选参数，用于指定查找的条件，可以是文件名、文件类型、文件大小等等。</font>

+ <font style="color:rgb(51, 51, 51);">-name pattern</font><font style="color:rgb(51, 51, 51);">：按文件名查找，支持使用通配符</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">*</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">和</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">?</font><font style="color:rgb(51, 51, 51);">。</font>
+ <font style="color:rgb(51, 51, 51);">-type type</font><font style="color:rgb(51, 51, 51);">：按文件类型查找，可以是</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">f</font><font style="color:rgb(51, 51, 51);">（普通文件）、</font><font style="color:rgb(51, 51, 51);">d</font><font style="color:rgb(51, 51, 51);">（目录）、</font><font style="color:rgb(51, 51, 51);">l</font><font style="color:rgb(51, 51, 51);">（符号链接）等。</font>
+ <font style="color:rgb(51, 51, 51);">-size [+-]size[cwbkMG]：按文件大小查找，支持使用 + 或 - 表示大于或小于指定大小，单位可以是 c（字节）、w（字数）、b（块数）、k（KB）、M（MB）或 G（GB）。</font>



#### <font style="color:rgb(36, 41, 47);">根据文件时间戳进行搜索</font>
<font style="color:rgb(36, 41, 47);">UNIX/Linux文件系统每个文件都有三种时间戳：</font>

+ **<font style="color:rgb(36, 41, 47);">访问时间</font>**<font style="color:rgb(36, 41, 47);"> </font><font style="color:rgb(36, 41, 47);">（-atime/天，-amin/分钟）：用户最近一次访问时间。</font>
+ **<font style="color:rgb(36, 41, 47);">修改时间</font>**<font style="color:rgb(36, 41, 47);"> </font><font style="color:rgb(36, 41, 47);">（-mtime/天，-mmin/分钟）：文件最后一次修改时间。</font>
+ **<font style="color:rgb(36, 41, 47);">变化时间</font>**<font style="color:rgb(36, 41, 47);"> （-ctime/天，-cmin/分钟）：文件数据元（例如权限等）最后一次修改时间</font><font style="color:rgb(36, 41, 47);">。</font>



**<font style="color:rgb(51, 51, 51);">正数应该表示时间之前，负数表示时间之内。</font>**

<font style="color:rgb(51, 51, 51);">例如：</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(236, 234, 230);">-mtime 0</font>**<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">表示查找今天修改过的文件，</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(236, 234, 230);">-mtime -7</font>**<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">表示查找一周以前修改过的文件。</font>

<font style="color:rgb(51, 51, 51);">关于时间 n 参数的说明：</font>

+ **<font style="color:rgb(51, 51, 51);background-color:rgb(236, 234, 230);">+n</font>**<font style="color:rgb(51, 51, 51);">：查找比 n 天前更早的文件或目录。</font>
+ **<font style="color:rgb(51, 51, 51);background-color:rgb(236, 234, 230);">-n</font>**<font style="color:rgb(51, 51, 51);">：查找在 n 天内更改过属性的文件或目录。</font>
+ **<font style="color:rgb(51, 51, 51);background-color:rgb(236, 234, 230);">n</font>**<font style="color:rgb(51, 51, 51);">：查找在 n 天前（指定那一天）更改过属性的文件或目录。</font>





#### <font style="color:rgb(36, 41, 47);">借助-exec选项与其他命令结合使用</font>
<font style="background-color:rgb(246, 248, 250);">find .</font><font style="color:rgb(36, 41, 47);background-color:rgb(246, 248, 250);"> </font><font style="background-color:rgb(246, 248, 250);">-name "*.txt" -exec </font><font style="background-color:rgb(246, 248, 250);">rm </font><font style="color:rgb(36, 41, 47);background-color:rgb(246, 248, 250);">{} \;</font>

<font style="background-color:rgb(246, 248, 250);">find .</font><font style="color:rgb(36, 41, 47);background-color:rgb(246, 248, 250);"> </font><font style="background-color:rgb(246, 248, 250);">-name "*.txt" -ok </font><font style="background-color:rgb(246, 248, 250);">rm </font><font style="color:rgb(36, 41, 47);background-color:rgb(246, 248, 250);">{} \; 确认是否删除</font>

<font style="background-color:rgb(246, 248, 250);">find . </font><font style="background-color:rgb(246, 248, 250);">-type</font><font style="color:rgb(36, 41, 47);background-color:rgb(246, 248, 250);"> f </font><font style="background-color:rgb(246, 248, 250);">-mtime</font><font style="color:rgb(36, 41, 47);background-color:rgb(246, 248, 250);"> +1 </font><font style="background-color:rgb(246, 248, 250);">-name "*.txt"  -exec </font><font style="background-color:rgb(246, 248, 250);">cp </font><font style="color:rgb(36, 41, 47);background-color:rgb(246, 248, 250);">{} dir2 \;</font>

