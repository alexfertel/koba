# koba

Generate deployment transaction data for Stylus contracts.

> [!WARNING]
> This project is still in a very early and experimental phase. It has never
> been audited nor thoroughly reviewed for security vulnerabilities. Do not use
> in production. Currently only works for a small subset of compiler outputs.

## Usage

For a contract like this:

```rust
sol_storage! {
    #[entrypoint]
    pub struct Counter {
        uint256 number;
    }
}

#[external]
impl Counter {
    pub fn number(&self) -> U256 {
        self.number.get()
    }

    pub fn set_number(&mut self, new_number: U256) {
        self.number.set(new_number);
    }

    pub fn increment(&mut self) {
        let number = self.number.get();
        self.set_number(number + U256::from(1));
    }
}
```

With a constructor like this:

```solidity
contract Counter {
    uint256 private _number;

    constructor() {
        _number = 5;
    }
}
```

the following command outputs the transaction data you would need to send to
deploy the contract.

```sh
$ koba generate --sol path_to_sol --wasm path_to_wasm
6080604052348015600e575f80fd5b...d81f197cb0f070175cce2fd57095700201
```

You can then use `cast` for example to deploy and activate the contract, like
this:

```sh
# Deploy the contract.
cast send --rpc-url https://stylusv2.arbitrum.io/rpc --private-key <private-key> --create <koba output>

# Activate the contract.
cast send --rpc-url https://stylusv2.arbitrum.io/rpc --private-key <private-key> --value "0.0001ether" 0x0000000000000000000000000000000000000071 "activateProgram(address)(uint16,uint256)" <contract address>

# Interact with the contract
cast call --rpc-url https://stylusv2.arbitrum.io/rpc <contract address> "number()"
0x0000000000000000000000000000000000000000000000000000000000000005

cast send --rpc-url https://stylusv2.arbitrum.io/rpc --private-key <private-key> <contract address> "increment()"

cast storage --rpc-url https://stylusv2.arbitrum.io/rpc <contract address> 0
0x0000000000000000000000000000000000000000000000000000000000000006
```

## Failure Cases

Currently, when there is a jump instruction gets executed with an argument that
was not pushed immediately before, the generated init_code fails to run.

The problem arises because we are doing passes over the bytecode without
actually emulating the stack, so there may be an arbitrary number of
instructions between a destination offset being computed, and it being used by
a jump instruction.
