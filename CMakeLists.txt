# 这个模板的名字为 __LIB_NAME__
# 查找并替换 __LIB_NAME__
# 项目结构：
# .
# ├── CMakeLists.txt
# ├── include
# │   └── __LIB_NAME__
# │       └── __LIB_NAME__.h
# ├── src
# │   └── __LIB_NAME__.cpp
# └── Test
#     ├── CMakeLists.txt
#     └── main.cpp

cmake_minimum_required(VERSION 3.29.0)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_STANDARD 23)
# set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()  

project(__LIB_NAME__)

file(GLOB LIB_SRC src/*.cpp include/__LIB_NAME__/*.h)

add_library(__LIB_NAME__ 
                            STATIC 
#                           SHARED
                            ${LIB_SRC}
)
add_library(__LIB_NAME__::__LIB_NAME__ ALIAS __LIB_NAME__)
set_target_properties(__LIB_NAME__ 
    PROPERTIES 
    LANGUAGE CXX
)
include(CMakePackageConfigHelpers)
# set install path:
#SET(CMAKE_INSTALL_PREFIX ${PROJECT_SOURCE_DIR}/install)
target_include_directories(__LIB_NAME__ PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)
# set_target_properties(__LIB_NAME__ PROPERTIES PUBLIC_HEADER "__LIB_NAME__/lib2.h")
install(TARGETS __LIB_NAME__
    EXPORT __LIB_NAME__-targets
    PUBLIC_HEADER DESTINATION include
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)
#use this if you want to install __LIB_NAME__ directory to install-path/include but not single header
install(DIRECTORY  ${CMAKE_SOURCE_DIR}/include/__LIB_NAME__ DESTINATION include)
install(EXPORT __LIB_NAME__-targets
    NAMESPACE __LIB_NAME__::
    FILE __LIB_NAME__-config.cmake
    DESTINATION lib/cmake/__LIB_NAME__
)

include_directories(${CMAKE_SOURCE_DIR}/include)
enable_testing()
add_subdirectory(Test)
