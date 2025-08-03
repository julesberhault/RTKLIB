include_directories(.)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# DLL Configuration - can be controlled via -DDLL=OFF in cmake command
option(DLL "Build as DLL/shared library" ON)

if(WIN32)
    if(DLL)
        add_definitions(-DWIN_DLL)
    endif()
else()
    if(DLL)
        add_definitions(-DDLL)
    endif()
endif()

if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 -Wall -O3 -ansi -pedantic")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-unused-but-set-variable -Wno-format-overflow -Wno-unused-result -Wno-int-to-pointer-cast -Wno-pointer-to-int-cast")
endif()

if ("${CMAKE_BUILD_TYPE}" STREQUAL "Release")
    if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
        set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS} -O3 -fsee -fomit-frame-pointer -fno-signed-zeros -fno-math-errno -funroll-loops")
    endif()
endif()

if(MSVC)
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif()

aux_source_directory(. DIR_SRCS_RTKLIB)
set(DIR_SRCS_RTKLIB_RCV
  ${DIR_SRCS_RTKLIB_RCV}
  rcv/binex.c
  rcv/crescent.c
  rcv/javad.c
  rcv/novatel.c
  rcv/nvs.c
  rcv/rt17.c
  rcv/septentrio.c
  rcv/skytraq.c
  rcv/swiftnav.c
  rcv/ublox.c
  rcv/unicore.c
)
list(APPEND DIR_SRCS ${DIR_SRCS_RTKLIB} ${DIR_SRCS_RTKLIB_RCV})

# Build library as shared or static based on DLL option
if(DLL)
    add_library(rtklib SHARED ${DIR_SRCS} rtklib.h)
else()
    add_library(rtklib STATIC ${DIR_SRCS} rtklib.h)
endif()
set_property(TARGET rtklib PROPERTY C_STANDARD 99)


target_include_directories(rtklib
    PUBLIC ${PROJECT_SOURCE_DIR}/include
)

if(WIN32)
  target_link_libraries(rtklib wsock32 ws2_32 winmm)
else()
  target_link_libraries(rtklib m pthread)
endif()

# Set proper export symbols for Windows DLL
if(WIN32 AND DLL)
    set_target_properties(rtklib PROPERTIES
        WINDOWS_EXPORT_ALL_SYMBOLS TRUE
    )
endif()
install(TARGETS rtklib 
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib)
