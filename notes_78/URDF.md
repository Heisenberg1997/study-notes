URDF：（Unified Robot Description Format），统一机器人描述格式，简称为URDF。ROS中的urdf功能包包含一个URDF的C++解析器，URDF文件使用XML格式描述机器人模型。
# URDF语法
## sensor/proposals
介绍传感器，如相机，光线传感器等
## link
描述链接的运动和动态属性。
## transmission
传动装置将执行器连接到接头并表示它们的机械耦合

## joint
描述关节的运动和动态特性。

## gazebo
描述仿真特性，如阻尼，摩擦等

## sensor
介绍传感器，如相机，光线传感器等

## model_state
在某个时间描述模型的状态

## model
描述机器人结构的运动学和动力学特性。

# URDF组件
http://www.guyuehome.com/wp-content/uploads/2016/05/11.png

# 教程
## 基础模型
```
<robot name="test_robot">
  <link name="link1" />
  <link name="link2" />
  <link name="link3" />
  <link name="link4" />
 
  <joint name="joint1" type="continuous">
    <parent link="link1"/>
    <child link="link2"/>
  </joint>
 
  <joint name="joint2" type="continuous">
    <parent link="link1"/>
    <child link="link3"/>
  </joint>
 
  <joint name="joint3" type="continuous">
    <parent link="link3"/>
    <child link="link4"/>
  </joint>
</robot>
```
上边的URDF模型定义了机器人的4个环节（link），然后定义了三个关节（joint）来描述环节之间的关联。

ROS为用户提供了一个检查URDF语法的工具：
```
$ sudo apt-get install liburdfdom-tools
```
安装完毕后，执行检查：
```
check_urdf my_robot.urdf
```
如果一切正常，将会有如下显示：
```
robot name is: test_robot

---------- Successfully Parsed XML ---------------

root Link: link1 has 2 child(ren)

    child(1):  link2

    child(2):  link3

        child(1):  link4
```
## 添加机器人尺寸
在表示尺寸大小时，只需要描述其相对于连接的关节的相对位置关系即可。URDF中的<origin>域就是用来表示这种相对关系。

例如，joint2相对于连接的link1在x轴和y轴都有相对位移，而且在x轴上还有90度的旋转变换，所以表示成<origin>域的参数就如下所示：
```
<origin xyz="-2 5 0" rpy="0 0 1.57" />
```
为所有关节应用尺寸：
```
<robot name="test_robot">
  <link name="link1" />
  <link name="link2" />
  <link name="link3" />
  <link name="link4" />
 
  <joint name="joint1" type="continuous">
    <parent link="link1"/>
    <child link="link2"/>
    <origin xyz="5 3 0" rpy="0 0 0" />
  </joint>
 
  <joint name="joint2" type="continuous">
    <parent link="link1"/>
    <child link="link3"/>
    <origin xyz="-2 5 0" rpy="0 0 1.57" />
  </joint>
 
  <joint name="joint3" type="continuous">
    <parent link="link3"/>
    <child link="link4"/>
    <origin xyz="5 0 0" rpy="0 0 -1.57" />
  </joint>
</robot>
```
## 添加运动学参数
如果我们为机器人的关节添加旋转轴参数，那么该机器人模型就可以具备基本的运动学参数。

例如，joint2围绕正y轴旋转，可以表示成：
```
<axis xyz="0 1 0" />
```
同理，joint1的旋转轴是：
```
<axis xyz="-0.707 0.707 0" />
```
URDF:
```
<robot name="test_robot">
  <link name="link1" />
  <link name="link2" />
  <link name="link3" />
  <link name="link4" />
 
  <joint name="joint1" type="continuous">
    <parent link="link1"/>
    <child link="link2"/>
    <origin xyz="5 3 0" rpy="0 0 0" />
    <axis xyz="-0.9 0.15 0" />
  </joint>
 
  <joint name="joint2" type="continuous">
    <parent link="link1"/>
    <child link="link3"/>
    <origin xyz="-2 5 0" rpy="0 0 1.57" />
    <axis xyz="-0.707 0.707 0" />
  </joint>
 
  <joint name="joint3" type="continuous">
    <parent link="link3"/>
    <child link="link4"/>
    <origin xyz="5 0 0" rpy="0 0 -1.57" />
    <axis xyz="0.707 -0.707 0" />
  </joint>
</robot>
```
## 图形化显示URDF模型
```
$ urdf_to_graphiz my_robot.urdf
```
让URDF图像化显示出来,
然后打开生成的pdf文件