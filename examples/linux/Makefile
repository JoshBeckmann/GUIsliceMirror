##
## Makefile for GUIslice library - LINUX
## - Calvin Hass
## - https://www.impulseadventure.com/elec/guislice-gui.html
## - https://github.com/ImpulseAdventure/GUIslice
##
## NOTE: This Makefile handles both SDL1.2 and SDL2 compilation.
##       A simpler Makefile can be used if this flexibility is
##       not required.

# ---------------------------------------------------------------------------
# Define compiler flags for GUIslice
# - In some cases, the makefile compilation options must match
#   settings in the GUIslice_config_linux.h configuration file.
#
# GRAPHICS DRIVER
# - make parameter: GSLC_DRV=(SDL1,SDL2)
# - On the makefile command-line, pass the GSLC_DRV parameter to select
#   the graphics driver matching the #define DRV_DISP_* setting in
#   GUIslice_config_linux.h, eg:
#     GUIslice_config_linux.h:  #define DRV_DISP_SDL1
#     Makefile:                 make <target> GSLC_DRV=SDL1
#
#
# TOUCH DRIVER
# - make parameter: GSLC_TOUCH=(TSLIB,SDL,NONE)
# - On the makefile command-line, pass the GSLC_TOUCH parameter to
#   indicate whether or not DRV_TOUCH_TSLIB is #defined in the
#   GUIslice_config_linux.h, eg:
#     GUIslice_config_linux.h:  #define DRV_TOUCH_TSLIB
#     Makefile:                 make <target> GSLC_TOUCH=TSLIB
#
#     GUIslice_config_linux.h:  #define DRV_TOUCH_SDL (or DRV_TOUCH_NONE)
#     Makefile:                 make <target> GSLC_TOUCH=NONE
# - Note that the makefile only checks to see if DRV_TOUCH=TSLIB and
#   will ignore any other values.
#
# NOTES:
# - Note that multiple parameters may be passed, for example:
#     make ex04_lnx_ctrls
#     make ex06_lnx_callback GSLC_TOUCH=TSLIB GSLC_DRV=SDL2
#     make all GSLC_TOUCH=TSLIB
#


DEBUG = -O3
CC = gcc
INCLUDE = -I/usr/local/include
CFLAGS = $(DEBUG) -Wall $(INCLUDE) -Winline -pipe -g
LDFLAGS = -L/usr/local/lib

GSLC_CORE = ../../src/GUIslice.c ../../src/GUIslice_config.h ../../src/elem/*.c


# Define default compiler flags which may be overridden by command line

# Define graphics driver (overridden by command line GSLC_DRV)
GSLC_DRV := SDL1
#GSLC_DRV := SDL2

# Define any extra linker settings (overridden by command line GSLC_TOUCH_TSLIB)
#GSLC_LDLIB_EXTRA := -lm
#GSLC_LDLIB_EXTRA := -lm -lts

ifndef GSLC_TOUCH
  GSLC_LDLIB_EXTRA := -lm -lts
  $(info GUIslice touch mode: tslib enabled by default - GUIslice config should use DRV_TOUCH_TSLIB mode)
else
  ifeq (TSLIB,${GSLC_TOUCH})
    $(info GUIslice touch mode: tslib enabled - GUIslice config should use DRV_TOUCH_TSLIB mode)
    GSLC_LDLIB_EXTRA := -lm -lts
  else
    $(info GUIslice touch mode: tslib not enabled - GUIslice config should use DRV_TOUCH_SDL mode)
    GSLC_LDLIB_EXTRA := -lm
  endif
endif

# === SDL1.2 ===
ifeq (SDL1,${GSLC_DRV})
  $(info GUIslice driver mode: SDL1)
  GSLC_SRCS = ../../src/GUIslice_drv_sdl.c
  # - Add extra linker libraries if needed
  LDLIBS = -lSDL -lSDL_ttf ${GSLC_LDLIB_EXTRA}
endif

# === SDL2.0 ===
ifeq (SDL2,${GSLC_DRV})
  $(info GUIslice driver mode: SDL2)
  GSLC_SRCS = ../../src/GUIslice_drv_sdl.c
  # - Add extra linker libraries if needed
  LDLIBS = -lSDL2 -lSDL2_ttf ${GSLC_LDLIB_EXTRA}
endif

# === Adafruit-GFX ===
# No makefile for Arduino as most users will use the IDE for compilation

# ---------------------------------------------------------------------------

# Examples that operate in all driver modes
SRC =   ex01_lnx_basic.c \
	ex02_lnx_btn_txt.c \
	ex03_lnx_btn_img.c \
	ex04_lnx_ctrls.c \
	ex05_lnx_pages.c \
	ex06_lnx_callback.c \
	ex07_lnx_slider.c \
	ex08_lnx_tuner.c \
	ex09_lnx_radial.c \
	ex10_lnx_textbox.c \
	ex11_lnx_graph.c \
	ex15_lnx_foreign.c \
	ex18_lnx_compound.c \
	ex22_lnx_input_key.c \
	ex24_lnx_tabs.c \
	ex27_lnx_alpha.c \
	ex31_lnx_listbox.c \
	ex42_lnx_ring.c \
	ex43_lnx_glowball.c

# Add simple example for specific driver modes
ifeq (SDL1,${GSLC_DRV})
  SRC += test_sdl1.c
endif
ifeq (SDL2,${GSLC_DRV})
  SRC += test_sdl2.c
endif


OBJ = $(SRC:.c=.o)

BINS = $(SRC:.c=)

all: $(BINS)

clean:
	@echo "Cleaning directory..."
	$(RM) $(BINS)


test_sdl1: test_sdl1.c
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ test_sdl1.c $(LDFLAGS) -lSDL

test_sdl2: test_sdl2.c
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ test_sdl2.c $(LDFLAGS) -lSDL2

ex01_lnx_basic: ex01_lnx_basic.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex01_lnx_basic.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex02_lnx_btn_txt: ex02_lnx_btn_txt.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex02_lnx_btn_txt.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex03_lnx_btn_img: ex03_lnx_btn_img.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex03_lnx_btn_img.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex04_lnx_ctrls: ex04_lnx_ctrls.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex04_lnx_ctrls.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex05_lnx_pages: ex05_lnx_pages.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex05_lnx_pages.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex06_lnx_callback: ex06_lnx_callback.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex06_lnx_callback.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -lm -I . -I ../../src

ex07_lnx_slider: ex07_lnx_slider.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex07_lnx_slider.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex08_lnx_tuner: ex08_lnx_tuner.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex08_lnx_tuner.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex09_lnx_radial: ex09_lnx_radial.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex09_lnx_radial.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex10_lnx_textbox: ex10_lnx_textbox.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex10_lnx_textbox.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex11_lnx_graph: ex11_lnx_graph.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex11_lnx_graph.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex15_lnx_foreign: ex15_lnx_foreign.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex15_lnx_foreign.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex18_lnx_compound: ex18_lnx_compound.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex18_lnx_compound.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex22_lnx_input_key: ex22_lnx_input_key.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex22_lnx_input_key.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex24_lnx_tabs: ex24_lnx_tabs.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex24_lnx_tabs.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex27_lnx_alpha: ex27_lnx_alpha.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex27_lnx_alpha.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex31_lnx_listbox: ex31_lnx_listbox.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex31_lnx_listbox.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex42_lnx_ring: ex42_lnx_ring.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex42_lnx_ring.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src

ex43_lnx_glowball: ex43_lnx_glowball.c $(GSLC_CORE) $(GSLC_SRCS)
	@echo [Building $@]
	@$(CC) $(CFLAGS) -o $@ ex43_lnx_glowball.c $(GSLC_CORE) $(GSLC_SRCS) $(LDFLAGS) $(LDLIBS) -I . -I ../../src



