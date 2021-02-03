cmake_minimum_required(VERSION 3.8 FATAL_ERROR)

project(ExpengineKDRender VERSION 0.0.1)

 set(CMAKE_CXX_STANDARD 20)
 set(CMAKE_CXX_STANDARD_REQUIRED True)

# Header configuration
set(ENGINE_APP_BUILD_TARGET_NAME ExpengineKDRender)
configure_file(${ENGINE_APP_BUILD_TARGET_NAME}.h.in ${ENGINE_APP_BUILD_TARGET_NAME}.h)

# Project sources
set(KDRenderDir ${CMAKE_CURRENT_SOURCE_DIR}/KDRender-fork)
set(BUILDER_SOURCES 
		${KDRenderDir}/src_builder/KDTreeBuilder.cpp
		${KDRenderDir}/src_builder/KDTreeBuilderData.cpp		
		${KDRenderDir}/src_builder/Map.cpp
		${KDRenderDir}/src_builder/mapbuilder.cpp
		${KDRenderDir}/src_builder/MapParser.cpp
		${KDRenderDir}/src_builder/SectorInclusionOperator.cpp
		${KDRenderDir}/src_builder/WallBreakerOperator.cpp)
set(COMMON_SOURCES 
		${KDRenderDir}/src_common/ImageFromFileOperator.cpp
		${KDRenderDir}/src_common/KDTreeMap.cpp		
		${KDRenderDir}/src_common/Light.cpp
		${KDRenderDir}/src_common/Sprite.cpp)
set(RENDERER_SOURCES 
		${KDRenderDir}/src_renderer/FlatSurfacesRenderer.cpp
		${KDRenderDir}/src_renderer/KDTreeRenderer.cpp		
		${KDRenderDir}/src_renderer/KDTreeRendererData.cpp
		${KDRenderDir}/src_renderer/Screen.cpp
		${KDRenderDir}/src_renderer/WallRenderer.cpp)

# Project executables
add_executable(MapBuilder ${COMMON_SOURCES} ${BUILDER_SOURCES})
add_executable(${ENGINE_APP_BUILD_TARGET_NAME} ${COMMON_SOURCES} ${RENDERER_SOURCES})

# Project includes
target_include_directories(MapBuilder PUBLIC
	# Binary dir for CMake generated headers
    ${PROJECT_BINARY_DIR}
	${KDRenderDir}/include
)
target_include_directories(${ENGINE_APP_BUILD_TARGET_NAME} PUBLIC
	# Binary dir for CMake generated headers
    ${PROJECT_BINARY_DIR}
	${KDRenderDir}/include
)

# Add engine CMake and link to the built engine lib
add_subdirectory(ExperimEngine)
target_link_libraries(${ENGINE_APP_BUILD_TARGET_NAME} LibExperimEngine)

# Project includes
target_include_directories(${ENGINE_APP_BUILD_TARGET_NAME} PUBLIC
	# For CMake generated headers
    ${PROJECT_BINARY_DIR}
)
