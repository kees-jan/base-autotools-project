# Base Autotools Project - Starting point for some of my projects
# Copyright (C) 2009-2012 Kees-Jan Dijkzeul
# 
# This library is free software; you can redistribute it and/or
# modify it under the terms of the GNU Lesser General Public
# License, version 2, as published by the Free Software Foundation.
# 
# This library is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# Lesser General Public License for more details.
# 
# You should have received a copy of the GNU Lesser General Public
# License along with this library; if not, write to the Free Software
# Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301  USA

AC_PREREQ([2.64])
### AC_INIT([Scroom],
###         m4_esyscmd([build-aux/git-version-gen | tr -d '\n']),
###         [http://fuzzy.homedns.org/trac/scroom/newticket],
###         [scroom],
###         [http://fuzzy.homedns.org/trac/scroom])
AM_INIT_AUTOMAKE([-Wall -Werror foreign silent-rules])
AM_SILENT_RULES([yes])
LT_PREREQ([2.2])
LT_INIT([dlopen])

AC_CONFIG_MACRO_DIR([m4])
### AC_CONFIG_SRCDIR([inc/scroom/presentationinterface.hh])
AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CXXCPP
dnl AC_PROG_CC
AC_PROG_LN_S
dnl AC_PROG_INSTALL

AC_LANG([C++])
SCROOM_COMPILE_STDCXX_0X
SCROOM_ENABLE_STDCXX_0X
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wall])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-W])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wextra])
dnl SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wconversion])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wpointer-arith])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wwrite-strings])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wunused])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wredundant-decls])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wuninitialized])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Winit-self])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wcast-qual])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wswitch-enum])
dnl SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Weffc++])
dnl SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wold-style-cast])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wfloat-equal])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wnon-virtual-dtor])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Woverloaded-virtual])
SCROOM_ADD_CXXFLAGS_IF_SUPPORTED([-Wsign-promo])
SCROOM_TEST_ISYSTEM

# Configurable features
AC_ARG_ENABLE([mutracx-hacks],
  [AS_HELP_STRING([--enable-mutracx-hacks], 
    [Build for a Mutracx environment @<:@default=no@:>@])],
  [want_mutracx_hacks=${enableval}],
  [want_mutracx_hacks=unknown])
AC_ARG_ENABLE([doxygen-all-code],
  [AS_HELP_STRING([--enable-doxygen-all-code], 
    [Generate doxygen documentation for all code, instead of just the api @<:@default=no@:>@])],
  [want_doxygen_all_code=${enableval}],
  [want_doxygen_all_code=no])
AC_ARG_ENABLE([xml-test-output],
  [AS_HELP_STRING([--enable-xml-test-output], 
    [Generate test results in XML format @<:@default=no@:>@])],
  [want_xml_test_output=${enableval}],
  [want_xml_test_output=no])
  

# Doxygen
DX_DOXYGEN_FEATURE(ON)
DX_DOT_FEATURE(ON)
DX_PDF_FEATURE(OFF)
DX_PS_FEATURE(OFF)
DX_INIT_DOXYGEN([$PACKAGE_NAME], [doxyfile], [doc])
  
# Checks for packages.

# Checks for libraries.

# Checks for header files.
AC_CHECK_HEADERS([stdlib.h string.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_INLINE
AC_TYPE_SIZE_T

# Checks for library functions.
AC_FUNC_MALLOC
AC_CHECK_FUNCS([memset pow select sqrt strerror])

# Mutracx specifics:
AC_MSG_CHECKING([for Mutracx development environment])
need_mutracx_hacks=no
if pkg-config "gtk+-2.0 >= 2.16.0"
then
  need_mutracx_hacks=no
  AC_MSG_RESULT([no])
else
  need_mutracx_hacks=yes
  AC_MSG_RESULT([yes])
fi

#checking for boost unittests
have_boost_test_headers=no
AC_CHECK_HEADERS([boost/test/unit_test.hpp], [have_boost_test_headers=yes])
have_boost_test_lib=no
SCROOM_SEARCH_LIBS([_ZN5boost9unit_test16results_reporter10set_streamERSo],
                   [boost_unit_test_framework-mt libboost_unit_test_framework-gcc42-mt], [have_boost_test_lib=yes])
AC_SUBST([BOOSTTESTLIB], [$ac_cv_search__ZN5boost9unit_test16results_reporter10set_streamERSo])

want_boost_tests=no
if test x$have_boost_test_headers = xyes -a x$have_boost_test_lib = xyes
then
  want_boost_tests=yes
fi
AM_CONDITIONAL([BOOST_UNITTESTS], [test x$want_boost_tests = xyes])
if test "x${need_mutracx_hacks}" = xyes
then
  # Print a warning if the user said "no" to mutracx hacks. If he said
  # "yes", or nothing at all, be quiet and enable anyway.
  if test "x${want_mutracx_hacks}" = xno
  then
    AC_MSG_WARN([Mutracx environment detected. Enabling mutracx-hacks])
  fi
  want_mutracx_hacks=yes
elif test "x${want_mutracx_hacks}" = xunknown
then
  want_mutracx_hacks=$need_mutracx_hacks
fi

# If no doxygen, then we can't process all code
if DX_TEST_FEATURE([doc])
then
    want_doxygen=yes
else
    want_doxygen=no
    want_doxygen_all_code=no
fi

# want_mutracx_hacks is now set to "yes" if they are either needed or
# requested.
if test "x${want_mutracx_hacks}" = xyes
then
  AC_DEFINE([MUTRACX_HACKS], 1, [Hacks for Mutracx environment enabled])
fi

### if test "x${want_doxygen_all_code}" = xyes
### then
###   DX_EXTRA_SRC="\$(top_srcdir)/gui \$(top_srcdir)/libs \$(top_srcdir)/plugins"
### else
###   DX_EXTRA_SRC=
### fi
### AC_SUBST(DX_EXTRA_SRC)

if test "x${want_xml_test_output}" = xyes
then
  AC_DEFINE([XML_TEST_OUTPUT], 1, [Have all Boost unittests report in xml format])
fi

AC_CONFIG_FILES([Makefile
                 doxyfile
])

AC_OUTPUT


echo \
"-------------------------------------------------

 ${PACKAGE_NAME} version ${PACKAGE_VERSION}

 Prefix:   '${prefix}'.
 Compiler: '${CXX} ${CXXFLAGS} ${CPPFLAGS}'
           
 Package features:
   Mutracx hacks: ${want_mutracx_hacks}
   Doxygen:       ${want_doxygen}
     all code:    ${want_doxygen_all_code}
   Unittests:     ${want_boost_tests}

 Now type 'make @<:@<target>@:>@'
   where the optional <target> is:
     all                - build all binaries
     install            - install everything
     doxygen-doc        - generate documentation
                          (if enabled)
     check              - perform tests

--------------------------------------------------"


