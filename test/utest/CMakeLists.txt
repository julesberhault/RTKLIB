
if(WIN32)
    set(EXTRA_LIBS winmm)
    # On Windows with MSVC, math functions are built into the C runtime
    set(MATH_LIBS)
    # Try to find BLAS and LAPACK for Windows
    find_package(BLAS QUIET)
    find_package(LAPACK QUIET)
    if(BLAS_FOUND AND LAPACK_FOUND)
        set(LINALG_LIBS ${BLAS_LIBRARIES} ${LAPACK_LIBRARIES})
    else()
        # Use built-in matrix operations if BLAS/LAPACK not available
        message(WARNING "BLAS/LAPACK not found on Windows. Tests may use built-in matrix operations.")
        set(LINALG_LIBS)
    endif()
else()
    set(MATH_LIBS m)
    # Try to find BLAS and LAPACK on Unix/Linux
    find_package(BLAS QUIET)
    find_package(LAPACK QUIET)
    if(BLAS_FOUND AND LAPACK_FOUND)
        set(LINALG_LIBS ${BLAS_LIBRARIES} ${LAPACK_LIBRARIES})
    else()
        # Fall back to standard library names if find_package fails
        set(LINALG_LIBS lapack blas)
        message(WARNING "Using standard BLAS/LAPACK library names. Install liblapack-dev and libblas-dev if build fails.")
    endif()
endif()

set(RTKLBI_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../../src)

add_executable(t_matrix t_matrix.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/preceph.c)
target_link_libraries(t_matrix ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})

add_executable(t_time t_time.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/preceph.c)
target_link_libraries(t_time ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})

add_executable(t_coord t_coord.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/geoid.c ${RTKLBI_DIR}/preceph.c)
target_link_libraries(t_coord ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})

add_executable(t_rinex t_rinex.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/rinex.c ${RTKLBI_DIR}/preceph.c)
target_link_libraries(t_rinex ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})

add_executable(t_lambda t_lambda.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/lambda.c ${RTKLBI_DIR}/preceph.c)
target_link_libraries(t_lambda ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})

add_executable(t_atmos t_atmos.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/preceph.c)
target_link_libraries(t_atmos ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})

add_executable(t_misc t_misc.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/preceph.c)
target_link_libraries(t_misc ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})

add_executable(t_preceph t_preceph.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/preceph.c ${RTKLBI_DIR}/rinex.c ${RTKLBI_DIR}/ephemeris.c ${RTKLBI_DIR}/sbas.c)
target_link_libraries(t_preceph ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})

add_executable(t_gloeph t_gloeph.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/rinex.c ${RTKLBI_DIR}/ephemeris.c ${RTKLBI_DIR}/sbas.c ${RTKLBI_DIR}/preceph.c)
target_link_libraries(t_gloeph ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})

add_executable(t_geoid t_geoid.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/preceph.c ${RTKLBI_DIR}/geoid.c)
target_link_libraries(t_geoid ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})

add_executable(t_ppp t_ppp.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/ephemeris.c ${RTKLBI_DIR}/preceph.c ${RTKLBI_DIR}/sbas.c ${RTKLBI_DIR}/ionex.c ${RTKLBI_DIR}/pntpos.c ${RTKLBI_DIR}/ppp.c ${RTKLBI_DIR}/ppp_ar.c ${RTKLBI_DIR}/lambda.c ${RTKLBI_DIR}/tides.c)
target_link_libraries(t_ppp ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})

add_executable(t_ionex t_ionex.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/preceph.c ${RTKLBI_DIR}/ionex.c)
target_link_libraries(t_ionex ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})

add_executable(t_tle t_tle.c ${RTKLBI_DIR}/rtkcmn.c ${RTKLBI_DIR}/trace.c ${RTKLBI_DIR}/rinex.c ${RTKLBI_DIR}/ephemeris.c ${RTKLBI_DIR}/sbas.c ${RTKLBI_DIR}/preceph.c ${RTKLBI_DIR}/tle.c)
target_link_libraries(t_tle ${MATH_LIBS} ${LINALG_LIBS} ${EXTRA_LIBS})


add_test(NAME matrix_test COMMAND t_matrix WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME time_test COMMAND t_time WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME coord_test COMMAND t_coord WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME rinex_test COMMAND t_rinex WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME lambda_test COMMAND t_lambda WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME atmos_test COMMAND t_atmos WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME misc_test COMMAND t_misc WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME preceph_test COMMAND t_preceph WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME gloeph_test COMMAND t_gloeph WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME geoid_test COMMAND t_geoid WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME ppp_test COMMAND t_ppp WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME ionex_test COMMAND t_ionex WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME tlr_test COMMAND t_tle WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
