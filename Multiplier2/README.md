# Verifying a proof from a Smart Contract

1) First, we generated the solidity code using the command: 
> snarkjs zkey export solidityverifier multiplier2_0001.zkey verifier.sol

After it must be analyzed the inputs in function `verifyProof()` at this .sol file.

2) Need to create `prove.sol` file for proving part:

```solidity
pragma solidity ^0.8.23;

import "./verifier.sol";

contract Prove is Groth16Verifier {
    event proved(address indexed _from, uint output, bool proved);

    constructor(){}

    function doProof(
        // these inputs from verifyProof() at verifier.sol file
        uint[2] calldata a,
        uint[2][2] calldata b,
        uint[2] calldata c,
        uint[1] calldata input
    )
    public {
        emit proved(msg.sender, input[0], verifyProof(a,b,c,input));
    }
}
```

3) Deploy this contract (`prove.sol`).
4) For ability to verify, generate the parameters of the call by typing:
> snarkjs generatecall

After generating, we received:

`["0x01dd525c38fceec59536b341d3b31dbdfc3013811d96444402032c9ca6b67129","0x188e056cb71d33fc8394413b04510d33384b590078d8acd1073c4fd8557c9f6d"],[["0x068426996a2a272639d531dcb137699a919ed8e9b5e7176805b839411acdeaa4","0x25602cce97b82c671d384a1d856ececce2aaa2af3b0a3514ecd549d075af5794"],["0x0d0e8ba98df41ac9cb065377c1f6f7410baeafed9a5489c7f8802f3b3b0168d7","0x1265dd5be1d4b4453878196df650052e3f30859bd502ddb91cf136f8a2103a47"]], 
["0x1423798ea2f90d682c050279ed2bdb0be54d6cfdf05200cac326f494e8f49a8e", "0x2bcdfecf2e913793230d8d17cc3c28b81fa228d7fc9b5792df609f1c3e2e6e35"],
["0x0000000000000000000000000000000000000000000000000000000000000021"]`

5) The received parameters put in space with button "verifyProof" and verify them. After verifying, we received:

`decoded output	{
"0": "bool: true"
}`

So, we successfully generated a Solidity verifier that allows verifying proofs on Ethereum blockchain, and verified our proof from a Smart Contract. 