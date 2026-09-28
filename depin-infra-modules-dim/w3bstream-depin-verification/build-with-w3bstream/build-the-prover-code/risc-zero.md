# Risc Zero

This section of the documentation explains how to create a W3bstream ZK prover using Risc Zero as the ZK framework.

<details>

<summary>Prerequisites</summary>

#### 1. Rust language

Ensure you have the Rust compiler correctly installed

```sh
rustc --version
```

#### 2. Cargo

Ensure you have cargo 1.72.0 or higher:

<pre><code><strong>cargo version
</strong></code></pre>

update cargo with:

```
rustup update
```

#### 3. rustzero toolchain

```
cargo install cargo-risczero
cargo risczero install
```

</details>

## Build the prover code

### Clone the W3bstream repository

Start by cloning the W3bstream repository, where you can start from a template prover:

```sh
git clone https://github.com/iotexproject/w3bstream.git
cd examples/risc0-circuit/
```

Open the folder in a code editor and make the required modifications based on your actual data message.

{% hint style="success" %}
Learn more about developing Risc Zero provers:

[→](https://dev.risczero.com/api/) [Go to the official Risc Zero documentation](https://dev.risczero.com/api/)
{% endhint %}

### Build the prover logic

```
cargo build --release
```

The prover bytecode is saved in a file named `methods.rs` inside the `release` folder. The correct path is logged at the end of the build:

```sh
warning: range-method@0.1.0: methods_path is: "/path/to/w3bstream/examples/risc0-circuit/target/debug/build/range-method-2bec077ba5c1d7b6/out/methods.rs"
```

The `method.rs` file also includes the image ID of the prover that is needed by the verifier when verifying a proof:

```javascript
pub const RANGE_ID: [u32; 8] = [512905798, 3876631256, 1803615688, 344980519, 987991463, 241449906, 813598757, 370978982];
```



<table data-view="cards"><thead><tr><th></th></tr></thead><tbody><tr><td><a href="../deploy-to-w3bstream/">-> Go to the <strong>Create a W3bstream Project</strong> section to create a W3bstream configuration file based on this prover</a>.</td></tr><tr><td><a href="../on-chain-integration/verify-risc0-proofs.md">→ See the <strong>On-chain Verification</strong> section to learn how to verify W3bstream proofs in your Dapp </a></td></tr><tr><td></td></tr></tbody></table>

