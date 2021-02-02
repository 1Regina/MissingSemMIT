1. Last year's [security and privacy lecture](https://missing.csail.mit.edu/2019/security/) focused on security for the user.
2. Entropy is a measure of randomness so correct horse battery staple is better than @,0 uses in passwords. Entropy formula is ```log_2 (# of possibilities)```
3. ### Hash Functions ###
   1.  A hash function example is **SHA1** which is used in Git. It maps arbitrary-sized inputs to 160-bit outputs (which can be represented as 40 hexadecimal characters). Use ```sha1sum``` for SHA1. But note that it is no longer considered a strong cryptographic hash function.
        ```
        $ printf 'hello' | sha1sum
        aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d
        $ printf 'hello' | sha1sum
        aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d
        $ printf 'Hello' | sha1sum 
        f7ff9e8b7bb2e09b70935a5d785e0cc5d9d0abf0

        ```
    2. Properties
        * Deterministic: the same input always generates the same output.
        * Non-invertible: it is hard to find an input m such that hash(m) = h for some desired output h.
        * Target collision resistant: given an input m_1, it’s hard to find a different input m_2 such that hash(m_1) = hash(m_2).
        * Collision resistant: it’s hard to find two inputs m_1 and m_2 such that hash(m_1) = hash(m_2) (note that this is a strictly stronger property than target collision resistance).
    3. Applications in Git and Commitment Schemes (The head or tails coin toss) to ensure no cheating after the guesser reveals.
4.  ### Key Derivative Functions ### 
    1. Many application of [key derivative functions](https://en.wikipedia.org/wiki/Key_derivation_function) uses but usually slow to slow down offline brute-force attacks.
    2. Applications
       * Produce keys from passphrases for use in other cryptographic algorithms
       * Storing login credentials so that even when database is hacked, without the hashed algo, the hacker cant obtain the passwords. So generate and store a random [salt](https://en.wikipedia.org/wiki/Salt_(cryptography)) ```salt = random()``` for each user, store ```KDF(password + salt)```, and verify login attempts by re-computing the KDF given the entered password and the stored salt.

5.  ### Symmetric Cryptography ### 
    1. Symmetric cryptography needs the key to encrypt the message. Likewise, the key is needed to decipher. has this functionality
        ```
        keygen() -> key  (this function is randomized)

        encrypt(plaintext: array<byte>, key) -> array<byte>  (the ciphertext)
        decrypt(ciphertext: array<byte>, key) -> array<byte>  (the plaintext)
        ```
        * Input (plaintext) + key = Output (cipher text)
        ```
        decrypt(encrypt(m, k), k) = m
        ```
    2. Encrypting files for storage in an untrusted cloud service ie combined with KDFs, so you can encrypt a file with a passphrase. Generate ```key = KDF(passphrase)```, and then store ```encrypt(file, key)``` .
   
6.  ### Asymmetric Cryptography ###
    1.  2 different keys 
            1.  Private key: meant for u to produce signature 
            2.  Public key : can be shared publicly and wont affect security. To verify.
        ```
        keygen() -> (public key, private key)  (this function is randomized)

        encrypt(plaintext: array<byte>, public key) -> array<byte>  (the ciphertext)
        decrypt(ciphertext: array<byte>, private key) -> array<byte>  (the plaintext)

        sign(message: array<byte>, private key) -> array<byte>  (the signature)
        verify(message: array<byte>, signature: array<byte>, public key) -> bool  (whether or not the signature is valid)
        ```
    2. Somewhat similar to symmetric cryptosystems. A message can be encrypted using the public key.
    3. The decrypt function has the obvious correctness property, that ```decrypt(encrypt(m, public key), private key) = m```.
7. ### Symmetric vs Asymmetric Cryptography ### 
    1. Symmetric cryptosystem ~ a door lock: anyone with the key can lock and unlock it.
    2. Asymmetric encryption ~ a padlock with a key. You could give the unlocked lock to someone (the public key), they could put a message in a box and then put the lock on, and after that, only you could open the lock because you kept the key (the private key).
    3. Sign/ Verify Process ~ physical signatures
       1. Private key for producing the signature such that ```verify(message, signature, public key)``` returns true ie ```verify(message, sign(message, private key), public key) = true```.
    4. Applications
       1. Private messaging. Apps like [Signal](https://signal.org/) and [Keybase](https://keybase.io/) use asymmetric keys to establish private communication channels.
       2. Signing software. Git can have GPG-signed commits and tags. With a posted public key, authenticity of downloaded software can be verified.
    5. Challenge - distributing public keys / mapping public keys to real-world identities & Solutions....
       1. Trust on first use and support out-of-band public key exchange (you verify your friends’ “safety numbers” in person - Signal
       2. [Web of trust](https://en.wikipedia.org/wiki/Web_of_trust) at PGP
       3. [Social Proof](https://keybase.io/blog/chat-apps-softer-than-tofu) with Keybase --> liked by MIT 
8.  ### Other passwords security ###
    1.  