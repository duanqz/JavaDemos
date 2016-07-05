build:
	javac WhereIsString.java

dumpclass: build
	javap -verbose WhereIsString.class | tee WhereIsString.txt
	echo "dump class file content into WhereIsString.txt"
