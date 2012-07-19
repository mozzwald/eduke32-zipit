CC=$(TARGET_CC)
OBJ=arm-linux-obj
OBJNAME?=libenet.a
PRETTY_OUTPUT=0
EROOT?=build
RELEASE?=0
OPTLEVEL=2
SRC=src
INC=include

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

CPPFLAGS=-I$(INC) -I$(SRC)

OBJECTS=$(OBJ)/callbacks.o \
	$(OBJ)/host.o \
	$(OBJ)/list.o \
	$(OBJ)/packet.o \
	$(OBJ)/peer.o \
	$(OBJ)/protocol.o \
	$(OBJ)/compress.o

ifeq ($(PLATFORM),WINDOWS)
 OBJECTS+= $(OBJ)/win32.o
 OBJNAME=libenet_win32.a
 OBJ=obj_win
else
 OBJECTS+= $(OBJ)/unix.o
endif

# OBJECTS=$(SOURCES:%.c=$(OBJ)/%.o)

$(OBJNAME): $(OBJECTS)
	$(AR) cr $@ $^

$(OBJECTS): $(OBJ)/%.o: $(SRC)/%.c $(INC)/enet/*.h
	-mkdir -p $(OBJ)
	$(COMPILE_STATUS)
	if $(CC) $(CPPFLAGS) $(CFLAGS) -c $< -o $@ $(LIBS); then $(COMPILE_OK); else $(COMPILE_FAILED); fi

ifeq ($(PRETTY_OUTPUT),1)
.SILENT:
endif
.PHONY: clean
clean:
	-rm -f $(OBJECTS) $(OBJNAME)
