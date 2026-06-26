# Finding-Obfuscated-Il2Cpp-Symbols
How to find & map obfuscated Il2Cpp symbols for games on Android (probably works for pc as well)

# Introduction

Many games will obfuscate their il2cpp symbols, which makes it very hard to interact with the game itself. some games have it easier where they *don't* strip the log for the symbol, as shown here

![image](images/img1.png)

vs other games that strip those as well, which make it hard to find. As shown here

![image](images/img2.png)

This python script works on both versions, so how to do it is the same. I will be using IDA-Pro for this, but ghidra is roughly the same process.

# Step 1

To start, open the libunity.so for the target game in your decompiler (if you're using ghidra, wait for it to fully finish analyzing)
To make it easier, you can run llvm-readelf from your Android NDK 

`llvm-readelf -Ws /path/to/libil2cpp.so | grep "FUNC" > ./functions.txt` 

in the functions.txt file, it shouldn't take much scrolling until you find an obfuscated symbol. Some games may look like this

![image](images/img3.png)

# Step 2

copy the symbol's name, search that in the libunity.so file's strings, and double click

if you only want to use the libunity.so file, just go to strings and scroll until you find a symbol. Something like this (may vary per game)

![image](images/img4.png)

# Step 3

then double click

if should bring you to a screen with the string, there should be only one xref, double click the xref
decompile/get the pseudo code for the function, Ctrl+A and Ctrl+C the contents, and put that into bad.txt

# Final step

Now for the next step, you'll need to get the same Unity version's file but without anyhing obfuscated, a good github with every version is [MelonLoader.UnityDependencies](https://github.com/LavaGang/MelonLoader.UnityDependencies/releases). I use this and it comes in handy. You may also download the unity version yourself, build for your target device, and get the file from there.

Repeat the same process for the decrypted file, but this time save the pseudo code in good.txt
To finish off, just run 

`python3 Il2CppSymbolMapMaker.py` 

and it should do it.

# Final results
The exported file with all the symbols should just be SymbolMap.json, here's what it should look like

![image](images/img5.png)
