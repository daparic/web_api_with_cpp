cmake_minimum_required(VERSION 3.22)

project(CalculatorAPI)

# add_custom_command(OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/dlib_face_recognition_resnet_model_v1.dat.c
#     WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
#     COMMAND xxd -i dlib_face_recognition_resnet_model_v1.dat dlib_face_recognition_resnet_model_v1.dat.c
#     COMMENT "Converting model dlib_face_recognition_resnet_model_v1.dat ..."
# )

include(FetchContent)
FetchContent_Declare(
    restbed
    GIT_REPOSITORY https://github.com/corvusoft/restbed.git
    GIT_TAG 0c99284fba4f37a89de24972e48b8154abc5dc75
    # FIND_PACKAGE_ARGS NAMES restbed
)
set(BLAHBLAH "-DBUILD_SSL=NO -DBUILD_TESTS=NO")
FetchContent_MakeAvailable(restbed ${BLAHBLAH})
# add_library(restbed INTERFACE IMPORTED)

add_executable(CalculatorAPI 
    main.cpp 
    CalcResourceFactory.cpp
    CalcService.cpp
    CalcServiceSettingsFactory.cpp)

target_link_libraries(CalculatorAPI restbed)
# target_include_directories(CalculatorAPI PUBLIC ${CMAKE_SOURCE_DIR}/include "$ENV{RESTBED_DISTRIBUTION_PATH}/include")
# target_link_directories(CalculatorAPI PUBLIC "$ENV{RESTBED_DISTRIBUTION_PATH}/library")
target_include_directories(CalculatorAPI PUBLIC ${CMAKE_SOURCE_DIR}/include ${restbed_INCLUDE_DIRS})
set_property(TARGET CalculatorAPI PROPERTY CXX_STANDARD 17)

message(STATUS "---> ${restbed_INCLUDE_DIRS} <-----")
message(STATUS "--- " ${restbed_INCLUDE_DIRS})

# if(DEFINED ENV{RESTBED_DISTRIBUTION_PATH})
# 
# endif()
