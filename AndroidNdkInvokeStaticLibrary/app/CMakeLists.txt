# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

# 隐藏符号
set(CMAKE_C_VISIBILITY_PRESET hidden)
set(CMAKE_CXX_VISIBILITY_PRESET hidden)

#打印LOG
message(STATUS "Cmake build type is: "${CMAKE_BUILD_TYPE})
message(STATUS "Cmake build android abi is: "${ANDROID_ABI})

#设置变量
set(LOCAL_PATH ${CMAKE_SOURCE_DIR}/src/main/cpp)

#引入头文件
include_directories(
        ${LOCAL_PATH}/libs/include/cjson/
        ${LOCAL_PATH}/cjsonutils/
        )

#导入静态库
add_library(cjson STATIC IMPORTED)
set_target_properties(cjson    PROPERTIES IMPORTED_LOCATION    ${LOCAL_PATH}/libs/${ANDROID_ABI}/libcjson.a )

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

add_library( # Sets the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/native-lib.cpp
             ${LOCAL_PATH}/cjsonutils/CCjsonUtils.cpp  #创建的C++工具类, 用于对C源码进行封装, 便于简化接口
             )

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
                       native-lib
                       cjson  #链接静态库

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )