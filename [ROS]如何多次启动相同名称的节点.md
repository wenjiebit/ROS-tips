这个问题一般出现的场合有几种，需要用一份驱动程序打开多个相同的传感器，或者不同计算机通过协作，可能有节点和话题的名称产生冲突。 
对于这种情况，ROS给出了很简便的解决方案，要做的只是采用launch文件启动节点，在launch文件中使用名字空间，将一个节点用两个同的节点名称来启动。例如我们有一个GPS传感器的驱动程序节点，节点名称为GPS_Driver，如果我们需要启动一个GPS传感器时，我们只需要写这样一个启动文件
<launch>
    <node ns="rtk" pkg="GPS_Driver" type="GPS_Driver" name="GPS_Driver">
    <param name="port" value="/dev/ttyS0" />
    </node>

    <node ns="garmin" pkg="GPS_Driver" type="GPS_Driver" name="GPS_Driver">
    <param name="port" value="/dev/ttyS1" />
    </node>
</launch>
这样，使用同一份驱动程序，通过配置两个不同的串口，就能够启动两个不同的GPS传感器。运行上面的launch文件，启动的两个GPS_Driver将被冠上’ns’指定的名字空间，使用命令rosnode list可以看到
rtk/GPS_Driver
garmin/GPS_Driver
需要注意的是，这样的方式启动节点后，该节点发布的话题也将被加上名字空间，因此可能会导致有的接收节点无法接收正确的话题消息，可以使用remap消息名称的方式来解决。例如，rtk gps原先的话题名字为/fix，加上名字空间变为/rtk/fix，有个名为“process_rtk_gps”的需要接收rtk gps的定位信息，但是该程序源代码中接收的话题为/fix，这时候我们不需重新修改和编译将该程序，只要在启动该文件时remap消息名称即可，例如
<launch>
    <node pkg="process_rtk_gps" type="process_rtk_gps" name="process_rtk_gps">
    <remap from="rtk/fix" to="/fix"/>
    </node>
</launch>
