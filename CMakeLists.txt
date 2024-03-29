cmake_minimum_required(VERSION 3.12)
project(OpenCTMPlugin VERSION 0.1)

find_package(SofaBase REQUIRED)
#find_package(OpenCTM REQUIRED)

set(SOFA_OPENCTM_SRC_DIR src/OpenCTMPlugin)

set(HEADER_FILES
    ${SOFA_OPENCTM_SRC_DIR}/config.h.in
    ${SOFA_OPENCTM_SRC_DIR}/OpenCTMExporter.h
    ${SOFA_OPENCTM_SRC_DIR}/OpenCTMLoader.h
)

set(SOURCE_FILES
    ${SOFA_OPENCTM_SRC_DIR}/OpenCTMExporter.cpp
    ${SOFA_OPENCTM_SRC_DIR}/OpenCTMLoader.cpp
    ${SOFA_OPENCTM_SRC_DIR}/initOpenCTMPlugin.cpp
)

set(README_FILES README.md)

add_library(${PROJECT_NAME} SHARED ${HEADER_FILES} ${SOURCE_FILES} ${README_FILES})

# Set define dllimport/dllexport mechanism on Windows.
target_compile_definitions(${PROJECT_NAME} PRIVATE "-DSOFA_BUILD_SOFAOPENCTM")

target_link_libraries(${PROJECT_NAME} SofaBaseTopology SofaOpenglVisual ${OPENCTM_LIBRARY})

#target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${OPENCTM_INCLUDE_PATH}>")
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>")
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/include>")
target_include_directories(${PROJECT_NAME} PUBLIC "$<INSTALL_INTERFACE:include>")


## Install rules for the library; CMake package configurations files
sofa_create_package_with_targets(
    PACKAGE_NAME ${PROJECT_NAME}
    PACKAGE_VERSION ${PROJECT_VERSION}
    TARGETS ${PROJECT_NAME} AUTO_SET_TARGET_PROPERTIES
    INCLUDE_SOURCE_DIR "src"
    INCLUDE_INSTALL_DIR ${PROJECT_NAME}
    RELOCATABLE "plugins"
    )
