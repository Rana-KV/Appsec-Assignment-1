# Part-3 Report
##Changes

- Added conv1.gft and conv2.gft improve code coverage
- Added fuzzer1.gft and fuzzer2.gft which are crashes and hangs from fuzzing
- Fixed the issues cauisng the program to crash/hang by fuzzer1.gft and fuzzer2.gft
- Fixed other potential issues in the animate function


**Table of Contents**

[TOC]

##Code Coverage
### Testcase conv1.gft
* Intial code coverage: 59.44 %
* About Testcase:
	* The testcase explore the op codes "0x07", "0x00", "0x01", "0x02", and "0x08"
	* The program uses:
		* "\x07\x00\x00"
		* "\x00\x00\x00"
		* "\x01\x02\x02"
		* "\x02\x02\x00"
		* "\x08\x08\x08"
* Final code coverage: 65.00 %

### Testcase conv2.gft
* Intial code coverage: 65.00 %
* About Testcase:
	* The testcase explore the op codes "0x07", "0x04", "0x05", "0x06", and "0x08"
	* The program uses:
		* "\x07\x00\x00"
		* "\x04\x01\x01"
		* "\x05\x02\x02"
		* "\x06\x02\x01"
		* "\x08\x08\x08"
* Final code coverage: 71.11 %

##Fuzzing
###Setup

- Used afl-clang-fast for instrumentation
- Used all the testcases available till now as inputs
- Results in few dozens crahes and half a dozen hangs

###Testcase fuzzer1.gft
* About Testcase: The testcase hangs the program and the core reason for the program to go into infinite loop is because of the op code "0x10" in animate function. With a large value of arg1 due to typecasting as char it will decrease the pc variable which acts as progrom counter for the min-program in the animate function. Any value larger than 127 in arg1 will be interpretted as negative value and decrease the pc variable.

* Fix: The fix for the issue is a simple change of typcasting in the op code "0x10". The char is changed to unsigned char to interpret large values as positive and this makes sure that the pc variable is always increasing.

###Testcase fuzzer2.gft
* About Testcase: The testcase crashes the program and the core reason for the program to crash is because of the "0x05" in animate function. With a value larger than 15 is arg1 or arg2 will crash the system due to the reason that only 16 registers [array elements] where declared at the beginning of the program.

* Fix: The fix for the issue is a simple additonal check in the op code "0x05". The values of arg1 and arg2 are checked before executing the next instructions. In case if the value of either arg1 or agr2 or both is greater than 15 then the op code exits the program throwing an error.
