# Fast Multiparty Threshold ECDSA with Fast Trustless Setup
## Steven Goldfeder

# Digital signatures authorize transactions in cryptocurrencies
* Alice's device, containing her private key, is a single point of failure

# Multiparty authentication
* Instead of having a single device store your key material...
  - You can split it into multiple devices
  - Designed your address as protected by multiple keys
* Bitcoin has a multisignature feature that does this!
  - Specify a threshold of `t` keys that must sign to spend from that address (more generally, `m` of `n`)

# We good?
* Nope
* Problems with multisigs:
  * Multisigs are inflexible
    - Any change in policy (adding or removing a key) requires moving to a new address
  * Multisigs are public
    - Bad for privacy, all public keys need to be visible on-chain
  * Can get large; transactions contain `t` signatures instead of just 1

# Threshold signatures to the rescue!
* Is basically a "stealth" multisignature
* Difference:
  - The splitting is done client-side
  - But only a signature signature is produced
  - Private—nobody knows how it was created
  - Efficient—only a signle signature goes on the blockchain

# Multiparty protocols
  * Many of the protocols we've discussed involve one person with a key, splitting the key up to get more security over the key.
  * But other times you want multiple people to sign a document and aggregate signatures.
    - Big space / bandwidth savings!
    - Less verification time as well
  
# Signature aggregation for layer 2 protocols
  * State channels: transactions signed by all `n` participants
  * If everyone signs individually, `n` signatures must be posted/verified
  * Threshold `(n, n)` signatures => single signature


# No single point of failure
* Private key never has to exist on a single device
  - No trusted dealing setup - key can be generated in a distributed manner
  - Key not reconstructed when signing

# Threshold ECDSA
* The digital signature algorithm used by BTC, ETH, etc.
  - Developed by NIST, standard for elliptic curve digital signatures
* Can threshold signatures be done over ECSDA?
  - Yep.
  - We have developed a very efficient scheme for doing that.

# The difficulty of threshold signatures on ECSDA
* Given:
  - Group `G` of order `N`
  - Generator `g`
  - Private key `x`
* To sign a message `m`:
  - Pick a nonce `k` such that 1 <= k <= N - 1
  - `r` = `g^k`
  - `s = 1/k * (m + x * r) mod N`
  - Signature is `(r, s)`
* Multiplication, addition, inversion, and exponentiation make this a difficult protocol to implement in a distributed manner

# Previous work
* Built a threshold ECSDA from threshold additively homomorphic encryption (Paillier)
  - We know how to do distributed decryption, but don't know how to do efficient distributed key generation (for threshold Paillier)
  - Very expensive, may take hours to do
  - Only practical with a trusted dealer who generates the key and distributes it
  - Single point of failure!

# New protocol
* Doesn't use threshold Paillier, instead everyone has their own Paillier key
* Highly efficient dealerless setup, distributed key generation runs in <1 second
* Faster, simpler protocol
* Greatly reduces bandwidth

# Takeaway
* ECDSA threshold signatures are now here, and they're highly efficient!