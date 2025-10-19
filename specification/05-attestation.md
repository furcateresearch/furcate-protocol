# Cryptographic Attestation

## Purpose

Attestation provides cryptographic proof that:
1. Correct code is executing
2. On untampered hardware
3. In a secure enclave
4. Producing trustworthy results

## Attestation Flow

```
┌──────────┐         ┌──────────┐         ┌──────────┐
│  Device  │────────▶│   TEE    │────────▶│Verifier  │
│          │ Request │          │  Quote  │          │
└──────────┘         └──────────┘         └──────────┘
                          │
                          ▼
                    ┌──────────┐
                    │ Platform │
                    │  (SGX)   │
                    └──────────┘
```

## Components

### 1. Measurement
- Hash of enclave code (MRENCLAVE for SGX)
- Hash of enclave signer (MRSIGNER)
- Security version number
- Product ID

### 2. Quote Generation
Platform generates signed quote containing:
- Measurements
- User data (nonce, public key)
- Platform signature

### 3. Verification
Verifier checks:
- Signature validity
- Measurement matches expected values
- Security version is current
- Platform is genuine

## Message Format

```json
{
  "quote": "<base64_encoded_quote>",
  "public_key": "<base64_rsa_or_ecdsa_key>",
  "timestamp": 1729353600,
  "tee_type": "SGX|TrustZone|Nitro|SEV",
  "device_id": "edge-device-001",
  "measurement": "<base64_mrenclave>",
  "nonce": "<base64_random_nonce>"
}
```

## Use Cases

### Model Inference Verification
Client verifies ML inference occurred in genuine TEE

### Federated Learning Integrity
Coordinator verifies gradient updates from legitimate devices

### Audit Trail
Compliance requires proving computations were secure

## Security Properties

- **Freshness**: Nonces prevent replay
- **Integrity**: Signatures prevent tampering
- **Authenticity**: Platform cert chain proves genuineness
- **Confidentiality**: Does NOT reveal enclave data

## Performance

- Quote generation: ~50ms (SGX)
- Verification: ~10ms
- Network overhead: ~2-4 KB per attestation
