# ====================================================================
# Copyright (c) 2004 Carnegie Mellon University.  All rights reserved.
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

DATAPATH = /lab/speech/sphinx4/data/tidigits/test/
TIDIGITSRAWPATH = $(DATAPATH)/raw16k
TIDIGITSCEPPATH = $(DATAPATH)/cepstra 
TIDIGITSRESULTPATH = ${top_builddir}/src/tests/performance/tidigits/
PROGRAMDIR = ${top_builddir}/src/programs
BMBIN = ${top_builddir}/src/test/programs/batchmetrics
LPBIN = $(PROGRAMDIR)/sphinx3_livepretend
DECODEBIN = $(PROGRAMDIR)/sphinx3_decode
DECANYBIN = $(PROGRAMDIR)/sphinx3_decode_anytopo
# Add 
# "/afs/cs.cmu.edu/user/robust/archive/third_party_packages/NIST_scoring_tools/sctk-1.2c/linux/bin" 
# to your path if you have alinux machine
ALIGNBIN = sclite -i wsj -o sum rsum -f 0 

ALLTARGET=tidigits-lp tidigits-quick-lp tidigits-decode tidigits-quick-decode

#legacy targets, For tidigits, batchmetric doesn't work at all.
#tidigits-bm tidigits-quick-bm:
#	rm -f $@.results $@.ctl
#	cat ARGS ARGS.live > ARGS.$@
#	$(BMBIN) $(subst -bm,,$@).batch $(TIDIGITSRAWPATH) ARGS.$@ > $@.results 2>&1

#all : $(ALLTARGET)
quick : tidigits-quick-lp tidigits-quick-decode tidigits-quick-decany
slow : tidigits-lp tidigits-decode tidigits-decany

CLEANFILES = *.ctl *.sent  *.lex *.results *.match *.match.* ARGS.tidigits-*

#Targets which we are actively using. 
tidigits-lp tidigits-quick-lp : 
	-rm -f $@.results $@.match $@.match.*
	awk '{print $$1}' $(subst -lp,,$@).batch | sed "s/\.wav\.raw//" > ./$@.ctl
	cat ARGS ARGS.live > ARGS.$@
	echo "-hyp $@.match" >> ARGS.$@
	echo "-rawext .wav.raw" >> ARGS.$@
	$(LPBIN) ./$@.ctl $(TIDIGITSRAWPATH) ARGS.$@ > $@.results  2>&1 
	awk '{for(i=2;i<=NF;i++){printf("%s ",$$i)}; printf("(%s)\n",$$1)}' $(subst -lp,,$@).batch | sed "s/\.wav\.raw//" > $@.sent
	$(ALIGNBIN) -r $@.sent -h $@.match

tidigits-decode tidigits-quick-decode: 
	-rm -f $@.results $@.match $@.match.*
	awk '{print $$1}'  $(subst -decode,,$@).batch | sed "s/\.wav\.raw//" > ./$@.ctl
	/bin/cp ARGS ARGS.$@
	echo "-ctl $@.ctl" >> ARGS.$@
	echo "-cepdir $(TIDIGITSCEPPATH)" >> ARGS.$@
	echo "-hyp $@.match" >> ARGS.$@
	$(DECODEBIN) ARGS.$@ > $@.results 2>&1
	awk -F/ '{print $$NF}' $(subst -decode,,$@).batch | awk '{for(i=2;i<=NF;i++){printf("%s ",$$i)}; printf("(%s)\n",$$1)}'  | sed "s/\.wav\.raw//" > $@.sent
	$(ALIGNBIN) -r $@.sent -h $@.match


tidigits-decany tidigits-quick-decany: 
	-rm -f $@.results $@.match $@.match.*
	awk '{print $$1}'  $(subst -decany,,$@).batch | sed "s/\.wav\.raw//" > ./$@.ctl
	/bin/cp ARGS.decany ARGS.$@
	echo "-ctl $@.ctl" >> ARGS.$@
	echo "-cepdir $(TIDIGITSCEPPATH)" >> ARGS.$@
	echo "-hyp $@.match" >> ARGS.$@
	$(DECANYBIN) ARGS.$@ > $@.results 2>&1
	awk -F/ '{print $$NF}' $(subst -decany,,$@).batch | awk '{for(i=2;i<=NF;i++){printf("%s ",$$i)}; printf("(%s)\n",$$1)}'  | sed "s/\.wav\.raw//" > $@.sent
	$(ALIGNBIN) -r $@.sent -h $@.match









