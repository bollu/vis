cmake_minimum_required(VERSION 3.17)
project(vis C CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g -fdiagnostics-color=always -fsanitize=address,undefined")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fdiagnostics-color=always -O2")

find_package(Lua REQUIRED)

find_package(PkgConfig)
pkg_check_modules(LIBTERMKEY REQUIRED IMPORTED_TARGET termkey)
pkg_check_modules(LIBCURSES REQUIRED IMPORTED_TARGET ncurses)


# add_compile_definitions(VERSION=$(shell git describe --always --dirty 2>/dev/null || echo "v0.7-git"))
add_compile_definitions(VERSION="2")
add_compile_definitions(CONFIG_CURSES=1)
add_compile_definitions(CONFIG_LUA=0)
add_compile_definitions(CONFIG_LPEG=0)
add_compile_definitions(CONFIG_TRE=0)
add_compile_definitions(CONFIG_ACL=0)
add_compile_definitions(CONFIG_SELINUX=0)
add_compile_definitions(HAVE_MEMRCHR=1)

add_executable(vis
array.c
buffer.c
libutf.c
main.c
map.c
sam.c
text.c
text-common.c
text-io.c
text-iterator.c
text-motions.c
text-objects.c
text-regex.c
# text-regex-tre.c
text-util.c
ui-terminal.c
# ui-terminal-curses.c
# ui-terminal-vt100.c
view.c
vis.c
# vis-cmds.c
# vis-digraph.c
vis-lua.c
vis-marks.c
# vis-menu.c
vis-modes.c
vis-motions.c
vis-operators.c
vis-prompt.c
vis-registers.c
# vis-single.c
vis-text-objects.c)



target_include_directories(vis PRIVATE ${CMAKE_CURRENT_SOURCE_DIR} ${LUA_INCLUDE_DIR} ${LIBTERMKEY_INCLUDE_DIRS})
target_link_libraries(vis PkgConfig::LIBTERMKEY PkgConfig::LIBCURSES)
install(TARGETS vis DESTINATION bin)

