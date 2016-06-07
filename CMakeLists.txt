project(BitRiot)
cmake_minimum_required(VERSION 3.2)

set(CMAKE_BUILD_TYPE Release)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=gnu++0x -Wno-comment -Werror")

set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -ggdb -O0")

set(CMAKE_CSS_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O2")


if(CMAKE_BUILD_TYPE MATCHES "Release")
	SET (CMAKE_SHARED_LINKER_FLAGS ${CMAKE_SHARED_LINKER_FLAGS_INIT} -s) 
endif()

find_package(SDL REQUIRED)
find_package(SDL_mixer REQUIRED)
find_package(SDL_ttf REQUIRED)


if(NOT SDL_FOUND)
	message(SEND_ERROR "Failed to find SDL.")
endif()

if(NOT SDL_MIXER_FOUND)
	message(SEND_ERROR "Failed to find SDL_mixer.")
endif()

if(NOT SDL_TTF_FOUND)
	message(SEND_ERROR "Failed to find SDL_ttf.")
endif()


file(GLOB SOURCES_BASE src/*.cpp)
file(GLOB SOURCES_ENTITY src/entity/*.cpp)

set(SOURCES ${SOURCES_BASE} ${SOURCES_ENTITY})

include_directories(${SDL_INCLUDE_DIR} ${SDL_TTF_INCLUDE_DIRS} ${SDL_MIXER_INCLUDE_DIRS} ${PROJECT_SOURCE_DIR} ${PROJECT_SOURCE_DIR}/src ${PROJECT_SOURCE_DIR}/src/entity )
add_executable(BitRiot ${SOURCES})



target_link_libraries(BitRiot ${SDL_LIBRARY} ${SDL_MIXER_LIBRARIES} ${SDL_TTF_LIBRARIES})

add_custom_command(
    TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ln -sf ${PROJECT_SOURCE_DIR}/data ${PROJECT_BINARY_DIR}/
    COMMAND ln -sf ${PROJECT_SOURCE_DIR}/bitmaps ${PROJECT_BINARY_DIR}/
    COMMAND ln -sf ${PROJECT_SOURCE_DIR}/audio ${PROJECT_BINARY_DIR}/
)

