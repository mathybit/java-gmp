# java-gmp
A Java wrapper for some commonly used integer functions in libgmp.

## Usage:
See the TestGMP.java file for examples of how to call the functions available in GMP.

The /lib folder contains the necessary libraries for building/running:
* jna.jar
* guava-19.0.jar (needed for timing the functions in the TestGMP.java script)

I also include a build of libgmp-10.dll for Windows (compiled on Win 10, 64-bit, using mingw32-64). If you want to use this on Linux, you must modify the following lines in LibGMP.java:
'''
static {//load any required libraries (for now libgmp only) by name
	//loadlib("libgmp");//linux use after building/installing libgmp
	loadlib("libgmp-10");//windows build with mingw32
}
'''
