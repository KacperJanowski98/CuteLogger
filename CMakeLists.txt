CMAKE_MINIMUM_REQUIRED(VERSION 3.1)

SET(CMAKE_CXX_STANDARD 11)
SET(CMAKE_CXX_STANDARD_REQUIRED ON)

PROJECT(CuteLogger)

FIND_PACKAGE(Qt6Core REQUIRED)
find_package(Qt6 REQUIRED COMPONENTS Core5Compat)

ADD_DEFINITIONS(-DCUTELOGGER_LIBRARY)

INCLUDE_DIRECTORIES(BEFORE include)

SET(sources
  ${CMAKE_CURRENT_LIST_DIR}/src/Logger.cpp
  ${CMAKE_CURRENT_LIST_DIR}/src/AbstractAppender.cpp
  ${CMAKE_CURRENT_LIST_DIR}/src/AbstractStringAppender.cpp
  ${CMAKE_CURRENT_LIST_DIR}/src/ConsoleAppender.cpp
  ${CMAKE_CURRENT_LIST_DIR}/src/FileAppender.cpp
  ${CMAKE_CURRENT_LIST_DIR}/src/RollingFileAppender.cpp
)

SET(includes
  # ${CMAKE_CURRENT_LIST_DIR}/include
  ${CMAKE_CURRENT_LIST_DIR}/include/Logger.h
  ${CMAKE_CURRENT_LIST_DIR}/include/FileAppender.h
  ${CMAKE_CURRENT_LIST_DIR}/include/CuteLogger_global.h
  ${CMAKE_CURRENT_LIST_DIR}/include/ConsoleAppender.h
  ${CMAKE_CURRENT_LIST_DIR}/include/AbstractStringAppender.h
  ${CMAKE_CURRENT_LIST_DIR}/include/AbstractAppender.h
  ${CMAKE_CURRENT_LIST_DIR}/include/RollingFileAppender.h
)

# OutputDebugAppender is only for Windows systems
IF(WIN32)
  SET(sources ${sources} ${CMAKE_CURRENT_LIST_DIR}/src/OutputDebugAppender.cpp)
  SET(includes ${includes} ${CMAKE_CURRENT_LIST_DIR}/include/OutputDebugAppender.h)
ENDIF(WIN32)

SET(library_target CuteLogger)

add_library(${library_target} STATIC ${sources} ${includes})

target_link_libraries(${library_target}
  PUBLIC
    Qt6::Core
    Qt6::Core5Compat
)

target_include_directories(${library_target}
  PUBLIC
    ${CMAKE_CURRENT_LIST_DIR}/include
)

target_sources(${library_target}
  PUBLIC
    # ${includes}
    ${CMAKE_CURRENT_LIST_DIR}/include
  PRIVATE
    ${sources}
)

# ADD_LIBRARY(${library_target} SHARED ${sources} ${includes})
# TARGET_LINK_LIBRARIES(${library_target} 
#     Qt6::Core
#     Qt6::Core5Compat
# )
# TARGET_INCLUDE_DIRECTORIES(${library_target} PUBLIC include)

# INSTALL(TARGETS ${library_target} DESTINATION ${CMAKE_SOURCE_DIR}/build/lib/Logger)

# SET(ENABLE_TESTS OFF CACHE BOOL "Enable building CuteLogger tests")
# IF (ENABLE_TESTS)
#   SET(CMAKE_AUTOMOC ON)
#   FIND_PACKAGE(Qt6Test REQUIRED)

#   ADD_EXECUTABLE(basictest test/basictest.cpp)
#   TARGET_LINK_LIBRARIES(basictest Qt6::Core Qt6::Test CuteLogger)
# ENDIF ()
