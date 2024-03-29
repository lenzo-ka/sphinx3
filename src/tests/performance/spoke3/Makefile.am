# ====================================================================
# Copyright (c) 2000 Carnegie Mellon University.  All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#
# 1. Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer. 
#
# 2. Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in
#    the documentation and/or other materials provided with the
#    distribution.
#
# This work was supported in part by funding from the Defense Advanced 
# Research Projects Agency and the National Science Foundation of the 
# United States of America, and the CMU Sphinx Speech Consortium.
#
# THIS SOFTWARE IS PROVIDED BY CARNEGIE MELLON UNIVERSITY ``AS IS'' AND 
# ANY EXPRESSED OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, 
# THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
# PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL CARNEGIE MELLON UNIVERSITY
# NOR ITS EMPLOYEES BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT 
# LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, 
# DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY 
# THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT 
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE 
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
#
# ====================================================================
#
#  Sphinx III
#
# ====================================================================

AUTOMAKE_OPTIONS = -Wno-portability

AM_CFLAGS =-I$(top_srcdir)/include \
       -I$(top_builddir)/include


LDADD = $(top_builddir)/src/libs3decoder/libs3decoder.la \
	$(top_builddir)/src/libs3audio/libs3audio.la \
	$(top_builddir)/src/libutil/libs3util.la \
	-lm @ad_libs@ 

SPOKE3DIR = /net/scylla/usr3/dhuggins/adaptation/Spoke3
PROGRAMDIR = ${top_builddir}/src/programs
LPBIN = $(PROGRAMDIR)/sphinx3_livepretend
DECODEBIN = $(PROGRAMDIR)/sphinx3_decode
DECANYBIN = $(PROGRAMDIR)/sphinx3_decode_anytopo
# Add 
# "/afs/cs.cmu.edu/user/robust/archive/third_party_packages/NIST_scoring_tools/sctk-1.2c/linux/bin" 
# to your path if you have alinux machine
ALIGNBIN = sclite -i wsj -o sum rsum -f 0 

decode := all-decode all_baseline-decode 4u0-decode 4u0_14cd_ci-decode 4u0_baseline-decode
decany := all-decany all_baseline-decany 4u0-decany 4u0_14cd_ci-decany 4u0_baseline-decany

#all : decode
decode : $(decode)
decany : $(decany)

CLEANFILES = *.tmp *.ctl *.sent  *.lex *.results *.match *.match.* ARGS.*-{lp,decode,decany}

$(decode) :
	-rm -f $@.results $@.match $@.match.*
	cat ARGS ARGS.$(subst -decode,,$@) > ARGS.$@
	echo "-hyp $@.match" >> ARGS.$@
	$(DECODEBIN) ARGS.$@ > $@.results  2>&1 
	$(ALIGNBIN) -r $(shell echo -n $@ | sed -e 's,[_-].*,,').sent -h $@.match

$(decany) :
	-rm -f $@.results $@.match $@.match.*
	cat ARGS.decany ARGS.$(subst -decany,,$@) > ARGS.$@
	echo "-hyp $@.match" >> ARGS.$@
	$(DECANYBIN) ARGS.$@ > $@.results  2>&1 
	$(ALIGNBIN) -r $(shell echo -n $@ | sed -e 's,[_-].*,,').sent -h $@.match

