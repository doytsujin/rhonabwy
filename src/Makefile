#
# Rhonabwy Library
#
# Makefile used to build the software
#
# Copyright 2020 Nicolas Mora <mail@babelouest.org>
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU Lesser General Public License
# as published by the Free Software Foundation;
# version 2.1 of the License.
#
# This library is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.	See the
# GNU GENERAL PUBLIC LICENSE for more details.
#
# You should have received a copy of the GNU General Public
# License along with this library.	If not, see <http://www.gnu.org/licenses/>.
#
PROJECT_NAME=rhonabwy
PROJECT_DESCRIPTION=OAuth2 client library
PROJECT_BUGREPORT_PATH=https://github.com/babelouest/rhonabwy/issues
PKGCONF_REQ=
PKGCONF_REQ_PRIVATE=
RHONABWY_INCLUDE=../include
PKGCONFIG_FILE=../librhonabwy.pc
PKGCONFIG_TEMPLATE=../librhonabwy.pc.in
DESTDIR=/usr/local
CC=gcc
CFLAGS+=-c -pedantic -std=gnu99 -fPIC -Wall -Werror -Wextra -D_REENTRANT -I$(RHONABWY_INCLUDE) $(ADDITIONALFLAGS) $(CPPFLAGS)
LIBS=-L$(DESTDIR)/lib -lc -lorcania -lyder -ljansson -lgnutls $(LDFLAGS)
SONAME=-soname
OBJECTS=rhonabwy.o
OUTPUT=librhonabwy.so
VERSION=0.3.0

.PHONY: all clean

all: release

$(PKGCONFIG_FILE):
	@cp $(PKGCONFIG_TEMPLATE) $(PKGCONFIG_FILE)
	@echo Pkgconfig file $(PKGCONFIG_FILE) generated
	@sed -i -e 's#@CMAKE_INSTALL_PREFIX@#$(DESTDIR)#g' $(PKGCONFIG_FILE)
	@sed -i -e 's/@CMAKE_INSTALL_LIBDIR@/lib/g' $(PKGCONFIG_FILE)
	@sed -i -e 's/@CMAKE_INSTALL_INCLUDEDIR@/include/g' $(PKGCONFIG_FILE)
	@sed -i -e 's/@PROJECT_NAME@/$(PROJECT_NAME)/g' $(PKGCONFIG_FILE)
	@sed -i -e 's/@PROJECT_DESCRIPTION@/$(PROJECT_DESCRIPTION)/g' $(PKGCONFIG_FILE)
	@sed -i -e 's|@PROJECT_BUGREPORT_PATH@|$(PROJECT_BUGREPORT_PATH)|g' $(PKGCONFIG_FILE)
	@sed -i -e 's/@LIBRARY_VERSION@/$(VERSION)/g' $(PKGCONFIG_FILE)
	@sed -i -e 's/@PKGCONF_REQ@/$(PKGCONF_REQ)/g' $(PKGCONFIG_FILE)
	@sed -i -e 's/@PKGCONF_REQ_PRIVATE@/$(PKGCONF_REQ_PRIVATE)/g' $(PKGCONFIG_FILE)

target: $(OBJECTS)

%.o: %.c $(CONFIG_FILE) $(RHONABWY_INCLUDE)/rhonabwy.h
	$(CC) $(CFLAGS) $<

librhonabwy.so: $(OBJECTS)
	$(CC) -shared -fPIC -Wl,$(SONAME),$(OUTPUT) -o $(OUTPUT).$(VERSION) $(OBJECTS) $(LIBS) $(LYDER) $(LJANSSON) $(LCURL) $(LGNUTLS)
	ln -sf $(OUTPUT).$(VERSION) $(OUTPUT)

librhonabwy.a: $(OBJECTS)
	ar rcs librhonabwy.a $(OBJECTS)

clean:
	rm -f *.o *.so *.a $(OUTPUT) $(OUTPUT).* $(CONFIG_FILE) $(PKGCONFIG_FILE)

install: all $(PKGCONFIG_FILE)
	install $(OUTPUT).$(VERSION) $(DESTDIR)/lib
	install -m644 $(PKGCONFIG_FILE) $(DESTDIR)/lib/pkgconfig
	install -m644 $(RHONABWY_INCLUDE)/rhonabwy.h $(DESTDIR)/include
	install -m644 $(CONFIG_FILE) $(DESTDIR)/include
	-ldconfig $(DESTDIR)/lib

uninstall:
	rm -f $(DESTDIR)/lib/$(OUTPUT) $(DESTDIR)/lib/librhonabwy.a
	rm -f $(DESTDIR)/lib/$(OUTPUT).*
	rm -f $(DESTDIR)/include/rhonabwy.h
	rm -f $(DESTDIR)/lib/pkgconfig/$(PKGCONFIG_FILE)

debug: ADDITIONALFLAGS=-DDEBUG -g -O0

debug: librhonabwy.so

release: ADDITIONALFLAGS=-O3

release: librhonabwy.so