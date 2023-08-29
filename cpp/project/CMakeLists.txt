cmake_minimum_required(VERSION 3.27)
project(project_name LANGUAGES CXX)

# C++标准
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 生成编译数据库
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# 项目
add_subdirectory("module_a")
