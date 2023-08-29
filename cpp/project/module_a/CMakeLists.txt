file(
    GLOB_RECURSE SRC_FILES
    RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}"
    CONFIGURE_DEPENDS
    "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp"
)

add_executable(module_a ${SRC_FILES})

target_include_directories(module_a PRIVATE ".")

target_link_libraries(module_a PRIVATE pthread)
