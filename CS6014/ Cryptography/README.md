# Crypto

## Vocab (Lingo)

1. Alice and Bob communicating
2. Eve and Mallory in the middle. Stand for Eavesdropper and man in the middle.
3. Trudy means Intruder.

## Secure Communication

* **Confidentiality** - Nobody can read the message.
* **Data Integrity** - Msg received have the content that sender intended.
* **Authentication** -  Verify who the sender was.
* **Non-Repudiation** - Msg send from A cannot deny by the A.

## Adversaries

Assume adversaries have NSA data data center.

* **Man-in-the-middle(MITM)** - person have full network control between sender and receiver.
* **Eavesdropper** - Can listen to message but can't change them.

## Keys

* **Shared secret** - A value both A and B know but no one else.
* **Symmetric** - Same key be used for encryption and decryption.
* **Asymmetric** - different keys for encrypt and decrypt. These are mathematically linked.
  * Public key crypto - each user has a public key and private key.

## Caesar Cypher (Symmetric algorithm)

> Bc it is symmetric. U can encrypt and decrypt using same algorithm but different key. $$encrypt = CC(plainText, Key), decrypt = CC(cypherText, -Key)$$

Take each letter -> letter + 1
**Substitution** - replaced each bytes with different byte.

## Pig Latin

//TODO: Do more research on it.
It uses **Shifting and Permutation**

## Building blocks of cyphers

1. Must be **deterministic**(must be repeatable)
2. Efficient and fast.
3. Want cypher be random but cant be because it has to be reversible.

**Substitution**: Often bytes.
**Permutation**: Bit shifting. (xor)
Benefits of `xor` and `+`. `+` need carrying (which is inconvenient)

## Idea crypto-system to be secure

> Require attacker need to brute-force all possibilities.

### Kinds of attacks

* Most difficult: **Cyphertext only**
    The attacker has access only to the encrypted ciphertext, but not the corresponding plaintext or the key used to encrypt it. The goal of the attacker is to try to deduce any useful information about the plaintext or the key from the ciphertext.

* **Known Plaintext**
    CT/PT pairs - know particular plain text encrypted cypher text is
    The attacker has access to both the plaintext and the corresponding ciphertext. The attacker's goal is to use this information to try to deduce the key used to encrypt the plaintext.
    Caesar Cypher is well known weak to know plaintext attack.

* **Chosen Plaintext**
    Attacker can generate PT/CT pairs with PT of their choice.
    The attacker has the ability to choose and encrypt their own plaintext messages, and observe the resulting ciphertext. The attacker's goal is to use this information to try to deduce the key used to encrypt the plaintext. This type of attack is considered the most powerful, as the attacker has more control over the input data and can potentially learn more about the encryption system.

#### Properties of CT to against this

* CT should appear random (But not actually random, otherwise, it can't be decrypted)
* Small change in PT -> Big change in CT. ***`"Avalanche Effect"`***
The ciphertext avalanche effect is a desirable property of a secure cryptographic algorithm. It means that if a small change is made to the plaintext (even one bit), the resulting ciphertext should be drastically different.
The avalanche effect is achieved through the use of complex substitution and permutation operations such as AES.

## Block Cyphers (Symmetric)

Operates on "block".
Plaintext: "Block sized" (produce) Cyphertext: same size.
**Security depends on key size and block size**
n bit key
$2^n$ keys expect $2(^n-^1)$ (i.e., all possible keys except the correct one) guesses to decode without knowing the key.

2 bit block -> <ins>12 possible output tables</ins>
how to crack this?
![block cypher](/CS6014/%20Cryptography/Images/block%20cypher.png)

64 bit block size
"Perfect" block cypher

* Output is totally random
* No leak information
* Can only with substitution
* Has avalanche effect

**Substitution table**
![Substitution table](/CS6014/%20Cryptography//Images/Substitution%20table.png)

### DES - Data encryption Standard '77

//TODO: Look at youtube and find out what F (mangler function), key round is
64 bits Block
Key size: 56 bits (too small)

#### **"Feistel networks Network"**

![Feistel networks](/CS6014/%20Cryptography/Images/Feistel%20networks.png)

$^Kn$ = round key
Encryption and Decryption are same algorithm.
F function can be one way (not reversible)

Shared Secret Key k -> k0, k1, k2 `"Key scheduling algorithm"`
Don't have to be reversible. Both have same key so get same round key.
**Downsides of DES**
Designed for hardware implementation not software.
56 bits is too small for a key.
EFF built a DES cracker in the 90's

> Additional notes from chatGPT:
> DES is vulnerable to brute-force attacks.

### 3 DES

(Lecture only talk not detail)
> **SLOW** & **Developed in Secret**

$$Encrypt(Decrypt(Encrypt(plainText, k1), k2))$$

### Advanced Encryption Standard (AES)

> <ins>**Developed by contest by Rinjdal**</ins>
> 128 bits block size (Rinjdal is variable)
> Key size: 128 (10 rounds), 192(12 rounds), 256(14 rounds) bits
> Efficient in Software. Specialized hardware instructions on modern CPU's

Numbers of rounds encryption depends on key size: bigger key -> more rounds

1. Sub Bytes - Each byte in the state using a 256 entry table (defined by the standard). substitute

    ``` C
    b[i] = table[b[i]]
    ```

2. Shift rows - for each i rotate row i left i columns.

    ![Shift row](/CS6014/%20Cryptography/Images/Shift%20row.png)

    ``` C
    // Note that row 0 is left alone
    for i = 0...4
        rotate row[i] left
            i slots
    ```

3. Mix columns -

    ``` C
    for each column
        for each row
            lookup column for byte[row][col]
            rotate that result
        xor all the column entrees together
    ```

4. Add Round key - xor with the round key

[AES Explain](https://www.youtube.com/watch?v=O4xNJsjtN6E&t=326s)

## What to do with long messages? Stream Cypher

**Stream Cypher**
Take plain text split into bytes(bits), then create pseudo random number sequence called "key stream". Xor together. -> CT
![Stream Cypher](/CS6014/%20Cryptography/Images/Stream%20Cypher.png)
`CypherText xor Key stream -> (PlainText xor Key stream) xor key stream
PlainText xor (key stream xor key stream) = PlainText`

### Key stream

Pseudo-random (unpredictable, but not random(can't decrypt))

Way produce key stream:

#### "Cryptographically secure Pseudo random number generator (CSPRNG)"

CS requirements
attacker sees k bits of output: they can't predict next bit.

Maximize "period" of CSPRNG as long of a sequence as possible before getting duplicate internal state.

#### RC4 - (broken)

> Ron's Code. Ron Rivest

Initialization

1. takes a key
2. shuffle internal state based on that key

Generation
byte nextByte() - Give next byte also shuffle in internal state mix it can't tell what happened before.

Internal state
![RC4](/CS6014/%20Cryptography/Images/RC4.png)

**Init(key):**

```C
s = [0,1.......255]
j = 0
for i = 0...255
    j = (j + s[i] + key[i % length]) % 256
    swap(s[i], s[j])
i = j = 0
```

**byte nextByte():**

```C
i++ % 256
j += s[i] % 256
swap(s[i], s[j])
output = s[s[i]/s[j] % 256]
```

#### ChaCha20 (Used it now) Modern Stream cypher

3 inputs to "init"
init(key, stream pos, "nonce")
"nonce" -> number used once

xor rotate instead of shuffle array

#### Attack

"Bit flipping attack"
Only know the message and can modify the cypher text

Confidential
No message integrity

Block Cypher also have confidential, but has closer message integrity

## Hashing

Crypto Hash functions must be REALLY collision resistant.
Output should appear random - ***`"Avalanche Effect"`***
Don't want to be reversible. Intend to lose information.

Scenario:

* Attack knows input X someone try to send. And attacker want to find Y with H(x) == H(Y). It slightly harder.
* Attacker can pick X,Y. It easier. But still should be infeasible for attacker.
* Knowing H(x), still don't know anything about X.
  Sometime use hash function to lose information intentionally.

### Chosen prefix attack

Document -> H(Doc)
Forge Fake Document -> H (Doc)
// ~~~~

### Collision Avoidance

**Birthday Paradox**
2 people 364/365) not have the same day birthday.
2,3,4 people $(364/365) (363/365) (362/365)$

d possible hashes -> $2^b$
n hashes
50% chance of collision $ around sqrt(2log2*3)$ $sqrt(2log2 *2^b)$ = $2(^b/^2)$

### Examples "Merkle Damgard" (MD)

IV (Initialization Vector)
//TODO: Give images here

#### Wide used: MD5, SHA-1(Secure hash Algorithm 128bits) But broken

**SHA-2** (256, 512 bit SHA-1) Similar to SHA-1 but bigger.

**SHA-3** Totally different design to SHA-2.

**BLAKE** -> Built on ChaCha20

#### HMAC Message authentication Code

$$H(K+H(K+M))$$

Passwords
service don't want to know your password. They want to know that you know it.

Server cannot store x and y bc it might get leaded. So it store H(x). After log in it H(y) to see.

Server store hashes of password. They hash provided password on log in.
$H(x) = H(y)$
x = y
Difference encrypted password and hash password. Encrypted password is reversible.
