# Cmake_notes
some notes

```cmake
FOREACH(LIB ${OpenCV_LIBS})
  install(FILES "/usr/lib64/lib${LIB}.so" DESTINATION /usr/lib64 )
  install(FILES "/usr/lib64/lib${LIB}.so.${OpenCV_VERSION_MAJOR}.${OpenCV_VERSION_MINOR}" DESTINATION /usr/lib64 )
  install(FILES "/usr/lib64/lib${LIB}.so.${OpenCV_VERSION_MAJOR}.${OpenCV_VERSION_MINOR}.${OpenCV_VERSION_PATCH}" DESTINATION /usr/lib64 )
ENDFOREACH(LIB)

FOREACH(LIB ${TBB_LIBRARIES})
  install(FILES ${LIBS} DESTINATION /usr/local/lib)
ENDFOREACH(LIB)

install(FILES ${TBB_LIBRARIES} DESTINATION /usr/local/lib )

install(FILES  "/usr/local/lib/libtbb.so" DESTINATION /usr/local/lib )
install(FILES  "/usr/local/lib/libtbbmalloc.so" DESTINATION /usr/local/lib )
install(FILES  "/usr/local/lib/libtbbmalloc_proxy.so" DESTINATION /usr/local/lib )

```
