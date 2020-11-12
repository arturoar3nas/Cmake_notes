# Cmake_notes
some notes

```cmake
FOREACH(LIB ${OpenCV_LIBS})
  install(FILES "/usr/lib64/lib${LIB}.so" DESTINATION /usr/lib64 )
  install(FILES "/usr/lib64/lib${LIB}.so.${OpenCV_VERSION_MAJOR}.${OpenCV_VERSION_MINOR}" DESTINATION /usr/lib64 )
  install(FILES "/usr/lib64/lib${LIB}.so.${OpenCV_VERSION_MAJOR}.${OpenCV_VERSION_MINOR}.${OpenCV_VERSION_PATCH}" DESTINATION /usr/lib64 )
ENDFOREACH(LIB)
```
