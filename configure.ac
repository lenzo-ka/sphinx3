dnl Welcome to the Sphinx-3 automated build system. 
dnl try not to hurt yourself ;)

AC_INIT(sphinx3, 0.7)
AM_INIT_AUTOMAKE([no-define foreign])
AC_CONFIG_MACRO_DIR([m4])

CFLAGS=${CFLAGS:--g -O2 -Wall}

AC_CANONICAL_HOST

AC_ISC_POSIX
AC_PROG_CC
AM_PROG_CC_STDC
AC_C_BIGENDIAN
AC_HEADER_STDC

AC_C_CONST
AC_TYPE_SIZE_T
AC_CHECK_FUNCS(memmove bcopy)

AC_TYPE_SIGNAL

LT_INIT

dnl
dnl Check whether visibility is supported
dnl
AC_DEFUN([CC_ATTRIBUTE_VISIBILITY], [
	tmp_CFLAGS=$CFLAGS
	CFLAGS="$CFLAGS -fvisibility=hidden"

	AC_CACHE_CHECK([if compiler supports __attribute__((visibility("default")))],
		[cc_cv_attribute_visibility],
		[AC_COMPILE_IFELSE([
			void __attribute__((visibility("default"))) visibility_function() { }
			],
			[cc_cv_attribute_visibility=yes],
			[cc_cv_attribute_visibility=no])
		])

	CFLAGS=$tmp_CFLAGS

	if test "x$cc_cv_attribute_visibility" = "xyes"; then
		AC_DEFINE([HAVE_ATTRIBUTE_VISIBILITY], 1, [Define this if the compiler supports the visibility attributes.])
		CFLAGS="$CFLAGS -fvisibility=hidden"
		$1
	else
		true
		$2
	fi
])

dnl
dnl Automatically collect installed files
dnl

epdata=`(cd $srcdir/model/ep && find . -type f -print | egrep -v '(Makefile|\.svn)')`
epdata=`echo $epdata`
AC_SUBST(epdata)

hmmdata=`(cd $srcdir/model/hmm/hub4_cd_continuous_8gau_1s_c_d_dd && find . -type f -print | egrep -v '(Makefile|\.svn)')`
hmmdata=`echo $hmmdata`
AC_SUBST(hmmdata)

tidigitsdata=`(cd $srcdir/model/hmm/tidigits && find . -type f -print | egrep -v '(Makefile|\.svn)')`
tidigitsdata=`echo $tidigitsdata`
AC_SUBST(tidigitsdata)

lmdata=`(cd $srcdir/model/lm/an4 && find . -type f -print | egrep -v '(Makefile|\.svn)' | sed 's/\.in//g' | sort | uniq)`
lmdata=`echo $lmdata`
AC_SUBST(lmdata)

docdata=`(cd $srcdir/doc && find . -type f -print | egrep -v '(Makefile|\.svn)')`
docdata=`echo $docdata`
AC_SUBST(docdata)

dnl
dnl Get SphinxBase source from command line if given
dnl
AC_ARG_WITH(sphinxbase,
	AS_HELP_STRING([--with-sphinxbase=DIRECTORY],
			[look for SphinxBase source files in DIRECTORY]),
			sphinxbase=$withval)

dnl
dnl Get SphinxBase build directory from command line if given
dnl
AC_ARG_WITH(sphinxbase-build,
	AS_HELP_STRING([--with-sphinxbase-build=DIRECTORY],
			[look for SphinxBase object files in DIRECTORY]),
	sphinxbasebuild=$withval)

dnl
dnl Check for installed SphinxBase
dnl FIXME: How do we find the installed headers robustly?
dnl
AC_CHECK_HEADER(sphinxbase/sphinx_config.h,
CPPFLAGS="-I/usr/include/sphinxbase -I/usr/local/include/sphinxbase",[
if test x$sphinxbase = x; then
   # Look for sphinxbase in the parent directory
   for sb in ../sphinxbase*; do
       AC_MSG_CHECKING([for sphinxbase in $sb])
       if test -f "$sb/include/sphinxbase/prim_type.h"; then
          sphinxbase="`pwd`/$sb"
	  AC_MSG_RESULT(yes)
       else
	  AC_MSG_RESULT(no)
       fi
   done
fi
])


dnl
dnl Now verify SphinxBase if defined
dnl Sadly, this doesn't work when cross-compiling (for some dumb reason...)
dnl
: ${sphinxbasebuild=$sphinxbase}
if test x$sphinxbase != x && test x$cross_compiling = xyes; then
   CPPFLAGS="-I$sphinxbase/include -I$sphinxbasebuild/include $CPPFLAGS"
   LDFLAGS="$LDFLAGS -L$sphinxbasebuild/src/libsphinxfe -L$sphinxbasebuild/src/libsphinxad \
		     -L$sphinxbasebuild/src/libsphinxbase"
fi
if test x$sphinxbase != x && test x$cross_compiling != xyes; then
   AC_CHECK_FILE($sphinxbase/include/sphinxbase/prim_type.h,CPPFLAGS="-I$sphinxbase/include $CPPFLAGS",
   AC_ERROR(
[SphinxBase headers not found in $sphinxbase.  Please use the
--with-sphinxbase option to `configure' to specify the location of
SphinxBase.  Run $0 --help for more information.]))
fi
if test x$sphinxbasebuild != x && test x$cross_compiling != xyes; then
   AC_CHECK_FILE($sphinxbasebuild/src/libsphinxbase/libsphinxbase.la,
[CPPFLAGS="-I$sphinxbasebuild/include $CPPFLAGS"
LDFLAGS="$LDFLAGS -L$sphinxbasebuild/src/libsphinxfe -L$sphinxbasebuild/src/libsphinxad \
		  -L$sphinxbasebuild/src/libsphinxbase"],
		   AC_ERROR(
[SphinxBase libraries were not found in $sphinxbasebuild.
Use the --with-sphinxbase-build option to `configure' to specify
the build directory for SphinxBase.  Run $0 --help for more information.]))
fi

AC_PATH_PROG(PERL,perl)
AC_PATH_PROG(CSH,csh)
AC_SUBST(sphinxbase)
AC_SUBST(ad_files)
AC_SUBST(ad_libs)
AC_SUBST(ad_backend)
AC_SUBST(PERL)
AC_SUBST(CSH)

AC_OUTPUT([
sphinx3.pc
Makefile
src/Makefile
src/libs3decoder/Makefile
src/libs3decoder/libAPI/Makefile
src/libs3decoder/libam/Makefile
src/libs3decoder/libconfidence/Makefile
src/libs3decoder/libcommon/Makefile
src/libs3decoder/libdict/Makefile
src/libs3decoder/libep/Makefile
src/libs3decoder/liblm/Makefile
src/libs3decoder/libsearch/Makefile
src/libs3decoder/libcfg/Makefile
src/programs/Makefile
src/tests/Makefile
src/tests/performance/Makefile
src/tests/performance/an4/Makefile
src/tests/performance/hub4/Makefile
src/tests/performance/rm1/Makefile
src/tests/performance/wsj20k/Makefile
src/tests/performance/wsj5k/Makefile
src/tests/performance/tidigits/Makefile
src/tests/performance/ti46/Makefile
src/tests/performance/Communicator/Makefile
src/tests/performance/ICSI/Makefile
src/tests/performance/spoke3/Makefile
src/tests/regression/Makefile
src/tests/regression/checkStyle.sh
src/tests/regression/testfuncs.sh
src/tests/unit_tests/Makefile
src/tests/unit_tests/test_logs3/Makefile
src/tests/unit_tests/test_hmm/Makefile
scripts/Makefile
scripts/sphinx3-simple
model/Makefile
model/ep/Makefile
model/hmm/Makefile
model/hmm/hub4_cd_continuous_8gau_1s_c_d_dd/Makefile
model/hmm/tidigits/Makefile
model/hmm/RM1_cd_semi/Makefile
model/lm/Makefile
model/lm/an4/Makefile
model/lm/an4/args.an4
model/lm/an4/args.an4.test
model/lm/an4/args.an4.test.cls
model/lm/an4/args.an4.test.mllr
model/lm/an4/args.an4.test.fsg
model/lm/an4/an4.ug.cls.lmctl
include/Makefile
doc/Makefile])

chmod +x scripts/sphinx3-simple
chmod +x src/tests/regression/*.sh
