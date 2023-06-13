#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.
#
### Copyright (C) 2015, 2016 Olaf Till <i7tiol@t-online.de>
###
### This program is free software; you can redistribute it and/or
### modify it under the terms of the GNU General Public License as
### published by the Free Software Foundation; either version 3 of the
### License, or (at your option) any later version.
###
### This program is distributed in the hope that it will be useful,
### but WITHOUT ANY WARRANTY; without even the implied warranty of
### MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
### General Public License for more details.
###
### You should have received a copy of the GNU General Public License
### along with this program; if not, see
### <http://www.gnu.org/licenses/>.

AC_PREREQ([2.62])
AC_INIT([optim], [1.5.0], [i7tiol@t-online.de])
AC_CONFIG_SRCDIR([numgradient.cc])
AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AC_PROG_CXX
AC_CHECK_PROG(MKOCTFILE, mkoctfile, mkoctfile)
AC_CHECK_PROG(OCTAVE_CONFIG, octave-config, octave-config)

# Checks for libraries.

# Checks for header files.
AC_CHECK_HEADERS([float.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_CHECK_HEADER_STDBOOL

# Checks for library functions.
AC_FUNC_ERROR_AT_LINE
AC_CHECK_FUNCS([sqrt])

# Start of checks for Octave features, preparations for checks.
OCTLIBDIR=${OCTLIBDIR:-`$OCTAVE_CONFIG -p OCTLIBDIR`}
OCTINCLUDEDIR=${OCTINCLUDEDIR:-`$OCTAVE_CONFIG -p OCTINCLUDEDIR`}
AC_LANG_PUSH([C++])
TCXXFLAGS=$CXXFLAGS
TLDFLAGS=$LDFLAGS
TLIBS=$LIBS
LDFLAGS="-L$OCTLIBDIR $LDFLAGS"
LIBS="-loctinterp $LIBS"
CXXFLAGS="-I$OCTINCLUDEDIR $CXXFLAGS"

## Presence of 'error_state' -- does _not_ indicate no exceptions are
## used.
AC_LINK_IFELSE(
  [AC_LANG_PROGRAM([[#include <oct.h>]
                    [#include <stdio.h>]],
                   [[printf ("%i", error_state);]])],
  [AC_DEFINE([HAVE_OCTAVE_ERROR_STATE], 1)])

## Presence of 'verror (octave_execution_exception&, const char *,
## va_list)'
AC_LINK_IFELSE(
  [AC_LANG_PROGRAM([[#include <oct.h>]],
                   [[octave_execution_exception e;]
                    [va_list args;]
                    [verror (e, "test", args);]])],
  [AC_DEFINE([HAVE_OCTAVE_VERROR_ARG_EXC], 1)])

LIBS=$TLIBS
LDFLAGS=$TLDFLAGS
CXXFLAGS=$TCXXFLAGS
AC_LANG_POP([C++])
# End of checks for Octave features.

AC_CONFIG_FILES([Makefile])
AC_OUTPUT
