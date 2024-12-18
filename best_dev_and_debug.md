# Best Development and Debug Practices

- Use Modules
  Follow the following program structure for your programs
  ```
   program_folder/
        src/ 
            / instructions
            / state
            / errors.rs
            / lib.rs
        / Cargo.Toml
  ```
> [!NOTE]  
- Starting with Anchor v0.29.0, we can use ```anchor init -t multiple <NEW_PROGRAM_NAME>``` to generate this structure above

- Always check the data received in your solana program
- Test very widely
  - shuffle instruction accounts 
  - test invalid inputs
  - test calculations
  - fuzz tests

  - when testing and some transactions fails due to not been able to be simulated
    you can use skipPreflight on the transaction call that caused that fail to see a better error message in the logs

