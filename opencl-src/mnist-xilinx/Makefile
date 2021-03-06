# Copyright (C) 2013-2016 Altera Corporation, San Jose, California, USA. All rights reserved.
# Permission is hereby granted, free of charge, to any person obtaining a copy of this
# software and associated documentation files (the "Software"), to deal in the Software
# without restriction, including without limitation the rights to use, copy, modify, merge,
# publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to
# whom the Software is furnished to do so, subject to the following conditions:
# The above copyright notice and this permission notice shall be included in all copies or
# substantial portions of the Software.
# 
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
# EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
# OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
# NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
# HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
# WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
# FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
# OTHER DEALINGS IN THE SOFTWARE.
# 
# This agreement shall be governed in all respects by the laws of the State of California and
# by the laws of the United States of America.
# This is a GNU Makefile.

# You must configure ALTERAOCLSDKROOT to point the root directory of the Altera SDK for OpenCL
# software installation.
# See http://www.altera.com/literature/hb/opencl-sdk/aocl_getting_started.pdf 
# for more information on installing and configuring the Altera SDK for OpenCL.
VERBOSE:=1
ifeq ($(VERBOSE),1)
ECHO := 
else
ECHO := @
endif

# Where is the Xilinx SDAccel SDK for OpenCL software?
ifeq ($(wildcard $(XILINX_SDACCEL)),)
$(error Set XILINX_SDACCEL to the root directory of the Xilinx SDAccel install for OpenCL software installation)
endif

# OpenCL compile and link flags.
XCL_COMPILE_CONFIG := -I$(XILINX_SDACCEL)/runtime/include/1_2
XCL_LINK_CONFIG := -L$(XILINX_SDACCEL)/runtime/lib/x86_64 -lxilinxopencl 

# Compilation flags
ifeq ($(DEBUG),1)
CXXFLAGS += -g
else
CXXFLAGS += -O2
endif

# Compiler
CXX := g++

# Target
TARGET := host
TARGET_DIR := bin

# Directories
INC_DIRS := host/inc ../common/inc
LIB_DIRS := 

# Files
INCS := $(wildcard host/inc/*.h)
SRCS := $(wildcard host/src/*.cpp ../common/src/AOCLUtils/*.cpp)
LIBS := rt

# Make it all!
all : $(TARGET_DIR)/$(TARGET)

# Host executable target.
$(TARGET_DIR)/$(TARGET) : Makefile $(SRCS) $(INCS) $(TARGET_DIR)
	$(ECHO)$(CXX) $(CPPFLAGS) $(CXXFLAGS) -fPIC $(foreach D,$(INC_DIRS),-I$D) \
			$(XCL_COMPILE_CONFIG) $(SRCS) $(XCL_LINK_CONFIG) \
			$(foreach D,$(LIB_DIRS),-L$D) \
			$(foreach L,$(LIBS),-l$L) \
			-o $(TARGET_DIR)/$(TARGET)

$(TARGET_DIR) :
	$(ECHO)mkdir $(TARGET_DIR)

emu: $(TARGET_DIR)/$(TARGET)
	XCL_EMULATION_MODE=1 $(TARGET_DIR)/$(TARGET) -img=./mnist_test_img_0.pgm
run: $(TARGET_DIR)/$(TARGET)
	$(TARGET_DIR)/$(TARGET) -img=./mnist_test_img_0.pgm

# Standard make targets
clean :
	$(ECHO)rm -f $(TARGET_DIR)/$(TARGET)

print :
	@echo "Sources : $(SRCS)"
	@echo "Includes : $(INC_DIRS)"
.PHONY : all clean
