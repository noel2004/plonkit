# zkUtil ![GitHub Workflow Status](https://img.shields.io/github/workflow/status/poma/zkutil/Rust) [![Crates.io](https://img.shields.io/crates/v/zkutil.svg)](https://crates.io/crates/zkutil)

A tool to work with zkSNARK circuits generated by circom compiler. Based on circom import code by @kobigurk

**The generated keys are currently incompatible with upstream Websnark and require using a custom fork** 

## Prerequisite
+ `npm install circomlib@0.2.4`
+ `npm install snarkjs@0.3.44`
+ axel

## Usage examples:

```shell script
# Getting help
> zkutil --help
zkutil
A tool to work with SNARK circuits generated by circom

USAGE:
    zkutil <SUBCOMMAND>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    export-keys          Export proving and verifying keys compatible with snarkjs/websnark
    generate-verifier    Generate verifier smart contract
    help                 Prints this message or the help of the given subcommand(s)
    prove                Generate a SNARK proof
    setup                Generate trusted setup parameters
    verify               Verify a SNARK proof

# Getting help for a subcommand
> zkutil prove --help
zkutil-prove
Generate a SNARK proof

USAGE:
    zkutil prove [OPTIONS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
    -c, --circuit <circuit>    Circuit R1CS or JSON file [default: circuit.r1cs]
    -p, --params <params>      Snark trusted setup parameters file [default: params.bin]
    -r, --proof <proof>        Output file for proof JSON [default: proof.json]
    -i, --public <public>      Output file for public inputs JSON [default: public.json]
    -w, --witness <witness>    Witness JSON file [default: witness.json]

# Suppose we have circuit file and a sample inputs
> ls
circuit.circom  input.json

# Compile the circuit
> circom -rw circuit.circom
Constraints: 10000
Constraints: 20000
Constraints: 30000
Constraints: 40000
Constraints: 50000

# Generate a local trusted setup
> zkutil setup
Loading circuit...
Generating trusted setup parameters...
Has generated 28296 points
Writing to file...
Done!

# Calculate witness from the input.json
# At the moment we still need to calculate witness using snarkjs
> snarkjs calculatewitness

# Generate a snark proof
> zkutil prove
Loading circuit...
Proving...
Saved proof.json and public.json

# Verify the proof
> zkutil verify
Proof is correct

# Generate a solidity verifier contract
> zkutil generate-verifier
Created verifier.sol

# Export keys to snarkjs/websnark compatible format
> zkutil export-keys
Exporting params.bin...
Created proving_key.json and verification_key.json

# Verify the same proof with snarkjs
> snarkjs verify
OK

# Here's a list of files that we have after this
> ls
circuit.circom  circuit.r1cs  circuit.wasm  input.json  params.bin  proof.json  public.json  Verifier.sol  proving_key.json  verifying_key.json  witness.json
```

Also see `test_poseidon_groth16.sh` and `test_poseidon_plonk.sh` for example

# Installation

Install Rust

```shell script
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Install zkutil globally

```shell script
cargo install zkutil
# Make sure `~/.cargo/bin` is in $PATH (should be added automatically during Rust installation)
```

Or alternatively you can compile and run it instead:

```shell script
git clone https://github.com/poma/zkutil
cd zkutil
cargo run --release -- prove --help
```
