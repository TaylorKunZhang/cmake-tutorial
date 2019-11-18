# add the library that runs
# 添加运行时库
add_library(MathFunctions MathFunctions.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
# 说明与我们链接的任何人都需要包括当前源目录才能找到MathFunctions.h，而我们不需要。
target_include_directories(MathFunctions
        INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:include>
        )

# should we use our own math functions
# 我们是否使用自己的数学函数
option(USE_MYMATH "Use tutorial provided math implementation" ON)
if (USE_MYMATH)
    target_compile_definitions(MathFunctions PRIVATE "USE_MYMATH")

    # first we add the executable that generates the table
    # 首先，我们添加生成表的可执行文件
    add_executable(MakeTable MakeTable.cxx)
    target_link_libraries(MakeTable tutorial_compiler_flags)

    # add the command to generate the source code
    # 添加命令以生成源代码
    add_custom_command(
            OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/Table.h
            COMMAND MakeTable ${CMAKE_CURRENT_BINARY_DIR}/Table.h
            DEPENDS MakeTable
    )

    # library that just does sqrt
    # 只包含sqrt的库
    add_library(SqrtLibrary STATIC
            mysqrt.cxx
            ${CMAKE_CURRENT_BINARY_DIR}/Table.h
            )

    # state that we depend on our binary dir to find Table.h
    # 声明我们依靠二进制目录找到Table.h
    target_include_directories(SqrtLibrary PRIVATE
            ${CMAKE_CURRENT_BINARY_DIR}
            )

    # state that SqrtLibrary need PIC when the default is shared libraries
    # 声明默认为共享库时，SqrtLibrary需要PIC
    set_target_properties(SqrtLibrary PROPERTIES
            POSITION_INDEPENDENT_CODE ${BUILD_SHARED_LIBS}
            )

    target_link_libraries(SqrtLibrary PUBLIC tutorial_compiler_flags)
    target_link_libraries(MathFunctions PRIVATE SqrtLibrary)
endif ()

target_link_libraries(MathFunctions PUBLIC tutorial_compiler_flags)

# define the symbol stating we are using the declspec(dllexport) when
# building on windows
# 定义标记在Windows上构建时使用declspec(dllexport)
target_compile_definitions(MathFunctions PRIVATE "EXPORTING_MYMATH")

# install rules
# 安装规则
install(TARGETS MathFunctions tutorial_compiler_flags
        DESTINATION lib
        EXPORT MathFunctionsTargets)
install(FILES MathFunctions.h DESTINATION include)