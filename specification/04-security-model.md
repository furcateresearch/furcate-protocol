# Security Model

## Threat Model

### Assumptions
- Network adversaries can eavesdrop and inject messages
- Some edge devices may be compromised
- Coordinators may be honest-but-curious
- Physical access to devices is possible

### Non-Goals
- Protection against nation-state attackers with hardware implants
- Resistance to quantum computing (post-quantum crypto planned for v2)
- Covert channel elimination (timing attacks remain theoretical risk)

## Security Mechanisms

### 1. Encrypted Communication
- TLS 1.3 for all network traffic
- Perfect forward secrecy
- Certificate pinning for known devices

### 2. TEE Execution
- Intel SGX / ARM TrustZone / AWS Nitro
- Isolated memory regions
- Sealed storage for model weights
- Side-channel mitigations enabled

### 3. Attestation
- Remote attestation using platform quotes
- Measurement of enclave code (MRENCLAVE)
- Signature verification chain
- Replay protection via nonces

### 4. Differential Privacy
- Gaussian noise addition to gradients
- Privacy budget tracking (ε, δ parameters)
- Gradient clipping
- Composition theorems for multi-round privacy

### 5. Secure Aggregation
- Additive secret sharing
- Masked model updates
- Dropout resilience
- No single-party access to individual updates

## Attack Mitigations

| Attack Type | Mitigation |
|-------------|------------|
| Model inversion | Differential privacy, aggregation |
| Data poisoning | Byzantine-resilient aggregation |
| Model poisoning | Gradient validation, outlier detection |
| Membership inference | DP, secure aggregation |
| Eavesdropping | TLS 1.3, encrypted updates |
| Man-in-the-middle | Certificate pinning, attestation |
| Replay attacks | Nonces, timestamps |
| Side channels | Constant-time operations, TEEs |

## Compliance

Designed to support:
- GDPR (EU)
- CCPA (California)
- HIPAA (Healthcare)
- ISO 27001 (Information Security)
- IEC 62443 (Industrial Control Systems)
