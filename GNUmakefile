#----------------------------------------------------------------------------
#  Makefile for VapourSynth-DePan - Clean Static Build
#----------------------------------------------------------------------------

include config.mak

vpath %.cpp $(SRCDIR)
vpath %.h $(SRCDIR)

SRCS = DePan/DePan.cpp
OBJS = $(SRCS:%.cpp=%.o)

# Ultra-aggressive static linking
STATIC_FLAGS = -static -static-libgcc -static-libstdc++ -Wl,--whole-archive -lpthread -Wl,--no-whole-archive -Wl,--exclude-libs,ALL

.PHONY: all clean

all: $(LIBNAME)

$(LIBNAME): $(OBJS)
	$(LD) -o $@ $(LDFLAGS) $(STATIC_FLAGS) $^ $(LIBS)
	-@strip -x $@ 2>/dev/null || true

%.o: %.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@

clean:
	$(RM) *.dll *.o .depend

config.mak:
	./configure