cmake_minimum_required(VERSION 3.18)

# Project Information
project(blockSQP
    DESCRIPTION "Sequential quadratic programming for problems with block-diagonal Hessian matrix."
    HOMEPAGE_URL https://github.com/djanka2/blockSQP
    LANGUAGES CXX
    )

# Find dependencies: LAPACK
find_package(LAPACK REQUIRED)

# Find dependencies: qpOASES
find_path(QPOASES_PREFIX
    include/qpOASES.hpp
    DOC "qpOASES directory prefix."
    REQUIRED
    )
find_library(QPOASES_LIBRARY
    qpOASES
    REQUIRED
    )

# Add target
add_library(blockSQP SHARED
    src/blocksqp_matrix.cpp
    src/blocksqp_problemspec.cpp
    src/blocksqp_general_purpose.cpp
    src/blocksqp_glob.cpp
    src/blocksqp_hess.cpp
    src/blocksqp_iter.cpp
    src/blocksqp_main.cpp
    src/blocksqp_options.cpp
    src/blocksqp_qp.cpp
    src/blocksqp_restoration.cpp
    src/blocksqp_stats.cpp
    )
set_property(TARGET blockSQP PROPERTY PUBLIC_HEADER
    include/blocksqp_defs.hpp
    include/blocksqp_general_purpose.hpp
    include/blocksqp_iterate.hpp
    include/blocksqp_lapack.h
    include/blocksqp_matrix.hpp
    include/blocksqp_method.hpp
    include/blocksqp_options.hpp
    include/blocksqp_problemspec.hpp
    include/blocksqp_restoration.hpp
    include/blocksqp_stats.hpp
    )
target_include_directories(blockSQP PUBLIC include ${QPOASES_PREFIX}/include)
target_link_libraries(blockSQP LAPACK::LAPACK ${QPOASES_LIBRARY})
install(TARGETS blockSQP
    LIBRARY
    PUBLIC_HEADER
        DESTINATION include/blockSQP
    )

