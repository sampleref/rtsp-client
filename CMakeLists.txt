cmake_minimum_required(VERSION 3.8)
project(GstRtspClient)
set(CMAKE_CXX_STANDARD 11)

find_package(PkgConfig REQUIRED)

pkg_check_modules(GSTLIBS REQUIRED
        gobject-2.0
        glib-2.0
        gstreamer-webrtc-1.0
        gstreamer-sdp-1.0
        gstreamer-pbutils-1.0
        libsoup-2.4
        json-glib-1.0
        gstreamer-rtsp-server-1.0)

include_directories(/usr/lib/x86_64-linux-gnu/glib-2.0/include
        /usr/include/glib-2.0
        /usr/local/include/gstreamer-1.0
        /usr/local/include/libsoup-2.4
        /usr/local/include/json-glib-1.0)

file(GLOB SOURCE_FILES_RTSP_CLIENT src/client/*.cpp src/client/*.hpp src/common/*.hpp)
file(GLOB SOURCE_FILES_RTSP_SERVER src/server/*.cpp src/server/*.hpp src/common/*.hpp)

file(COPY src/common/rtsp_parameters.conf DESTINATION ${CMAKE_CURRENT_BINARY_DIR})

link_directories(${GSTLIBS_LIBRARY_DIRS})

add_executable(rtsp_client ${SOURCE_FILES_RTSP_CLIENT})
add_executable(rtsp_server ${SOURCE_FILES_RTSP_SERVER})

target_link_libraries(rtsp_client ${GSTLIBS_LIBRARIES})
target_link_libraries(rtsp_server ${GSTLIBS_LIBRARIES})