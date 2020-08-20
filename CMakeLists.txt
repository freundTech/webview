cmake_minimum_required(VERSION 2.6)

project(webview LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 17)

set(BUILD_WEBVIEW_EXAMPLES ON "Enables building the Webview example")

if (APPLE)
    find_library(WEBKIT_LIBRARY WebKit)

    set(LIBRARIES ${WEBKIT_LIBRARY})
elseif (WIN32)
    #TODO
else ()
    find_package(PkgConfig REQUIRED)
    pkg_check_modules(GTK3 REQUIRED gtk+-3.0)
    pkg_check_modules(WEBKIT2GTK REQUIRED webkit2gtk-4.0)

    set(LIBRARIES ${GTK3_LIBRARIES} ${WEBKIT2GTK_LIBRARIES})
    set(INCLUDE_DIRS ${GTK3_INCLUDE_DIRS} ${WEBKIT2GTK_INCLUDE_DIRS})
endif ()

if (BUILD_WEBVIEW_EXAMPLES)
    add_executable(webview-example main.cc webview.h)
    target_link_libraries(webview-example ${LIBRARIES})
    target_include_directories(webview-example PRIVATE ${INCLUDE_DIRS})
endif ()

add_library(webview++ STATIC webview.h)
set_target_properties(webview++ PROPERTIES LINKER_LANGUAGE CXX)
target_link_libraries(webview++ ${LIBRARIES})
target_include_directories(webview++ PUBLIC ${INCLUDE_DIRS} .)

add_library(webview STATIC webview.h)
set_target_properties(webview PROPERTIES LINKER_LANGUAGE C)
target_link_libraries(webview ${LIBRARIES})
target_include_directories(webview PUBLIC ${INCLUDE_DIRS} .)
