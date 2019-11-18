# 设置运行此配置文件所需的CMake最低版本
cmake_minimum_required(VERSION 3.15)

# set the project name and version
# 设置项目名称和版本
project(Tutorial VERSION 1.0)

# specify the C++ standard
# 指定C ++标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
# 我们应该使用自己的数学函数吗
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
# 配置头文件以将某些CMake设置传递给源代码
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
# 添加 MathFunctions 库
if (USE_MYMATH)
    add_subdirectory(MathFunctions)
    list(APPEND EXTRA_LIBS MathFunctions)
    list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif ()

# add the executable
# 添加一个可执行文件
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
# 将二进制目录添加到包含文件的搜索路径中，以便我们找到TutorialConfig.h
target_include_directories(Tutorial PUBLIC
        "${PROJECT_BINARY_DIR}"
        ${EXTRA_INCLUDES}
        )