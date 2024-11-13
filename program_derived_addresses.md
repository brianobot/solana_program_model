# Program Derived Addresses (PDA)

- Recap
  - All solana everything are stored in Accounts
  - think of it as key-value pair
  - this database is stored in validator
  - address of account 32 bytes long  public key

You can think of Public keys that are intentionally generated in such a way that they do not have private
keys


## Motivation
- when using key-pair approach, we need to keep track of all the data program owned by an account in our application
- but when using PDA, we can simply derive those data account addresses based on some agreed upon seed within our application
  this leads to deterministic account addresses
- so if you have 20 users, and these user have data account representing some information in your application
 you do noy have to store the addresses of all those 20 users data accoutn, but they can be derived, from a seed, the user public key and a bump

 Calculate for PDA
   - the first seed is the user public address
   - the second seed is some static string eg. favourite_color

 After the calculating, the resulting public key (PDA) is assured to always be the same

## NOTES
- do not have corresponding private keys
- derived to fall off the Ed25519 curve 
- calculated from (optional)seeds, program_id and bump (this is needed to ensure the derived address is off the curve)
- creating a PDA does not automatically create on-chain account


### Example
Imagine we have a program called Counter, that allows user to increment a global Count, and we have 2 users, alice and bob
our program would have a key-pair because our program is an account on the solana blockchain, and so would alice and bob account
now to store the state of the global counter, our program would use a data account to store that state, when bob or alice
which to access or modify that state, they must know the public key (location) of the data account that stores that count state,
this is a very simple case, but by using PDA, we do not have to keep track of the address of that data account, since it is not
deterministic and anyone with the rights seeds and program_id for our parent program can arrive at the counter data program location
(address)