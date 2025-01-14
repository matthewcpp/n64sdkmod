#!smake -k
# --------------------------------------------------------------------
#        Copyright (C) 1997,1998 Nintendo. (Originated by SGI)
#
#        $RCSfile: Makefile,v $
#        $Revision: 1.9 $
#        $Date: 1999/04/02 10:10:48 $
# --------------------------------------------------------------------
#
# Makefile for game template
#
#

include $(ROOT)/usr/include/make/PRdefs

#FINAL = YES
ifeq ($(FINAL), YES)
OPTIMIZER       = -O2
LCDEFS          = -D_FINALROM -DNDEBUG -DF3DEX_GBI_2
N64LIB          = -lultra_rom
else
OPTIMIZER       = -g
LCDEFS          = -DDEBUG -DF3DEX_GBI_2
N64LIB          = -lultra_d
endif

APP =		game.out

TARGETS =	game.n64

HFILES =	boot.h game.h controller.h font.h font_ext.h \
		sphere_dl.h sphere_vtx.h static.h texture.h

CODEFILES   =	boot.c game.c controller.c font.c dram_stack.c

CODEOBJECTS =	$(CODEFILES:.c=.o)

DATAFILES   =	gfxinit.c gfxstatic.c texture.c cfb.c rsp_cfb.c zbuffer.c

DATAOBJECTS =	$(DATAFILES:.c=.o)

CODESEGMENT =	codesegment.o

OBJECTS =	$(CODESEGMENT) $(DATAOBJECTS)

LCDEFS +=	$(HW_FLAGS)
LCINCS =	-I.
LCOPTS =	-G 0
LDFLAGS =	$(MKDEPOPT)  -L$(ROOT)/usr/lib $(N64LIB) -L$(N64_LIBGCCDIR) -lgcc  -lnustd

LDIRT  =	$(APP)

default:	$(TARGETS)

include $(COMMONRULES)

gfxstatic.o:	$(TEXHFILES)

$(CODESEGMENT):	$(CODEOBJECTS)
		$(LD) -o $(CODESEGMENT) -r $(CODEOBJECTS) $(LDFLAGS)

ifeq ($(FINAL), YES)
$(TARGETS) $(APP):      spec $(OBJECTS)
	$(MAKEROM) -s 9 -r $(TARGETS) -e $(APP) spec
	makemask $(TARGETS)
else
$(TARGETS) $(APP):      spec $(OBJECTS)
	$(MAKEROM) -r $(TARGETS) -e $(APP) spec
endif

# for exeGCC CELF
ifeq ($(GCC_CELF), ON)
ifneq ($(FINAL), YES)
CELFDIR = .
include $(CELFRULES)
$(CODEOBJECTS) $(DATAOBJECTS) :	$(CELFINFOFILES)
endif
endif
