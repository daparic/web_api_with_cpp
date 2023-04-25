cmake_minimum_required(VERSION 3.22)

project(CalculatorAPI)

add_custom_command(OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/restbed
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    COMMAND git clone https://github.com/corvusoft/restbed.git
    COMMAND git -C restbed checkout 0c99284fba4f37a89de24972e48b8154abc5dc75
    COMMAND mkdir restbed/build
    COMMAND cmake -DBUILD_SSL=NO -DBUILD_TESTS=NO -S restbed -B restbed/build
    COMMAND cmake --build restbed/build
    COMMAND cmake --install restbed/build 
)

add_executable(CalculatorAPI 
    main.cpp 
    CalcResourceFactory.cpp
    CalcService.cpp
    CalcServiceSettingsFactory.cpp
    restbed)

target_link_libraries(CalculatorAPI restbed)
target_include_directories(CalculatorAPI PUBLIC ${CMAKE_SOURCE_DIR}/include "${CMAKE_SOURCE_DIR}/restbed/distribution/include")
target_link_directories(CalculatorAPI PUBLIC "${CMAKE_SOURCE_DIR}/restbed/distribution/library")
set_property(TARGET CalculatorAPI PROPERTY CXX_STANDARD 17)
