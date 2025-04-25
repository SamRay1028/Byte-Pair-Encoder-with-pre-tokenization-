# Byte-Pair-Encoder-with-pre-tokenization-
A byte pair encoding algorithm that pre-tokenizes on white space.

This program is a work in progress.

The basic training architecture for this algorithm is complete; however, its still in very early stages of development and their are bugs that need to be worked out. Additionally an encoding mechnism has not yet been added. This bpe algorithm, instead of loading the text into memory (whether fully or in chunks) instead compiles a list of words and their frequencies and character pairs that appear in each word. This allows the program to be faster and leaner. Currently the code is not yet parallelized and only runs on one core. It takes around 13-15 seconds to complete and around 60 mb of RAM.

# Credits:
This code makes use of robin hood hash maps. The link for the necessary cpp file can be found here: 
https://github.com/martinus/robin-hood-hashing/blob/master/src/include/robin_hood.h
