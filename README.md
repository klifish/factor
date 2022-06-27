## Compiling circuits:
 `circom multiplier2.circom --r1cs --wasm --sym --c`
## Computing the witness: 
`node ./multiplier2_js/generate_witness.js ./multiplier2_js/multiplier2.wasm input.json witness.wtns`
## Proving circuits with ZK: 
### Trusted setup
1. Powers of tau
> - start a new "powers of tau" ceremony: `snarkjs powersoftau new bn128 12 pot12_0000.ptau -v`
> - contribute to the ceremony: `snarkjs powersoftau contribute pot12_0000.ptau pot12_0001.ptau --name="First contribution" -v`

2. Phase 2
> - start the generation of phase 2: `snarkjs powersoftau prepare phase2 pot12_0001.ptau pot12_final.ptau -v`
> - start a new zkey: `snarkjs groth16 setup multiplier2.r1cs pot12_final.ptau multiplier2_0000.zkey`
> - Contribute to the phase 2 of the ceremony: `snarkjs zkey contribute multiplier2_0000.zkey multiplier2_0001.zkey --name="1st Contributor Name" -v`
> - Export the verification key: `snarkjs zkey export verificationkey multiplier2_0001.zkey verification_key.json`

### Generating a Proof
generate a zk-proof:
`snarkjs groth16 prove multiplier2_0001.zkey witness.wtns proof.json public.json`

### Verifying a Proof
verify the proof:
`snarkjs groth16 verify verification_key.json public.json proof.json`

## Reference 
[Circom 2 Documentation](https://docs.circom.io/)