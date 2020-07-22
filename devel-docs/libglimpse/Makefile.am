## Process this file with automake to produce Makefile.in

# The name of the module.
DOC_MODULE = libglimpse

# The top-level SGML file.
DOC_MAIN_SGML_FILE = $(DOC_MODULE)-docs.sgml

# The directory containing the source code.
DOC_SOURCE_DIR = $(top_srcdir)/$(DOC_MODULE)

# Extra options to pass to gtkdoc-scangobj
SCANGOBJ_OPTIONS =

# Extra options to supply to gtkdoc-scan
SCAN_OPTIONS = --deprecated-guards="GIMP_DISABLE_DEPRECATED"

# Extra options to supply to gtkdoc-mkdb
MKDB_OPTIONS = --sgml-mode --output-format=xml --name-space=gimp

# Extra options to supply to gtkdoc-fixref
FIXXREF_OPTIONS = --extra-dir=../libglimpsebase/html --extra-dir=libglimpsecolor/html --extra-dir=libglimpseconfig/html --extra-dir=libglimpsemath/html --extra-dir=libglimpsemodule/html --extra-dir=libglimpsethumb/html --extra-dir=libglimpsewidgets/html

# Used for dependencies
HFILE_GLOB = $(DOC_SOURCE_DIR)/*.h
CFILE_GLOB = $(DOC_SOURCE_DIR)/*.c

# Header files to ignore when scanning
IGNORE_HFILES = \
	gimptilebackendplugin.h	\
	gimpunit_pdb.h		\
	gimpunitcache.h		\
	libglimpse-intl.h		\
	stdplugins-intl.h

# Images to copy into HTML directory
HTML_IMAGES=

# Extra SGML files that are included by $(DOC_MAIN_SGML_FILE)
content_files =

# Other files to distribute
extra_files =

# CFLAGS and LDFLAGS for compiling scan program. Only needed
# if $(DOC_MODULE).types is non-empty.
GTKDOC_CFLAGS = \
	@CFLAGS@		\
	-I$(top_srcdir) 	\
	-I$(top_builddir) 	\
	$(GEGL_CFLAGS)		\
	@GTK_CFLAGS@		\
	-UGTK_DISABLE_SINGLE_INCLUDES

GTKDOC_LIBS = \
	$(top_builddir)/libglimpsebase/libglimpsebase-$(GIMP_API_VERSION).la		\
	$(top_builddir)/libglimpsecolor/libglimpsecolor-$(GIMP_API_VERSION).la	\
	$(top_builddir)/libglimpsemath/libglimpsemath-$(GIMP_API_VERSION).la		\
	$(top_builddir)/libglimpseconfig/libglimpseconfig-$(GIMP_API_VERSION).la	\
	$(top_builddir)/libglimpsewidgets/libglimpsewidgets-$(GIMP_API_VERSION).la	\
	$(top_builddir)/libglimpse-$(GIMP_API_VERSION).la			\
	$(top_builddir)/libglimpseui-$(GIMP_API_VERSION).la		\
	@GTK_LIBS@


include $(top_srcdir)/gtk-doc.make

# Other files to distribute
EXTRA_DIST += version.in
