# Contact Sharing


basic idea is to hash the pair of phone numbers (me, other) so that there are many collisions and then publish this (plausible deniable, because of collisions). and the contact your friend out of band if there is a hash that could be a match (which it is most likely not).



General problem: 
 - Data is readable by anyone
 - The problem is not so much, that the server must not learn the contacts, but  that 
     - no one must learn them, while all data is public

Alice announces its connection with Bob by:

- calculating H(A_phone_number + B_phone_number) 
- applying costly scrypt or PBKDF2 hashing to derive H'
- truncating H’ to a low number of bits H'', so that collisions are likely
- publicly registers its unique pubkey_alice_bob for  H''

Notes so far: 
- rainbow tables of 10^10^2 size (i.e. all phone number combinations) are impractical
- calculating all combinations is prohibitively costly 
- collisions of H’’ allow plausible deniability
- Problem: an adversary can impersonate Alice, need spam protection (PoW) here

Bob wants to know if Alice is participating in the network:
- Bob calculates H(A_phone_number + B_phone_number) 
- Bob generates H’ and H’'
- for all matching entries for H’’ (including the one generated by Alice)
 - Bob encrypts truncated H(mobile_bob) for pubkey_alice_bob
 - and sends the message to Alice inbox on the server
 - Alice loads and decrypts the message
 - Alice can match  truncated H(mobile_bob) in hear contact list with a very low false positive rate
 - if Alice thinks she knows Bob, she might reply out of band

Notes:
- Bob can still plausibly deny that he knows Alice
- Bob can still plausibly deny that he participates in the system
- add some proof of work or similar for spam protection

This is hard against:
- Leaking users contact lists
- An adversary with a hypothesis about a connection can learn bobs phone number though

Limitations:
- New users (Bob) won’t see others directly
- Existing users (Alice) need to invite Bob
- Quite some computation and communication overhead
 - ~ 500 x 10 messages send
 - 2x key pairs to remember for each


The Difficulty Of Private Contact Discovery 
https://whispersystems.org/blog/contact-discovery/
https://news.ycombinator.com/item?id=7007554
https://news.ycombinator.com/item?id=11288169
https://news.ycombinator.com/item?id=11289223

Lame solution which is prone to rainbow table attacks. 
https://github.com/SilentCircle/contact-discovery