CC=$(TARGET_CC)
OBJ=arm-linux-obj
OBJNAME?=libjfaudiolib.a
PRETTY_OUTPUT=0
EROOT?=build
RELEASE?=0
OPTLEVEL=2
SRC=src

#DXROOT ?= c:/sdks/directx/dx8

include ../../$(EROOT)/Makefile.shared

ifneq (0,$(RELEASE))
  # Debugging disabled
  debug=-fomit-frame-pointer -funswitch-loops -O$(OPTLEVEL)
else
  # Debugging enabled
  debug=-ggdb -O0 -DDEBUGGINGAIDS
endif

ifneq (0,$(DEBUGANYWAY))
  debug+=-ggdb
endif

OURCFLAGS	+= -I$(STAGING_DIR)/usr/include -I$(STAGING_DIR)/usr/include/SDL
LIBS		+= -L$(STAGING_DIR)/usr/lib -L$(STAGING_DIR)/usr/lib/SDL -lSDL -lSDL_mixer -lvorbisfile -lvorbis -logg

CFLAGS=$(debug) -W -Wall -Wimplicit -Werror-implicit-function-declaration \
    -funsigned-char -fno-strict-aliasing -DNO_GCC_BUILTINS -D_FORTIFY_SOURCE=2 \
    $(F_JUMP_TABLES) $(F_NO_STACK_PROTECTOR) $(OURCFLAGS)

CPPFLAGS=-Iinclude -Isrc -DHAVE_VORBIS

OBJECTS=$(OBJ)/drivers.o \
	$(OBJ)/fx_man.o \
	$(OBJ)/multivoc.o \
	$(OBJ)/mix.o \
	$(OBJ)/mixst.o \
	$(OBJ)/pitch.o \
	$(OBJ)/vorbis.o \
	$(OBJ)/driver_nosound.o

ifeq ($(PLATFORM),WINDOWS)
 CPPFLAGS+= -I$(DXROOT)/include -Ithird-party/mingw32/include
 OBJECTS+= $(OBJ)/driver_directsound.o
 OBJNAME=libjfaudiolib_win32.a
 OBJ=obj_win
else
 CPPFLAGS+= -DHAVE_SDL
 OBJECTS+= $(OBJ)/driver_sdl.o
endif

# OBJECTS=$(SOURCES:%.c=$(OBJ)/%.o)

$(OBJNAME): $(OBJECTS)
	$(AR) cr $@ $^

$(OBJECTS): $(OBJ)/%.o: $(SRC)/%.c
	-mkdir -p $(OBJ)
	$(COMPILE_STATUS)
	if $(CC) $(CPPFLAGS) $(CFLAGS) $(OURCFLAGS) -c $< -o $@ $(LIBS); then $(COMPILE_OK); else $(COMPILE_FAILED); fi

ifeq ($(PRETTY_OUTPUT),1)
.SILENT:
endif
.PHONY: clean
clean:
	-rm -f $(OBJECTS) $(OBJNAME)
