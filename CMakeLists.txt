cmake_minimum_required(VERSION 3.8)
project(pproto)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -Wno-dev)
endif()

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

set(target pproto)

add_compile_definitions(APPLICATION_NAME="ROS Demo")
add_compile_definitions(VERSION_PROJECT="0")
add_compile_definitions(VERSION_PROJECT_MAJOR=1)
add_compile_definitions(VERSION_PROJECT_MINOR=0)
add_compile_definitions(VERSION_PROJECT_PATCH=0)
add_compile_definitions(QDATASTREAM_VERSION=QDataStream::Qt_5_12)
add_compile_definitions(PPROTO_VERSION_LOW=0)
add_compile_definitions(PPROTO_VERSION_HIGH=0)
add_compile_definitions(PPROTO_JSON_SERIALIZE)
add_compile_definitions(PPROTO_QBINARY_SERIALIZE)
add_compile_definitions(PPROTO_UDP_SIGNATURE="RSDO")
add_compile_definitions(DEFAULT_PORT=54000)
add_compile_definitions(QT_CORE_LIB)
#add_definitions(QT_CORE_LIB)

find_package(ament_cmake REQUIRED)
find_package(Qt5 COMPONENTS Core Gui Widgets Network REQUIRED)
find_package(shared REQUIRED)

set(CMAKE_AUTOMOC ON)
set(AM_MOC_COMPILE_DEFINITIONS ";QT4;QT4;QT_NO_DEBUG;QT_GUI_LIB;QT_CORE_LIB")

include_directories(
    "${CMAKE_CURRENT_SOURCE_DIR}"
    pproto
    "${shared_INCLUDE_DIRS}"
)

add_library(pproto
    pproto/commands/base.cpp
    pproto/commands/base.h
    pproto/commands/paging.cpp
    pproto/commands/paging.h
    pproto/commands/pool.cpp
    pproto/commands/pool.h
    pproto/commands/time_range.cpp
    pproto/commands/time_range.h
    pproto/serialize/byte_array.cpp
    pproto/serialize/byte_array.h
    pproto/serialize/functions.cpp
    pproto/serialize/functions.h
    pproto/serialize/json.cpp
    pproto/serialize/json.h
    pproto/serialize/qbinary.h
    pproto/serialize/result.cpp
    pproto/serialize/result.h
    pproto/transport/base.cpp
    pproto/transport/base.h
    pproto/transport/local.cpp
    pproto/transport/local.h
    pproto/transport/tcp.cpp
    pproto/transport/tcp.h
    pproto/transport/udp.cpp
    pproto/transport/udp.h
    pproto/bserialize_space.h
    pproto/error_sender.h
    pproto/func_invoker.h
    pproto/host_point.cpp
    pproto/host_point.h
    pproto/logger_operators.cpp
    pproto/logger_operators.h
    pproto/message.cpp
    pproto/message.h
    pproto/utils.cpp
    pproto/utils.h
)

target_include_directories(pproto PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

ament_export_targets(pproto-targets HAS_LIBRARY_TARGET)

ament_target_dependencies(pproto
#    rclcpp
    shared
)

ament_export_libraries(pproto)
ament_export_include_directories(pproto)

#ament_export_dependencies(
#    rclcpp
#    shared
#)


target_link_libraries(pproto Qt5::Core Qt5::Gui Qt5::Widgets Qt5::Network)

target_compile_features(pproto PUBLIC cxx_std_17)

install(
  DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
  DESTINATION include
)

install(
  TARGETS pproto
  EXPORT pproto-targets
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include/pproto
                       include/pproto/pproto
)

ament_package()
