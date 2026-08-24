# Robot Description files for OpenArm

This package contains description files to generate OpenArm URDFs (Universal Robot Description Files). See [documentation](https://docs.openarm.dev/software/description) for details.

## Related links

- 📚 Read the [documentation](https://docs.openarm.dev/software/description)
- 💬 Join the community on [Discord](https://discord.gg/FsZaZ4z3We)
- 📬 Contact us through <openarm@enactic.ai>

## License

[Apache License 2.0](LICENSE.txt)

Copyright 2025 Enactic, Inc.

## Code of Conduct

All participation in the OpenArm project is governed by our
[Code of Conduct](CODE_OF_CONDUCT.md).


为什么这样设计？
这种入口文件 + 宏定义 + YAML 配置分离的架构有以下好处：

层	作用	可替换性
{type}.urdf.xacro	顶层入口，定义参数	换 v11.urdf.xacro 就是新版本
openarm_robot.xacro	宏定义模板，v10/v11 共用	不用改，除非换结构
config/arm/v10/*.yaml	纯数据参数	调参数不碰代码
urdf/arm/*.xacro	几何和运动学宏	改结构时才动

这样设计后，如果要升级到 v11，只需要：
新建 v11.urdf.xacro（指向 config/arm/v11/ 的新参数文件）
arm_type:=v11 启动即可
宏定义和包含结构完全复用


openarm.bimanual.ros2_control.xacro的作用形式：
controller_manager 订阅 robot_description，读到 <ros2_control> 标签后：
找到 <plugin>openarm_hardware/OpenArm_v10HW</plugin>
调用 pluginlib 去加载对应的 C++ 类
本质上openarm_robot.xacro里的 <ros2_control> 标签就是一个配置清单：告诉 controller_manager 加载哪个硬件驱动、传什么参数、有哪些关节、每个关节支持什么指令

功能包使用：
ros2 launch openarm_description display_openarm.launch.py arm_type:=v11
# jgzharm_description
Robot URDF/SDF models and meshes for dual-arm manipulation simulation.
