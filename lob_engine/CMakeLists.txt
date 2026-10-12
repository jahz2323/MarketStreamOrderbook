set(CMAKE_POLICY_VERSION_MINIMUM 3.5)
cmake_minimum_required(VERSION 3.20)
cmake_policy(SET CMP0079 NEW)
if(POLICY CMP0167)
    cmake_policy(SET CMP0167 NEW)
endif()


PROJECT(lob_engine 
    LANGUAGES CXX C 
    DESCRIPTION "A simple LOB engine"
    VERSION 1.0.0
)

SET(CMAKE_CXX_STANDARD 20)
SET(CMAKE_CXX_STANDARD_REQUIRED ON)

SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

#setup boost library
find_package(OpenSSL REQUIRED)
find_package(Boost REQUIRED COMPONENTS headers json)

#kdb+ 
find_package(Threads REQUIRED) # required for kdb+ c.o 

#setup pybind11 library
find_package(pybind11 CONFIG REQUIRED)

#Find API_Connector project
# add lob_engine folder 
if (NOT TARGET API_Connector)
    set(NET_ORCHESTRATE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../NetOrchestrate" CACHE PATH "Path to lob_engine directory")
    if(EXISTS "${NET_ORCHESTRATE_DIR}")
        add_subdirectory(${NET_ORCHESTRATE_DIR} "${CMAKE_CURRENT_BINARY_DIR}/NetOrchestrate")
    else()
        message(FATAL_ERROR "Could not find NET_ORCHESTRATE_DIR: ${NET_ORCHESTRATE_DIR}")
    endif()
endif()

if(NOT TARGET lob_engine)
    add_library(lob_engine INTERFACE)
endif()


target_include_directories(lob_engine INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/include)
#add external includes 
target_include_directories(lob_engine INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/external/include)
target_link_libraries(lob_engine INTERFACE ${Boost_LIBRARIES})
target_link_libraries(lob_engine INTERFACE API_Connector Boost::headers) 
