set(EXAMPLE_FILES 
  1_extract_from_image.cpp
  2_extract_pose_or_heatmat_from_image.cpp)

foreach(EXAMPLE_FILE ${EXAMPLE_FILES})

  get_filename_component(SOURCE_NAME ${EXAMPLE_FILE} NAME_WE)

  if (UNIX AND NOT APPLE)
    set(EXE_NAME "${SOURCE_NAME}.bin")
  elseif (WIN32)
    set(EXE_NAME "${SOURCE_NAME}")
  endif ()

  message(STATUS "Adding Example ${EXE_NAME}")
  add_executable(${EXE_NAME} ${EXAMPLE_FILE})
  target_link_libraries(${EXE_NAME} openpose ${examples_3rdparty_libraries})

  if (WIN32)
    set_property(TARGET ${EXE_NAME} PROPERTY FOLDER "Examples/Tutorial/Pose")
    configure_file(${CMAKE_SOURCE_DIR}/cmake/OpenPose${VCXPROJ_FILE_GPU_MODE}.vcxproj.user
        ${CMAKE_CURRENT_BINARY_DIR}/${EXE_NAME}.vcxproj.user @ONLY)
    # Properties->General->Output Directory
    set_property(TARGET ${EXE_NAME} PROPERTY RUNTIME_OUTPUT_DIRECTORY_RELEASE ${PROJECT_BINARY_DIR}/$(Platform)/$(Configuration))
    set_property(TARGET ${EXE_NAME} PROPERTY RUNTIME_OUTPUT_DIRECTORY_DEBUG ${PROJECT_BINARY_DIR}/$(Platform)/$(Configuration))
  endif (WIN32)

endforeach()
