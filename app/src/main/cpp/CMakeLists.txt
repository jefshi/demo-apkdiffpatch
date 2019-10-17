# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

aux_source_directory(apk_diff_patch APK_DIFF_PATCH_SOURCE_FILES)
aux_source_directory(bzip2 BZIP2_SOURCE_FILES)
aux_source_directory(HDiffPatch HDIFF_PATCH_SOURCE_FILES)
aux_source_directory(lzma/C LZMA_SOURCE_FILES)
aux_source_directory(zlib1.2.11 ZLIB_SOURCE_FILES)

add_library( # Sets the name of the library.
        native-lib

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        native-lib.cpp)

add_library( # Sets the name of the library.
        apk-diff-patch-lib

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        ${APK_DIFF_PATCH_SOURCE_FILES}
        ${BZIP2_SOURCE_FILES}
        ${HDIFF_PATCH_SOURCE_FILES}
        ${LZMA_SOURCE_FILES}
        ${ZLIB_SOURCE_FILES})

# 指定头文件库路径
include_directories(
        bzip2
        include
        lzma/C)

# Searches for a speLzmaDeccified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
        log-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        log)

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
        native-lib

        # Links the target library to the log library
        # included in the NDK.
        ${log-lib})