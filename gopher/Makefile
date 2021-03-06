#********************************************************************
# $Author: jgoerzen $
# $Revision: 1.5 $
# $Date: 2002/01/11 15:55:59 $
# $Source: /home/jgoerzen/tmp/gopher-umn/gopher/head/gopher/Makefile.in,v $
# $State: Exp $
#
# Paul Lindner, University of Minnesota CIS.
#
# Copyright 1991, 1992 by the Regents of the University of Minnesota
# see the file "Copyright" in the distribution for conditions of use.
#********************************************************************
# MODULE: Makefile
# Makefile for gopher client
#*********************************************************************
# Revision History:
# $Log: Makefile.in,v $
# Revision 1.5  2002/01/11 15:55:59  jgoerzen
#   * Using $(INSTALL) from subdirectories broke on systems
#     where install-sh was used because $(INSTALL) would always
#     resolve to ./install-sh.  Replaced all instances of
#     $(INSTALL) with /usr/bin/install -c (autoconf would provide the correct
#     path) and removed INSTALL from Makefile.config.  Reported
#     by Don Fike <fike@cs.utk.edu> on Solaris 2.6.
#   * Updated our copy of install-sh with the one from
#     a current automake.
#
# Revision 1.4  2002/01/09 16:35:25  jgoerzen
# More bugfixing.
#
# Revision 1.3  2002/01/09 15:54:48  jgoerzen
# Modifications to assist with installations
#
# Revision 1.2  2002/01/08 17:36:14  jgoerzen
# Finally builds!
#
# Changes:
#
#   * config.h.in: rebuilt by autoheader
#
#   * configure: rebuilt by autoconf
#
#   * configure.in:
#     * Added REGEXLIBS test for NetBSD -- look for re_comp in -lcompat
#     * Added checkes for term.h, re_comp.h, regex.h
#
#   * gopher/ourutils.c, gopher/CURcurses.c: Use term.h check
#
#   * gopher/Makefile.in, gopherd/Makefile.in, gophfilt/Makefile.in:
#     * Use REGEXLIBS
#
#   * gopher/globals.h, gopher/gopher.c: Remove sys_errlist
#
#   * object/GSgopherobj.c: Removed <regex.h> #include, now use
#     "Regex.h" that has proper regex determining logic
#
#   * object/Regex.h: Moved regex.h include to here.  Make it conditional
#     based on configure test.  Add conditional re_comp.h include.
#
# Revision 1.1.1.1  2000/08/19 00:28:56  jgoerzen
# Import from UMN Gopher 2.3.1 after GPLization
#
# Revision 3.1  1996/01/04  18:29:18  lindner
# Updates for autoconf
#
# Revision 3.14  1994/09/29  19:23:50  lindner
# Make sure directories are made before installing
#
# Revision 3.13  1994/08/19  16:52:07  lindner
# Make sure that gopher.o gets recompiled when patchlevel changes
#
# Revision 3.12  1994/07/19  19:29:40  lindner
# Add INTLOPTS, add locales dir target for clean
#
# Revision 3.11  1994/07/03  21:35:42  lindner
# Add upload option to code
#
# Revision 3.10  1994/05/26  19:41:14  lindner
# Add yet another definition
#
# Revision 3.9  1994/04/25  20:47:22  lindner
# Remove html stuff from menu
#
# Revision 3.8  1994/04/25  02:16:50  lindner
# Fix for last line without newline
#
# Revision 3.7  1994/02/20  16:17:43  lindner
# Add form object file
#
# Revision 3.6  1993/09/08  05:21:22  lindner
# Add pager.c
#
# Revision 3.5  1993/08/04  22:06:35  lindner
# Rebuild client if patchlevel.h changes
#
# Revision 3.4  1993/07/27  05:28:45  lindner
# Mondo Debug overhaul from Mitra
#
# Revision 3.3  1993/04/15  21:28:25  lindner
# Mods for remote access
#
# Revision 3.2  1993/02/19  21:03:13  lindner
# Added stuff for global RC file.
#
# Revision 3.1.1.1  1993/02/11  18:02:57  lindner
# Gopher+1.2beta release
#
# Revision 1.4  1993/02/09  21:34:47  lindner
# Fixed install target
#
# Revision 1.3  1993/01/07  22:51:44  lindner
# Added download.o to list of objects
#
# Revision 1.2  1992/12/31  04:01:06  lindner
# moved html.c to html2.c for VMS
#
# Revision 1.1  1992/12/10  23:32:16  lindner
# gopher 1.1 release
#
#********************************************************************/


include ../Makefile.config

srcdir = .
top_srcdir = ..


OBJS	= manager.o gopher.o globals.o ourutils.o cso.o \
	subprocs.o html2.o CURcurses.o gopherrc.o \
        download.o pager.o form.o upload.o

CCFLAGS	= $(OPT) $(DEBUGGING) -I.. -I$(top_srcdir) -I$(top_srcdir)/object \
	  $(INTLOPTS) $(CLIENTOPTS)  -DGOPHERLIB=\"$(CLIENTLIB)\" \
	 -DGOPHERHELP=\"${PREFIX}/share/gopher/gopher.hlp\" \
	 -DGLOBALRC=\"$(CLIENTLIB)/gopher.rc\" \
	 -DREMOTERC=\"$(CLIENTLIB)/gopherremote.rc\"

TARGET	= gopher

all:	$(TARGET)

.c.o:
	$(CC) $(CCFLAGS) -c $<

globals.o : $(srcdir)/globals.h $(srcdir)/version.h

../object/libgopher.a:
	(cd ..; $(MAKE) $(MFLAGS) objects)

$(TARGET) : $(OBJS) ../object/libgopher.a
	$(CC) $(LDFLAGS) -o $@ $(OBJS)  ../object/libgopher.a  -lcurses -ltermcap -ltls -lssl -lcrypto  -lcrypto -lssl -ltls -ltermcap -lcurses -lm 

install : all
	-mv $(CLIENTDIR)/$(TARGET) $(CLIENTDIR)/$(TARGET).old
	-mkdir $(CLIENTDIR)
	-mkdir $(CLIENTLIB)
	/usr/bin/install -c $(TARGET) $(CLIENTDIR)
	/usr/bin/install -c -d $(CLIENTLIB)
	/usr/bin/install -c gopher.rc $(CLIENTLIB)
	/usr/bin/install -c gopherremote.rc $(CLIENTLIB)
	/usr/bin/install -c gopher.hlp $(CLIENTLIB)

clean:
	-rm -f $(TARGET) $(OBJS) *.out *~ core
	(cd locales;     $(MAKE) $(MFLAGS) clean)

$(OBJS): $(srcdir)/gopher.h $(top_srcdir)/conf.h
globals.o: $(srcdir)/globals.h
gopherrc.o: $(top_srcdir)/patchlevel.h
manager.o: $(top_srcdir)/patchlevel.h
gopher.o: $(top_srcdir)/patchlevel.h

