# Cipher modes of operation
## What is a mode of operation?
Block ciphers, as the name suggests take their input in blocks of predefined length. For example DES has a block size of 8 bytes, whereas AES supports a block size of 16 bytes. But what do you do, if you want to encrypt more than those, say 16 bytes? Just split the stuff up in blocks of the block size and encrypt? That would be too easy. Well, these algorithms to chain together a single block encryption to a multi-block encryption are called modes of operation.
What will not be covered in this article is, what you do, if your input length does not divide the block size, i.e., if your cipher input does not fit into a multiple of blocks and how to fill the remaining block. For this, "padding" is the word you should google.
Let for the rest of the article be K a key, P_i the i-th block of the plaintext, C_i, the i-th block of the ciphertext. Furthermore let E_K() be the encryption with key K and D_K() the decryption with key K.

## ECB - Electronic Codebook
ECB is the naive approach. You split the text you want to encrypt in pairs, where the length equals the block size and encrypt one by one.
That looks loke the following:

    C_i=E_K(P_i)

and for decryption:

    P_i=D_K(C_i)

The speed is the same as that of the block cipher itself and both encryption and decryption are parallelizable.
Unfortunately the same plaintextblock will be encrypted to the same ciphertextblock and vice-versa. Thus this mode does not conceal any patterns in the plaintext. Thus it is possible to manipulate the ciphertext in a sensible way by repeating or permuting some decrypted blocks.
Any synchronizing errors will multiply. If you steal one bit, then the limits of the block do not match anymore. Thus all of the ciphertext up from the missing bit is lost.
Bit flipping is recoverable, since it only affects one block. This block is lost, but the rest remains untouched.
You see, it is a stupid idea to use ECB. ECB is just good for very small data. For example other keys.

## CBC - Cipher Block Chaining
In CBC, every plaintextblock is chained, that means xored, with the previous ciphertextblock. The first block is chained with a random initialisation vector (IV). This vector can be public. In fact, all the cipher text, which is the input of the chaining of the next plaintextblocks are public.
Pseudo code:

    C_i=E_K(P_i xor C_{i-1})

where the first P is an IV.
and for decryption:

    P_i=C_{i-1} xor D_K(C_i)

With this chaining, possible patterns in the plaintext will be concealed. The chaining increases the entropy of the cipher input. If the cipher output is impossible to distinguish from a random function, than the input to the next block is also indistinguishable from a random function, because it it some plaintext xored with something random-looking.
The cipher will possibly be one block larger than the plaintext and encryption is not parallelizable, whereas decryption can be parallelized.
Any synchronizing errors, as in the ECB-mode will multiply. CBC recovers also from bit flips. One block is then completely lost, and in the next block one bit is swapped at the same place as in the previous one. This is due to the xor of the bitflipped ciphertext with the plaintext. So, with malicious intention, one can flip certain bits in the next ciphertext block. This is bad, for some applications, for instance if you have a database, with bank transfers and want to change the cash, you do not care about the other blocks.

## CTR - Counter mode
In Counter mode, you build a stream generator from a block cipher. The generated stream can be xored with the plaintext to encrypt. The stream behaves like a random number generator. You have an inner state and a function to update this inner state. You can use a counter for this, or another pseudo-random number generator, if you like. But a simple counter suffices. Your output is then the encryption of the inner state.
Let CTR be the counter, i.e. the inner state. At first CTR is some IV.

    for i=1,...
      CTR++
      C_i=P_i xor E_K(CTR)

Decryption is analogous:

    for i=1,...
      CTR++
      P_i=C_i xor E_K(CTR)

Patterns in the plaintext will be obscured, and one can even encrypt multiple messages with the same key, if the IVs are different. On the other hand it is very easy to manipulate the plaintext by flipping some bits. But thus, flipped bits do not destroy the whole data. Synchronizing errors cannot be recovered.
The ciphertext has the same length as the plaintext. The output of the random number generator can be computed in advance.


# Summary
These are the most common modes for ciphers. Perhaps Cipher Feedback Mode (CFB) and Output Feedback Mode (OFB) are also commonly used. OFB has almost the same characteristics as CTR. There are a bunch of other rather exotic modes like Propagating Cipher Block Chaining (PCBC) or Plaintext Block Chaining (PBC). Like with all in cryptography you should avoid the exotic stuff. Stick with the thoroughly tested ones. They are known to be good. For disk encryption, when there are nearly no synchronisation errors, but bit flips, there are some alternative modes, which are also known to be good. They can be found in IEEE P1619.