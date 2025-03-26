# ClarityWill -  Enhanced Smart Contract for Automated Will Execution

## Overview
This smart contract is designed for the automated execution of a will upon the confirmation of the testator's death. It includes features such as time-locked distributions, multi-oracle death verification, NFT inheritance, phased inheritance releases, and a dispute resolution mechanism. It is built to ensure that the inheritance process is secure, transparent, and governed by defined rules.

### Key Features:
- **Automated Execution of Will**: The will’s execution is triggered upon confirmation of death, based on a multi-oracle mechanism.
- **Time-Locked Distributions**: Beneficiaries’ inheritance can be time-locked, preventing early claims before the specified block height.
- **NFT Inheritance**: NFTs can be included as part of the inheritance, and their ownership is transferred to the beneficiaries.
- **Phased Inheritance**: The inheritance can be distributed in phases, ensuring gradual releases.
- **Multi-Oracle Death Verification**: The death of the testator must be confirmed by a set number of oracles before the inheritance can be claimed.
- **Dispute Resolution**: There is a mechanism for beneficiaries or other participants to raise disputes with an evidence submission, which can be voted on and resolved.
- **Inheritance Tax**: A fixed percentage of the inheritance is deducted as an inheritance tax for contract maintenance.

## Smart Contract Structure

### Variables:
1. **Contract Owner**:
   - `contract-owner`: The principal who deployed the contract (will be responsible for managing the contract).

2. **Oracles**:
   - `oracles`: A map tracking all oracle principals authorized to confirm the testator's death.
   - `required-confirmations`: The number of oracles needed to confirm the death.
   - `confirmation-count`: The number of death confirmations received so far.
   - `death-confirmed`: A boolean flag indicating whether the death has been confirmed.

3. **Beneficiaries**:
   - `beneficiaries`: A map tracking beneficiaries, their shares, claimed status, time-lock for claims, and associated NFT tokens.
   - `total-shares`: The total percentage of shares allocated among beneficiaries (should equal 100).

4. **NFT Ownership**:
   - `nft-ownership`: A map to track which beneficiary owns a specific NFT token.

5. **Will Document**:
   - `last-will-hash`: A hash representing the last version of the will document.

6. **Tax**:
   - `inheritance-tax`: A fixed percentage (e.g., 2%) of the inheritance that is deducted for contract maintenance.

### Error Codes:
- `ERR-NOT-AUTHORIZED`: The caller is not authorized to perform the action.
- `ERR-ALREADY-CLAIMED`: The beneficiary has already claimed their inheritance.
- `ERR-INVALID-SHARE`: The share percentage is invalid.
- `ERR-NOT-ACTIVE`: The contract is inactive.
- `ERR-DEATH-NOT-CONFIRMED`: Death has not been confirmed by the required oracles.
- `ERR-TIME-LOCK`: The time-lock has not expired for claiming inheritance.
- `ERR-INVALID-NFT`: The NFT ID is invalid.
- `ERR-INSUFFICIENT-CONFIRMATIONS`: Not enough oracles have confirmed the death.
- `ERR-PHASE-1-NOT-CLAIMED`: Phase 1 of the inheritance has not been claimed.
- `ERR-ALREADY-VOTED`: The user has already voted on the dispute.
- `ERR-NO-DISPUTE`: No dispute exists for the action.

### Core Functions:

#### Contract Initialization:
- **initialize-contract**: Initializes the contract and sets up multiple oracles who will confirm the testator’s death. Only the contract owner can initialize the contract.

#### Beneficiary Management:
- **add-beneficiary**: Adds a beneficiary to the will, with a specified share, lock period, and a list of NFTs allocated to them. Only the contract owner can add beneficiaries.

#### Death Confirmation (Multi-Oracle):
- **confirm-death**: Oracles confirm the death of the testator. The contract requires a minimum number of confirmations before the inheritance can be claimed.

#### Last Will Update:
- **update-will-hash**: Updates the hash of the last will document. Only the contract owner can update the will hash.

#### Claim Inheritance:
- **claim-inheritance**: Allows a beneficiary to claim their inheritance after the death confirmation. It checks for time-locks and ensures that the beneficiary has not already claimed their share. Inheritance includes both STX (Stacks tokens) and NFTs.

#### NFT Transfer:
- **transfer-nft**: Transfers an NFT to the beneficiary based on the NFT IDs allocated to them.

#### Dispute Resolution:
- **raise-dispute**: Allows a beneficiary or other participant to raise a dispute with an evidence hash, which will be reviewed and voted on.
- **resolve-dispute**: Resolves the dispute once the required voting threshold is met.

#### Phased Inheritance Release:
- **claim-phase-1**: Allows beneficiaries to claim the first phase of their inheritance, ensuring it is not claimed multiple times.
- **inheritance-phases**: Manages the tracking of inheritance distribution in phases.

#### Emergency Functions:
- **deactivate-contract**: Allows the contract owner to deactivate the contract, preventing further execution.
- **update-required-confirmations**: Allows the contract owner to update the number of confirmations required for the death to be confirmed.

#### Getters:
- **get-beneficiary-info**: Retrieves the information of a specific beneficiary.
- **get-contract-status**: Provides the status of the contract, including whether it is active, death confirmation status, and other key details.
- **get-nft-owner**: Retrieves the current owner of a specific NFT token.

### Workflow

1. **Initialization**:
   The contract owner initializes the contract by adding oracles who will confirm the death.

2. **Beneficiary Setup**:
   The contract owner adds beneficiaries, specifying their inheritance share, lock period (time-lock), and any NFTs allocated to them.

3. **Death Confirmation**:
   Oracles confirm the death of the testator. Once enough confirmations are received, the death status is marked as confirmed.

4. **Claiming Inheritance**:
   Beneficiaries can claim their inheritance once the death is confirmed and any time-locks have expired. The inheritance includes both STX tokens (minus tax) and NFTs.

5. **Dispute Resolution**:
   If there is a dispute, it can be raised by a participant, and the dispute is resolved through voting. If the required threshold is met, the dispute is resolved.

6. **Phased Inheritance**:
   Beneficiaries can claim their inheritance in phases. Phase 1 and Phase 2 amounts are predefined, and each phase must be claimed separately.

## Security and Auditing

- **Multi-Signature**: Death confirmation requires multiple oracles, reducing the risk of single-point failures.
- **Time-locks**: Time-locks ensure that beneficiaries cannot claim inheritance before the specified time.
- **NFT Ownership**: NFTs are transferred securely, and ownership is tracked through the contract.
- **Dispute Mechanism**: Disputes can be raised and voted on by the community or other interested parties, ensuring transparency in the inheritance process.

