# Used by OpenGL vendor-neutral dispatch library

MAPI_PREFIX = src/GLdispatch/vnd-glapi

include $(top_srcdir)/src/GLdispatch/vnd-glapi/entry_files.mk

if HAVE_PYTHON
glapi_gen_gl_xml := \
	$(top_srcdir)/src/generate/xml/gl.xml \
	$(top_srcdir)/src/generate/xml/gl_other.xml
glapi_gen_mapi_script = $(top_srcdir)/src/generate/gen_gldispatch_mapi.py
glapi_gen_mapi_deps = \
	$(glapi_gen_mapi_script) \
	$(top_srcdir)/src/generate/genCommon.py \
	$(glapi_gen_gl_xml)
glapi_gen_mapi = $(AM_V_GEN)$(PYTHON) $(PYTHON_FLAGS) $(glapi_gen_mapi_script)
endif

BUILT_SOURCES =

noinst_HEADERS = \
	glapi.h \
	entry_common.h \
	entry.h \
	mapi_tmp.h \
	stub.h \
	table.h \
	u_compiler.h \
	u_current.h \
	u_macros.h

noinst_LTLIBRARIES = libglapi.la

libglapi_la_SOURCES = \
	$(MAPI_GLDISPATCH_ENTRY_FILES) \
	mapi_glapi.c \
	stub.c \
	table.c

# Select the appropriate file for looking up the current dispatch table.
if GLDISPATCH_USE_TLS
libglapi_la_SOURCES += u_current_tls.c
else
libglapi_la_SOURCES += u_current_tsd.c
endif

libglapi_la_LDFLAGS = -no-undefined
libglapi_la_LIBADD = $(top_builddir)/src/util/libutils_misc.la

noinst_HEADERS += glapi_mapi_tmp.h
if HAVE_PYTHON
BUILT_SOURCES += glapi_mapi_tmp.h
glapi_mapi_tmp.h : $(glapi_gen_mapi_deps)
	$(glapi_gen_mapi) gldispatch $(glapi_gen_gl_xml) > $@
endif

COMMON_CPPFLAGS = \
	$(DEFINES) \
	-I$(top_srcdir)/include \
	-I$(top_srcdir)/src/util \
	-I$(top_srcdir)/$(MAPI_PREFIX) \
	-I$(top_builddir)/$(MAPI_PREFIX) \
	-I$(top_srcdir)/$(MAPI_MESA_PREFIX)

libglapi_la_CPPFLAGS = \
	$(COMMON_CPPFLAGS) \
	-DMAPI_ABI_HEADER=\"$(builddir)/glapi_mapi_tmp.h\"


ENTRYPOINT_SOURCES = \
	$(MAPI_GLDISPATCH_ENTRY_FILES) \
	stub.c

ENTRYPOINT_CPPFLAGS = \
	$(COMMON_CPPFLAGS) \
	-DSTATIC_DISPATCH_ONLY

noinst_LTLIBRARIES += libglapi_gl.la
libglapi_gl_la_SOURCES = $(ENTRYPOINT_SOURCES)
libglapi_gl_la_CPPFLAGS = $(ENTRYPOINT_CPPFLAGS) \
	-DMAPI_ABI_HEADER=\"$(builddir)/g_glapi_mapi_gl_tmp.h\"
noinst_HEADERS += g_glapi_mapi_gl_tmp.h
if HAVE_PYTHON
BUILT_SOURCES += g_glapi_mapi_gl_tmp.h
g_glapi_mapi_gl_tmp.h : $(glapi_gen_mapi_deps) $(top_srcdir)/src/GL/gl.symbols
	$(glapi_gen_mapi) $(top_srcdir)/src/GL/gl.symbols $(glapi_gen_gl_xml) > $@
endif

noinst_LTLIBRARIES += libglapi_opengl.la
libglapi_opengl_la_SOURCES = $(ENTRYPOINT_SOURCES)
libglapi_opengl_la_CPPFLAGS = $(ENTRYPOINT_CPPFLAGS) \
	-DMAPI_ABI_HEADER=\"$(builddir)/g_glapi_mapi_opengl_tmp.h\"
noinst_HEADERS += g_glapi_mapi_opengl_tmp.h
if HAVE_PYTHON
BUILT_SOURCES += g_glapi_mapi_opengl_tmp.h
g_glapi_mapi_opengl_tmp.h : $(glapi_gen_mapi_deps)
	$(glapi_gen_mapi) opengl $(glapi_gen_gl_xml) > $@
endif

noinst_LTLIBRARIES += libglapi_glesv1.la
libglapi_glesv1_la_SOURCES = $(ENTRYPOINT_SOURCES)
libglapi_glesv1_la_CPPFLAGS = $(ENTRYPOINT_CPPFLAGS) \
	-DMAPI_ABI_HEADER=\"$(builddir)/g_glapi_mapi_glesv1_tmp.h\"
noinst_HEADERS += g_glapi_mapi_glesv1_tmp.h
if HAVE_PYTHON
BUILT_SOURCES += g_glapi_mapi_glesv1_tmp.h
g_glapi_mapi_glesv1_tmp.h : $(glapi_gen_mapi_deps)
	$(glapi_gen_mapi) glesv1 $(glapi_gen_gl_xml) > $@
endif

noinst_LTLIBRARIES += libglapi_glesv2.la
libglapi_glesv2_la_SOURCES = $(ENTRYPOINT_SOURCES)
libglapi_glesv2_la_CPPFLAGS = $(ENTRYPOINT_CPPFLAGS) \
	-DMAPI_ABI_HEADER=\"$(builddir)/g_glapi_mapi_glesv2_tmp.h\"
noinst_HEADERS += g_glapi_mapi_glesv2_tmp.h
if HAVE_PYTHON
BUILT_SOURCES += g_glapi_mapi_glesv2_tmp.h
g_glapi_mapi_glesv2_tmp.h : $(glapi_gen_mapi_deps)
	$(glapi_gen_mapi) glesv2 $(glapi_gen_gl_xml) > $@
endif

CLEANFILES = $(BUILT_SOURCES)
EXTRA_DIST = meson.build
