DEMO=WhereIsString

clean:
	@rm -rf $(DEMO).class $(DEMO).dex $(DEMO).txt

build: clean
	@javac $(DEMO).java

dx: build
	@tools/dx --dex --output $(DEMO).dex $(DEMO).class

dumpclass: build
	@echo ">>> Dump $(DEMO).class ..."
	@javap -verbose $(DEMO).class | tee $(DEMO).class.txt
	@echo "<<< Dump $(DEMO).class done"
	@echo ""

dumpdex: dx
	@echo ">>> Dump $(DEMO).dex ..."
	@tools/dexdump $(DEMO).dex | tee $(DEMO).dex.txt
	@echo "<<< Dump $(DEMO).dex done."

dumpall: dumpclass dumpdex
	@echo ""
