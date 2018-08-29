cmake_minimum_required(VERSION 3.1)   #uebernommen aus megamol Projekt
#wir brauchen CMake 3.1 wegen den Compiler settings
project(asio) #mit version  

#SET(CMAKE_CXX_FLAGS "-std=c++0x")
set (CMAKE_CXX_STANDARD 11)

#set(${ASIO_STANDALONE} "ASIO_STANDALONE")

#add_compile_definitions(OPENCV_VERSION=${OpenCV_VERSION} -DASIO_STANDALONE) #preprocessor settings setzten

#add_compile_options(-D <ASIO_STANDALONE>)	

ADD_DEFINITIONS(-DASIO_STANDALONE )

ADD_DEFINITIONS(-DASIO_HAS_STD_ADDRESSOF)
ADD_DEFINITIONS(-DASIO_HAS_STD_ARRAY)
ADD_DEFINITIONS(-DASIO_HAS_CSTDINT)
ADD_DEFINITIONS(-DASIO_HAS_STD_SHARED_PTR)
ADD_DEFINITIONS(-DASIO_HAS_STD_TYPE_TRAITS)
ADD_DEFINITIONS(-DASIO_HAS_VARIADIC_TEMPLATES)
ADD_DEFINITIONS(-DASIO_HAS_STD_FUNCTION)
ADD_DEFINITIONS(-DASIO_HAS_STD_CHRONO)
ADD_DEFINITIONS(-DBOOST_ALL_NO_LIB)
ADD_DEFINITIONS(-DASIO_SEPARATE_COMPILATION)
ADD_DEFINITIONS(-D_WIN32_WINNT=0x0501) #
ADD_DEFINITIONS(-D_WINSOCK_DEPRECATED_NO_WARNINGS) #

if (CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set(CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR} CACHE PATH "default install path" FORCE)
endif()

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)  #laut https://stackoverflow.com/questions/19981534/cmake-include-directories schliest cmake unterordner mit ein

#<cmake> --build . [--config <config>] [--target <target>] [-- -i]	

set (${PROJECT_NAME}_headers "${CMAKE_CURRENT_SOURCE_DIR}/include/asio.hpp")
set (${PROJECT_NAME}_sources "${CMAKE_CURRENT_SOURCE_DIR}/src/asio.cpp")
#Zeile 102 bis 267 ignorieren, weil ausschlieslich Testfiles generiert weden

#include directories nicht sicher wo genau 

add_library(${PROJECT_NAME} SHARED ${${PROJECT_NAME}_headers}
									 ${${PROJECT_NAME}_sources})								 
#linker
#target_link_libraries(${PROJECT_NAME} asio)

install(TARGETS ${PROJECT_NAME}
		RUNTIME DESTINATION bin
        ARCHIVE DESTINATION lib)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION "include")

if(MSVC)  #nach snappy cmake file
	set_target_properties(asio PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS ON)
	set(ASIO_LIB "${CMAKE_CURRENT_SOURCE_DIR}/lib/asio.lib" CACHE INTERNAL "")
	set(ASIO_DLLDIR "${CMAKE_CURRENT_SOURCE_DIR}/bin")	
else()
	include(GNUInstallDirs)
	set(ASIO_LIB "${CMAKE_CURRENT_SOURCE_DIR}/libasio.so") #
endif()		

#mit anfuehrungszeichen hinten?  nach https://cmake.org/cmake/help/v3.0/command/add_library.html

