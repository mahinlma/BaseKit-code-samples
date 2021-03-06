#cmake_minimum_required (VERSION 2.8)
#project(CCL_SAMPLES)
set(CCL_TEST_INCLUDE_DIR "$ENV{PWD}/../include")
set(CMAKE_INSTALL_PREFIX "$ENV{PWD}/_install")
if(${CMAKE_CXX_COMPILER_ID} STREQUAL "GNU")
    file(GLOB sources "cpu_*.c" "cpu_*.cpp")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${CMAKE_CLANG_FLAGS} -std=c++11")
    set(CCL_INCLUDE_DIR "$ENV{CCL_ROOT}/include/cpu_icc")
    set(CCL_LIB_DIR "$ENV{CCL_ROOT}/lib/cpu_icc")
    foreach(src ${sources})
        include_directories(${CCL_INCLUDE_DIR})
        include_directories(${CCL_TEST_INCLUDE_DIR})
        link_directories(${CCL_LIB_DIR})
        get_filename_component(executable ${src} NAME_WE)
        add_executable(${executable} ${src})
        target_link_libraries(${executable} PUBLIC rt)
        target_link_libraries(${executable} PUBLIC m)
        target_link_libraries(${executable} PRIVATE ccl)
        target_link_libraries(${executable} PUBLIC pthread dl stdc++)
        install(TARGETS ${executable} RUNTIME DESTINATION "${CMAKE_INSTALL_PREFIX}")
    endforeach()
endif()

if(${CMAKE_CXX_COMPILER_ID} STREQUAL "Clang")
    set(CCL_INCLUDE_DIRS "${CCL_INCLUDE_DIRS} $ENV{SYCL_BUNDLE_ROOT}/include")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsycl -std=c++11")
    file(GLOB sources "sycl_*.c" "sycl_*.cpp")
    set(CCL_INCLUDE_DIR "$ENV{CCL_ROOT}/include/cpu_gpu_dpcpp")
    set(CCL_LIB_DIR "$ENV{CCL_ROOT}/lib/cpu_gpu_dpcpp")
    foreach(src ${sources})
        include_directories(${CCL_INCLUDE_DIR})
        include_directories(${CCL_TEST_INCLUDE_DIR})
        link_directories(${CCL_LIB_DIR})
        get_filename_component(executable ${src} NAME_WE)
        add_executable(${executable} ${src})
        target_link_libraries(${executable} PUBLIC rt)
        target_link_libraries(${executable} PUBLIC m)
        target_link_libraries(${executable} PRIVATE ccl)
        target_link_libraries(${executable} PRIVATE OpenCL)
        target_link_libraries(${executable} PRIVATE sycl)
        install(TARGETS ${executable} RUNTIME DESTINATION "${CMAKE_INSTALL_PREFIX}")
    endforeach()
endif()




