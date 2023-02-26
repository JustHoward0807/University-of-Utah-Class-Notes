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
    Attacker only knows cyphertext(CT).

* **Known Plaintext**
    CT/PT pairs - know particular plain text encrypted cypher text is

* **Chosen Plaintext**
    Attacker can generate PT/CT pairs with PT of their choice.

#### Properties of CT to against this

* CT should appear random (But not actually random, otherwise, it can't be decrypted)
* Small change in PT -> Big change in CT. ***`"Avalanche Effect"`***

## Block Cyphers (Symmetric)

Operates on "block".
Plaintext: "Block sized" (produce) Cyphertext: same size.
**Security depends on key size and block size**
n bit key
$2^n$ keys expect $2(^n-^1)$ guesses to decode without knowing the key.

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

### 3 DES

(Lecture only talk not detail)
> **SLOW** & **Developed in Secret**

$$Encrypt(Decrypt(Encrypt(plainText, k1), k2))$$

### Advanced Encryption Standard (AES)

> <ins>**Developed by contest by Rinjdal**</ins>
> 128 bits block size (Rinjdal is variable)
> Key size: 128, 192, 256 bits
> Efficient in Software. Specialized hardware instructions on modern CPU's

Numbers of rounds encryption depends on key size: bigger key -> more rounds

Sub Bytes - Each byte in the state using a 256 entry table (defined by the standard).

``` C
b[i] = table[b[i]]
```

Shift rows - for each i rotate row i left i columns.

![Shift row](/CS6014/%20Cryptography/Images/Shift%20row.png)

``` C
// Note that row 0 is left alone
for i = 0...4
    rotate row[i] left
        i slots
```

Mix columns -

``` C
for each column
    for each row
        lookup column for byte[row][col]
        rotate that result
    xor all the column entrees together
```

Add Round key -
xor with the round key

## What to do with long messages? Stream Cypher

**Stream Cypher**
Take plain text split into bytes(bits), then create pseudo random number sequence called "key stream". Xor together.
![Stream Cypher](/CS6014/%20Cryptography/Images/Stream%20Cypher.png)
`CypherText xor Key stream -> (PlainText xor Key stream) xor key stream
PlainText xor (key stream xor key stream) = PlainText`
