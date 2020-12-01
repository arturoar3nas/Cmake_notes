# Cmake_notes

CPACK Properties

```cmake
#
# CPack Properties
#
set(CPACK_PACKAGE_NAME "package_name")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "")
set(CPACK_PACKAGE_VENDOR "company")
set(CPACK_PACKAGE_VERSION_MAJOR "1")
set(CPACK_PACKAGE_VERSION_MINOR "1")
set(CPACK_PACKAGE_VERSION_PATCH "1")
set(CPACK_PACKAGE_INSTALL_DIRECTORY "CMake ${CMake_VERSION_MAJOR}.${CMake_VERSION_MINOR}")
set(CPACK_GENERATOR "RPM")
set(CPACK_RPM_COMPONENT_INSTALL ON)
set(CPACK_RPM_PACKAGE_AUTOREQPROV NO)
set(CPACK_RPM_PACKAGE_REQUIRES "gtk2, ffmpeg, libev, boost, openssl, gsl")
include(CPack)

```

Installing and Packaging

```cmake
set (CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH};${CMAKE_CURRENT_SOURCE_DIR}/cmake")
find_package (Matlab) # runs FindMatlab.cmake
include(TestInline) # defines a macro:
test_inline (CONFIG_C_INLINE)
include(stdint) # simply executes flat CMake code
```

```cmake
FOREACH(LIB ${OpenCV_LIBS})
  install(FILES "/usr/lib64/lib${LIB}.so" DESTINATION /usr/lib64 )
  install(FILES "/usr/lib64/lib${LIB}.so.${OpenCV_VERSION_MAJOR}.${OpenCV_VERSION_MINOR}" DESTINATION /usr/lib64 )
  install(FILES "/usr/lib64/lib${LIB}.so.${OpenCV_VERSION_MAJOR}.${OpenCV_VERSION_MINOR}.${OpenCV_VERSION_PATCH}" DESTINATION /usr/lib64 )
ENDFOREACH(LIB)

``` 
```cmake
FOREACH(LIB ${Boost_LIBRARIES}) 
    message(STATUS "Boost: ${LIB} => ${LIB}.${Boost_VERSION_STRING}")
    install(FILES ${LIB} DESTINATION /usr/lib64)
    install(FILES ${LIB}.${Boost_VERSION_STRING} DESTINATION /usr/lib64)
ENDFOREACH(LIB)
``` 
```cmake
FOREACH(LIB ${TBB_LIBRARIES})
  install(FILES ${LIBS} DESTINATION /usr/local/lib)
ENDFOREACH(LIB)

install(FILES ${TBB_LIBRARIES} DESTINATION /usr/local/lib )

install(FILES  "/usr/local/lib/libtbb.so" DESTINATION /usr/local/lib )
install(FILES  "/usr/local/lib/libtbbmalloc.so" DESTINATION /usr/local/lib )
install(FILES  "/usr/local/lib/libtbbmalloc_proxy.so" DESTINATION /usr/local/lib )
```
```cmake
include( FindPackageHandleStandardArgs )
set( AMQPCPP_DIR "/usr/local/" )
find_library(AMQPCPP_LIBRARY NAMES amqpcpp HINTS ${AMQPCPP_DIR})
find_path(AMQPCPP_INCLUDE_DIR amqpcpp.h HINTS ${AMQPCPP_DIR})

find_package_handle_standard_args(AMQPCPP DEFAULT_MSG AMQPCPP_INCLUDE_DIR AMQPCPP_LIBRARY)

if(AMQPCPP_FOUND)
  set( AMQPCPP_INCLUDE_DIRS ${AMQPCPP_INCLUDE_DIR} )
  set( AMQPCPP_LIBRARIES ${AMQPCPP_LIBRARY} )
  mark_as_advanced(AMQPCPP_LIBRARY AMQPCPP_INCLUDE_DIR AMQPCPP_DIR)
else()
  set( AMQPCPP_DIR "" CACHE STRING "An optional hint to a directory for finding `AMQPCPP`")
endif()

```
