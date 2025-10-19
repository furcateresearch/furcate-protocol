# Protocol Message Specification

## Overview

All Furcate protocol messages use Protocol Buffers (protobuf) for efficient serialization and strong typing.

## Core Messages

See `schemas/messages.proto` for complete definitions.

### ModelUpdate

Sent by edge devices to coordinator with encrypted gradients.

```protobuf
message ModelUpdate {
  string device_id = 1;
  int64 timestamp = 2;
  bytes encrypted_gradients = 3;
  bytes signature = 4;
  AttestationReport attestation = 5;
  int32 sample_count = 6;
  float loss = 7;
}
```

**Fields:**
- `device_id`: Unique device identifier
- `timestamp`: Unix timestamp (milliseconds)
- `encrypted_gradients`: Model gradients encrypted with coordinator's public key
- `signature`: Device signature over timestamp + gradients
- `attestation`: TEE attestation report
- `sample_count`: Number of training samples used
- `loss`: Local training loss

### AggregatedModel

Sent by coordinator to participants with updated global model.

```protobuf
message AggregatedModel {
  string round_id = 1;
  bytes encrypted_weights = 2;
  int32 num_participants = 3;
  repeated string participant_ids = 4;
  float average_loss = 5;
  bytes signature = 6;
}
```

### DeviceRegistration

Initial registration of edge device to network.

```protobuf
message DeviceRegistration {
  string device_id = 1;
  string device_type = 2;
  bytes public_key = 3;
  AttestationReport initial_attestation = 4;
  map<string, string> capabilities = 5;
}
```

### ConsensusBlock

Proof of Authority consensus block.

```protobuf
message ConsensusBlock {
  int64 block_number = 1;
  string previous_hash = 2;
  int64 timestamp = 3;
  string validator_id = 4;
  repeated ModelUpdate updates = 5;
  bytes signature = 6;
}
```

## Wire Protocol

### Transport
- Default: TLS 1.3 over TCP
- Alternative: MQTT (for IoT devices)
- Alternative: QUIC (low-latency scenarios)

### Framing
```
[4 bytes: message length][N bytes: protobuf message]
```

### Encryption
- TLS provides transport encryption
- Payloads (gradients, weights) additionally encrypted with recipient's public key
- Double encryption for defense in depth

## Error Handling

### Error Codes
- `INVALID_SIGNATURE`: Message signature verification failed
- `ATTESTATION_FAILED`: TEE attestation invalid
- `DEADLINE_EXCEEDED`: Round deadline passed
- `INSUFFICIENT_PARTICIPANTS`: Not enough devices joined round
- `MALFORMED_MESSAGE`: Protobuf parsing failed

### Retry Logic
- Exponential backoff: 1s, 2s, 4s, 8s, 16s (max)
- Max retries: 5
- Idempotency: All messages have unique IDs

## Versioning

Protocol version in every message header:
```
version: "1.0.0"
```

Breaking changes increment major version.
