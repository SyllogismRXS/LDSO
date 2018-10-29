SET (LIBRARY_NAME ldso)

add_library(${LIBRARY_NAME} SHARED
        Feature.cc
        Frame.cc
        Point.cc
        Setting.cc
        Camera.cc
        Map.cc

        internal/PointHessian.cc
        internal/FrameHessian.cc
        internal/GlobalCalib.cc
        internal/FrameFramePrecalc.cc
        internal/Residuals.cc
        internal/ImmaturePoint.cc
        internal/PR.cc

        internal/OptimizationBackend/AccumulatedSCHessian.cc
        internal/OptimizationBackend/AccumulatedTopHessian.cc
        internal/OptimizationBackend/EnergyFunctional.cc

        frontend/CoarseTracker.cc
        frontend/CoarseInitializer.cc
        frontend/FullSystem.cc
        frontend/DSOViewer.cc
        frontend/FeatureDetector.cc
        frontend/FeatureMatcher.cc
        frontend/LoopClosing.cc
        frontend/PixelSelector2.cc
        frontend/Undistort.cc
        frontend/ImageRW_OpenCV.cc
)

target_link_libraries(${LIBRARY_NAME}
  PUBLIC
    ${DBoW3_LIBS}
    ${OpenCV_LIBS}
    ${GLOG_LIBRARY}
    ${Pangolin_LIBRARIES}
    ${PROJECT_SOURCE_DIR}/thirdparty/g2o/lib/libg2o${CMAKE_SHARED_LIBRARY_SUFFIX}
    ${PROJECT_SOURCE_DIR}/thirdparty/DBoW3/build/src/libDBoW3${CMAKE_SHARED_LIBRARY_SUFFIX}
    ${OPENGL_LIBRARIES}
    )

target_include_directories(${LIBRARY_NAME}
  PUBLIC
  ${EIGEN3_INCLUDE_DIR}
  ${OpenCV_INCLUDE_DIR}
  ${Glog_INCLUDE_DIRS}
  ${Pangolin_INCLUDE_DIRS}
  "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include;${PROJECT_SOURCE_DIR}/thirdparty;${PROJECT_SOURCE_DIR}/thirdparty/g2o;${PROJECT_SOURCE_DIR}/thirdparty/DBoW3;${PROJECT_SOURCE_DIR}/examples>"
  )

if (LIBZIP_LIBRARY)
  target_compile_definitions(${LIBRARY_NAME}
    INTERFACE -DHAS_ZIPLIB=1
    )
  target_link_libraries(${LIBRARY_NAME}
    PUBLIC

    ${LIBZIP_LIBRARY}
    )
endif()

install(TARGETS ${LIBRARY_NAME}
  # IMPORTANT: Add the library to the "export-set"
  EXPORT ${PROJECT_NAME}-targets
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
)
