# Crypto

## Vocab (Lingo)

1. Alice and Bob communicating.
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

> Take message one block size, and key -> produce one block of CT.

Operates on "block".
Plaintext: "Block sized" (produce) Cyphertext: same size.
**Security depends on key size and block size**
n bit key
$2^n$ keys expect $2(^n-^1)$ (i.e., all possible keys except the correct one) guesses to decode without knowing the key.

2 bit block -> <ins>12 possible output tables</ins>
how to crack this?
![block cypher](https://github.com/JustHoward0807/University-of-Utah-Class-Notes/blob/main/CS6014/%20Cryptography/Images/block%20cypher.png?raw=true)

64 bit block size
"Perfect" block cypher

* Output is totally random
* No leak information
* Can only with substitution
* Has avalanche effect

**Substitution table**
![Substitution table](https://github.com/JustHoward0807/University-of-Utah-Class-Notes/blob/main/CS6014/%20Cryptography/Images/Substitution%20table.png?raw=true)

### Mode of  Operation
Using Block Cipher on long message.
#### Electronic codebook (ECB) *Outdated*

![ECB](/CS6014/%20Cryptography/Images/ECB_encryption.png)
![ECB](/CS6014/%20Cryptography/Images/ECB_decryption.png)
> [!NOTE]
> The simplest (and not to be used anymore) of the encryption modes is the **electronic codebook** (ECB) mode. The message is divided into blocks, and each block is encrypted separately. Efficient, bc decrypt in parallel and access random.

| ECB                       |     |
| ------------------------- | --- |
| Encryption parallelizable | Yes |
| Decryption parallelizable | Yes |
| Random read access        | Yes |




#### Cipher Block Chaining (CBC)

![CBC](/CS6014/%20Cryptography/Images/CBC_encryption.png)
![CBC](/CS6014/%20Cryptography/Images/CBC_decryption.png)
> [!NOTE]
> In CBC mode, each block of plaintext is [XORed](https://en.wikipedia.org/wiki/XOR "XOR") with the previous cipher-text block before being encrypted. This way, each cipher-text block depends on all plaintext blocks processed up to that point. To make each message unique, **an [initialization vector](https://en.wikipedia.org/wiki/Initialization_vector "Initialization vector") must be used in the first block.**

| CBC                       |     |
| ------------------------- | --- |
| Encryption parallelizable | No |
| Decryption parallelizable | Yes |
| Random read access        | Yes |

#### Output feedback (OFB)

![OFB](/CS6014/%20Cryptography/Images/OFB_encryption.png)
![OFB](/CS6014/%20Cryptography/Images/OFB_decryption.png)
> [!NOTE]
> **The _output feedback_ (OFB) mode makes a block cipher into a synchronous [stream cipher](https://en.wikipedia.org/wiki/Stream_cipher "Stream cipher")**. It generates [keystream](https://en.wikipedia.org/wiki/Keystream "Keystream") blocks, which are then [XORed](https://en.wikipedia.org/wiki/XOR "XOR") with the plaintext blocks to get the ciphertext. **Just as with other stream ciphers, flipping a bit in the ciphertext produces a flipped bit in the plaintext at the same location. This property allows many [error-correcting codes](https://en.wikipedia.org/wiki/Error-correcting_code "Error-correcting code") to function normally even when applied before encryption.**
> ![OFB_formula](/CS6014/%20Cryptography/Images/OFB_formula.png)

| OFB                       |     |
| ------------------------- | --- |
| Encryption parallelizable | No  |
| Decryption parallelizable | No  |
| Random read access        | No  |



#### Counter (CTR)

![CTR](/CS6014/%20Cryptography/Images/CTR_encryption.png)
![CTR](/CS6014/%20Cryptography/Images/CTR_decryption.png)

> [!NOTE]
> **Like OFB, counter mode turns a [block cipher](https://en.wikipedia.org/wiki/Block_cipher "Block cipher") into a [stream cipher](https://en.wikipedia.org/wiki/Stream_cipher "Stream cipher")**. It generates the next [keystream](https://en.wikipedia.org/wiki/Keystream "Keystream") block by encrypting successive values of a "counter". The counter can be any function which produces a sequence which is guaranteed not to repeat for a long time, although an actual increment-by-one counter is the simplest and most popular.

| CTR                       |     |
| ------------------------- | --- |
| Encryption parallelizable | Yes |
| Decryption parallelizable | Yes |
| Random read access        | Yes |

#### Galois Counter (GCM) *Most popular*

![CTR](/CS6014/%20Cryptography/Images/GCM-Galois_Counter_Mode_with_IV.png)
Counter mode + authentication
Efficient. Its "Authenticated Encryption"

> [!NOTE]
> Galois/counter mode (GCM) combines the well-known counter mode of encryption with the new Galois mode of authentication. The key feature is the ease of parallel computation of the Galois field multiplication used for authentication. This feature permits higher throughput than encryption algorithms.

| GCM                       |     |
| ------------------------- | --- |
| Encryption parallelizable | Yes |
| Decryption parallelizable | Yes |
| Random read access        | Yes |

### DES - Data encryption Standard '77

//TODO: Look at youtube and find out what F (mangler function), key round is
64 bits Block
Key size: 56 bits (too small)

#### **"Feistel networks Network"**

![Feistel networks](https://github.com/JustHoward0807/University-of-Utah-Class-Notes/blob/main/CS6014/%20Cryptography/Images/Feistel%20networks.png?raw=true)

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

$Encrypt(Decrypt(Encrypt(plainText, k1), k2))$

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

## Stream Cypher

**Stream Cypher**
Take plain text split into bytes(bits), then create pseudo random number sequence called "key stream". Xor together. -> CT

> *$M xor ke*y$ -> **CipherText. Could be any size.**

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

## Hash Function

> H(x) -> fixed size hash
> H(k) = H(y) -> x == y. Hash the same input the same.

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

| Username | ==Salt== | H(password + salt) |
| -------- | -------- | ------------------ |
| Ben      | random   | ...                |
| Varun    | random   | ...                |
| Nabil    | random   | ...                |
Hash functions for password should be slow. Because more slow take longer time to guess. **Don't work well in GPU.**
Another thing can slow down is too increase number of time running hashes.

**==Bcrypt==** designed for password hashing. Pretty good. Not for general purpose message authentication. And would not want to use SHA-256 for password.

## Public Key Crypto

> Solved the problem getting shared secret key over internet.
> Establish a shared secret on the insecure internet.
> Invented in 1970's by Diffie & Hellman.

%%***Insert image of public key crypto***%% 
### Diffie & Hellman key exchange:

> Generate shared secret.

1. Agree an G and N.
2. Each pick secret #
3. Send G to the secret number % N, the other side %%==Check this part==%%
4. Both side do same thing, they have number they know.

**SA = "private key"**. - Not sent and share with anybody, 
**TA = "public key"**. - Mathematical related the private key, its shared safe to public. Doesn't reveal.

### RSA

> Sign/Encrypt

$M(^x*x) mod N$ = M (Get back M).
X,Y,M chosen carefully for this to work.

Start with 2 big prime numbers (randomly chosen).
Do some `number theory`. 
And end up with three numbers: (d ,e ,n). $m(^de) mod N$ = M for all M. (d & e and N is carefully chosen)
public key -> <e, N>
private key -> <d, N>

**==Signature==** - Anyone can verify. take signature and raise to public power and compare message and match means that it come from someone who have the private key.
$M(^pvK) mod N$ = Signature -> Verify with public key
$M(^pbK) mod N$ = Encryption -> decrypt with private key

**Encrypt/Sign/Decrypt/Verify**

We want DH, but want to replace $X(^ymodN)$ with something similar. Properties we want have to be able to change things. Associative, get some value on both side,  fast exponential trick works.

**Elliptic Curves** using now in DH

> [!NOTE] [Elliptic-curve Diffie–Hellman (ECDH)](https://en.wikipedia.org/wiki/Elliptic-curve_Diffie%E2%80%93Hellman#:~:text=Elliptic%2Dcurve%20Diffie%E2%80%93Hellman%20(,or%20to%20derive%20another%20key.)
>  Is a key agreement protocol that allows two parties, each having an elliptic-curve public–private key pair, to establish a shared secret over an insecure channel.This shared secret may be directly used as a key, or to derive another key.

Benefits: G and the N can be much smaller so faster. Research further behind, so no discover weakness for long time.

Replace $g^e mod N$, it gonna be fast and secure.
Method: "Multiplication of points on an EC"
* $e * g$. e is number, g is point. -> (g+g+g+..... (e times))
![Elliptic-curve](/CS6014/%20Cryptography/Images/Elliptic-curve.png)

## Crypto System
***(Add all things together)***

Sending a password before authentication is a bad idea.
* **Authentication**
	Need someway to identify people.
	* **Certificate**
		With someone with trust signature and sign
		$TrustedSign(publicKey + Sign[publicKey])$.
		If don't trust so much, we can sign with more.
		$ExtraTrustedSign(TrustedSign(publicKey + Sign[publicKey]))$.
		***"Trust store": Set of certificates that we trust without further verification.***
	* **Shared Secret Based Protocols**
		"Sending a challenge"
		The simplest example of a challenge–response protocol is [password](https://en.wikipedia.org/wiki/Password "Password") authentication, where the challenge is asking for the password and the valid response is the correct password.
		An adversary who can eavesdrop on a password authentication can then authenticate itself by reusing the intercepted password. One solution is to issue multiple passwords, each of them marked with an identifier. The verifier can then present an identifier, and the prover must respond with the correct password for that identifier. Assuming that the passwords are chosen independently, an adversary who intercepts one challenge–response message pair has no clues to help with a different challenge at a different time.
		For example, when other [communications security](https://en.wikipedia.org/wiki/Communications_security "Communications security") methods are unavailable, the [U.S.](https://en.wikipedia.org/wiki/United_States "United States") military uses the [AKAC-1553](https://en.wikipedia.org/w/index.php?title=AKAC-1553&action=edit&redlink=1 "AKAC-1553 (page does not exist)") TRIAD numeral cipher to authenticate and encrypt some communications. TRIAD includes a list of three-letter challenge codes, which the verifier is supposed to choose randomly from, and random three-letter responses to them. For added security, each set of codes is only valid for a particular time period which is ordinarily 24 hours.
	 * **Mutual Authentication**




