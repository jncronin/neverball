cmake_minimum_required(VERSION 3.11 FATAL_ERROR)

project(neverball C CXX)

add_executable(neverball)

find_package(SDL2 REQUIRED)
find_package(SDL2_ttf REQUIRED)
find_package(PkgConfig REQUIRED)
pkg_check_modules(PNG REQUIRED libpng)
if(GAMEKID)
	set(JPEG_LIBRARIES jpeg)
else()
	pkg_check_modules(JPEG REQUIRED libjpeg)
endif()
pkg_check_modules(VORBIS REQUIRED vorbis)
pkg_check_modules(VORBISFILE REQUIRED vorbisfile)
pkg_check_modules(OGG REQUIRED ogg)

set(BALL_OBJS
	${CMAKE_CURRENT_SOURCE_DIR}/share/lang.c        
	${CMAKE_CURRENT_SOURCE_DIR}/share/st_common.c   
	${CMAKE_CURRENT_SOURCE_DIR}/share/vec3.c        
	${CMAKE_CURRENT_SOURCE_DIR}/share/base_image.c  
	${CMAKE_CURRENT_SOURCE_DIR}/share/image.c       
	${CMAKE_CURRENT_SOURCE_DIR}/share/solid_base.c  
	${CMAKE_CURRENT_SOURCE_DIR}/share/solid_vary.c  
	${CMAKE_CURRENT_SOURCE_DIR}/share/solid_draw.c  
	${CMAKE_CURRENT_SOURCE_DIR}/share/solid_all.c   
	${CMAKE_CURRENT_SOURCE_DIR}/share/mtrl.c        
	${CMAKE_CURRENT_SOURCE_DIR}/share/part.c        
	${CMAKE_CURRENT_SOURCE_DIR}/share/geom.c        
	${CMAKE_CURRENT_SOURCE_DIR}/share/ball.c        
	${CMAKE_CURRENT_SOURCE_DIR}/share/ease.c        
	${CMAKE_CURRENT_SOURCE_DIR}/share/transition.c  
	${CMAKE_CURRENT_SOURCE_DIR}/share/gui.c         
	${CMAKE_CURRENT_SOURCE_DIR}/share/font.c        
	${CMAKE_CURRENT_SOURCE_DIR}/share/theme.c       
	${CMAKE_CURRENT_SOURCE_DIR}/share/base_config.c 
	${CMAKE_CURRENT_SOURCE_DIR}/share/config.c      
	${CMAKE_CURRENT_SOURCE_DIR}/share/video.c       
	${CMAKE_CURRENT_SOURCE_DIR}/share/glext.c       
	${CMAKE_CURRENT_SOURCE_DIR}/share/binary.c      
	${CMAKE_CURRENT_SOURCE_DIR}/share/state.c       
	${CMAKE_CURRENT_SOURCE_DIR}/share/audio.c       
	${CMAKE_CURRENT_SOURCE_DIR}/share/text.c        
	${CMAKE_CURRENT_SOURCE_DIR}/share/common.c      
	${CMAKE_CURRENT_SOURCE_DIR}/share/list.c        
	${CMAKE_CURRENT_SOURCE_DIR}/share/queue.c       
	${CMAKE_CURRENT_SOURCE_DIR}/share/cmd.c         
	${CMAKE_CURRENT_SOURCE_DIR}/share/array.c       
	${CMAKE_CURRENT_SOURCE_DIR}/share/dir.c         
	${CMAKE_CURRENT_SOURCE_DIR}/share/fbo.c         
	${CMAKE_CURRENT_SOURCE_DIR}/share/glsl.c        
	${CMAKE_CURRENT_SOURCE_DIR}/share/fs_common.c   
	${CMAKE_CURRENT_SOURCE_DIR}/share/fs_png.c      
	${CMAKE_CURRENT_SOURCE_DIR}/share/fs_jpg.c      
	${CMAKE_CURRENT_SOURCE_DIR}/share/fs_ov.c       
	${CMAKE_CURRENT_SOURCE_DIR}/share/log.c         
	${CMAKE_CURRENT_SOURCE_DIR}/share/joy.c         
	${CMAKE_CURRENT_SOURCE_DIR}/share/package.c     
	${CMAKE_CURRENT_SOURCE_DIR}/share/st_package.c  
	${CMAKE_CURRENT_SOURCE_DIR}/share/mapclib.c     
	${CMAKE_CURRENT_SOURCE_DIR}/ball/hud.c          
	${CMAKE_CURRENT_SOURCE_DIR}/ball/game_common.c  
	${CMAKE_CURRENT_SOURCE_DIR}/ball/game_client.c  
	${CMAKE_CURRENT_SOURCE_DIR}/ball/game_server.c  
	${CMAKE_CURRENT_SOURCE_DIR}/ball/game_proxy.c   
	${CMAKE_CURRENT_SOURCE_DIR}/ball/game_draw.c    
	${CMAKE_CURRENT_SOURCE_DIR}/ball/score.c        
	${CMAKE_CURRENT_SOURCE_DIR}/ball/level.c        
	${CMAKE_CURRENT_SOURCE_DIR}/ball/progress.c     
	${CMAKE_CURRENT_SOURCE_DIR}/ball/set.c          
	${CMAKE_CURRENT_SOURCE_DIR}/ball/demo.c         
	${CMAKE_CURRENT_SOURCE_DIR}/ball/demo_dir.c     
	${CMAKE_CURRENT_SOURCE_DIR}/ball/util.c         
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_conf.c      
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_demo.c      
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_save.c      
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_goal.c      
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_fail.c      
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_done.c      
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_level.c     
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_over.c      
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_play.c      
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_set.c       
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_start.c     
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_title.c     
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_help.c      
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_name.c      
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_shared.c    
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_pause.c     
	${CMAKE_CURRENT_SOURCE_DIR}/ball/st_ball.c      
	${CMAKE_CURRENT_SOURCE_DIR}/ball/main.c

    ${CMAKE_CURRENT_SOURCE_DIR}/share/solid_sim_sol.c
    ${CMAKE_CURRENT_SOURCE_DIR}/share/fs_stdio.c
    ${CMAKE_CURRENT_SOURCE_DIR}/share/zip.c
    ${CMAKE_CURRENT_SOURCE_DIR}/share/tilt_null.c
    ${CMAKE_CURRENT_SOURCE_DIR}/share/hmd_null.c
    ${CMAKE_CURRENT_SOURCE_DIR}/share/fetch_null.c

	${CMAKE_CURRENT_SOURCE_DIR}/share/dummy.cpp
)

target_sources(neverball
PRIVATE
    ${BALL_OBJS})

if(GAMEKID)
	set(TTF_LIB SDL2_ttf::SDL2_ttf-static)
else()
	set(TTF_LIB SDL2_ttf::SDL2_ttf)
endif()
target_link_libraries(neverball
PRIVATE
    SDL2::SDL2
    ${TTF_LIB}
    GL
    m
    ${PNG_LIBRARIES}
    ${JPEG_LIBRARIES}
    ${VORBISFILE_LIBRARIES}
    ${VORBIS_LIBRARIES}
    ${OGG_LIBRARIES}
	z
)

target_include_directories(neverball
PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/ball
    ${CMAKE_CURRENT_SOURCE_DIR}/share
    ${PNG_INCLUDE_DIRS}
    ${JPEG_INCLUDE_DIRS}
    ${VORBIS_INCLUDE_DIRS}
    ${VORBISFILE_LIBRARIES}
    ${OGG_INCLUDE_DIRS}
)

target_compile_definitions(neverball
PRIVATE
    ENABLE_FETCH=0
    ENABLE_NLS=0
    CONFIG_USER=".neverball"
    CONFIG_DATA="./data"
    CONFIG_LOCALE="./locale"
)

install(TARGETS neverball)
include(CPack)
