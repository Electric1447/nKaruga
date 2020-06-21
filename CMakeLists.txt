cmake_minimum_required(VERSION 2.8)

if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
  if(DEFINED ENV{VITASDK})
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VITASDK}/share/vita.toolchain.cmake" CACHE PATH "toolchain file")
  else()
    message(FATAL_ERROR "Please define VITASDK to point to your SDK path!")
  endif()
endif()

project(nkaruga)
include("${VITASDK}/share/vita.cmake" REQUIRED)

set(VITA_APP_NAME "nKaruga")
set(VITA_TITLEID  "NKARUGA00")
set(VITA_VERSION  "01.00")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wl, -std=gnu11")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wl,-q -O3 -std=gnu++14 -fpermissive -fexceptions")

file(GLOB src_files
    "src/*.cpp"
	"src/*.c"
)

include_directories(
  ../common
)

add_executable(${PROJECT_NAME}
  ${src_files}
)

target_link_libraries(${PROJECT_NAME}
	SDL2_mixer
	SDL2
	vita2d
	SceDisplay_stub
	SceCtrl_stub
	SceAudio_stub
	SceSysmodule_stub
	SceGxm_stub
	SceCommonDialog_stub
	ScePower_stub
	SceTouch_stub
	SceHid_stub
  	m
	z
	c
	mpg123
	FLAC
	mikmod
	vorbisfile
	vorbis
	ogg
)

vita_create_self(${PROJECT_NAME}.self ${PROJECT_NAME})
vita_create_vpk(${PROJECT_NAME}.vpk ${VITA_TITLEID} ${PROJECT_NAME}.self
	VERSION ${VITA_VERSION}
	NAME ${VITA_APP_NAME}
	
	FILE sce_sys/icon0.png sce_sys/icon0.png
	FILE sce_sys/livearea/contents/bg.png sce_sys/livearea/contents/bg.png
	FILE sce_sys/livearea/contents/startup.png sce_sys/livearea/contents/startup.png
	FILE sce_sys/livearea/contents/template.xml sce_sys/livearea/contents/template.xml

	FILE sfx/bullet/fire_enemy_0.wav resources/sfx/bullet/fire_enemy_0.wav
	FILE sfx/bullet/fire_enemy_1.wav resources/sfx/bullet/fire_enemy_1.wav
	FILE sfx/bullet/fire_enemy_2.wav resources/sfx/bullet/fire_enemy_2.wav
	FILE sfx/bullet/fire_enemy_homing.wav resources/sfx/bullet/fire_enemy_homing.wav
	FILE sfx/bullet/fire_player_bullet.wav resources/sfx/bullet/fire_player_bullet.wav
	FILE sfx/bullet/fire_fragment.wav resources/sfx/bullet/fire_fragment.wav
	FILE sfx/bullet/impact.wav resources/sfx/bullet/impact.wav
	FILE sfx/enemy/explosion_big.wav resources/sfx/enemy/explosion_big.wav
	FILE sfx/enemy/explosion_small.wav resources/sfx/enemy/explosion_small.wav
	FILE sfx/enemy/laser_charge.wav resources/sfx/enemy/laser_charge.wav
	FILE sfx/enemy/laser_release.wav resources/sfx/enemy/laser_release.wav
	FILE sfx/player/chain.wav resources/sfx/player/chain.wav
	FILE sfx/player/death.wav resources/sfx/player/death.wav
	FILE sfx/player/switch.wav resources/sfx/player/switch.wav
	
	FILE sfx/menu/accept.wav resources/sfx/menu/accept.wav
	FILE sfx/menu/back.wav resources/sfx/menu/back.wav
	FILE sfx/menu/cursor.wav resources/sfx/menu/cursor.wav
	FILE sfx/menu/select.wav resources/sfx/menu/select.wav
	FILE sfx/menu/start.wav resources/sfx/menu/start.wav
	FILE sfx/boss_alert.wav resources/sfx/boss_alert.wav
	
	FILE sfx/music/Ideal_main.ogg resources/sfx/music/Ideal_main.ogg
	FILE sfx/music/Ideal_loop.ogg resources/sfx/music/Ideal_loop.ogg
	FILE sfx/music/Butsutekkai.ogg resources/sfx/music/Butsutekkai.ogg
	FILE sfx/music/Trial_main.ogg resources/sfx/music/Trial_main.ogg
	FILE sfx/music/Trial_loop.ogg resources/sfx/music/Trial_loop.ogg
)