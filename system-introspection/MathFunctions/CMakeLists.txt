add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
# 说明与我们链接的任何人都需要包含当前源目录才能找到 MathFunctions.h，而我们不需要。
target_include_directories(MathFunctions
        INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
        PRIVATE ${CMAKE_BINARY_DIR}
        )

# install rules
# 安装规则
install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)