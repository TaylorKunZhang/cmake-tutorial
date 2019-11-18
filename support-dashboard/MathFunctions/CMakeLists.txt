# first we add the executable that generates the table
# 首先，我们添加生成表的可执行文件
add_executable(MakeTable MakeTable.cxx)

# add the command to generate the source code
# 添加命令以生成源代码
add_custom_command(
        OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/Table.h
        COMMAND MakeTable ${CMAKE_CURRENT_BINARY_DIR}/Table.h
        DEPENDS MakeTable
)

# add the main library
# 添加主库
add_library(MathFunctions
        mysqrt.cxx
        ${CMAKE_CURRENT_BINARY_DIR}/Table.h
        )

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
# 说明与我们链接的任何人都需要包含当前源目录才能找到 MathFunctions.h，而我们不需要。
# state that we depend on Tutorial_BINARY_DIR but consumers don't, as the
# Table.h include is an implementation detail
# state that we depend on our binary dir to find Table.h
# 声明我们依赖Tutorial_BINARY_DIR但消费者不依赖，因为包含Table.h是一个实现细节，我们依赖二进制目录来查找Table.h
target_include_directories(MathFunctions
        INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
        PRIVATE ${CMAKE_CURRENT_BINARY_DIR}
        )

# install rules
# 安装规则
install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)