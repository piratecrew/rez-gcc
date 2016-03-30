cmake_minimum_required(VERSION 2.8)

include(RezBuild)
include(RezRepository)
include(ExternalProject)

# ------------------------------------------------------------------------------
# gcc has been successfully built on:
#
# (1)
# Description:    Centos 6.7 x64
# ------------------------------------------------------------------------------


set(mpc_version 1.0.1)
set(mpfr_version 3.1.2)
set(gmp_version 5.1.3)
set(isl_version 0.11.1)
set(cloog_version 0.18.0)

set(gcc_version $ENV{REZ_BUILD_PROJECT_VERSION})

rez_set_archive(
    url_mpc gcc/mpc-${mpc_version}.tar.gz
    http://ftp.gnu.org/gnu/mpc/mpc-${mpc_version}.tar.gz
)   

rez_set_archive(
    url_mpfr gcc/mpfr-${mpfr_version}.tar.bz2
    http://www.mpfr.org/mpfr-${mpfr_version}/mpfr-${mpfr_version}.tar.bz2
)

rez_set_archive(
    url_gmp gcc/gmp-${gmp_version}.tar.bz2
    http://ftp.gnu.org/gnu/gmp/gmp-${gmp_version}.tar.bz2
)

rez_set_archive(
    url_isl gcc/isl-${isl_version}.tar.bz2
    ftp://ftp.irisa.fr/pub/mirrors/gcc.gnu.org/gcc/infrastructure/isl-${isl_version}.tar.bz2
)   

rez_set_archive(
    url_cloog gcc/cloog-${cloog_version}.tar.gz
    ftp://ftp.irisa.fr/pub/mirrors/gcc.gnu.org/gcc/infrastructure/cloog-${cloog_version}.tar.gz
)

rez_set_archive(
    url_gcc gcc/gcc-${gcc_version}.tar.bz2
    ftp://ftp.gnu.org/gnu/gcc/gcc-${gcc_version}/gcc-${gcc_version}.tar.bz2
)




add_custom_target(
    build_gcc
    ALL
    COMMAND bash ./build.sh ${CMAKE_BINARY_DIR} ${CMAKE_INSTALL_PREFIX} $ENV{REZ_REPO_PAYLOAD_DIR} ${gcc_version} ${gmp_version} ${mpfr_version} ${mpc_version} ${isl_version} ${cloog_version}
    WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}
    VERBATIM
)

install(
    CODE
    "execute_process(
        COMMAND make install
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/objdir
    )"
)

