SilentMatrix
Plaintext

###########################################
##                                       ##
##   H I L L  C I P H E R  S Y S T E M   ##
##          [ Matrix 2x2 Mode ]          ##
##                                       ##
###########################################
Overview
SilentMatrix is a C++ cryptographic tool implementing the Hill Cipher algorithm using 2x2 matrix transformations. It is designed for users interested in linear algebra-based encryption and cryptographic analysis. The system extends the standard English alphabet to include spaces, providing a seamless encryption experience for full sentences.

Key Features
Mathematical Core: Implements encryption and decryption using modular arithmetic and matrix inversion.

Extended Alphabet: Supports 27 characters (A-Z and Space), mapping the space character to index 26.

Analytical History: Includes a session-based logging system that stores encrypted records with custom tags for later retrieval and analysis.

Manual and Automated Decryption: Allows users to manually input ciphered numbers or decrypt stored records directly from the internal database.

Technical Specifications
Cipher Type: Symmetric Key (Hill Cipher)

Alphabet Size: 27

Encryption Key (K)
Plaintext

| 3  5 |
| 1  2 |
Decryption Key (K^-1)
Plaintext

| 2  22 |
| 26  3 |
How It Works
The system treats text as vectors and multiplies them by the key matrix. For decryption, it utilizes the modular multiplicative inverse of the determinant to ensure the original message is recovered accurately.

Encryption: Converts text to numeric vectors, performs matrix multiplication modulo 27, and outputs a sequence of integers.

Tagging: Users can assign a unique label to each encryption session.

Storage: The result is pushed to a vector-based history log.

Decryption: Processes either user-provided integers or historical records to return the original plaintext.

Installation and Usage
Requirements: A C++ compiler (GCC, Clang, or MSVC).

Compilation:

Bash

g++ -o SilentMatrix main.cpp
Execution:

Bash

./SilentMatrix
Project Structure
main.cpp: Contains the core logic, including matrix operations, history management, and the command-line interface.
