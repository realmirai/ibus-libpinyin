# vim:set noet ts=4:
#
# ibus-libpinyin - Intelligent Pinyin engine based on libpinyin for IBus
#
# Copyright (c) 2008-2010 Peng Huang <shawn.p.huang@gmail.com>
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2, or (at your option)
# any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA.


libexec_PROGRAMS = ibus-engine-libpinyin
ibus_engine_libpinyin_built_c_sources = \
	$(NULL)
ibus_engine_libpinyin_built_h_sources = \
	PYPunctTable.h \
	PYSimpTradConverterTable.h \
	$(NULL)
ibus_engine_libpinyin_c_sources = \
	PYConfig.cc \
	PYEditor.cc \
	PYEngine.cc \
	PYFallbackEditor.cc \
	PYHalfFullConverter.cc \
	PYMain.cc \
	PYPinyinProperties.cc \
	PYPunctEditor.cc \
	PYSimpTradConverter.cc \
	$(NULL)
ibus_engine_libpinyin_h_sources = \
	PYBus.h \
	PYConfig.h \
	PYEditor.h \
	PYEngine.h \
	PYExtEditor.h \
	PYFallbackEditor.h \
	PYHalfFullConverter.h \
	PYLookupTable.h \
	PYObject.h \
	PYPinyinProperties.h \
	PYPointer.h \
	PYProperty.h \
	PYPunctEditor.h \
	PYRawEditor.h \
	PYSignal.h \
	PYSimpTradConverter.h \
	PYString.h \
	PYText.h \
	PYTypes.h \
	PYUtil.h \
	PYStrokeEditor.h \
	PYEnglishEditor.h \
	PYLibPinyin.h \
	PYPPhoneticEditor.h \
	PYPPinyinEditor.h \
	PYPFullPinyinEditor.h \
	PYPDoublePinyinEditor.h \
	PYPBopomofoEditor.h \
	PYPPinyinEngine.h \
	PYPBopomofoEngine.h \
	PYPConfig.h \
	$(NULL)

ibus_engine_libpinyin_c_sources += \
	PYPConfig.cc \
	PYLibPinyin.cc \
	PYPPhoneticEditor.cc \
	PYPPinyinEditor.cc \
	PYPFullPinyinEditor.cc \
	PYPDoublePinyinEditor.cc \
	PYPBopomofoEditor.cc \
	PYPPinyinEngine.cc \
	PYPBopomofoEngine.cc \
	$(NULL)


if IBUS_BUILD_LUA_EXTENSION
ibus_engine_libpinyin_c_sources += PYExtEditor.cc
endif

if IBUS_BUILD_STROKE_INPUT_MODE
ibus_engine_libpinyin_c_sources += PYStrokeEditor.cc
endif

if IBUS_BUILD_ENGLISH_INPUT_MODE
ibus_engine_libpinyin_c_sources += PYEnglishEditor.cc
endif

ibus_engine_libpinyin_SOURCES = \
	$(ibus_engine_libpinyin_c_sources) \
	$(ibus_engine_libpinyin_h_sources) \
	$(ibus_engine_libpinyin_built_c_sources) \
	$(ibus_engine_libpinyin_built_h_sources) \
	$(NULL)


ibus_engine_libpinyin_CXXFLAGS = \
	@IBUS_CFLAGS@ \
	@SQLITE_CFLAGS@ \
	@LIBPINYIN_CFLAGS@ \
	@OPENCC_CFLAGS@ \
	-DGETTEXT_PACKAGE=\"@GETTEXT_PACKAGE@\" \
	-DLOCALEDIR=\"$(localedir)\" \
	-DPKGDATADIR=\"$(pkgdatadir)\" \
	-DLIBEXECDIR=\"$(libexecdir)\" \
	$(NULL)

if HAVE_BOOST
ibus_engine_libpinyin_CXXFLAGS += \
	@BOOST_CPPFLAGS@ \
	$(NULL)
else
ibus_engine_libpinyin_CXXFLAGS += \
	-std=c++0x \
	$(NULL)
endif

ibus_engine_libpinyin_LDADD = \
	@IBUS_LIBS@ \
	@SQLITE_LIBS@ \
	@LIBPINYIN_LIBS@ \
	@OPENCC_LIBS@ \
	$(NULL)

ibus_engine_libpinyin_CXXFLAGS += \
	-DLIBPINYIN_DATADIR=\"@LIBPINYIN_DATADIR@\/data\" \
	$(NULL)

if IBUS_BUILD_LUA_EXTENSION
ibus_engine_libpinyin_CXXFLAGS +=  \
	@LUA_CFLAGS@ \
	-DIBUS_BUILD_LUA_EXTENSION \
	-I$(top_srcdir)/lua/ \
	$(NULL)
endif

if IBUS_BUILD_LUA_EXTENSION
    ibus_engine_libpinyin_LDADD += \
	@LUA_LIBS@ \
	-L../lua/ \
	-lpylua \
	$(NULL)
endif

if IBUS_BUILD_ENGLISH_INPUT_MODE
   ibus_engine_libpinyin_CXXFLAGS += \
	-DIBUS_BUILD_ENGLISH_INPUT_MODE \
	$(NULL)
endif

if IBUS_BUILD_STROKE_INPUT_MODE
   ibus_engine_libpinyin_CXXFLAGS += \
	-DIBUS_BUILD_STROKE_INPUT_MODE \
	$(NULL)
endif

BUILT_SOURCES = \
	$(ibus_engine_built_c_sources) \
	$(ibus_engine_built_h_sources) \
	$(NULL)

pkgdata_DATA = \
	phrases.txt \
	$(NULL)

component_DATA = \
	libpinyin.xml \
	$(NULL)
componentdir = @datadir@/ibus/component

EXTRA_DIST = \
	libpinyin.xml.in \
	phrases.txt \
	$(NULL)

CLEANFILES = \
	libpinyin.xml \
	ZhConversion.* \
	$(NULL)

PYPunctTable.h:
	$(AM_V_GEN) \
	$(PYTHON) $(top_srcdir)/scripts/genpuncttable.py > $@ || \
		( $(RM) $@; exit 1 )

PYSimpTradConverterTable.h:
	$(AM_V_GEN) \
	$(PYTHON) $(top_srcdir)/scripts/update-simptrad-table.py > $@ || \
		( $(RM) $@; exit 1 )

update-simptrad-table:
	$(RM) ZhConversion.php ZhConversion.py PYSimpTradConverterTable.h
	$(MAKE) ZhConversion.php
	$(MAKE) ZhConversion.py
	$(MAKE) PYSimpTradConverterTable.h

libpinyin.xml: libpinyin.xml.in
	$(AM_V_GEN) \
	( \
		libexecdir=${libexecdir}; \
		pkgdatadir=${pkgdatadir}; \
		s=`cat $<`; \
		eval "echo \"$${s}\""; \
	) > $@

test: ibus-engine-libpinyin
	$(ENV) \
		G_DEBUG=fatal_criticals \
		$(builddir)/ibus-engine-libpinyin

# test: ibus-engine-pinyin
# 	$(ENV) G_DEBUG=fatal_warnings \
# 	$(builddir)/ibus-engine-pinyin
ZhConversion.php:
	$(AM_V_GEN) \
	svn cat http://svn.wikimedia.org/svnroot/mediawiki/trunk/phase3/includes/ZhConversion.php > $@ || \
		( $(RM) $@; exit 1 )

ZhConversion.py: ZhConversion.php
	$(AM_V_GEN) \
	php $(top_srcdir)/scripts/php_2_py.php > $@ || \
		( $(RM) $@; exit 1 )

