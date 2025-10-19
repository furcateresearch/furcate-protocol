# Proof of Authority Consensus

## Overview

Furcate uses Proof of Authority (PoA) for consensus among edge devices, optimized for permissioned industrial networks.

## Validator Selection

### Requirements
- Pre-authenticated organizational identity
- Hardware attestation capability (TPM/TEE)
- Minimum computational resources
- Network reliability threshold
- Reputation score > threshold

### Registration Process
1. Device submits registration with attestation
2. Organization validates identity
3. Initial reputation assigned
4. Added to validator pool

## Block Generation

### Leader Selection
Deterministic round-robin based on:
```
current_leader = validators[(current_time / block_interval) % num_validators]
```

### Block Structure
- Block number
- Previous block hash
- Timestamp
- Validator ID
- Model updates (transactions)
- Validator signature

## Consensus Rules

### Validity Conditions
- Correct leader for time slot
- Valid signature from leader
- Attestations verified
- No double-spending of compute

### Fork Resolution
- Longest chain wins
- Same length → higher cumulative reputation
- Validators penalized for forks

## Security Properties

### Byzantine Fault Tolerance
- Tolerates up to 51% malicious validators
- Validator banning mechanism
- Reputation-based trust

### Attack Resistance
- **51% Attack**: Requires compromising majority of known organizations
- **Time Manipulation**: Synchronized time with tolerance bounds
- **DoS**: Rate limiting, validator rotation

## Validator Reputation

### Scoring Factors
- Uptime percentage
- Correct attestations provided  
- Blocks produced on time
- Challenges passed

### Penalties
- Missed block: -10 reputation
- Invalid attestation: -50 reputation
- Byzantine behavior: removal

### Rewards
While Furcate doesn't require tokens, implementations may incentivize validators through:
- Organization recognition
- Priority inference slots
- Enhanced data access

## Performance Characteristics

- **Block Time**: 5 seconds (configurable)
- **Finality**: 1 block (no forks expected)
- **Throughput**: 1000+ model updates/block
- **Latency**: Sub-second confirmation

## Future Extensions

- Integration with blockchain networks (Minima, etc.)
- Cross-organization validator pools
- Dynamic validator set updates
- Stake-weighted voting (optional)
