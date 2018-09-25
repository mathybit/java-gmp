# java-gmp
A Java wrapper for some libgmp integer functions commonly used in cryptography.

## Usage:
See the TestGMP.java file for examples of how to call the functions available in GMP.

The /lib folder contains the necessary libraries for building/running:
* jna.jar
* guava-19.0.jar (needed for timing the functions in the TestGMP.java script)

I also include a build of libgmp-10.dll for Windows (compiled on Win 10, 64-bit, using mingw32-64). If you want to use this on Linux, you must modify the following lines in LibGMP.java:
```
static {//load any required libraries (for now libgmp only) by name
	//loadlib("libgmp");//linux use after building/installing libgmp
	loadlib("libgmp-10");//windows build with mingw32
}
```

You will need to specify where the libgmp-10.dll is located at runtime. To do this, use the -D JVM parameter
`-Djna.library.path="C:\dev\lib\dll"`

On my dev environment, I keep all my .dll files under the C:\dev\lib\dll folder.

## Test Output:
Running the test script makes it pretty obvious that sometimes Java's BigInteger functions are faster (e.g. addition, multiplication), but when it comes to more complex arithmetic, libgmp shines. Here's the output of the test script on my machine:
```
Addition operation (50000 times):
   128 bits | Java: 12.53 ms | GMP: 389.2 ms
   256 bits | Java: 5.616 ms | GMP: 116.2 ms
   512 bits | Java: 3.956 ms | GMP: 129.2 ms
   1024 bits | Java: 7.531 ms | GMP: 167.3 ms
   2048 bits | Java: 6.993 ms | GMP: 328.0 ms
Done.
Multiplication operation (50000 times):
   128 bits | Java: 5.822 ms | GMP: 102.3 ms
   256 bits | Java: 4.404 ms | GMP: 110.1 ms
   512 bits | Java: 6.518 ms | GMP: 135.7 ms
   1024 bits | Java: 22.58 ms | GMP: 250.5 ms
   2048 bits | Java: 93.08 ms | GMP: 553.6 ms
Done.
Group operation a^e mod n (20000 times):
   128 bits | Java: 137.1 ms | GMP (insecure): 61.11 ms | GMP (secure): 93.97 ms
   256 bits | Java: 172.6 ms | GMP (insecure): 81.11 ms | GMP (secure): 137.1 ms
   512 bits | Java: 376.0 ms | GMP (insecure): 172.7 ms | GMP (secure): 310.3 ms
   1024 bits | Java: 1.531 s | GMP (insecure): 461.7 ms | GMP (secure): 940.3 ms
   2048 bits | Java: 4.951 s | GMP (insecure): 1.495 s | GMP (secure): 4.662 s
Done.
Group operation a^{-1} mod n (10000 times):
   128 bits | Java: 199.4 ms | GMP: 32.99 ms
   256 bits | Java: 284.2 ms | GMP: 39.08 ms
   512 bits | Java: 548.0 ms | GMP: 59.98 ms
   1024 bits | Java: 2.023 s | GMP: 111.6 ms
   2048 bits | Java: 7.236 s | GMP: 253.2 ms
Done.
Primality testing:
   128 bits | Java: 25.79 ms | GMP: 930.1 ?s
   256 bits | Java: 66.71 ms | GMP: 2.817 ms
   512 bits | Java: 383.7 ms | GMP: 21.12 ms
   1024 bits | Java: 4.401 s | GMP: 223.8 ms
   2048 bits | Java: 53.43 s | GMP: 2.805 s
Done.
```
