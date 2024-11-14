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
Starting with Anchor v0.29.0, we can use ```anchor init -t multiple <NEW_PROGRAM_NAME>``` to generate this structure above