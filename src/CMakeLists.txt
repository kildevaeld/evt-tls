
set(SOURCES
    evt_tls.c
)

add_library(evt_tls ${SOURCES})
target_link_libraries(evt_tls ${OPENSSL_LIBRARIES})