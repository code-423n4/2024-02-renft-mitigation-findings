# Lines of code

https://github.com/re-nft/smart-contracts/blob/97e5753e5398da65d3d26735e9d6439c757720f5/src/policies/Guard.sol#L1


# Vulnerability details

## [L-01] Rental safe owners are unable to upgrade to new Fallback policy implementations
If a new upgrade of the fallback policy was pushed to mainnet, the rental safe owners will not be able to upgrade to it even if the fallback policy is whitelisted. Simply because the guard bans any attempt to set a new fallback handler regardless of whether or not it's a whitelisted or blasklisted fallback policy

```solidity

            // Revert if the `setFallbackHandler` selector is specified.
            if (selector == gnosis_safe_set_fallback_handler_selector) {
                revert Errors.GuardPolicy_UnauthorizedSelector(
                    gnosis_safe_set_fallback_handler_selector
                );
            }

```

Additionally, if the protocol admins try to disable the old fallback policy after deploying the new ones. EIP-1271 support will break on all rental safes.


## [L-02] Any stuck/leftover tokens in the Create Policy can be hijacked

Any stuck/leftover tokens in the Create Policy can be hijacked using the attack vector described in my other report allowing flash theft of NFTs. Same attack vector can be used by an attacker to hijack any leftover funds in the Create Policy, with very small modifications (like there no being a need to transfer tokens back to create policy after using them, or no need for a legitimate order: a malicious zone order and a malicious pay order would be enough)


## Assessed type

Other