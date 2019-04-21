CMAKE_MINIMUM_REQUIRED(VERSION 3.2)
PROJECT(ncorr_library)

# Only tested for g++ on Ubuntu 12.04. This assumes all required libraries have been 
# installed, so directories to dependent libraries and their headers are not explicitly 
# included, since the install directories are searched automatically by g++.

# Set files
SET(ncorr_src ../src/ncorr.cpp ../src/Strain2D.cpp ../src/Disp2D.cpp ../src/Data2D.cpp ../src/ROI2D.cpp ../src/Image2D.cpp ../src/Array2D.cpp)
SET(ncorr_h ../include/ncorr.h ../include/Strain2D.h ../include/Disp2D.h ../include/Data2D.h ../include/ROI2D.h ../include/Image2D.h ../include/Array2D.h)

# Set include directory
INCLUDE_DIRECTORIES(../include)
INCLUDE_DIRECTORIES(/usr/include/suitesparse/)
# Set output for library
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ../lib)

# Add library
ADD_LIBRARY(ncorr STATIC ${ncorr_src})

# Set C++11 support
set_property(TARGET ncorr PROPERTY CXX_STANDARD 17)
set_property(TARGET ncorr PROPERTY CXX_STANDARD_REQUIRED ON)

# Set -03 optimization
INCLUDE(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-O3" COMPILER_SUPPORTS_O3)
if (COMPILER_SUPPORTS_O3)
	SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3")
endif()

# Disable debugging
#ADD_DEFINITIONS(-DNDEBUG)

set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_CXX_COMPILE_FEATURES "cxx_template_template_parameters;cxx_alias_templates;cxx_alignas;cxx_alignof;cxx_attributes;cxx_auto_type;cxx_constexpr;cxx_decltype;cxx_decltype_incomplete_return_types;cxx_default_function_template_args;cxx_defaulted_functions;cxx_defaulted_move_initializers;cxx_delegating_constructors;cxx_deleted_functions;cxx_enum_forward_declarations;cxx_explicit_conversions;cxx_extended_friend_declarations;cxx_extern_templates;cxx_final;cxx_func_identifier;cxx_generalized_initializers;cxx_inheriting_constructors;cxx_inline_namespaces;cxx_lambdas;cxx_local_type_template_args;cxx_long_long_type;cxx_noexcept;cxx_nonstatic_member_init;cxx_nullptr;cxx_override;cxx_range_for;cxx_raw_string_literals;cxx_reference_qualified_functions;cxx_right_angle_brackets;cxx_rvalue_references;cxx_sizeof_member;cxx_static_assert;cxx_strong_enums;cxx_thread_local;cxx_trailing_return_types;cxx_unicode_literals;cxx_uniform_initialization;cxx_unrestricted_unions;cxx_user_literals;cxx_variadic_macros;cxx_variadic_templates;cxx_attribute_deprecated;cxx_binary_literals;cxx_contextual_conversions;cxx_decltype_auto;cxx_digit_separators;cxx_generic_lambdas;cxx_lambda_init_captures;cxx_return_type_deduction")

# Install library
INSTALL(TARGETS ncorr DESTINATION lib)
INSTALL(FILES ${ncorr_h} DESTINATION include)
