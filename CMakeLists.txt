cmake_minimum_required(VERSION 3.11.0 FATAL_ERROR)

project(lua)

set(LUA_VERSION 5.4.0)

set(INSTALL_LUA_LIB_DIR "${CMAKE_INSTALL_PREFIX}/lib" CACHE PATH "Installation directory for libraries")
set(INSTALL_LUA_INCLUDE_DIR "${CMAKE_INSTALL_PREFIX}/include" CACHE PATH "Installation directory for headers")

if(WIN32)
    add_definitions( -D_CRT_SECURE_NO_WARNINGS )
    add_definitions( -DLUA_BUILD_AS_DLL )
endif()

include_directories(src)

aux_source_directory(src LUA_SOURCES)
list(REMOVE_ITEM LUA_SOURCES "src/lua.c" "src/luac.c")

set(LUA_LIBRARY lua-${LUA_VERSION})

set( LUA_LIBRARY_TYPE STATIC )

if(WIN32)
    set( LUA_LIBRARY_TYPE SHARED )
endif()

add_library(${LUA_LIBRARY} ${LUA_LIBRARY_TYPE} ${LUA_SOURCES})


set(LUA_PUBLIC_H
    src/lua.h
    src/luaconf.h
    src/lualib.h
    src/lauxlib.h 
    src/lua.hpp
)

install(FILES ${LUA_PUBLIC_H} DESTINATION "${INSTALL_LUA_INCLUDE_DIR}")
install(TARGETS ${LUA_LIBRARY} DESTINATION "${INSTALL_LUA_LIB_DIR}")
