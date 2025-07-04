# TODO: should not use this
if (WIN32)
    if (BUILD_SHARED_LIBS)
        set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
    endif()
endif()

#
# libraries
#

# llama

add_library(llama
            ../include/llama.h
            llama.cpp
            llama-vocab.cpp
            llama-grammar.cpp
            llama-sampling.cpp
            unicode.h
            unicode.cpp
            unicode-data.cpp
            )

target_include_directories(llama PUBLIC . ../include ../libpsirngclient/src)
target_compile_features   (llama PUBLIC cxx_std_11) # don't bump

target_link_libraries(llama PUBLIC ggml psirngclient)

# Add MeterFeeder library if it exists
if (EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/../meterfeeder/builds/linux/libmeterfeeder.so")
    target_link_libraries(llama PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/../meterfeeder/builds/linux/libmeterfeeder.so")
    target_link_directories(llama PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/../meterfeeder/builds/linux")
endif()

if (BUILD_SHARED_LIBS)
    set_target_properties(llama PROPERTIES POSITION_INDEPENDENT_CODE ON)
    target_compile_definitions(llama PRIVATE LLAMA_SHARED LLAMA_BUILD)
endif()
