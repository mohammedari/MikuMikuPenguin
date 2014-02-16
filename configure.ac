#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.64])
AC_INIT([MikuMikuPenguin], [0.2], [ibenrunnin@gmail.com])
AM_INIT_AUTOMAKE([foreign])
LT_PREREQ([2.2])
LT_INIT([dlopen])
AC_CONFIG_MACRO_DIR([m4])


case "$host" in
*-*-mingw*|*-*-cygwin*)
AC_DEFINE(PLATFORM_WIN32, 1, [Platform is Win32])
bullet_CXXFLAGS="-I/usr/include/bullet"
opengl_LIBS="-lglfw -lGLU -lGLEW -lGL"
PLATFORM_STRING="Win32"
;;
*-*-linux*)
AC_DEFINE(PLATFORM_LINUX, 1, [Platform is Linux])
bullet_CXXFLAGS="-I/usr/include/bullet"
opengl_LIBS="-lglfw -lGLU -lGLEW -lGL"
PLATFORM_STRING="Linux"
;;
*-*-darwin*)
AC_MSG_WARN([Hello])
AC_DEFINE(PLATFORM_APPLE, 1, [Platform is Apple])
bullet_CXXFLAGS="-stdlib=libc++ -I/usr/local/include/bullet"
opengl_LIBS="-framework AGL -framework OpenGL -lglfw -lglew"
PLATFORM_STRING="Apple"
#AC_SUBST(CXXFLAGS, "-stdlib=libc++ -I/usr/local/include/bullet")
#AC_SUBST(LDFLAGS, "-L/System/Library/Frameworks/OpenGL.framework/Libraries")
;;
*)
AC_MSG_WARN([*** Please add $host to configure.ac checks!])
;;
esac
AC_SUBST(bullet_CXXFLAGS)
AC_SUBST(opengl_LIBS)


#--enable-sound flag/optional SDL/SDL_mixer check
AC_ARG_ENABLE([sound],
  [AS_HELP_STRING([--disable-sound], [Enable Sound support @<:@check@:>@])],
  [:],
  [enable_sound=check])
AS_IF([test "$enable_sound" != "no"],
  [PKG_CHECK_MODULES([SDL2MIXER],
    [SDL2_mixer],
    [enable_sound=yes],
    [AS_IF([test "$enable_sound" = "yes"],
      [AC_MSG_ERROR([SDL2_mixer required, but not found.])],
      [enable_sound=no])])])
AM_CONDITIONAL([ENABLE_SOUND], [test "$enable_sound" = "yes"])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CXXCPP
AC_PROG_CC
AC_LANG([C++])

# Checks for libraries.
AS_IF([test "$PLATFORM_STRING" != "Apple"],
AC_CHECK_LIB(GL, glBegin,
			[AC_DEFINE(HAVE_GL)],
 			[AC_MSG_ERROR(You must have libgl installed)]))

AC_CHECK_LIB(glfw, glfwInit,
	[AC_DEFINE(HAVE_LIBGLFW)],
	[AC_MSG_ERROR(You must have libglfw installed.)])
	
AC_CHECK_LIB(SOIL, SOIL_load_image,
			[AC_DEFINE(HAVE_SOIL)],
			[AC_MSG_ERROR(You must have libSOIL installed)])
	
AC_CHECK_LIB(BulletDynamics, plCreateDynamicsWorld,
			[AC_DEFINE(HAVE_BULLETDYNAMICS)],
			[AC_MSG_ERROR(You must have BulletDynamics installed)])

# Checks for header files.
AC_CHECK_HEADERS([stdlib.h string.h])

AC_CHECK_HEADERS([OpenGL/glew.h GL/glew.h],
	[HAVE_GLEW=yes; break])
AS_IF([test "x$HAVE_GLEW" != "xyes"],
	[AC_MSG_ERROR([You must have libglew installed])])

AC_CHECK_HEADERS([OpenGL/glu.h GL/glu.h],[HAVE_GLU=yes; break])
AS_IF([test "x$HAVE_GLU" != "xyes"],
	[AC_MSG_ERROR([You must have libglu installed])])

AC_CHECK_HEADERS([glm/gtc/matrix_transform.hpp],
			[AC_DEFINE(HAVE_LIBGLM)],
			[AC_MSG_ERROR(You must have libglm installed.)])

#Check for library versions
#PKG_CHECK_MODULES([LIBGLU], [glu >= 9.0.0])
#PKG_CHECK_MODULES([LIBGLEW], [glew >= 1.8.0])

#PKG_CHECK_MODULES([LIBGLFW], [libglfw >= 2.7])
#PKG_CHECK_MODULES([LIBGLFW], [libglfw < 3.0])

#PKG_CHECK_MODULES([LIBBULLET], [bullet >= 2.81])


# Checks for typedefs, structures, and compiler characteristics.
#AC_CHECK_HEADER_STDBOOL
AC_TYPE_INT16_T
AC_TYPE_INT8_T
AC_TYPE_SIZE_T
AC_TYPE_UINT32_T
AC_TYPE_UINT8_T

# Checks for library functions.
AC_FUNC_MALLOC
AC_CHECK_FUNCS([memset])

# files to generate via autotools (.am or .in source files)
AC_CONFIG_FILES([Makefile])
AC_CONFIG_FILES([src/Makefile])
AC_CONFIG_FILES([shaders/Makefile])
AC_CONFIG_FILES([data/share/Makefile])
AC_CONFIG_FILES([include/Makefile])
AC_CONFIG_FILES([libmmp/Makefile])
AC_CONFIG_FILES([doc/Makefile])
AC_CONFIG_FILES([man/Makefile])
AC_CONFIG_FILES([scripts/Makefile])
AC_CONFIG_FILES([test/Makefile])

AC_OUTPUT
