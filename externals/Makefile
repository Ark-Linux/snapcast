#    This file is part of snapcast
#   Copyright (C) 2014-2020  Johannes Pohl
#
#   This program is free software: you can redistribute it and/or modify
#   it under the terms of the GNU General Public License as published by
#   the Free Software Foundation, either version 3 of the License, or
#   (at your option) any later version.
#
#   This program is distributed in the hope that it will be useful,
#   but WITHOUT ANY WARRANTY; without even the implied warranty of
#   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#   GNU General Public License for more details.
#
#   You should have received a copy of the GNU General Public License
#   along with this program.  If not, see <http://www.gnu.org/licenses/>.

.PHONY: all check-env flac ogg opus tremor oboe soxr

all: flac ogg opus tremor oboe soxr

check-env:
#	if [ ! -d "flac" ]; then \
#		$(error flac directory missing); \
#	fi; \
#	if [ ! -d ogg ]; then \
#		$(error ogg directory missing); \
#	fi; \
#	if [ ! -d tremor ]; then \
#		$(error tremor directory missing); \
#	fi
ifndef NDK_DIR
	$(error android NDK_DIR is not set)
endif
ifndef ARCH
	$(error ARCH is not set ("arm" or "aarch64" or "x86"))
endif
ifeq ($(ARCH), x86)
	$(eval CPPFLAGS:=-DLITTLE_ENDIAN=1234 -DBIG_ENDIAN=4321 -DBYTE_ORDER=LITTLE_ENDIAN)
	$(eval PROGRAM_PREFIX:=$(NDK_DIR)/bin/i686-linux-android-)
else ifeq ($(ARCH), arm)
	$(eval CPPFLAGS:=-U_ARM_ASSEM_)
	$(eval PROGRAM_PREFIX:=$(NDK_DIR)/bin/arm-linux-androideabi-)
else ifeq ($(ARCH), aarch64)
	$(eval CPPFLAGS:=-U_ARM_ASSEM_ -DLITTLE_ENDIAN=1234 -DBIG_ENDIAN=4321 -DBYTE_ORDER=LITTLE_ENDIAN)
	$(eval PROGRAM_PREFIX:=$(NDK_DIR)/bin/aarch64-linux-android-)
else
	$(error ARCH must be "arm" or "aarch64" or "x86")
endif
	$(eval CC:=$(PROGRAM_PREFIX)clang)
	$(eval CXX:=$(PROGRAM_PREFIX)clang++)
	$(eval CPPFLAGS:=-I$(NDK_DIR)/include $(CPPFLAGS))

flac: check-env
	@cd flac; \
	export CC="$(CC)"; \
	export CXX="$(CXX)"; \
	export CPPFLAGS="$(CPPFLAGS)"; \
	./autogen.sh; \
	./configure --host=$(ARCH) --disable-ogg --prefix=$(NDK_DIR)/usr/local/; \
	make; \
	make install; \
	make clean;

ogg: check-env
	@cd ogg; \
	export CC="$(CC)"; \
	export CXX="$(CXX)"; \
	export CPPFLAGS="$(CPPFLAGS)"; \
	./autogen.sh; \
	./configure --host=$(ARCH) --prefix=$(NDK_DIR)/usr/local/; \
	make; \
	make install; \
	make clean;

opus: check-env
	@cd opus; \
	export CC="$(CC)"; \
	export CXX="$(CXX)"; \
	export CPPFLAGS="$(CPPFLAGS)"; \
	./autogen.sh; \
	./configure --host=$(ARCH) --prefix=$(NDK_DIR)/usr/local/; \
	make; \
	make install; \
	make clean;

tremor:	check-env
	@cd tremor; \
	export CC="$(CC)"; \
	export CXX="$(CXX)"; \
	export CPPFLAGS="$(CPPFLAGS)"; \
	./autogen.sh; \
	./configure --host=$(ARCH) --prefix=$(NDK_DIR)/usr/local/ --with-ogg=$(NDK_DIR)/usr/local/; \
	make; \
	make install; \
	make clean; \
	rm -rf .deps/; \
	rm Makefile; \
	rm Makefile.in; \
	rm Version_script; \
	rm aclocal.m4; \
	rm -rf autom4te.cache/; \
	rm compile; \
	rm config.guess; \
	rm config.h; \
	rm config.h.in; \
	rm config.log; \
	rm config.status; \
	rm config.sub; \
	rm configure; \
	rm depcomp; \
	rm install-sh; \
	rm libtool; \
	rm ltmain.sh; \
	rm missing; \
	rm stamp-h1; \
	rm vorbisidec.pc;

oboe: check-env
	@cd oboe; \
	export CC="$(CC)"; \
	export CXX="$(CXX)"; \
	export CPPFLAGS="$(CPPFLAGS)"; \
	mkdir build; \
	cd build; \
	cmake ..; \
	make; \
	make DESTDIR=$(NDK_DIR) install; \
	make clean; \
	cd ..; \
	rm -rf build;

soxr: check-env
	@cd /home/johannes/Develop/soxr; \
	export CC="$(CC)"; \
	export CXX="$(CXX)"; \
	export CPPFLAGS="$(CPPFLAGS)"; \
	mkdir build; \
	cd build; \
	cmake -DBUILD_SHARED_LIBS=OFF -DBUILD_TESTS=OFF -DWITH_OPENMP=OFF ..; \
	make; \
	make DESTDIR=$(NDK_DIR) install; \
	make clean; \
	cd ..; \
	rm -rf build;
