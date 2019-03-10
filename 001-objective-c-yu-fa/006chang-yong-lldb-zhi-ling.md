#### 常用LLDB指令



**1、LLDB 控制台打印信息**
- **1> print、p : 打印对象的指针**
![](/assets/Snip20190311_4.png)

- **2> po : 打印对象**
![](/assets/Snip20190311_3.png)



<br>
**2、 读取内存**
**memory read/数量格式字节数 内存地址**
![](/assets/Snip20190311_5.png)
- **1> 格式: x -> 是16进制, h -> 是浮点数, d -> 是 10 进制 **

- **2> 字节大小: b -> byte 1个字节, h -> half word 2个字节, w -> word 4个字节, g -> giant word 8个字节**