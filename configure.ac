m4_define([mate_utils_major], [1])
m4_define([mate_utils_minor], [24])
m4_define([mate_utils_micro], [0])

m4_define([mate_utils_version], [mate_utils_major.mate_utils_minor.mate_utils_micro])

AC_INIT([mate-utils],
        [mate_utils_version],
        [https://mate-desktop.org/])

AC_CONFIG_MACRO_DIR([m4])

AC_CONFIG_HEADERS([config.h])
AM_INIT_AUTOMAKE([1.11 foreign dist-xz no-dist-gzip check-news]) #the foreign flavour disables warnings if README, NEWS such files are not present which are issued in the default gnu flavour
AM_SILENT_RULES([yes])

MATE_COMMON_INIT
MATE_DEBUG_CHECK([no])
MATE_COMPILE_WARNINGS

AC_PROG_CXX
AC_PROG_CC
AC_PROG_CPP
AC_STDC_HEADERS
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET
AM_PROG_LIBTOOL
AC_PATH_PROG(GLIB_GENMARSHAL, [glib-genmarshal])
AC_PATH_PROG(GLIB_MKENUMS, [glib-mkenums])

dnl ICONS: convert svg to png
AC_PATH_PROG(RSVG_CONVERT, rsvg-convert)
AM_CONDITIONAL([HAVE_RSVG_CONVERT], [test "x$RSVG_CONVERT" != x])
AC_PATH_PROG(INKSCAPE, inkscape)
AM_CONDITIONAL([HAVE_INKSCAPE], [test "x$INKSCAPE" != x])

# GLIB_COMPILE_RESOURCES
AC_ARG_VAR([GLIB_COMPILE_RESOURCES],[glib-compile-resources bin])
AC_PATH_PROG([GLIB_COMPILE_RESOURCES],[glib-compile-resources],[])
if test -z "$GLIB_COMPILE_RESOURCES"; then
  AC_MSG_ERROR([glib-compile-resources not found])
fi

# XMLLINT
AC_ARG_VAR([XMLLINT],[xmllint bin])
AC_PATH_PROG([XMLLINT],[xmllint],[])
if test -z "$XMLLINT"; then
  AC_MSG_ERROR([xmllint not found])
fi

AC_PATH_XTRA

AC_C_CONST
AC_C_INLINE
AC_TYPE_OFF_T
AC_TYPE_PID_T
AC_TYPE_SIZE_T
AC_STRUCT_ST_RDEV
AC_HEADER_TIME
AC_STRUCT_TM
AC_TYPE_SIGNAL
AC_FUNC_STRFTIME
AC_CHECK_FUNCS([getpgid])


# Before making a release, the LT_VERSION string should be modified.
# The string is of the form C:R:A.
# - If interfaces have been changed or added, but binary compatibility has
#   been preserved, change to C+1:0:A+1
# - If binary compatibility has been broken (eg removed or changed interfaces)
#   change to C+1:0:0
# - If the interface is the same as the previous version, change to C:R+1:A

LIBGDICT_LT_VERSION=6:7:0
AC_SUBST(LIBGDICT_LT_VERSION)

dnl pkg-config check

GLIB_REQUIRED=2.50.0
GIO_REQUIRED=2.50.0
GIO_UNIX_REQUIRED=2.18.0
GTK_REQUIRED=3.22.0
LIBMATE_PANEL_APPLET_REQUIRED=1.17.0
LIBGTOP_REQUIRED=2.12.0
LIBCANBERRA_GTK_REQUIRED=0.4
UDISKS2_REQUIRED=1.90.0

AC_SUBST(GTK_REQUIRED)

# common checks
PKG_CHECK_MODULES(GLIB, glib-2.0 >= $GLIB_REQUIRED)
AC_SUBST(GLIB_CFLAGS)
AC_SUBST(GLIB_LIBS)

PKG_CHECK_MODULES(GTHREAD, gthread-2.0 >= $GLIB_REQUIRED)
AC_SUBST(GTHREAD_CFLAGS)
AC_SUBST(GTHREAD_LIBS)

PKG_CHECK_MODULES(GIO, gio-2.0 >= $GIO_REQUIRED)
AC_SUBST(GIO_CFLAGS)
AC_SUBST(GIO_LIBS)

PKG_CHECK_MODULES(GIO_UNIX, gio-unix-2.0 >= $GIO_UNIX_REQUIRED)
AC_SUBST(GIO_UNIX_CFLAGS)
AC_SUBST(GIO_UNIX_LIBS)

PKG_CHECK_MODULES(GTK, gtk+-3.0 >= $GTK_REQUIRED)
AC_SUBST(GTK_CFLAGS)
AC_SUBST(GTK_LIBS)

AC_ARG_ENABLE([disk-image-mounter],
               [AC_HELP_STRING([--enable-disk-image-mounter=@<:@yes/no@:>@],
                               [Whether to build the Disk Image Mounter utility])],
               [],
               [enable_disk_image_mounter=yes])

AS_CASE([$enable_disk_image_mounter],
        [yes],
        [
		PKG_CHECK_MODULES(UDISKS2, [udisks2 >= $UDISKS2_REQUIRED])
		AC_SUBST(UDISKS2_CFLAGS)
		AC_SUBST(UDISKS2_LIBS)
        ],

        [no], [],

        [*], [AC_MSG_ERROR([Invalid value for --enable-disk-image-mounter])]
)

AM_CONDITIONAL([BUILD_DISK_IMAGE_MOUNTER], [test "x$enable_disk_image_mounter" = "xyes"])

# libeggsmclient
PKG_CHECK_MODULES(LIBEGGSMCLIENT, gtk+-3.0 >= $GTK_REQUIRED)
AC_SUBST(LIBEGGSMCLIENT_CFLAGS)
AC_SUBST(LIBEGGSMCLIENT_LIBS)

# libgdict requires just gtk+
PKG_CHECK_MODULES(LIBGDICT, glib-2.0 >= $GLIB_REQUIRED
                            gtk+-3.0 >= $GTK_REQUIRED)
AC_SUBST(LIBGDICT_CFLAGS)
AC_SUBST(LIBGDICT_LIBS)

# For each cycle:
# 	first release: increment major += 1, minor = micro = 0;
# 	each release before API freeze: minor += 1;
# 	each release after API freeze: micro += 1;
# Even if this library is not part of the developers platform, we
# follow the same rules: no ABI breakage (unless unavoidable) and
# no API breakage past the API freeze.
m4_define([gdict_major_version], [1])
m4_define([gdict_minor_version], [1])
m4_define([gdict_micro_version], [0])
m4_define([gdict_version], [gdict_major_version.gdict_minor_version.gdict_micro_version])

GDICT_MAJOR_VERSION=gdict_major_version
GDICT_MINOR_VERSION=gdict_minor_version
GDICT_MICRO_VERSION=gdict_micro_version
GDICT_VERSION=gdict_version

AC_SUBST(GDICT_MAJOR_VERSION)
AC_SUBST(GDICT_MINOR_VERSION)
AC_SUBST(GDICT_MICRO_VERSION)
AC_SUBST(GDICT_VERSION)

# xext for mate-screenshot; in theory checking for xext should be
# enough but there are a lot of broken distros out there
PKG_CHECK_MODULES(XSHAPE, xext x11,
                  [AC_CHECK_HEADERS(X11/extensions/shape.h, XSHAPE_LIBS="-lXext -lX11")])
AC_SUBST(XSHAPE_LIBS)

AC_ARG_ENABLE([gdict-applet],
               [AC_HELP_STRING([--enable-gdict-applet=@<:@yes/no@:>@],
                               [Whether to build the Dictionary mate-panel applet])],
               [],
               [enable_gdict_applet=yes])

AS_CASE([$enable_gdict_applet],
        [yes],
        [
          # Gdict applet checks
          PKG_CHECK_MODULES(APPLET, libmatepanelapplet-4.0 >= $LIBMATE_PANEL_APPLET_REQUIRED)
          AC_SUBST(APPLET_LIBS)
          AC_SUBST(APPLET_CFLAGS)
        ],

        [no], [],

        [*], [AC_MSG_ERROR([Invalid value for --enable-gdict-applet])]
)

AM_CONDITIONAL([BUILD_GDICT_APPLET], [test "x$enable_gdict_applet" = "xyes"])

# Baobab checks
PKG_CHECK_MODULES(LIBGTOP, libgtop-2.0 >= $LIBGTOP_REQUIRED)
AC_SUBST(LIBGTOP_CFLAGS)
AC_SUBST(LIBGTOP_LIBS)

PKG_CHECK_MODULES(LIBCANBERRA_GTK, libcanberra-gtk3 >= $LIBCANBERRA_GTK_REQUIRED)
AC_SUBST(LIBCANBERRA_GTK_CFLAGS)
AC_SUBST(LIBCANBERRA_GTK_LIBS)

dnl ***************************************************************
dnl Other miscellaneous checks
dnl ***************************************************************

dnl Enable debug messages
AS_CASE([$ax_enable_debug],
        [yes],[GDICT_DEBUG_CFLAGS="-DGDICT_ENABLE_DEBUG"],
        [no],[GDICT_DEBUG_CFLAGS="-DG_DISABLE_ASSERT -DG_DISABLE_CHECKS -DG_DISABLE_CAST_CHECKS"],
        [*],[GDICT_DEBUG_CFLAGS="-DG_DISABLE_CAST_CHECKS"]
)
AC_SUBST(GDICT_DEBUG_CFLAGS)

dnl IPv6 support
AC_MSG_CHECKING([whether to enable IPv6])
AC_ARG_ENABLE([ipv6],
              [AC_HELP_STRING([--enable-ipv6=@<:@yes/no@:>@],
                              [Enables compilation of IPv6 code])],
              [],
              [enable_ipv6=yes])

AS_IF([test "x$enable_ipv6" = "xyes"],
      [
        AC_TRY_COMPILE(
          [
            #include <sys/socket.h>
            #include <sys/types.h>
          ],
          [
            struct sockaddr_storage ss;
            socket(AF_INET6, SOCK_STREAM, 0)
          ],
          [have_ipv6=yes],
          [have_ipv6=no]
        )
      ]
)

AS_IF([test "x$have_ipv6" = "xyes"],
      [
        have_getaddrinfo=no
        AC_CHECK_FUNC(getaddrinfo, have_getaddrinfo=yes)

        AS_IF([test "x$have_getaddrinfo" != "xyes"],
              [
                for lib in bsd socket inet; do
                  AC_CHECK_LIB($lib,
                               [getaddrinfo],
                               ["LIBS=$LIBS -l$lib"; have_getaddrinfo=yes; break])
                done
              ]
        )

        AS_IF([test "x$have_getaddrinfo" = "xyes"],
              [AC_DEFINE([ENABLE_IPV6], [1], [Define whether IPv6 support is enabled])]
        )
      ]
)

dnl mate-search-tool checks
withval=""
AC_ARG_WITH([grep],
            AC_HELP_STRING([--with-grep=@<:@grep command@:>@]
                           [Specify where to find the grep binary]),
            [
              AS_IF([test x$withval != x],
                    [AC_MSG_RESULT(${withval} is used for mate-search-tool.)],

                    [AC_MSG_RESULT(grep is used for mate-search-tool.)]
              )
            ])

AS_IF([test x$withval != x],
      [GREP_COMMAND="$withval"],

      [GREP_COMMAND="grep"]
)

AC_SUBST(GREP_COMMAND)


dnl logview checks
AC_ARG_ENABLE([zlib],
              [AC_HELP_STRING([--disable-zlib], [disable zlib support])])
msg_zlib=no
Z_LIBS=

AS_IF([test "x$enable_zlib" != "xno"],
      [
        AC_CHECK_HEADER([zlib.h], [AC_CHECK_LIB([z], [inflate], [msg_zlib=yes])])

        AS_IF([test "x$msg_zlib" = "xyes"],
              [
                AC_DEFINE(HAVE_ZLIB, [1],
                          [Define to 1 if we're building with ZLib support])
                Z_LIBS="-lz"
              ]
        )
      ]
)

AC_SUBST(Z_LIBS)

dnl Internationalization
AM_GNU_GETTEXT_VERSION([0.19.8])
AM_GNU_GETTEXT([external])

GETTEXT_PACKAGE=mate-utils
AC_DEFINE_UNQUOTED([GETTEXT_PACKAGE], "$GETTEXT_PACKAGE", [Define the gettext package to use])
AC_SUBST(GETTEXT_PACKAGE)

GLIB_GSETTINGS

dnl gtk-doc stuff
GTK_DOC_CHECK([1.10])

dnl yelp-tools stuff
YELP_HELP_INIT

AC_CONFIG_FILES([
Makefile

po/Makefile.in

baobab/Makefile
baobab/data/Makefile
baobab/data/org.mate.disk-usage-analyzer.gschema.xml
baobab/pixmaps/Makefile
baobab/src/Makefile
baobab/help/Makefile

logview/Makefile
logview/data/Makefile
logview/data/org.mate.system-log.gschema.xml
logview/data/icons/Makefile
logview/help/Makefile
logview/src/Makefile
logview/src/tests/Makefile

gsearchtool/Makefile
gsearchtool/data/Makefile
gsearchtool/data/org.mate.search-tool.gschema.xml
gsearchtool/help/Makefile
gsearchtool/libeggsmclient/Makefile
gsearchtool/libmateui-deprecated/Makefile
gsearchtool/src/Makefile

mate-dictionary/Makefile
mate-dictionary/libgdict/Makefile
mate-dictionary/libgdict/gdict-version.h
mate-dictionary/libgdict/mate-dict.pc
mate-dictionary/data/Makefile
mate-dictionary/data/org.mate.dictionary.gschema.xml
mate-dictionary/docs/Makefile
mate-dictionary/docs/reference/Makefile
mate-dictionary/docs/reference/gdict/Makefile
mate-dictionary/docs/reference/gdict/version.xml
mate-dictionary/help/Makefile
mate-dictionary/src/Makefile

mate-screenshot/Makefile
mate-screenshot/data/Makefile
mate-screenshot/data/org.mate.screenshot.gschema.xml
mate-screenshot/src/Makefile
])

AM_COND_IF([BUILD_DISK_IMAGE_MOUNTER],
           [AC_CONFIG_FILES([mate-disk-image-mounter/Makefile
                             mate-disk-image-mounter/data/Makefile
                             mate-disk-image-mounter/src/Makefile])
           ]
)

AC_OUTPUT

dnl <= Configuration summary =>

echo "
mate-utils $VERSION configuration summary:

                   Compiler flags : $CFLAGS
           Compiler warning flags : $WARN_CFLAGS
                     Linker flags : $LDFLAGS

                           prefix : $prefix
                      sysconf dir : $sysconfdir
                          bin dir : $bindir
                         sbin dir : $sbindir
                         data dir : $datadir

     Debug messages (libmatedict) : $enable_debug
      API Reference (libmatedict) : $enable_gtk_doc
  Logview built with ZLib support : $msg_zlib
     Dictionary mate-panel applet : $enable_gdict_applet
"
