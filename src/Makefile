# This is a makefile that builds the English morphological parser
# *****************************************************************


# This is a preliminary file that builds an English parser based upon
# xfst and not twolc.

# Here we build the final generator , an inverted transducer of the analyzer.
# It is dependent upon eng.save

ieng.fst: ../bin/ieng.fst
../bin/ieng.fst: ../bin/eng.fst
	@echo
	@echo "*** Building the inverse ieng.fst ***"
	@echo
	@printf "load ../bin/eng.save \n\
	invert net \n\
	save stack ../bin/ieng.fst \n\
	quit \n" > ../../tmp/ieng-fst-script
	@xfst < ../../tmp/ieng-fst-script
	@rm -f ../../tmp/ieng-fst-script


# This goal is to build the final analyser. It depends on all the files.

eng.fst: ../bin/eng.fst
../bin/eng.fst: ../bin/eng.save ../bin/caseconv.fst ../bin/tok.fst 
	@echo
	@echo "*** Building eng.fst ***" ;
	@echo
	@printf "read regex [[@\"../bin/eng.save\"] .o. \
	[@\"../bin/caseconv.fst\"]] ; \n\
	save stack ../bin/eng.fst \n\
	quit \n" > ../../tmp/eng-fst-script
	@xfst < ../../tmp/eng-fst-script
	@rm -f ../../tmp/eng-fst-script


# The second goal is to build the caseconv.fst file
# This goal depends on case.regex

caseconv.fst: ../bin/caseconv.fst
../bin/caseconv.fst: case.regex
	@echo
	@echo "*** Building caseconv.fst ***" ;
	@echo
	@printf "read regex < case.regex \n\
	save stack ../bin/caseconv.fst \n\
	quit \n" > ../../tmp/caseconv-script
	@xfst < ../../tmp/caseconv-script
	@rm -f ../../tmp/caseconv-script


# Another goal is to build a preprocessor.This goal depends on tok.txt

tok.fst: ../bin/tok.fst
../bin/tok.fst: tok.txt
	@echo
	@echo "*** Building the tokenizer tok.fst ***" ;
	@echo
	@printf "source tok.txt \n\
	save stack ../bin/tok.fst \n\
	quit \n" > ../../tmp/tok-script
	@xfst < ../../tmp/tok-script
	@rm -f ../../tmp/tok-script


eng.save: ../bin/eng.save
../bin/eng.save: ../bin/xfst-eng.bin ../bin/eng-lex.save
	@echo
	@echo "*** Building the parser eng.save ***"
	@echo
	@printf "read regex [[@\"../bin/eng-lex.save\"] .o. \
	[@\"../bin/xfst-eng.bin\"]] ; \n\
	save stack ../bin/eng.save \n\
	quit \n" > ../../tmp/eng-save-script
	@xfst < ../../tmp/eng-save-script
	@rm -f ../../tmp/eng-save-script

# The first goal is to build eng-lex.save
# This goal depends on a bunch of lexicon files

eng-lex.save: ../bin/eng-lex.save
../bin/eng-lex.save: eng-lex.txt noun-eng-morph.txt noun-eng-lex.txt \
        adj-eng-lex.txt interj-eng-lex.txt cc-eng-lex.txt cs-eng-lex.txt \
	adv-eng-lex.txt verb-eng-lex.txt pron-eng-lex.txt punct-eng-lex.txt \
	pp-eng-lex.txt
	@echo
	@echo "*** Building eng-lex.save ***" ;
	@echo
	printf "compile-source eng-lex.txt noun-eng-morph.txt noun-eng-lex.txt \
	adj-eng-lex.txt verb-eng-lex.txt verb-eng-morph.txt pron-eng-lex.txt \
	adv-eng-lex.txt pp-eng-lex.txt punct-eng-lex.txt \n\
	save-source ../bin/eng-lex.save \n\
	quit \n" > ../../tmp/eng-lex-save-script
	@lexc < ../../tmp/eng-lex-save-script
	rm -f ../../tmp/eng-lex-save-script

# The second goal is to build xfst-eng.bin
# This goal depends on xfst-eng.txt

xfst-eng.bin: ../bin/xfst-eng.bin
../bin/xfst-eng.bin: xfst-eng.txt
	@echo
	@echo "*** Building xfst-eng.bin ***" ;
	@echo
	@printf "source xfst-eng.txt \n\
	save stack ../bin/xfst-eng.bin \n\
	quit \n" > ../../tmp/xfst-eng-script
	@xfst < ../../tmp/xfst-eng-script
	@rm -f ../../tmp/xfst-eng-script

clean:
	@rm -f ../bin/*.bin ../bin/*.fst ../bin/*.save


