cmake_minimum_required(VERSION 3.22)
project(VulkanProject)

find_package(Vulkan REQUIRED)

include(cmake/Shaders.cmake)
include(FetchContent)

FetchContent_Declare(
    glm
    GIT_REPOSITORY "https://github.com/g-truc/glm.git"
    GIT_TAG "0.9.9.8"
    GIT_SHALLOW TRUE
)

FetchContent_MakeAvailable(glm)

FetchContent_Declare(
    spdlog
    GIT_REPOSITORY "https://github.com/gabime/spdlog.git"
    GIT_TAG "v1.12.0"
    GIT_SHALLOW TRUE
)

FetchContent_MakeAvailable(spdlog)

FetchContent_Declare(
    glfw
    GIT_REPOSITORY "https://github.com/glfw/glfw.git"
    GIT_TAG "3.3.8"
    GIT_SHALLOW TRUE
)

FetchContent_MakeAvailable(glfw)

FetchContent_Declare(
    microsoft-gsl
    GIT_REPOSITORY "https://github.com/microsoft/GSL.git"
    GIT_TAG "v4.0.0"
    GIT_SHALLOW TRUE
)

FetchContent_MakeAvailable(microsoft-gsl)

file(GLOB_RECURSE VulkanProjectSources CONFIGURE_DEPENDS
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*.h"
)

add_executable(VulkanProject ${VulkanProjectSources})

target_link_libraries(VulkanProject PRIVATE Vulkan::Vulkan)
target_link_libraries(VulkanProject PRIVATE glm)
target_link_libraries(VulkanProject PRIVATE glfw)
target_link_libraries(VulkanProject PRIVATE Microsoft.GSL::GSL)
target_link_libraries(VulkanProject PRIVATE spdlog)

target_include_directories(VulkanProject PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/src/")

target_compile_features(VulkanProject PRIVATE cxx_std_20)

target_precompile_headers(VulkanProject PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/src/precomp.h")

file(GLOB_RECURSE ShaderSources CONFIGURE_DEPENDS
    "${CMAKE_CURRENT_SOURCE_DIR}/shaders/*.vert"
    "${CMAKE_CURRENT_SOURCE_DIR}/shaders/*.frag"
)

add_shaders(VulkanProjectShaders ${ShaderSources})
add_dependencies(VulkanProject VulkanProjectShaders)

file(COPY "${CKAKE_CURRENT_SOURCE_DIR}nicolas_cage.jpg" DESTINATION "${CMAKE_CURRENT_BINARY_DIR}")