# Design Decisions

## Why Federated Learning?

**Decision**: Use federated averaging as core training paradigm

**Rationale**:
- Data privacy: Most critical requirement for industrial/medical applications
- Compliance: GDPR, CCPA mandate data minimization
- Bandwidth: Transmitting models cheaper than transmitting datasets
- Proven: Google, Apple use FL in production at scale

**Alternatives Considered**:
- Split learning: Too much latency for edge
- Centralized training: Violates privacy requirements
- Local-only models: Insufficient accuracy

## Why Proof of Authority?

**Decision**: Use PoA consensus instead of PoW or PoS

**Rationale**:
- Energy efficiency: No mining required
- Performance: 5-second block times achievable
- Permissioned networks: Industrial deployments have known participants
- Cost: No token required

**Alternatives Considered**:
- Proof of Work: Too energy-intensive for edge
- Proof of Stake: Requires token economics
- PBFT: Complex for large networks

## Why Protocol Buffers?

**Decision**: Use protobuf instead of JSON or custom binary

**Rationale**:
- Performance: 3-10x faster than JSON
- Size: 30-50% smaller messages
- Type safety: Strong schemas prevent errors
- Tooling: Code generation for many languages

**Alternatives Considered**:
- JSON: Too verbose for constrained devices
- MessagePack: Less tooling/IDE support
- FlatBuffers: Unnecessary zero-copy for our use case

## Why TEEs?

**Decision**: Require TEE support for production deployments

**Rationale**:
- Verifiability: Cryptographic proof of correct execution
- Compliance: Required for regulated industries
- Trust: Eliminates need to trust edge device operators
- Hardware availability: SGX, TrustZone widely deployed

**Alternatives Considered**:
- Software-only security: Insufficient for critical applications
- Homomorphic encryption: Too slow for real-time inference
- MPC: High communication overhead

## Why MQTT + Modbus?

**Decision**: Prioritize these protocols for initial release

**Rationale**:
- Ubiquity: MQTT is de-facto IoT standard, Modbus dominates industrial
- Maturity: Stable specs, extensive library support
- Coverage: Together address 80%+ of target use cases

**Future Additions**:
- OPC-UA: Next priority (manufacturing)
- LoRaWAN: For agriculture/remote sensing
- BACnet: For building automation

## Why Optional Blockchain?

**Decision**: Make blockchain integration optional, not required

**Rationale**:
- Independence: Protocol functions without blockchain
- Flexibility: Deployments can choose their preferred blockchain
- Future-proof: Blockchain landscape still evolving

**When Blockchain Adds Value**:
- Immutable audit logs
- Multi-organization coordination
- Token-based incentives (optional)
- Cross-platform attestation registry
