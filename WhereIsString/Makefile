DEMO=WhereIsString

clean:
	@rm -rf *.class *.dex *.dump.txt

build: clean
	@javac *.java

dx: build
	@tools/dx --dex --output classes.dex *.class

dumpclass: build
	@echo ">>> Dump classes ..."
	@for f in `ls *.class`; \
	 do \
	   javap -verbose $$f | tee $$f.dump.txt; \
	 done
	@echo "<<< Dump classes done."

dumpdex: dx
	@echo ">>> Dump classes.dex ..."
	@tools/dexdump -fd classes.dex | tee classes.dex.dump.txt
	@echo "<<< Dump classes.dex done."

dumpall: dumpclass dumpdex
	@echo ""
