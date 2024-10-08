cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Set C++ standard to C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Set debug build type
set(CMAKE_BUILD_TYPE Debug)

# Add debug flags to the compiler options
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g")

# Add OpenMP support
find_package(OpenMP REQUIRED)

if(OpenMP_CXX_FOUND)
    # Append OpenMP flags to the compiler
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()

# Add executable target
# add_executable(test_readMNIST tensorLib/test/test_readMNIST.cpp tensorLib/src/readMNIST.cpp tensorLib/src/Tensor.cpp)
# add_executable(test_tensor tensorLib/test/test_tensor.cpp tensorLib/src/Tensor.cpp)
# add_executable(test_readCSV tensorLib/test/test_readCSV.cpp tensorLib/src/readCSV.cpp tensorLib/src/Tensor.cpp)
# add_executable(test_modules tensorLib/test/nn/test_modules.cpp tensorLib/src/nn/modules.cpp tensorLib/src/Tensor.cpp)

add_executable(forward_MNIST app/forward_MNIST.cpp tensorLib/src/readMNIST.cpp tensorLib/src/Tensor.cpp tensorLib/src/readCSV.cpp)
add_executable(forward_MNIST_conv app/forward_MNIST_conv.cpp tensorLib/src/readMNIST.cpp tensorLib/src/Tensor.cpp tensorLib/src/readCSV.cpp)
add_executable(forward_MNIST_quantize app/forward_MNIST_quantize.cpp tensorLib/src/readMNIST.cpp tensorLib/src/Tensor.cpp tensorLib/src/readCSV.cpp)

# Add include directories
include_directories(tensorLib/include)

# Find the zlib library
find_package(ZLIB REQUIRED)
if (ZLIB_FOUND)
    # Include zlib headers
    include_directories(${ZLIB_INCLUDE_DIRS})

    # Link against the zlib library
    # target_link_libraries(test_readMNIST ${ZLIB_LIBRARIES})
    target_link_libraries(forward_MNIST ${ZLIB_LIBRARIES} ${OpenMP_CXX_LIBRARIES})
    target_link_libraries(forward_MNIST_conv ${ZLIB_LIBRARIES} ${OpenMP_CXX_LIBRARIES})
    target_link_libraries(forward_MNIST_quantize ${ZLIB_LIBRARIES} ${OpenMP_CXX_LIBRARIES})
else()
    message(FATAL_ERROR "Zlib library not found in the system, please install it first.")
endif()
