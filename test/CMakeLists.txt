cmake_minimum_required(VERSION 3.14)
include(ExternalData)

find_package(Sanitizers)

# tests
set(test_static_out test_static.out)
add_executable(${test_static_out} test_static.c)
add_test(NAME ${test_static_out} COMMAND ${test_static_out})
add_sanitizers(${test_static_out})

set(test_write_out test_write.out)
add_executable(${test_write_out} test_write.c)
target_link_libraries(${test_write_out} zip)
add_test(NAME ${test_write_out} COMMAND ${test_write_out})
add_sanitizers(${test_write_out})

set(test_append_out test_append.out)
add_executable(${test_append_out} test_append.c)
target_link_libraries(${test_append_out} zip)
add_test(NAME ${test_append_out} COMMAND ${test_append_out})
add_sanitizers(${test_append_out})

set(test_read_out test_read.out)
add_executable(${test_read_out} test_read.c)
target_link_libraries(${test_read_out} zip)
add_test(NAME ${test_read_out} COMMAND ${test_read_out})
add_sanitizers(${test_read_out})

set(test_extract_out test_extract.out)
add_executable(${test_extract_out} test_extract.c)
target_link_libraries(${test_extract_out} zip)
add_test(NAME ${test_extract_out} COMMAND ${test_extract_out})
add_sanitizers(${test_extract_out})

set(test_entry_out test_entry.out)
add_executable(${test_entry_out} test_entry.c)
target_link_libraries(${test_entry_out} zip)
add_test(NAME ${test_entry_out} COMMAND ${test_entry_out})
add_sanitizers(${test_entry_out})

set(test_permissions_out test_permissions.out)
add_executable(${test_permissions_out} test_permissions.c)
target_link_libraries(${test_permissions_out} zip)
if(LINUX OR UNIX)
    find_program(UNZIP_PROGRAM unzip)
    if(UNZIP_PROGRAM)
        message(STATUS "Found unzip: ${UNZIP_PROGRAM}")
        if("${CMAKE_C_COMPILER_ID}" STREQUAL "GNU" OR
          "${CMAKE_C_COMPILER_ID}" STREQUAL "Clang" OR
          "${CMAKE_C_COMPILER_ID}" STREQUAL "AppleClang")
            target_compile_definitions(${test_permissions_out}
              PRIVATE UNZIP_PROGRAM="${UNZIP_PROGRAM}"
            )
        endif()
    endif()
endif()
add_test(NAME ${test_permissions_out} COMMAND ${test_permissions_out})
add_sanitizers(${test_permissions_out})

set(test_open_out test_open.out)
add_executable(${test_open_out} test_open.c)
target_link_libraries(${test_open_out} zip)
add_test(NAME ${test_open_out} COMMAND ${test_open_out})
add_sanitizers(${test_open_out})

set(test_offset_out test_offset.out)
add_executable(${test_offset_out} test_offset.c)
target_link_libraries(${test_offset_out} zip)
add_test(NAME ${test_offset_out} COMMAND ${test_offset_out})
add_sanitizers(${test_offset_out})

set(test_data test_data.out)
add_executable(${test_data} test_data.c)
target_link_libraries(${test_data} zip)
ExternalData_Add_Test(test_data
  NAME test_data
  COMMAND ${test_data} DATA{data/data.bin}
)


