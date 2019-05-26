# Makefile to build the test-wrapper for Arapaho
# Undefine GPU, CUDNN if darknet was built without these defined. These 2 flags have to match darknet flags.
# https://github.com/prabindh/darknet

GPU=0
CUDNN=0
DEBUG=0
OPENCV=1

CC_CPP=g++
CFLAGS_CPP=-Wno-write-strings -std=c++11

OPTS=-Ofast
LDFLAGS= -L./ -ldarknet-cpp-shared
LDFLAGS+= -Wl,-rpath,'$$ORIGIN'
COMMON= -I../src/ -I../include
ifeq ($(OPENCV),1)
COMMON+="-DOPENCV"
LDFLAGS+= `pkg-config --libs opencv4`
COMMON+= `pkg-config --cflags opencv4`
endif
CFLAGS=-Wall -Wfatal-errors

ifeq ($(DEBUG), 1)
OPTS=-O0 -g
COMMON+= -D_DEBUG
CFLAGS+= -D_DEBUG
endif

CFLAGS+=$(OPTS)

ifeq ($(GPU), 1)
COMMON+= -DGPU -I/usr/local/cuda/include/
CFLAGS+= -DGPU
LDFLAGS+= -L/usr/local/cuda/lib64 -lcuda -lcudart -lcublas -lcurand
endif

ifeq ($(CUDNN), 1)
COMMON+= -DCUDNN
CFLAGS+= -DCUDNN
LDFLAGS+= -lcudnn
endif

EXEC_CPP=arapaho
OBJDIR_CPP=./obj-cpp/
OBJ=test.o arapaho.o
OBJS= $(addprefix $(OBJDIR), $(OBJ))
DEPS= $(wildcard *.hpp) Makefile

OBJS_CPP = $(addprefix $(OBJDIR_CPP), $(OBJ))

all: obj-cpp $(EXEC_CPP)

$(EXEC_CPP): obj-cpp clean $(OBJS_CPP)
	$(CC_CPP) $(COMMON) $(CFLAGS) $(OBJS_CPP) -o $@ $(LDFLAGS)

$(OBJDIR_CPP)%.o: %.cpp $(DEPS)
	$(CC_CPP) $(COMMON) $(CFLAGS_CPP) $(CFLAGS) -c $< -o $@


obj-cpp:
	mkdir -p $(OBJDIR_CPP)

.PHONY: clean

clean:
	rm -rf $(OBJS_CPP) $(EXEC_CPP)
