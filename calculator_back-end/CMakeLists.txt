cmake_minimum_required(VERSION 3.22)

project(CalculatorAPI)

add_custom_command(OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/restbed
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    COMMAND git clone --recursive https://github.com/corvusoft/restbed.git
    COMMAND git -C restbed checkout 0c99284fba4f37a89de24972e48b8154abc5dc75
    COMMAND mkdir restbed/build
    COMMAND cmake -DBUILD_SSL=NO -DBUILD_TESTS=NO -S restbed -B restbed/build
    COMMAND cmake --build restbed/build
    COMMAND cmake --install restbed/build 
)

add_custom_command(OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/paho.mqtt-c
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    COMMAND git clone https://github.com/eclipse/paho.mqtt.c.git paho.mqtt-c
    COMMAND git -C paho.mqtt-c checkout tags/v1.3.8
    COMMAND mkdir paho.mqtt-c/build
    COMMAND cmake -Bpaho.mqtt-c/build -S paho.mqtt-c 
    	-DPAHO_ENABLE_TESTING=OFF 
	-DPAHO_BUILD_STATIC=ON 
	-DPAHO_WITH_SSL=OFF 
	-DPAHO_HIGH_PERFORMANCE=ON 
	-DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_SOURCE_DIR}/paho.mqtt-c/build/installdir
    COMMAND cmake --build paho.mqtt-c/build/ --target install
)

add_executable(CalculatorAPI 
    main.cpp 
    CalcResourceFactory.cpp
    CalcService.cpp
    CalcServiceSettingsFactory.cpp
    restbed
    paho.mqtt-c
)

target_link_libraries(CalculatorAPI restbed paho-mqtt3c)
target_compile_options(CalculatorAPI PRIVATE -fpermissive)

target_include_directories(CalculatorAPI 
    PUBLIC 
    ${CMAKE_SOURCE_DIR}/include 
    "${CMAKE_SOURCE_DIR}/restbed/distribution/include" 
    "${CMAKE_SOURCE_DIR}/paho.mqtt-c/build/installdir/include"
)

target_link_directories(CalculatorAPI 
    PUBLIC 
    "${CMAKE_SOURCE_DIR}/restbed/distribution/library" 
    "${CMAKE_SOURCE_DIR}/paho.mqtt-c/build/installdir/lib"
)

set_property(TARGET CalculatorAPI PROPERTY CXX_STANDARD 17)
