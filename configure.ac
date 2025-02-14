AC_PREREQ(2.60)

AC_INIT([libdqlite], [0.1], [https://github.com/CanonicalLtd/dqlite])
AC_CONFIG_SRCDIR(include/dqlite.h)
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_AUX_DIR([ac])
AC_CANONICAL_HOST()
AC_USE_SYSTEM_EXTENSIONS
AC_LANG([C])
AC_PROG_CC

AM_INIT_AUTOMAKE([subdir-objects -Wall -Werror -Wno-portability foreign])
AM_SILENT_RULES([yes])

LT_INIT

AC_PROG_INSTALL
AC_PROG_LN_S

# Enable large file support. This is mandatory in order to interoperate with
# libuv, which enables large file support by default, making the size of 'off_t'
# on 32-bit architecture be 8 bytes instead of the normal 4.
AC_SYS_LARGEFILE

# TODO: eventually enable this
# AX_CHECK_COMPILE_FLAG([-Weverything], AM_CFLAGS+=" -Weverything")

# Enable debugging output.
AC_ARG_ENABLE(debug,
  AS_HELP_STRING(
    [--enable-debug],
    [enable debugging, default: no]),
    [case "${enableval}" in
      yes) debug=true ;;
      no)  debug=false ;;
      *)   AC_MSG_ERROR([bad value ${enableval} for --enable-debug]) ;;
    esac],
    [debug=false])
AM_CONDITIONAL(DEBUG, test x"$debug" = x"true")
AM_COND_IF(DEBUG,
AC_DEFINE(DEBUG, 1, [Define to 0 if this is a release build]),
AC_DEFINE(DEBUG, 0, [Define to 1 or higher if this is a debug build]))

# Whether to enable code coverage.
AX_CODE_COVERAGE

# Enable memory sanitizer.
AC_ARG_ENABLE(sanitize,
  AS_HELP_STRING(
    [--enable-sanitize],
    [enable compiler sanitize instrumentation (needs clang), default: no]),
  [case "${enableval}" in
     yes) sanitize=true ;;
     no)  sanitize=false ;;
     *)   AC_MSG_ERROR([bad value ${enableval} for --sanitize-debug]) ;;
   esac],
  [sanitize=false])
AM_CONDITIONAL(SANITIZE, test x"$sanitize" = x"true")
AM_COND_IF(SANITIZE,
  AX_CHECK_COMPILE_FLAG([-fsanitize=address],
   [true],
   [AC_MSG_ERROR([address sanitizer not supported, clang only])]))

# Checks for libraries
PKG_CHECK_MODULES(SQLITE, [sqlite3 >= 3.22.0], [], [])
PKG_CHECK_MODULES(UV, [libuv >= 1.8.0], [], [])
PKG_CHECK_MODULES(RAFT, [raft], [], [])
PKG_CHECK_MODULES(CO, [libco], [], [])

# Checks for header files.
AC_CHECK_HEADERS([arpa/inet.h fcntl.h stdint.h stdlib.h string.h sys/socket.h unistd.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_SIZE_T
AC_TYPE_SSIZE_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT8_T

# Checks for library functions.
AC_FUNC_MALLOC
AC_CHECK_FUNCS([socket strerror])

AC_CONFIG_FILES([
	Makefile
	dqlite.pc
])
AC_OUTPUT
