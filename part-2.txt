# Writeup for Part-2
**Table of Contents**

[TOC]

## Overview
- This write up contains information about the 3 testcases added to the project as part of the HW-1 Part-2.

## Task list
- [x] Carsh 1
    - [x] Find a bug to crash the program
    - [x] Create a test case to check the bug
    - [x] Fix the bug
- [x] Carsh 2
    - [x] Find a bug to crash the program
    - [x] Create a test case to check the bug
    - [x] Fix the bug
- [x] Hang
    - [x] Find a bug to Hang the program
    - [x] Create a test case to check the bug
    - [x] Fix the bug
- [x] Write a Report

## Crash 1
- **Reason**: Using the mptr variable in the animate function of the giftcardreader program an user has a capability to random point and overwrite memory location causing the program to crash. This happens because the mptr which is used to move the message print line is not limited to move to other memory locations.

- **Testcase**: At the start of the mptr is initated to point the memory location of msg varaible. A testcase is written to change the memory location that mptr is pointing to using '\x03' op code and overwrite using a memory location in the program '\x02' op code. The testcase overwrites memory location of a variable used in the parent function calling animate. The testcase uses giftcard record animate type.

- **Fix**: mptr variable in only used to cursor of the message being print in msg variable. An additional check is placed so that mptr doesn't point to memory location other than msg variable.

## Crash 2
- **Reason**: Using the "ret_val->num_bytes" variable in the gift_card_reader function of the giftcardreader program an user has a capability to send a negative integer/large number for malloc function and crash it. This happens because malloc function is unable to allocate the needed memory the program requested.

- **Testcase**: A testcase is created where the file size, which is written in the beginning of the program, is provided as '-1'. This value directly used by program for malloc. This testcase is independent of giftcard record type.

- **Fix**: Added an aditonal check to verfiy if the "ret_val->num_bytes" variable is negative and also if the malloc fails to allocate memory the program is safely exited.

## Hang
- **Reason**: The animate function allows the user to specify jumps for the mini-program it executes within the function. This possible through '\x09' op code, which increases the pc varaible in the function which acts like a program counter for the mini-program. The issue lies in the way the pc variable is incremented, because of the typecasting the higher values are executed as negative values and decrements the pc variable. In turn, making the mini-program re-execute the previous commands.

- **Testcase**: A testcase is created where a min-program is written to print message using op code '\x07' and then uses '\x09' op code to and arg1 as '\xfe' which is interpreted as -3 due to typecasting.The testcase uses giftcard record animate type.

- **Fix**: Corrected the wrong typecasting in op code '\x09' of animate function from "char" to "unsigned char". This ensures that what ever the jump value is provided by the user it only increments the pc variable. So without decrement the program can not be forced to hang.

