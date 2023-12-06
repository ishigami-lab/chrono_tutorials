cmake_minimum_required(VERSION 3.10)

set(PROGRAM demo_CH_archive)

project(${PROGRAM})

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

# Important!: なぜか公式Tutorialに従ってライブラリをビルドしても、CHRONO_LIBRARIESに静的ライブラリが含まれなかったため、手動でリンクを追加。
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoEngine.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoEngine_vehicle.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoModels_robot.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoModels_vehicle.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoEngine_vehicle_cosim.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoEngine_irrlicht.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoEngine_postprocess.so")
LIST(APPEND CHRONO_LIBRARIES "/sbel/Desktop/chrono_build/lib/libChronoEngine_vehicle_irrlicht.so")

include_directories(${CHRONO_INCLUDE_DIRS})

add_executable(${PROGRAM} ${PROGRAM}.cpp)

target_compile_definitions(${PROGRAM} PUBLIC "CHRONO_DATA_DIR=\"${CHRONO_DATA_DIR}\"") 
target_compile_options(${PROGRAM} PUBLIC ${CHRONO_CXX_FLAGS})
target_link_options(${PROGRAM} PUBLIC ${CHRONO_LINKER_FLAGS})

target_link_libraries(${PROGRAM} ${CHRONO_LIBRARIES})
