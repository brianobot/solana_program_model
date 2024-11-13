# Cross Program Invocation


## Program (Recap)
- piece of code that is deployed to and runs on the node (validator)
- similar to rust binaries (has an entrypoint function instead of the usual main in rust)
- programs are state, they operate upon data give to them through the account they act


## Meaning
one program invokes the instructions of another program, signer privileges are extended down the depth of invocation, 
this means, when a user sign a transaction in a program, and the program invokes another program, the user signature is used
to sign the invoked program and so on, and the maximum depth of program invocation is 4, 

We have 2 functions for calling programs from other programs
- invoke(): used when making a CPI that does not require PDA signers
  ```rust
  pub fn invoke(
        instruction: &Instruction,
        account_infos: &[AccountInfo<'_>],
  ) -> Result<(), ProgramError> 
  ```
  signers provided to the caller program automatically extends to the callee programs

- invoke_signed(): Used when making a CPI that require PDA signers
  ```rust
  pub fn invoke(
        instruction: &Instruction,
        account_infos: &[AccountInfo<'_>],
        signers_seeds: &[&[&[u8]]] // these are the seeds used to derive the signers PDA
  ) -> Result<(), ProgramError> 
  ```

### NOte: 
- PDA can sign CPI and not normal transaction directly

### Security
- 
- if a valid PDA is found the address is added as a valid signer
