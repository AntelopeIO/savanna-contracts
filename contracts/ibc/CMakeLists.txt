add_contract(ibc ibc ${CMAKE_CURRENT_SOURCE_DIR}/src/ibc.cpp)

target_include_directories(ibc
   PUBLIC
   ${CMAKE_CURRENT_SOURCE_DIR}/include
   ${CMAKE_CURRENT_SOURCE_DIR}/../common)

set_target_properties(ibc
   PROPERTIES
   RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}")

configure_file( ${CMAKE_CURRENT_SOURCE_DIR}/ricardian/ibc.contracts.md.in ${CMAKE_CURRENT_BINARY_DIR}/ricardian/ibc.contracts.md @ONLY )

target_compile_options( ibc PUBLIC -R${CMAKE_CURRENT_SOURCE_DIR}/ricardian -R${CMAKE_CURRENT_BINARY_DIR}/ricardian )
