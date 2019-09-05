cmake_minimum_required(VERSION 3.11.0 FATAL_ERROR)

project(lua)

set(LUA_VERSION 5.4.0)

set(INSTALL_LUA_LIB_DIR "${CMAKE_INSTALL_PREFIX}/lib" CACHE PATH "Installation directory for libraries")
set(INSTALL_LUA_INCLUDE_DIR "${CMAKE_INSTALL_PREFIX}/include" CACHE PATH "Installation directory for headers")

set( LUA_LIBRARY_TYPE SHARED )

if( ${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    if(IOS)
        set( LUA_LIBRARY_TYPE STATIC )
    else()  
        add_definitions( -DLUA_USE_MACOSX )
        add_definitions( -DLUA_USE_READLINE )
    endif(IOS)
elseif (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    add_definitions( -DLUA_USE_LINUX )
elseif (${CMAKE_SYSTEM_NAME} MATCHES "Android")
    set( LUA_LIBRARY_TYPE STATIC )
elseif (${CMAKE_SYSTEM_NAME} MATCHES "Windows")  
    add_definitions( -D_CRT_SECURE_NO_WARNINGS )
    add_definitions( -DLUA_BUILD_AS_DLL )
else()
    message( FATAL_ERROR  "unknown system" )
endif()



include_directories(src)

aux_source_directory(src LUA_SOURCES)
list(REMOVE_ITEM LUA_SOURCES "src/lua.c" "src/luac.c")

set(LUA_LIBRARY lua-${LUA_VERSION})


add_library(${LUA_LIBRARY} ${LUA_LIBRARY_TYPE} ${LUA_SOURCES})


set(LUA_PUBLIC_H
    src/lua.h
    src/luaconf.h
    src/lualib.h
    src/lauxlib.h 
    src/lua.hpp
)

install(FILES ${LUA_PUBLIC_H} DESTINATION "${INSTALL_LUA_INCLUDE_DIR}")
install(TARGETS ${LUA_LIBRARY} LIBRARY DESTINATION "${INSTALL_LUA_LIB_DIR}")
