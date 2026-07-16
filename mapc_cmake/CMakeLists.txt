cmake_minimum_required(VERSION 3.11 FATAL_ERROR)

project(mapc C)

add_executable(mapc)

find_package(PkgConfig REQUIRED)
pkg_check_modules(PNG REQUIRED libpng)
pkg_check_modules(JPEG REQUIRED libjpeg)

set(MAPC_OBJS
	${CMAKE_CURRENT_SOURCE_DIR}/../share/vec3.c        
	${CMAKE_CURRENT_SOURCE_DIR}/../share/base_image.c  
	${CMAKE_CURRENT_SOURCE_DIR}/../share/solid_base.c  
	${CMAKE_CURRENT_SOURCE_DIR}/../share/binary.c      
	${CMAKE_CURRENT_SOURCE_DIR}/../share/log.c         
	${CMAKE_CURRENT_SOURCE_DIR}/../share/base_config.c 
	${CMAKE_CURRENT_SOURCE_DIR}/../share/common.c      
	${CMAKE_CURRENT_SOURCE_DIR}/../share/fs_common.c   
	${CMAKE_CURRENT_SOURCE_DIR}/../share/fs_png.c      
	${CMAKE_CURRENT_SOURCE_DIR}/../share/fs_jpg.c      
	${CMAKE_CURRENT_SOURCE_DIR}/../share/dir.c         
	${CMAKE_CURRENT_SOURCE_DIR}/../share/array.c       
	${CMAKE_CURRENT_SOURCE_DIR}/../share/list.c        
	${CMAKE_CURRENT_SOURCE_DIR}/../share/mapclib.c     
	${CMAKE_CURRENT_SOURCE_DIR}/../share/mapc.c

    ${CMAKE_CURRENT_SOURCE_DIR}/../share/fs_stdio.c
    ${CMAKE_CURRENT_SOURCE_DIR}/../share/zip.c
)

target_sources(mapc
PRIVATE
    ${MAPC_OBJS}
)

target_include_directories(mapc
PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/../share
    ${PNG_INCLUDE_DIRS}
    ${JPEG_INCLUDE_DIRS}
)

target_link_libraries(mapc
PRIVATE
    m
    ${PNG_LIBRARIES}
    ${JPEG_LIBRARIES}
)
