cmake_minimum_required(VERSION 3.5)

project(ffms2)
set(CMAKE_TOOLCHAIN_FILE C:/Users/wrose/Documents/development/vcpkg/scripts/buildsystems/vcpkg.cmake)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake/)

find_package(FFmpeg REQUIRED)


set(ffms_src
    src/core/audiosource.cpp
    src/core/ffms.cpp
    src/core/filehandle.cpp
    src/core/indexing.cpp
    src/core/track.cpp
    src/core/utils.cpp
    src/core/videosource.cpp
    src/core/videoutils.cpp
    src/core/zipfile.cpp

    )
message("FFMS2  ****  ${FFMPEG_INCLUDE_DIRS} ")
message("FFMS2  ****  ${FFMPEG_LIBRARIES} ")

set(inc_dirs "include/" ${FFMPEG_INCLUDE_DIRS} "zlib")

add_library(ffms2_static ${ffms_src})
target_compile_definitions(ffms2_static PUBLIC FFMS_STATIC=1 WITH_SWRESAMPLE=1)
target_include_directories(ffms2_static PUBLIC ${inc_dirs})

add_library(ffms2_static_fat ${ffms_src})
target_compile_definitions(ffms2_static_fat PUBLIC FFMS_STATIC=1 WITH_SWRESAMPLE=1)
target_include_directories(ffms2_static_fat PUBLIC ${inc_dirs})
target_link_libraries(ffms2_static_fat ${FFMPEG_LIBRARIES})

add_library(ffms2_shared SHARED ${ffms_src})
target_compile_definitions(ffms2_shared PUBLIC FFMS_EXPORTS=1 WITH_SWRESAMPLE=1)
target_include_directories(ffms2_shared PUBLIC ${inc_dirs})
target_link_libraries(ffms2_shared ${FFMPEG_LIBRARIES})


add_executable(ffms_indexer src/index/ffmsindex.cpp)
target_link_libraries(ffms_indexer ffms2_static ${FFMPEG_LIBRARIES})

add_executable(ffms_indexer_sf src/index/ffmsindex.cpp)
target_link_libraries(ffms_indexer_sf ffms2_static_fat)




