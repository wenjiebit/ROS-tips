# tips
这个问题一般在这两种情况下出现，一是直接从其他计算机上拷贝的程序包放在本地计算机上编译时，二是对于本地已经写好的程序修改了msg文件，不起作用的意思是原本正常使用的msg文件失效了，就算重新编译，源程序也无法正确使用新编译的msg文件，例如原先有个程序节点发布了自定义消息my_msg，后来更改了my_msg.msg里的内容，重新编译后，该节点发布的my_msg使用命令rostopic echo 就无法输出了。 
这中情况的解决方案是，注释掉CMakeList.txt中生成可执行文件的部分，先编译msg文件，再将源程序编译成可执行文件。
