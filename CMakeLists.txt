cmake_minimum_required(VERSION 3.11.0 FATAL_ERROR)

project(lua)

set(LUA_VERSION 5.4.0)

set(INSTALL_LUA_LIB_DIR "${CMAKE_INSTALL_PREFIX}/lib" CACHE PATH "Installation directory for libraries")
set(INSTALL_LUA_INCLUDE_DIR "${CMAKE_INSTALL_PREFIX}/include" CACHE PATH "Installation directory for headers")

include_directories(src)

aux_source_directory(src LUA_SOURCES)
list(REMOVE_ITEM LUA_SOURCES "src/lua.c" "src/luac.c")

set(LUA_LIBRARY lua-${LUA_VERSION})

add_library(${LUA_LIBRARY} STATIC ${LUA_SOURCES})

set(LUA_PUBLIC_H
    src/zconf.h
    src/lua.h
    src/luaconf.h
    src/lualib.h
    src/lua.hpp
)

install(FILES ${LUA_PUBLIC_H} DESTINATION "${INSTALL_LUA_INCLUDE_DIR}")
install(TARGETS ${LUA_LIBRARY} DESTINATION "${INSTALL_LUA_LIB_DIR}")
#install (FILES ${CMAKE_BINARY_DIR}/src/lauxlib.h DESTINATION ${TWTWO_THIRD_PARTY}/include/lua)
#install (FILES ${CMAKE_BINARY_DIR}/src/lua.h DESTINATION ${TWTWO_THIRD_PARTY}/include/lua)
#install (FILES ${CMAKE_BINARY_DIR}/src/luaconf.h DESTINATION ${TWTWO_THIRD_PARTY}/include/lua)
#install (FILES ${CMAKE_BINARY_DIR}/src/lualib.h DESTINATION ${TWTWO_THIRD_PARTY}/include/lua)