# Glossary for Midterm

- Security Attack: Any action that compromises the security of information
- Security Mechanism: A mechanism that is designed to detect, prevent, or recover from a security attack
- Security Service: A service that enhances the security of data processing systems and information transfers.
- Four attack types
  - Interruption -> affect Availability
  - Interception -> affect Confidentiality
  - Modification -> affect Integrity
  - Fabrication  -> affect Authenticity
- Symmetric-key Encryption: if for each (e, d), it is easy computationally easy to compute e knowing d and d knowing e.
  - usually e = d
- Block cipher: breaks plaintext into block of fixed length, and encrypts one block at a time
- Stream cipher: takes a plaintext string and produces a ciphertext string using **keystream**
  - block cipher, but block length = 1

---

- Digital signature: a data string which associates a message with some originating entity
- Digital signature generation algorithm: a method for producing a digital signature
- Digital signature verification algorithm: a method for verifying that a digital signature is authentic
- Digital signature scheme: consists of a signature generation algorithm and an associated verification algorithm

---

- hash function is a function h that
  1. compression: h maps an input x of arbitrary finite bitlength, to an output h(x) of fixed bitlength n.
  2. ease of computation: h(x) is easy to compute for given x and h
- properties
  1. one-way: for a given y, finding preimage is hard. (find x such that h(x) = y)
  2. collision resistance: find x and x' such that h(x) = h(x')

---

- Data integrity: the property whereby data has not been altered in an unauthorized manner since the time it was created, transmitted, or stored by an authorized source.
- message authentication (data origin): provide to one party which receives a message assurance of the identity of the party which originated the message
  - a type of authentication whereby a party is corroborated as the (original) source of specified data created at some (typically unspecified) time in the past
  - includes data integrity
- entity authentication (identification): one party of both the **identity** of a second party involved, and that the second was **active** at the time the evidence was created or acquired.

---

- Key establishment: process to whereby a shared secret key becomes available to two or more parties
  - subdivided into key agreement + key transport
- Key management: the set of processes and mechanisms which support key establishment
  - maintenance of ongoing keying relationships between parties
- Kerckhoff's principle: Security should depend only on the key.
  - no security through obscurity