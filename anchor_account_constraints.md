# Account Constraints

Account constraints are applied to an account in a Account struct with the #[account(...)] attribute

### Normal Constraints
- #[account(signer)]: used to check that the account signed the transaction, consider using the Signer account
  if this is the only thing you want to check, Signer<'info>
  - custom errors can be supported via the @
    #[account(signer @ MyError::MyErrorCode)]

- #[account(mut)]: used to check that the account is mutable, state change are persisted on the account
  Note: if the mut is not provided and state is changed on the account in the instruction, the state change would not
  be persisted on chain and the instruction would silently pass
  - custom errors can be supported via the @
    #[account(mut @ MyError::MyErrorCode)]

- #[account(init)]: creates an account with CPI to system program, makes the account mutable, ensures the account is rent free
  excepted skipped by passing the ```rent_exempt = skip``` constraint

  - Required constraints when using ```init```
    - requires a ```payer``` constraint to be an account that pays for the account creation
    - requires the ```system program``` to exist in the struct and be called system_program
    - requires the ```space``` constraint is specified (remember to add 8 bytes for discreminator, this only applies to account owned by anchor)

  - Optional constraints when using init
    - ```owner```: to set the owner of the created account to an account other than the current program
    - use ```seeds``` together with ```bump``` to create PDAs, the bump value can be left empty
    - 

- #[account(init_if_needed)]: exact same functionality as init, but does not create the account if it exists already
  all other checks are also done even if the account exists already

## Note
- be weary of the init_if_needed constraint, as it can lead to potential reinitialization of an account or resetting of an account
- Always breaks your instruction into multipe execution flows, one for initialization, and another for using your account


- #[account(seeds = <seeds>, bump)]: checks that the provided pda is one derived from the current program with the provided seed and bump if provided

  - Optional constraints
    - you can use the ```seeds::program = <other_program>.key()``` to check that the pda belongs to another program
    - 

- #[account(has_one = <target_account>)]: this checks that the target_account key matches the same name attribute of the current account,
   
   ```rust
   #[account(has_one = signer)]
   pub account: Account<'info, DataAccount>,
   pub signer: Signer<'info>
   ```

   this checks that the ```account.signer == signer.key()```

   - custom errors can be supported via the @
    #[account(has_one = <target_account> @ <custom_error>)]

- #[account(address = <expr>)]: checks that the account key matches Public key
  
  ```rust
  #[account(address = crate::ID)]
  pub account: Account<'info, DataAccount>
  ```

  - custom errors can be supported via the @
    #[account(address = <expr> @ <custom_error>)]

- #[account(owner = <expr>)]: checks that the account owner matches the expr

  ```rust
  #[account(owner = Token::ID)]
  pub account: Account<'info, DataAccount>
  ```

  - custom errors can be supported via the @
    #[account(owner = <expr> @ <custom_error>)]


- #[account(executable)]: checks the account is a program, you might want to use the Program Account type instead

- #[account(close = <target_account>)]:
  - Closes the account by:
    - Sending the lamports to the specified account
    - Assigning the owner to the System Program
    - Resetting the data of the account

  Requires mut to exist on the account.
   
  ```rust
  #[account(mut, close = receiver)]
  pub data_account: Account<'info, MyData>,
  #[account(mut)]
  pub receiver: SystemAccount<'info>
  ```

- 
