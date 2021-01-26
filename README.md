# Cmake_notes

CPACK Properties

# Filtering all dependencies with file (dependencies.list)

```
/lib64/libpcre2-8.so.0
/lib64/libselinux.so.1
/lib64/libc.so.6
/lib64/libcap.so.2
/lib64/libgpg-error.so.0
/lib64/libgmp.so.10
/lib64/libexpat.so.1
/lib64/libcom_err.so.2
/lib64/libgcrypt.so.20
/lib64/libffi.so.6
/lib64/libp11-kit.so.0
/lib64/liblz4.so.1
/lib64/libkeyutils.so.1
/lib64/libnettle.so.6
/lib64/libgssapi_krb5.so.2
/lib64/libkrb5.so.3
/lib64/libkrb5support.so.0
/lib64/libblkid.so.1
/lib64/libmount.so.1
/lib64/libdbus-1.so.3

```

```cmake 
file(STRINGS "${MAINFOLDER}/dependencies.list" OUTPUT)

foreach(DEPENDENCY_FILE ${DEPENDENCIES})
  set(exlude FALSE)
  foreach(OP ${OUTPUT})
    #message(STATUS "${MAINFOLDER}/dependencies.list: ${OP}")
    if(${OP} MATCHES ${DEPENDENCY_FILE})
      message(STATUS "excluding from package: ${OP}")
      set(exclude TRUE)
    endif()
  endforeach()

  if(NOT exclude)
    get_filename_component(DEPENDENCY_NAME "${DEPENDENCY_FILE}" NAME)
    get_filename_component(DEPENDENCY "${DEPENDENCY_FILE}" REALPATH)
    get_filename_component(DEPENDENCY_PATH "${DEPENDENCY}" DIRECTORY)
    install(FILES ${DEPENDENCY} DESTINATION "${DEPENDENCY_PATH}" )
    message(STATUS "installing : ${DEPENDENCY_FILE}")
  endif()
endforeach()


```

# Basic CPack

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
Lazy Installing 

```cmake 
 include(GetPrerequisites)
 
 set(BINARY_LOCATION "${MAINFOLDER}/bin/${BIN}")
 get_prerequisites(${BINARY_LOCATION} DEPENDENCIES 0 0 "" "")
 
 foreach(DEPENDENCY_FILE ${DEPENDENCIES})
   get_filename_component(DEPENDENCY_NAME "${DEPENDENCY_FILE}" NAME)
   get_filename_component(DEPENDENCY "${DEPENDENCY_FILE}" REALPATH)
   get_filename_component(DEPENDENCY_PATH "${DEPENDENCY}" DIRECTORY)
   install(FILES ${DEPENDENCY} DESTINATION "${DEPENDENCY_PATH}" )
 endforeach()
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
```cmake 
include(FindPackageHandleStandardArgs)

find_path(ARAVIS_INCLUDE_PATH arv.h "$ENV{ARAVIS_INCLUDE_PATH}" /usr/local/include/aravis-0.8)
find_library(ARAVIS_LIBRARY aravis-0.8 "$ENV{ARAVIS_LIBRARY}" /usr/local/lib64)
find_package_handle_standard_args(ARAVIS DEFAULT_MSG ARAVIS_INCLUDE_PATH ARAVIS_LIBRARY)

if(ARAVIS_FOUND)
  get_filename_component(ARAVIS_RESOLVED ${ARAVIS_LIBRARY} REALPATH)
  get_filename_component(ARAVIS_PATH "${ARAVIS_RESOLVED}" DIRECTORY)
  message(STATUS "Aravis: ${ARAVIS_RESOLVED} ${ARAVIS_LIBRARY}.0")
  install(FILES "${ARAVIS_LIBRARY}.0" DESTINATION ${ARAVIS_PATH})
  install(FILES ${ARAVIS_RESOLVED} DESTINATION ${ARAVIS_PATH})
endif() 
```
