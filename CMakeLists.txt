cmake_minimum_required(VERSION 3.4)
project(alac)

include(conanbuildinfo.cmake)
conan_basic_setup()

file(GLOB ALAC_LIB_SRCS source_subfolder/codec/*.c source_subfolder/codec/*.cpp)
file(GLOB ALAC_LIB_HDRS source_subfolder/codec/*.h)

add_library(alac ${ALAC_LIB_SRCS})
target_include_directories(alac PUBLIC source_subfolder/codec)
set_property(TARGET alac PROPERTY WINDOWS_EXPORT_ALL_SYMBOLS TRUE)

install(FILES ${ALAC_LIB_HDRS} DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/alac")
install(TARGETS alac
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})

if(ALAC_BUILD_UTILITY)
    file(GLOB ALAC_CONVERTER_SRCS source_subfolder/convert-utility/*.cpp)

    add_executable(alacconvert ${ALAC_CONVERTER_SRCS})
    target_link_libraries(alacconvert PRIVATE alac)

    install(TARGETS alacconvert DESTINATION ${CMAKE_INSTALL_BINDIR})
endif()
