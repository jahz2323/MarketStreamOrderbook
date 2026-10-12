set(CMAKE_POLICY_VERSION_MINIMUM 3.5)
cmake_minimum_required(VERSION 3.20)
if(POLICY CMP0167)
    cmake_policy(SET CMP0167 NEW)
endif()
PROJECT(API_Connector LANGUAGES CXX C)

SET(CMAKE_CXX_STANDARD 20)
SET(CMAKE_CXX_STANDARD_REQUIRED ON)

SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# In your CMakeLists.txt for Debug builds
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g -O0 -fsanitize=address,undefined -fno-omit-frame-pointer")

# Library Groups
set(VENDOR_LIBS
    Boost::headers 
    Boost::json 
    OpenSSL::SSL 
    Threads::Threads
)

set(COMMON_SOURCES
    src/Handlers.cpp
    src/Connector.cpp
    src/StreamConfig.cpp
)

#setup boost library
find_package(OpenSSL REQUIRED)
find_package(Boost REQUIRED COMPONENTS headers json)
find_package(Boost REQUIRED COMPONENTS headers json thread chrono)
find_package(Threads REQUIRED)

add_library(API_Connector INTERFACE)

if (NOT TARGET lob_engine)
# add lob_engine folder 
    set(LOB_ENGINE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../lob_engine" CACHE PATH "Path to lob_engine directory")
    if(EXISTS "${LOB_ENGINE_DIR}")
        add_subdirectory(${LOB_ENGINE_DIR} "${CMAKE_CURRENT_BINARY_DIR}/lob_engine")
    else()
        message(FATAL_ERROR "Could not find lob_engine: ${LOB_ENGINE_DIR}")
    endif()
endif()


target_include_directories(API_Connector INTERFACE 
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_link_libraries(API_Connector INTERFACE 
    ${VENDOR_LIBS}
    ${Boost_LIBRARIES}
)
