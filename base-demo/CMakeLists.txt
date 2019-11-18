# 设置运行此配置文件所需的CMake最低版本
cmake_minimum_required(VERSION 3.15)

# set the project name
# 设置项目名称
project(Tutorial)

# add the executable
# 添加一个可执行文件
add_executable(Tutorial tutorial.cxx)