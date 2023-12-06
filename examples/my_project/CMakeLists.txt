cmake_minimum_required(VERSION 3.10)

project(my_project)

LIST(APPEND CMAKE_PREFIX_PATH "/usr/local/lib")
find_package(Chrono
             COMPONENTS Irrlicht
             OPTIONAL_COMPONENTS PardisoMKL
             CONFIG)

if (NOT Chrono_FOUND)
  message("Could not find Chrono or one of its required modules")
  return()
endif()

set(CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_STANDARD ${CHRONO_CXX_STANDARD})


LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoEngine.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoEngine_vehicle.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoModels_robot.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoModels_vehicle.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoEngine_vehicle_cosim.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoEngine_irrlicht.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoEngine_postprocess.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoEngine_vehicle_irrlicht.so")


include_directories(${CHRONO_INCLUDE_DIRS})

add_executable(my_demo my_example.cpp)

target_compile_definitions(my_demo PUBLIC "CHRONO_DATA_DIR=\"${CHRONO_DATA_DIR}\"") 
target_compile_options(my_demo PUBLIC ${CHRONO_CXX_FLAGS})
target_link_options(my_demo PUBLIC ${CHRONO_LINKER_FLAGS})

target_link_libraries(my_demo ${CHRONO_LIBRARIES})
