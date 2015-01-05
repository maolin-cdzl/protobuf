CMAKE_MINIMUM_REQUIRED(VERSION 2.8.12)

include(ExternalProject)

set(CMAKE_C_FLAGS "-std=c99 ${CMAKE_C_FLAGS}")
set(CMAKE_CXX_FLAGS "-std=c++11 ${CMAKE_CXX_FLAGS}")

add_custom_command(
	OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/configure
	COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/autogen.sh
	WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
	)

add_custom_target(
	pb-configure
	DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/configure
	)

ExternalProject_Add(protobuf
	DEPENDS pb-configure
	SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}
	CONFIGURE_COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/configure --prefix=/usr/local
	BINARY_DIR ${CMAKE_CURRENT_BINARY_DIR}/cmake-build
	BUILD_COMMAND make
	INSTALL_COMMAND sudo make install
	)


