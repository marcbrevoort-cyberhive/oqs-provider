include(GNUInstallDirs)
set(OQS_PROV_BINARY_DIR ${CMAKE_BINARY_DIR}/lib)

add_test(
  NAME oqs_signatures
  COMMAND oqs_test_signatures
          "oqsprovider"
          "${CMAKE_SOURCE_DIR}/test/oqs.cnf"
)
# openssl under MSVC seems to have a bug registering NIDs:
# It only works when setting OPENSSL_CONF, not when loading the same cnf file:
if (MSVC)
set_tests_properties(oqs_signatures
    PROPERTIES ENVIRONMENT "OPENSSL_MODULES=${OQS_PROV_BINARY_DIR};OPENSSL_CONF=${CMAKE_CURRENT_SOURCE_DIR}/openssl-ca.cnf"
)
else()
set_tests_properties(oqs_signatures
    PROPERTIES ENVIRONMENT "OPENSSL_MODULES=${OQS_PROV_BINARY_DIR}"
)
endif()

add_executable(oqs_test_signatures oqs_test_signatures.c test_common.c)
target_link_libraries(oqs_test_signatures PRIVATE ${OPENSSL_CRYPTO_LIBRARY} ${OQS_ADDL_SOCKET_LIBS})

add_test(
  NAME oqs_kems
  COMMAND oqs_test_kems
          "oqsprovider"
          "${CMAKE_SOURCE_DIR}/test/oqs.cnf"
)
# openssl under MSVC seems to have a bug registering NIDs:
# It only works when setting OPENSSL_CONF, not when loading the same cnf file:
if (MSVC)
set_tests_properties(oqs_kems
    PROPERTIES ENVIRONMENT "OPENSSL_MODULES=${OQS_PROV_BINARY_DIR};OPENSSL_CONF=${CMAKE_CURRENT_SOURCE_DIR}/openssl-ca.cnf"
)
else()
set_tests_properties(oqs_kems
    PROPERTIES ENVIRONMENT "OPENSSL_MODULES=${OQS_PROV_BINARY_DIR}"
)
endif()

add_executable(oqs_test_kems oqs_test_kems.c test_common.c)
target_link_libraries(oqs_test_kems PRIVATE ${OPENSSL_CRYPTO_LIBRARY} ${OQS_ADDL_SOCKET_LIBS})

add_test(
    NAME oqs_groups
    COMMAND oqs_test_groups
            "oqsprovider"
            "${CMAKE_CURRENT_SOURCE_DIR}/oqs.cnf"
            "${CMAKE_CURRENT_SOURCE_DIR}"
)
# openssl under MSVC seems to have a bug registering NIDs:
# It only works when setting OPENSSL_CONF, not when loading the same cnf file:
if (MSVC)
set_tests_properties(oqs_groups
    PROPERTIES ENVIRONMENT "OPENSSL_MODULES=${OQS_PROV_BINARY_DIR};OPENSSL_CONF=${CMAKE_CURRENT_SOURCE_DIR}/openssl-ca.cnf"
)
else()
set_tests_properties(oqs_groups
    PROPERTIES ENVIRONMENT "OPENSSL_MODULES=${OQS_PROV_BINARY_DIR}"
)
endif()
add_executable(oqs_test_groups oqs_test_groups.c test_common.c tlstest_helpers.c)
target_link_libraries(oqs_test_groups PRIVATE ${OPENSSL_SSL_LIBRARY} ${OPENSSL_CRYPTO_LIBRARY} ${OQS_ADDL_SOCKET_LIBS})

add_test(
    NAME oqs_tlssig
    COMMAND oqs_test_tlssig
            "oqsprovider"
            "${CMAKE_CURRENT_SOURCE_DIR}/openssl-ca.cnf"
            "${CMAKE_CURRENT_BINARY_DIR}/tmp"
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
)
# openssl under MSVC seems to have a bug registering NIDs:
# It only works when setting OPENSSL_CONF, not when loading the same cnf file:
if (MSVC)
set_tests_properties(oqs_tlssig
    PROPERTIES ENVIRONMENT "OPENSSL_MODULES=${OQS_PROV_BINARY_DIR};OPENSSL_CONF=${CMAKE_CURRENT_SOURCE_DIR}/openssl-ca.cnf"
)
else()
set_tests_properties(oqs_tlssig
    PROPERTIES ENVIRONMENT "OPENSSL_MODULES=${OQS_PROV_BINARY_DIR}"
)
endif()
add_executable(oqs_test_tlssig oqs_test_tlssig.c test_common.c tlstest_helpers.c)
target_link_libraries(oqs_test_tlssig PRIVATE ${OPENSSL_SSL_LIBRARY} ${OPENSSL_CRYPTO_LIBRARY} ${OQS_ADDL_SOCKET_LIBS})

add_executable(oqs_test_endecode oqs_test_endecode.c test_common.c)
target_link_libraries(oqs_test_endecode PRIVATE ${OPENSSL_CRYPTO_LIBRARY} ${OQS_ADDL_SOCKET_LIBS})
add_test(
  NAME oqs_endecode
  COMMAND oqs_test_endecode
          "oqsprovider"
          "${CMAKE_CURRENT_SOURCE_DIR}/openssl-ca.cnf"
)
# openssl under MSVC seems to have a bug registering NIDs:
# It only works when setting OPENSSL_CONF, not when loading the same cnf file:
if (MSVC)
set_tests_properties(oqs_endecode
    PROPERTIES ENVIRONMENT "OPENSSL_MODULES=${OQS_PROV_BINARY_DIR};OPENSSL_CONF=${CMAKE_CURRENT_SOURCE_DIR}/openssl-ca.cnf"
)
else()
set_tests_properties(oqs_endecode
    PROPERTIES ENVIRONMENT "OPENSSL_MODULES=${OQS_PROV_BINARY_DIR}"
)
endif()

if (OQS_PROVIDER_BUILD_STATIC)
  targets_set_static_provider(oqs_test_signatures
    oqs_test_kems
    oqs_test_groups
    oqs_test_tlssig
    oqs_test_endecode
  )
endif()
