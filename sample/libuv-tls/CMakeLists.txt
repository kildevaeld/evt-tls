set(SOURCES
    uv_tls.c
)

add_library(uv_tls ${SOURCES})
target_link_libraries(uv_tls evt_tls uv)


add_executable(test_server test_tls.c)
target_link_libraries(test_server uv_tls)


add_executable(test_client tls_client_test.c)
target_link_libraries(test_client uv_tls)