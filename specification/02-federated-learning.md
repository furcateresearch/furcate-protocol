# Federated Learning Protocol

## Overview

Furcate implements federated averaging (FedAvg) with privacy enhancements for distributed model training across edge devices.

## Training Round Protocol

### 1. Round Initialization
Coordinator broadcasts training round parameters:
- Round ID
- Global model weights (encrypted)
- Minimum participants required
- Deadline timestamp

### 2. Local Training
Each participating device:
- Decrypts global model in TEE
- Trains on local data
- Computes gradients
- Encrypts gradients
- Generates attestation

### 3. Secure Aggregation
Coordinator:
- Collects encrypted gradients
- Verifies attestations
- Performs secure aggregation
- Updates global model

### 4. Model Distribution
Updated model distributed to participants

## Privacy Mechanisms

### Differential Privacy
- Gradient clipping
- Gaussian noise addition
- Privacy budget tracking (ε, δ)

### Secure Multi-Party Computation
- Additive secret sharing
- No plaintext access to individual updates
- Cryptographic guarantees

## Communication Protocol

```
Device → Coordinator: ModelUpdate message
Coordinator → Device: AggregatedModel message
```

See `schemas/messages.proto` for message definitions.

## Byzantine Resilience

- Gradient validation
- Outlier detection
- Reputation scoring
- Malicious device identification

## Optimization Techniques

- Gradient compression
- Quantization (8-bit, 4-bit)
- Sparse updates
- Model pruning
