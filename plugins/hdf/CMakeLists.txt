cmake_minimum_required(VERSION 3.5)

if (STANDALONE)
    project(pdal-hdf)
    include("${CMAKE_SOURCE_DIR}/../../cmake/standalone.cmake" )
    include(${PDAL_CMAKE_DIR}/arbiter.cmake)
    include(${PDAL_CMAKE_DIR}/nlohmann.cmake)
    include(${PDAL_CMAKE_DIR}/libxml2.cmake)
    add_subdirectory("${CMAKE_SOURCE_DIR}/../../vendor/arbiter" "vendor/arbiter")
endif(STANDALONE)

#
# HDF plugin CMake configuration
#

include (${PDAL_CMAKE_DIR}/hdf5.cmake)


if (NOT PDAL_HAVE_HDF5)
    message(FATAL "HDF5 library is required to build HDF support.")
endif()

PDAL_ADD_PLUGIN(libname reader hdf
    FILES
        io/HdfReader.cpp
        io/Hdf5Handler.cpp
    LINK_WITH
        ${HDF5_LIBRARIES}
        ${PDAL_LIBRARIES}
    INCLUDES
        ${ROOT_DIR}
        ${NLOHMANN_INCLUDE_DIR}
)

if (WITH_TESTS)
    PDAL_ADD_TEST(pdal_io_hdf_reader_test
        FILES test/HdfReadertest.cpp
        LINK_WITH ${libname}
        INCLUDES
            ${NLOHMANN_INCLUDE_DIR})
endif()
