# Integration Example: Auto-Staking Workflow

The figure below demonstrates a typical workflow of how on‐chain rewards from a third‐party “Rewards Dapp” can be automatically re‑staked into a separate “Staking Dapp” via an off‑chain “Auto‑staking Service” and an MBA wallet module.&#x20;

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

The auto-staking process consists of the following five steps:

**Step 1: Rewards Distribution → MBA** The third‑party Rewards Dapp issues a token transfer to the MBA address (0x1234) by calling `transfer(address recipient, uint256 amount)` on the token contract. Those tokens land in the MBA’s on‑chain account.

**Step 2: Event Monitoring by Off‑chain Indexer** The Auto‑staking Service’s off‑chain indexer watches the blockchain for the `Transfer` event to the user’s MBA address. As soon as it sees a new reward deposit, it triggers the next step.

**Step3: Indexer → Auto‑staking Contract Invocation** The indexer sends an on‑chain transaction invoking the Auto‑staking Service’s smart contract, passing in the recipient address (0x1234) so that the service can act on behalf of that user.

**Step 4: Auto‑staking Contract → MBA DelegateCall** The Auto‑staking Contract uses Solidity’s `call` to execute a payload `{stake(uint256 amount)}` in the context of the MBA. This means the MBA itself is authorizing the staking action, preserving the user’s on‑device key/security assumptions.

**Step 5: MBA → Third‑party Staking Dapp** Finally, the  MBA makes a standard `stake(uint256 amount)` call to the third‑party Staking Dapp’s contract (staking service can be provided by other projects), locking the newly received reward tokens into the staking program.
