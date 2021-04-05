cmake_minimum_required(VERSION 3.1.0 FATAL_ERROR)
if(COMMAND cmake_policy)
    cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

project(mtfmapper CXX C)
# set the extension used for C++ files here
# this is used to find all sources using a GLOB
set(cpp_ext .cc)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")
SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_helpers)
set(CMAKE_CXX_STANDARD 17)


find_package(OpenCV REQUIRED)
find_package(TCLAP REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(Threads REQUIRED)
find_package(ZLIB REQUIRED)



# Include directories
set(INCLUDE_DIRS PRIVATE
	include/
)
# Build ffsdeamon application
set(MTFMAPPER_SRCS
	src/auto_crop.cc
	src/ca_core.cc
	src/ca_renderer_grid.cc
	src/ca_renderer_print.cc
	src/component_labelling.cc
	src/cropper.cc
	src/cubic_spline_surface.cc
	src/demosaic.cc
	src/display_profile.cc
	src/distortion_optimizer.cc
	src/ellipse.cc
	src/esf_model.cc
	src/esf_model_kernel.cc
	src/esf_model_loess.cc
	src/esf_sampler.cc
	src/esf_sampler_deferred.cc
	src/esf_sampler_line.cc
	src/esf_sampler_piecewise_quad.cc
	src/esf_sampler_quad.cc
	src/find_polynomial_roots_jenkins_traub.cc
	src/five_point_focal_length_radial_distortion.cc
	src/gradient.cc
	src/grid_interpolator.cc
	src/imatest_crop.cc
	src/laguerre_roots.cc
	src/loess_fit.cc
	src/mtf_core.cc
	src/mtf_mapper.cc
	src/mtf_renderer_annotate.cc
	src/mtf_renderer_focus.cc
	src/mtf_renderer_grid.cc
	src/mtf_renderer_lensprofile.cc
	src/pava.cc
	src/point_helpers.cc
	src/ratpoly_fit.cc
	src/srgb_render.cc
	src/thresholding.cc
	src/tiffsniff.cc
	src/undistort.cc
	src/undistort_equiangular.cc
	src/undistort_rectilinear.cc
	src/undistort_stereographic.cc
)

# Build main application
add_executable(mtfmapper ${MTFMAPPER_SRCS})
set_target_properties(mtfmapper PROPERTIES LINKER_LANGUAGE CXX)
target_include_directories(mtfmapper PRIVATE ${INCLUDE_DIRS})
target_link_libraries( mtfmapper ${OpenCV_LIBS} )
