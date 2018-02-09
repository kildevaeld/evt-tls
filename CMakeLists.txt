cmake_minimum_required(VERSION 3.0)
project(evt_tls)

option(EVT_TLS_SAMPLE "build sample" off)

## Find openssl
find_library(openssl QUIET)

if(NOT OPENSSL_FOUND)
    message(STATUS "Using local boringssl")
    add_subdirectory(vendor/boringssl)
    set(OPENSSL_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/vendor/boringssl/include)
    set(OPENSSL_CRYPTO_LIBRARY ${PROJECT_BINARY_DIR}/vendor/boringssl/crypto/libcrypto.a)
    set(OPENSSL_SSL_LIBRARY ${PROJECT_BINARY_DIR}/vendor/boringssl/ssl/libssl.a)
    set(OPENSSL_LIBRARIES ${OPENSSL_CRYPTO_LIBRARY} ${OPENSSL_SSL_LIBRARY} z)
endif(NOT OPENSSL_FOUND)

include_directories(${OPENSSL_INCLUDE_DIR})

include_directories(${PROJECT_SOURCE_DIR}/include)

add_subdirectory(src)

if (EVT_TLS_SAMPLE)
    add_subdirectory(sample/libuv-tls)
endif(EVT_TLS_SAMPLE)