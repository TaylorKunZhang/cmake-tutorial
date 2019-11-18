add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
# 说明与我们链接的任何人都需要包含当前源目录才能找到 MathFunctions.h，而我们不需要。
target_include_directories(MathFunctions
        INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
        )

# does this system provide the log and exp functions?
# 该系统是否提供log和exp函数？
include(CheckSymbolExists)
set(CMAKE_REQUIRED_LIBRARIES "m")
check_symbol_exists(log "math.h" HAVE_LOG)
check_symbol_exists(exp "math.h" HAVE_EXP)

if(HAVE_LOG AND HAVE_EXP)
    target_compile_definitions(MathFunctions
            PRIVATE "HAVE_LOG" "HAVE_EXP")
endif()

# install rules
# 安装规则
install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)