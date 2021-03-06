#/*============================================================================
#
#  CMAKECATCHMPI: A software package to demo MPI with CMake, CTest and Catch.
#
#  Copyright (c) University College London (UCL). All rights reserved.
#
#  This software is distributed WITHOUT ANY WARRANTY; without even
#  the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR
#  PURPOSE.
#
#  See LICENSE.txt in the top level directory for details.
#
#============================================================================*/

set(DATA_DIR ${CMAKE_SOURCE_DIR}/Testing/Data)
set(TMP_DIR ${CMAKE_BINARY_DIR}/Testing/Temporary)

set(TEST_CASES
  ccmpiMyFunctionsTest
  ccmpiPiSeriesOMPTest
  ccmpiPiSeriesMPITest
  ccmpiTriangleNumberSeriesMPITest
)

foreach(_test_case ${TEST_CASES})
  add_executable(${_test_case} ${_test_case}.cpp ccmpiCatchMain.cpp)
  target_link_libraries(${_test_case} ${CMAKECATCHMPI_LIBRARIES} ${ALL_THIRD_PARTY_LIBRARIES})
endforeach()

# Adding tests is done outside the above loop,
# as each test will need different arguments.

##################################################################################################
# Dont forget its:  add_test(<test name (globally unique) > <exe name> <argument1> <argument2>
##################################################################################################

add_test(MyFunc ${EXECUTABLE_OUTPUT_PATH}/ccmpiMyFunctionsTest)
add_test(PiOMP ${EXECUTABLE_OUTPUT_PATH}/ccmpiPiSeriesOMPTest)
add_test(PiMPI ${MPIEXEC} ${MPIEXEC_NUMPROC_FLAG} ${MPIEXEC_MAX_NUMPROCS} ${EXECUTABLE_OUTPUT_PATH}/ccmpiPiSeriesMPITest)
add_test(TriMPI ${MPIEXEC} ${MPIEXEC_NUMPROC_FLAG} ${MPIEXEC_MAX_NUMPROCS} ${EXECUTABLE_OUTPUT_PATH}/ccmpiTriangleNumberSeriesMPITest)
