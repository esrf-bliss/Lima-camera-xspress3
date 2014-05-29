############################################################################
# This file is part of LImA, a Library for Image Acquisition
#
# Copyright (C) : 2009-2013
# European Synchrotron Radiation Facility
# BP 220, Grenoble 38043
# FRANCE
#
# This is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3 of the License, or
# (at your option) any later version.
#
# This software is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, see <http://www.gnu.org/licenses/>.
############################################################################
include ../../../config.inc

SRCS = Xspress3Test.cpp hdftest.cpp


LDFLAGS = -pthread -L../../../build  -L../../../third-party/Processlib/build 
LDLIBS = -llimaxspress3 -llimacore  -lprocesslib 

CXXFLAGS = -I../include \
	       -I../../../hardware/include \
	       -I../../../common/include \
           -I../../../control/include \
           -I../../../control/software_operation/include \
	       -I../../../third-party/Processlib/core/include \
	       -I../../../third-party/Processlib/tasks/include \
	       -I../../../third-party/Sps/Include \
	       -I../sdk/include \
	       -I../../../third-party/hdf5-1.8.12/include \
	        -Wall -pthread -g

ifneq ($(COMPILE_CONFIG),0)
CXXFLAGS += -DWITH_CONFIG -I../../third-party/libconfig/lib
LDLIBS += -L../../../third-party/libconfig/lib/.libs -lconfig
endif

ifneq ($(COMPILE_HDF5_SAVING),0)
PROCESSOR_TYPE := $(shell uname -m)
ifeq ($(PROCESSOR_TYPE),x86_64)
LDLIBS += -L../../../third-party/hdf5-1.8.12/libs/linux.x86_64 -lhdf5_cpp -lhdf5 -lsz
XSPRESS3_LDLIBS	:= -L../sdk/libs/linux.x86_64 -lxspress3 -limg_mod
else
LDLIBS += -L../../../third-party/hdf5-1.8.12/libs/linux.x86 -lhdf5_cpp -lhdf5 -lsz
XSPRESS3_LDLIBS	:= -L../sdk/libs/linux.x86 -lxspress3 -limg_mod
endif
endif

ifneq ($(COMPILE_SPS),0)
CXXFLAGS += -DWITH_SPS_IMAGE -I../../third-party/libconfig/lib
LDLIBS += -L../../../third-party/sps/lib/.libs -lconfig
endif

test-progs = xspress3test hdf5

all: 	$(test-progs)

xspress3test:	Xspress3Test.o ../src/Xspress3.o
	$(CXX) $(LDFLAGS) -o $@ $+ $(XSPRESS3_LDLIBS) $(LDLIBS)

hdf5:	hdftest.o
	$(CXX) $(LDFLAGS) -o $@ $+  $(LDLIBS)

clean:
	rm -f *.o *.P Xspress3Test hdf5test

%.o : %.cpp
	$(COMPILE.cpp) -MD $(CXXFLAGS) -o $@ $<
	@cp $*.d $*.P; \
	sed -e 's/#.*//' -e 's/^[^:]*: *//' -e 's/ *\\$$//' \
	-e '/^$$/ d' -e 's/$$/ :/' < $*.d >> $*.P; \
	rm -f $*.d

-include $(SRCS:.cpp=.P)
