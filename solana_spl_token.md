# Solana SPL Token

Tokens are essential part of all the blockchains
- Think of tokens as coins built to function on an already existing blockchains



In solana the relationship between the accounts involved in the token accounts looks like so

Token Programs is used to generate a 
- Mint account: this account contains metadata about the token in question like
  - supply
  - decimals
  - is_initialized
  - mint authority
  - freeze authority

- Token account


At the same time, the regular user wallet, is represented as a System account and would be involved in the
token process as the mint authority needed in the mint account metadata attribute.

- the process of generating new Token account from a mint account is called Minting
- when a new token account is generated, the owner attribute of the data field of the token account points to a wallet which
was most likely used the mint authority of the mint account that minted the token account
- In order to create a new token you create must first create a mint account
- In Order to hold a token, you must create a token account, which is like the vault
