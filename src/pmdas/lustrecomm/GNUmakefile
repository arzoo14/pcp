#
# Copyright (c) 2008 Silicon Graphics, Inc.  All Rights Reserved.
# Copyright (c) 2015,2020 Red Hat, Inc.
#
# Author: Scott Emery <emery@sgi.com> 
#
# This program is free software; you can redistribute it and/or modify it
# under the terms of the GNU General Public License as published by the
# Free Software Foundation; either version 2 of the License, or (at your
# option) any later version.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY
# or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
# for more details.
#

TOPDIR = ../../..
include $(TOPDIR)/src/include/builddefs

IAM	= lustrecomm
DOMAIN	= LUSTRECOMM

PMDAADMDIR = $(PCP_PMDASADM_DIR)/$(IAM)
PMDATMPDIR = $(PCP_PMDAS_DIR)/$(IAM)

DFILES 	= README
HFILES	= libreadfiles.h
CFILES	= $(IAM).c file_indexed.c file_single.c refresh_file.c timespec_routines.c

LIBTARGET = pmda_$(IAM).so
CMDTARGET = pmda$(IAM)
TARGETS = $(CMDTARGET)

LDOPTS	=
LLDLIBS = $(PCP_PMDALIB) $(LIB_FOR_RT)
LCFLAGS = -I.

LDIRT	= domain.h *.o $(IAM).log pmda$(IAM) pmda_$(IAM).so

MAN_SECTION = 1
MAN_PAGES = pmda$(IAM).$(MAN_SECTION)
MAN_DEST = $(PCP_MAN_DIR)/man$(MAN_SECTION)

default: build-me

include $(BUILDRULES)

ifeq "$(TARGET_OS)" "linux"
build-me:	$(TARGETS)

install:	default
	$(INSTALL) -m 755 -d $(PMDAADMDIR)
	$(INSTALL) -m 755 -d $(PMDATMPDIR)
	$(INSTALL) -m 755 -t $(PMDATMPDIR) Install Remove $(CMDTARGET) $(PMDAADMDIR)
	$(INSTALL) -m 644 -t $(PMDATMPDIR) $(DFILES) root pmns domain.h help $(PMDAADMDIR)
	@$(INSTALL_MAN)
else
build-me:
install:
endif

default_pcp: default

install_pcp: install

$(OBJECTS): domain.h

domain.h: ../../pmns/stdpmid
	$(DOMAIN_MAKERULE)

check:: $(MAN_PAGES)
	$(MANLINT) $^
