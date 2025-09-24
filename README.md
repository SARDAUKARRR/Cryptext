Cryptext   https://sardaukarrr.github.io/Cryptext/ 
A minimalist, client-side, secure text encryption tool that runs entirely in your browser. No server, no data storage, just pure cryptography.

About Cryptext
Cryptext is a single, self-contained HTML file that provides strong, password-based encryption for your text. It's designed for anyone who needs to quickly secure a piece of text before sending it over an insecure channel (like email or instant messaging) or for storing it privately.

The core philosophy is zero trust: the application has no backend, sends no network requests, and stores no data. All cryptographic operations happen locally in your browser, ensuring that your password and your unencrypted text never leave your machine.

Features
Strong Encryption: Uses the industry-standard AES-256 algorithm.

Password Hardening: Implements PBKDF2 with 480,000 iterations to protect against brute-force attacks on your password.

Client-Side Only: Runs 100% in your browser. No internet connection is needed after the page is loaded.

Text Compression: Uses the DEFLATE algorithm to compress text before encryption, resulting in a smaller output, especially for longer texts.

Stateless: The application does not store any information. Your privacy is paramount.

Minimalist UI: A clean, icon-driven interface with no unnecessary text, providing a focused user experience.

Portable: A single HTML file that you can save and run anywhere.

How to Use
Download the index.html file.

Open it in any modern web browser (like Chrome, Firefox, or Edge).

To Encrypt:

Enter your secret text in the top text area.

Enter a strong, memorable password in the password field.

Click the lock icon (🔒) button.

The encrypted result will appear in the bottom text area. You can use the copy button to copy it to your clipboard.

To Decrypt:

Paste the encrypted text into the top text area.

Enter the exact same password used for encryption.

Click the unlock icon (🔓) button.

The original text will appear in the bottom text area.

Technical Deep Dive: The Cryptext Protocol
To ensure transparency and allow for independent verification, this section details the exact cryptographic protocol used by Cryptext.

Core Components & Parameters
The entire process is governed by a strict set of rules and cryptographic primitives.

Encryption Algorithm: AES (Advanced Encryption Standard)

AES Key Size: 256-bit (32 bytes)

AES Mode of Operation: CBC (Cipher Block Chaining)

AES Padding Scheme: PKCS7

Key Derivation Function: PBKDF2

PBKDF2 Hash Function: SHA-256

PBKDF2 Iterations: 480,000

Salt Size: 16 bytes

Initialization Vector (IV) Size: 12 bytes

Text Compression Algorithm: DEFLATE (via pako.js)

Character Encoding: UTF-8 for all text.

Binary-to-Text Encoding: Base64

Component Delimiter: :: (two colons)

Step-by-Step Encryption Process
Inputs:

plaintext (The original string in UTF-8)

password (The user's password string in UTF-8)

Process:

Compression: The plaintext is first compressed using the DEFLATE algorithm. This results in compressed_data (a byte array).

Salt Generation: A cryptographically secure random salt of 16 bytes is generated.

IV Generation: A cryptographically secure random iv (Initialization Vector) of 12 bytes is generated.

Key Derivation: The password and the generated salt are fed into the PBKDF2 function. Using the parameters defined above (480,000 iterations, SHA-256), a 256-bit derived_key is produced.

AES Encryption: The compressed_data from Step 1 is encrypted using AES. The process uses the derived_key from Step 4, the iv from Step 3, CBC mode, and PKCS7 padding. This produces the final ciphertext.

Assembly & Encoding:
a. The salt (from Step 2) is encoded into a Base64 string.
b. The iv (from Step 3) is encoded into a Base64 string.
c. The ciphertext (from Step 5) is encoded into a Base64 string.
d. These three Base64 strings are concatenated, separated by the :: delimiter.

Output: The final encrypted string has the following structure:
Base64(salt)::Base64(iv)::Base64(ciphertext)

Step-by-Step Decryption Process
Inputs:

encrypted_text (The structured string from the encryption output)

password (The user's password)

Process:

Disassembly: The encrypted_text string is split by the :: delimiter into three parts: Base64(salt), Base64(iv), and Base64(ciphertext).

Base64 Decoding: Each of the three parts is decoded from Base64 back into its raw byte array format, yielding the salt, iv, and ciphertext.

Key Derivation: The password and the extracted salt are fed into the exact same PBKDF2 process as in the encryption step. This regenerates the derived_key. If the password is wrong, a completely different and incorrect key will be generated.

AES Decryption: The ciphertext is decrypted using the AES algorithm. The process uses the derived_key from Step 3, the iv from Step 2, CBC mode, and PKCS7 padding. This results in decrypted_compressed_data. If the key is incorrect, this step will fail or produce garbage data.

Decompression: The decrypted_compressed_data is decompressed using the DEFLATE (inflate) algorithm.

Final Output: The resulting byte array is interpreted as a UTF-8 string to produce the original plaintext.

Libraries Used
Cryptext relies on two excellent, open-source JavaScript libraries to perform its functions:

CryptoJS: Used for all cryptographic operations, including AES and PBKDF2.

Pako: A high-performance zlib port to JavaScript, used for DEFLATE compression.

Security Notice
This tool is designed with a strong focus on security, using standard, well-vetted cryptographic primitives. However, the security of the encrypted text is critically dependent on the strength of the password you choose and the security of the device on which you run the tool.

Always use a strong, unique password.

Ensure your device is free from malware.

This tool is provided "as is" without warranty of any kind.
