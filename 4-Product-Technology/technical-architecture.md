# Technical Architecture (High‑Level)

The platform is built using an event‑driven microservices architecture on Google Cloud:

- **Inference & Training:** Reinforcement learning agents operate as stateless workers. A training scheduler retrains models on weekends and updates the model registry. An inference worker performs live and catch‑up inference on a per‑asset basis.
- **Data Loader:** Fetches market data (hourly OHLCV) and publishes messages to Pub/Sub topics.
- **Portfolio & Risk Engines:** Compute template deltas on regime flips, enforce risk limits by pulling positions via SnapTrade, and calculate value‑at‑risk.
- **Execution Gateway:** Uses SnapTrade to submit and cancel orders with idempotent client order identifiers.
- **Admin Surface:** Provides a secure interface for managing routing decisions, including bulk CSV import and conflict resolution via optimistic concurrency.
- **Compliance Adapter:** Caches compliance verdicts from Chainlink ACE and triggers refreshes on profile changes or large orders (Phase 2).
- **ReFIN L2 Anchoring:** In Phase 3 the system anchors trading records on a dedicated Layer 2 chain and Ethereum L1 for auditability.

The architecture uses Cloud Run, Pub/Sub, MongoDB Atlas, Redis and Docker containers, achieving scalability and reliability while remaining cloud‑agnostic.