# Execution & Delegation Interfaces

## **Universal Transaction Execution Interface**

All token bound accounts MUST implement an execution interface which allows valid signers to execute arbitrary operations on behalf of the account. Support for an execution interface MUST be signaled by the account using ERC-165 interface detection. `isValidExecutor` enables outsourcing specific contract interactions, such as automated staking, to a trusted contract on behalf of the account.

```solidity
/// @dev the ERC-165 identifier for this interface is `0x51945447`
interface IERC6551Executable {
     /**
      * @dev Executes a low-level operation if the caller is a valid signer on the account.
      *
      * Reverts and bubbles up error if operation fails.
      *
      * Accounts implementing this interface MUST accept the following operation parameter values:
      * - 0 = CALL
      * - 1 = DELEGATECALL
      * - 2 = CREATE
      * - 3 = CREATE2
      *
      * Accounts implementing this interface MAY support additional operations or restrict a signer's
      * ability to execute certain operations.
      *
      * @param to        The target address of the operation
      * @param value     The Ether value to be sent to the target
      * @param data      The encoded operation calldata
      * @param operation A value indicating the type of operation to perform
      * @return The result of the operation
      */
     function execute(address to, uint256 value, bytes calldata data, uint8 operation)
         external
         payable
         returns (bytes memory);
      
     /**
      * @notice Returns whether a given account is authorized to execute transactions on behalf of
      * this account
      *
      * @param executor The address to query authorization for
      * @param to        The target address of the operation
      * @param data      The encoded operation calldata
      * @return True if the executor is authorized, false otherwise
      */
     function _isValidExecutor(address executor, address to, bytes calldata data) 
        internal 
        view 
        virtual 
        override 
        returns (bool); 
}
```

## **Account Execution Delegation**

The `ExecutionDelegation` interface provides a standardized mechanism for token bound accounts to manage permissions for delegators to execute specific functions on behalf of the account (aka whitelist). Each account includes a boolean flag that can be set by the account owner to enable or disable the delegation feature. When the delegation feature is enabled, the account’s delegation permissions are managed by a trusted `Account Execution Delegation` contract, which controls the authorized functions globally for accounts, offering a standardized approach to permission management. This interface enables the account (or the trusted contract, when delegation is enabled) to authorize and revoke permissions for delegators to call particular functions, identified by their 4-byte signatures, enhancing the flexibility and security of account interactions.

```solidity
interface ExecutionDelegation {
     /**
     * @dev Authorizes the specified delegator to execute the function identified by the given 4-byte signature on the specified contract (`to`) on behalf of the account.
     * @param delegator The address of the entity being granted permission.
     * @param to The address of the contract that the delegator is allowed to interact with.
     * @param functionSignature The 4-byte signature of the function that the delegator is allowed to execute on the `to` contract.
     * @notice This function should only be called by authorized entities, such as the account owner or an authorized executor.
     */
    function authorizeDelegator(address delegator, address to, bytes4 functionSignature) external;
    /**
     * @dev Revokes the permission previously granted to the specified delegator for the function identified by the given 4-byte signature on the specified contract (`to`).
     * @param delegator The address of the entity whose permission is being revoked.
     * @param to The address of the contract for which the permission is being revoked.
     * @param functionSignature The 4-byte signature of the function for which permission is being revoked.
     * @notice This function should only be called by authorized entities, such as the account owner or an authorized executor.
     */
    function revokeDelegator(address delegator, address to, bytes4 functionSignature) external;
    /**
     * @dev Returns an array of 4-byte function signatures that the specified delegator is authorized to execute on the specified contract (`to`) on behalf of the account.
     * @param delegator The address of the entity to check authorizations for.
     * @param to The address of the contract to check authorizations against.
     * @param functionSignature The 4-byte signature of the function for which permission is being revoked.
     * @return a bool for whether to execute on the `to` contract.
     * @notice This function does not modify state and can be called by anyone to check the permissions of a delegator for a specific contract.
     */
    function isAuthorized(address delegator, address to, bytes4 functionSignature) external view returns (bool);
}
```
