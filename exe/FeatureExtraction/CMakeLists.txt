#TBB library
include_directories(${TBB_ROOT_DIR}/include)

add_executable(FeatureExtraction FeatureExtraction.cpp)
#AWSLIBS
set(AWSSDK_LIBS "aws-cpp-sdk-core;aws-cpp-sdk-dynamodb")
# AWS PART
find_package(AWSSDK)
SET(SERVICE core dynamodb)
AWSSDK_LIB_DEPS(SERVICE DEPS)
link_directories("${AWSSDK_LIB_DIR}")


# Local libraries
include_directories(${LandmarkDetector_SOURCE_DIR}/include)

include_directories(../../lib/local/LandmarkDetector/include)
include_directories(../../lib/local/FaceAnalyser/include)
include_directories(../../lib/local/GazeAnalyser/include)
include_directories(../../lib/local/Utilities/include)
include_directories(FeatureExtraction PUBLIC ${AWSSDK_INCLUDE_DIRS})
target_link_libraries(FeatureExtraction LandmarkDetector)
target_link_libraries(FeatureExtraction FaceAnalyser)
target_link_libraries(FeatureExtraction GazeAnalyser)
target_link_libraries(FeatureExtraction Utilities)
target_link_libraries(FeatureExtraction dlib)

target_link_libraries(FeatureExtraction ${OpenCV_LIBS} ${Boost_LIBRARIES} ${TBB_LIBRARIES} ${BLAS_LIBRARIES} aws-cpp-sdk-core aws-cpp-sdk-dynamodb)

install (TARGETS FeatureExtraction DESTINATION bin)
