cmake_minimum_required(VERSION 2.8)
project(fiberserver)

set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -std=c++11 -ftemplate-depth=256")
if(APPLE)
    set(CMAKE_LINK_FLAGS "${CMAKE_LINK_FLAGS} -stdlib=libc++")
endif(APPLE)

find_package(Boost 1.55.0 COMPONENTS chrono atomic system thread coroutine context REQUIRED)

INCLUDE_DIRECTORIES(
    ${Boost_INCLUDE_DIRS}
    boost-fiber/include
)

file(GLOB BOOST_FIBER_SOURCES
    "boost-fiber/src/*.cpp"
    "boost-fiber/src/asio/*.cpp"
    "boost-fiber/src/detail/*.cpp"
)

add_library(boost_fiber STATIC ${BOOST_FIBER_SOURCES})

set(SOURCES
    main.cpp
)
add_executable(fiberserver ${SOURCES})
TARGET_LINK_LIBRARIES(fiberserver boost_fiber ${Boost_LIBRARIES} pthread)
