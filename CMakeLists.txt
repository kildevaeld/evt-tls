cmake_minimum_required(VERSION 3.10)
project(evt_tls)

option(EVT_TLS_SAMPLE "build sample" off)
option(EVT_TLS_BORING "use local boringssl" off)


## Find openssl
#find_library(openssl QUIET)
if(EVT_TLS_BORING)
    message(STATUS "Using local boringssl")
    include(cmake/boringssl.cmake)
else()
    find_package(OpenSSL QUIET)
    
    if (NOT OPENSSL_FOUND)
        find_package(PkgConfig)
        pkg_check_modules(OPENSSL QUIET openssl)
        if (APPLE AND OPENSSL_FOUND)
            include_directories(/usr/local/opt/openssl/include)
            link_directories(/usr/local/opt/openssl/lib)
        endif(APPLE AND OPENSSL_FOUND)
    endif(NOT OPENSSL_FOUND)

    if(NOT OPENSSL_FOUND)
        include(cmake/boringssl.cmake)
    endif(NOT OPENSSL_FOUND)

    endif(EVT_TLS_BORING)


include_directories(${OPENSSL_INCLUDE_DIRS})

include_directories(${PROJECT_SOURCE_DIR}/include)

add_subdirectory(src)

if (EVT_TLS_SAMPLE)
    add_subdirectory(sample/libuv-tls)
endif(EVT_TLS_SAMPLE)