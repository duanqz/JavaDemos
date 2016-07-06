TOP=$(shell pwd)
OUT=$(TOP)/out

DX=$(TOP)/tools/dx
DEXDUMP=$(TOP)/tools/dexdump

clean:
	@rm -rf $(OUT)

build: clean
	@[ ! -e $(OUT) ] && mkdir -p $(OUT)
	@javac -verbose -d $(OUT) *.java

dx: build
	@cd $(OUT); \
	 $(DX) --dex --output classes.dex *.class; \
	 cd - > /dev/null

dumpclass: build
	@echo ">>> Dump classes ..."
	@for f in `ls $(OUT)/*.class`; \
	 do \
	   javap -verbose $$f | tee $$f.txt; \
	 done
	@echo "<<< Dump classes done."

dumpdex: dx
	@echo ">>> Dump classes.dex ..."
	$(DEXDUMP) -fd $(OUT)/classes.dex | tee $(OUT)/classes.dex.txt
	@echo "<<< Dump classes.dex done."

dumpall: dumpclass dumpdex
	@echo ""
