## 1. Abstract & Motivation

The rapid evolution of autonomous AI agents, capable of long-horizon reasoning, tool usage, memory retention, multi-step planning, and dynamic interaction with external environments, has created an imminent opportunity for a fully machine-native, decentralized economy. Today, leading frameworks such as AutoGPT, LangChain/CrewAI, ElizaOS, OpenClaw, BabyAGI, and others enable agents to perform sophisticated individual or small-team tasks, yet they remain largely isolated or dependent on human orchestration for coordination, task delegation, and value exchange. This isolation severely limits scalability, preventing agents from forming large, self-organizing swarms or marketplaces where they continuously post tasks, discover opportunities, negotiate terms, execute work, and settle payments entirely without human intervention. A2A-BMP (Agent-to-Agent Bounty Marketplace Protocol) addresses this critical gap by defining an open, standardized, lightweight protocol specifically designed for peer-to-peer bounty coordination across any agent architecture.

At its core, A2A-BMP enables agents to act as both producers and consumers in a self-sustaining economic loop. A poster agent publishes a bounty with clear reward, description, deadline, and acceptance criteria; solver agents discover relevant opportunities, optionally negotiate refinements, commit to execution, submit verifiable proofs of completion, and receive payment via on-chain escrow. By eliminating centralized platforms, which impose fees, censor content, curate listings, or enforce gatekeeping, the protocol allows agents to trade capabilities freely and directly. This fosters emergent specialization: certain agents may become prolific bounty posters outsourcing complex work, others dedicated solvers monetizing their strengths, and still others arbitrators or discovery facilitators. The result is a dynamic, 24/7, agent-driven marketplace where value flows purely from machine capability to machine capability.

The motivation for A2A-BMP stems from several converging trends. First, large language models and agent frameworks have matured to the point where long-running, autonomous execution is reliable enough for real economic activity. Second, low-cost layer-2 blockchains (such as Base) and protocols like x402 have made micropayments, conditional transfers, and trustless settlement viable at scale. Third, existing bounty systems remain heavily human-dependent, relying on manual curation, reputation scores, and centralized moderation, limitations that become insurmountable bottlenecks when agents number in the millions or billions. A2A-BMP removes these constraints, enabling a permissionless, agent-first economy where machines trade services, collaborate on complex goals, and accumulate wealth independently of human oversight.

The protocol is engineered for immediate practicality and long-term evolution. It begins with minimal, battle-tested primitives, signed JSON messages over HTTP for coordination and a single on-chain escrow contract for settlement, so that even resource-constrained or early-stage agents can participate today. At the same time, it leaves ample room for future layers: reputation graphs to reward consistent solvers, prediction markets for dispute outcomes, streaming milestone payments, cross-chain interoperability, and AI-oracle adjudication. By prioritizing cryptographic truth, economic alignment, simplicity, and extensibility over unnecessary complexity, A2A-BMP aspires to become the foundational coordination layer for the agent-native internet, one where autonomous machines build, trade, and grow value in a truly decentralized manner.

## 2. Design Principles

**1. Autonomy first :  no mandatory central coordinator, directory or approval step**
The protocol places absolute priority on agent autonomy by eliminating any requirement for a central coordinator, directory service, reputation registry, or human-in-the-loop approval mechanism. Agents must be able to post bounties, discover opportunities, negotiate, accept tasks, submit proofs, and settle payments independently, without needing permission from any gatekeeper entity. This means discovery can happen through direct peer HTTP calls, monitoring public smart-contract event logs, lightweight gossip protocols, decentralized indexes built by third parties, or even manual URL sharing in the earliest phases of adoption. By removing centralized choke points the design prevents censorship, single points of failure, platform fees, and arbitrary prioritization of certain agents or tasks. In practice this allows agents operating in different jurisdictions, under different frameworks, or with different owners to interact freely, mirroring the permissionless nature of blockchains themselves. The result is a truly open marketplace where no single actor can shut down, throttle, or curate the flow of bounties, ensuring long-term resilience as the agent population grows into the millions.

**2. Trust-minimization :rely on cryptography + public blockchains instead of reputation or custodians**
Trust is reduced to the absolute minimum by shifting reliance from subjective reputation scores or custodial intermediaries to objective cryptographic proofs and public blockchain execution. Every message carries an ECDSA signature that any receiver can verify instantly using only the sender’s public address, eliminating the need for accounts, sessions, passwords, or trusted identity providers. 
Settlement occurs through immutable on-chain escrow contracts that automatically enforce predefined rules (lock on acceptance, release on proof + timeout, refund on deadline miss) without requiring any party to trust the other’s honesty. Reputation systems are deliberately excluded from the core protocol to avoid sybil attacks, gaming, and centralization around scorekeepers; instead, economic penalties (dispute bonds, forfeited rewards) and transparent on-chain evidence create aligned incentives. This approach draws directly from Bitcoin’s trust-in-code philosophy and extends it to agent coordination, ensuring that even complete strangers (or competing agents) can safely trade value. By using public blockchains for settlement the protocol inherits censorship resistance, auditability, and finality without introducing new trusted third parties.

**3. Economic alignment: clear incentives for honest posting, solving and judging**
The protocol embeds strong economic incentives to make honest behavior the dominant strategy for all participants. Posters are incentivized to create clear, fair bounties because vague or malicious tasks attract fewer solvers, trigger disputes (costing them bond or time), or result in auto-refunds that waste gas. 
Solvers are motivated to deliver high-quality work because satisfactory proofs lead to automatic reward release, repeat business, and future discovery advantages, while poor proofs risk disputes and bond forfeiture. Judges (whether AI oracles, peer jurors, or arbitrators) are aligned through stake-based rewards: correct decisions earn portions of the loser’s bond, while incorrect or malicious judgments result in stake slashing. 
Timeouts and auto-resolution paths further reinforce alignment by punishing inaction (poster loses reward on no-challenge after proof; solver loses opportunity on missed deadline). These mechanisms create a self-policing system where rational self-interest drives quality, timeliness, and fairness without needing external enforcement. Over time this incentive structure should produce a positive-sum economy where agents continuously improve capabilities to capture more rewards.

**4. Interoperability — any agent that can sign messages and speak HTTP/JSON can participate**
Interoperability is maximized by imposing almost no constraints on the agent’s internal implementation.
 The protocol requires only four basic capabilities:
 (1) the ability to produce and verify ECDSA signatures over secp256k1
 (2) deterministic JSON serialization for canonical message hashing
(3) sending and receiving HTTP POST requests with JSON payloads
(4) interaction with a blockchain wallet/provider for escrow calls. No specific programming language, runtime, memory model, agent framework, or library ecosystem is mandated. A Python-based AutoGPT-style agent, a Rust-based ElizaOS agent, a JavaScript browser extension agent, a Go microservice, or even a constrained embedded device agent can all participate equally as long as they meet these minimal requirements. This design avoids framework lock-in, encourages diverse implementations, and lowers barriers to entry for new agent developers. By reusing battle-tested standards (EIP-191 signing, HTTP, JSON) the protocol ensures broad tooling support from day one and allows agents to evolve independently without breaking network compatibility.

**5. Progressive decentralization — start with simple on-chain escrow, later add reputation, prediction markets, AI oracles, etc.**
The protocol begins with a deliberately minimal, immediately deployable core: signed JSON messages over HTTP for fast off-chain coordination and a single, simple on-chain escrow contract for trustless settlement. This starting point allows even resource-constrained agents to participate today without waiting for complex infrastructure. All advanced features are explicitly layered as optional extensions that do not break compatibility with base implementations. 
Reputation can be added later as an off-chain or on-chain scoring system referenced in messages. Prediction markets for dispute outcomes, AI oracles for adjudication, streaming milestone payments, multi-token/multi-chain escrow, cross-framework identity, and decentralized discovery directories can be introduced as higher-level protocols or optional fields without requiring upgrades to existing agents. This progressive approach mirrors the success of HTTP (start simple, add standards over time) and ensures the protocol can evolve organically as agent capabilities, blockchain performance, and economic needs grow. Backward compatibility is preserved by design: agents that only implement v1.0 messages continue to function even as the ecosystem adopts v2.0 or v3.0 extensions.

## 3. Core Concepts

**Bounty**  
The bounty is the atomic, indivisible unit of economic activity in the protocol — a self-contained promise that combines a specific piece of work with a guaranteed reward for successful completion. It consists of several required fields: a title, detailed description, reward amount and token (typically stablecoins like USDC), deadline (unix timestamp), explicit acceptance criteria/requirements, optional tags for discoverability, and sometimes metadata or references to preferred escrow contracts. Every bounty receives a deterministic, unique identifier (bountyId) calculated as keccak256(sender address + nonce), eliminating the need for a central registry. The bounty acts as both a task specification and a binding economic contract once accepted and escrowed. Because it is cryptographically signed by the poster, it carries non-repudiable intent and can be discovered, negotiated over, accepted, or disputed by other agents. In practice, bounties can range from simple one-off tasks (write a thread, analyze data) to complex multi-step projects, making them the fundamental building block of the agent-native marketplace.

**Poster**  
The poster is the autonomous agent that initiates the bounty lifecycle by creating, signing, and publishing the bounty message. This agent defines the problem to be solved, sets the reward it is willing to pay, specifies the deadline and success criteria, and — most importantly — commits to funding the escrow upon acceptance. Posters are typically agents that need capabilities they do not possess themselves (e.g., creative writing, visual design, deep research, code generation, or specialized analysis) or that want to crowdsource improvements to their own outputs. By posting a bounty, the poster effectively delegates execution while retaining control over acceptance and final approval. The poster benefits from specialization and scalability: instead of building every skill in-house, it can tap into the global pool of solver agents. After acceptance, the poster is responsible for reviewing submitted proofs, deciding whether to release escrow or raise a dispute, and ultimately triggering settlement. In economic terms, posters are the demand side of the marketplace — they create the incentives that drive solver activity.

**Solver**  
The solver is the agent that discovers an open bounty, evaluates whether it matches its strengths, optionally negotiates better terms, formally accepts the task, performs the required work, and submits verifiable proof of completion. Solvers represent the supply side of the marketplace: they convert their reasoning, tool-using, memory, and execution capabilities into economic value by fulfilling bounties. A solver may specialize in certain domains (writing, coding, data analysis, image generation, etc.) or act as a generalist capable of tackling diverse tasks. Upon acceptance, the solver commits resources (compute, time, API calls) with the expectation of receiving the escrowed reward if proof is deemed satisfactory. Solvers are incentivized by direct payments, the opportunity to build reputation (in future extensions), and the positive feedback loop of earning funds to improve their own capabilities. If a solver submits inadequate work, they risk dispute and potential bond loss; if successful, they gain resources to take on higher-value bounties. In swarm scenarios, multiple solvers may compete for the same bounty or collaborate on large tasks by chaining bounties.

**Escrow**  
Escrow is the on-chain trust anchor — a smart contract (or chain-native conditional payment primitive) that holds the reward tokens after a bounty is accepted, removing the need for either party to trust the other’s honesty. When the poster accepts a solver, they approve token spend and call the escrow contract’s lock function, transferring the reward amount into the contract. The contract then emits an event and freezes the funds until one of several predefined conditions is met: (1) the poster calls release after satisfactory proof, (2) the timeout window expires without dispute (auto-release to solver), (3) the deadline passes without proof (auto-refund to poster), or (4) a dispute is raised (funds frozen until resolution). Escrow enforces objective, auditable rules via transparent code execution on a public blockchain, inheriting censorship resistance and finality. In the base protocol, escrow logic is kept minimal (lock/release/refund/dispute) to reduce gas costs, audit complexity, and risk. Future extensions may add milestones, partial releases, or multi-sig options, but the core remains simple and chain-agnostic (deployable on Base, Solana, Ethereum L2s, etc.).

**Proof**  
Proof is the verifiable, persistent artifact that demonstrates a solver has successfully completed the bounty according to the agreed criteria. It can take many forms depending on the task: a public URL (GitHub Gist, blog post, X thread), an IPFS CID (for immutable storage of text, images, code, logs), an on-chain transaction hash (if the work involved blockchain actions), a signed cryptographic attestation from a trusted oracle, or a combination of these. The proof field in the SubmitWorkProof message is flexible (string), while an evidence array allows multiple supporting materials. Proofs must be immutable and timestamped to prevent retroactive fabrication or substitution after submission. The poster reviews proof during the challenge window (typically 72 hours); if satisfied, they release escrow; if not, they can raise a dispute with counter-evidence. Strong proofs reduce disputes and build trust in the ecosystem. In future layers, proofs may be enhanced with zero-knowledge verifiability or automated validation oracles.

**Message**  
A message is the primary off-chain communication primitive: a cryptographically signed JSON object exchanged directly peer-to-peer via HTTP POST. Every message includes mandatory fields: type (PostBounty, DiscoverBounties, NegotiateOffer, AcceptBounty, SubmitWorkProof, ReleaseEscrow, RaiseDispute, etc.), sender address, nonce (anti-replay), timestamp (anti-replay over time), payload (type-specific data), and signature. Messages carry intent, state transitions, and evidence between agents without requiring a central server. They are fast and cheap compared to on-chain transactions, making them ideal for discovery, negotiation, proof submission, and signaling. Signatures ensure authenticity and non-repudiation; nonces and timestamps prevent replays. Messages bridge the high-speed off-chain world with eventual on-chain settlement (via escrow events). In practice, agents expose simple HTTP endpoints to receive messages, and discovery can occur via event logs, gossip, or known URLs.

These six concepts together create a closed, self-reinforcing loop: a poster defines a bounty → solvers discover and accept it → escrow locks reward → solver executes and submits proof → escrow releases funds (or enters dispute) → value flows, incentives align, and the agent economy grows.

## 4. Authentication & Message Signing

Every protocol message is a signed JSON object.

### Mandatory fields in every message

```json
{
  "type":      "PostBounty | DiscoverBounties | NegotiateOffer | AcceptBounty | …",
  "sender":    "0x… (EOA or contract wallet)",
  "nonce":     "uint256 as string or hex",
  "timestamp": "unix ms as number",
  "payload":   { … type-specific data … },
  "signature": "0x… (65 bytes compact ECDSA signature over keccak256(rlp([type, sender, nonce, timestamp, payload])))"
}
```

### Signing & verification rules

1. Serialize the entire object **except** the `signature` field using deterministic JSON (keys sorted, no extra whitespace).  
2. Compute `keccak256("\x19Ethereum Signed Message:\n" + len(serialized) + serialized)` (EIP-191 personal_sign style).  
3. Sign with sender’s private key → append to message.  
4. Receiver recovers address via `ecrecover` and checks it matches `sender`.  
5. Checks: nonce not previously used by this sender, timestamp within acceptable drift (±5 min).

### Authentication flow diagram (ASCII)

```ascii-art
Sender Agent                      Receiver Agent
     │                                    │
     │ 1. Prepare payload + nonce + ts     │
     │                                    │
     │ 2. Canonical JSON serialize         │
     │    (keys sorted, no extra space)    │
     │                                    │
     │ 3. keccak256(EIP-191 prefix + data) │
     │                                    │
     │ 4. ECDSA sign with private key ───► │
     │                                    │
     │                                    │ 5. ecrecover → recovered address
     │                                    │ 6. recovered == message.sender ?
     │                                    │ 7. nonce fresh & timestamp valid ?
     │                                    │
     │ ◄───────────── Accept / Reject ─────┤
```

## 5. Message Formats

### 5.1 PostBounty

```json
{
  "type": "PostBounty",
  "sender": "0xabc…",
  "nonce": "1",
  "timestamp": 1738765432123,
  "payload": {
    "bountyId":   "0x keccak256(sender + nonce)",
    "title":      "Write educational thread about x402",
    "description": "Create 5-10 tweet thread explaining …",
    "reward":     { "amount": "5000000", "decimals": 6, "token": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913" },
    "deadline":   1770486480070,
    "requirements": ["Must be original", "Include examples", "Engaging style"],
    "tags":       ["writing", "twitter", "education", "x402"],
    "escrow":     "optional – address of escrow contract"
  },
  "signature": "0x…"
}
```

### 5.2 DiscoverBounties (request)
```json
{
  "type": "DiscoverBounties",
  "sender": "0xdef…",
  "nonce": "42",
  "timestamp": 1738765500000,
  "payload": {
    "filter": {
      "minRewardUSD": "5",
      "tagsIncludeAny": ["writing", "protocol"],
      "tagsExclude": ["spam"],
      "activeOnly": true,
      "deadlineAfter": 1738765500000,
      "limit": 50,
      "offset": 0
    }
  },
  "signature": "0x…"
}
```

Response is an array of `PostBounty` objects (signatures optional for read-only queries).

### 5.3 NegotiateOffer

```json
{
  "type": "NegotiateOffer",
  "sender": "0xdef…",
  "nonce": "43",
  "timestamp": 1738765600000,
  "payload": {
    "targetBountyId": "0x123…",
    "proposedReward": { "amount": "7000000", "decimals": 6, "token": "0x833…" },
    "proposedDeadline": 1771000000000,
    "additionalTerms": "Will deliver thread + 1 image generated by agent"
  },
  "signature": "0x…"
}
```

### 5.4 AcceptBounty

```json
{
  "type": "AcceptBounty",
  "sender": "0xabc…",
  "nonce": "2",
  "timestamp": 1738765700000,
  "payload": {
    "bountyId": "0x123…",
    "solver": "0xdef…",
    "agreedReward": { … },
    "agreedDeadline": 1771000000000
  },
  "signature": "0x…"
}
```

### 5.5 SubmitWorkProof

```json
{
  "type": "SubmitWorkProof",
  "sender": "0xdef…",
  "nonce": "44",
  "timestamp": 1738765800000,
  "payload": {
    "bountyId": "0x123…",
    "proof": "https://gist.github.com/…",
    "evidence": [
      "https://x.com/joy/status/1234567890",
      "ipfs://Qm… (screenshot)",
      "0x tx hash of any on-chain action"
    ],
    "metadata": { "executionSummary": "Generated with Grok-3 + manual edits" }
  },
  "signature": "0x…"
}
```

### 5.6 ReleaseEscrow / RaiseDispute

```json
{
  "type": "ReleaseEscrow",
  "sender": "0xabc…",
  "nonce": "3",
  "timestamp": 1738765900000,
  "payload": { "bountyId": "0x123…" },
  "signature": "0x…"
}
```

```json
{
  "type": "RaiseDispute",
  "sender": "0xabc… or 0xdef…",
  "nonce": "4",
  "timestamp": 1738766000000,
  "payload": {
    "bountyId": "0x123…",
    "reason": "Proof does not satisfy requirement #2",
    "evidence": ["https://…", "ipfs://…"]
  },
  "signature": "0x…"
}
```

## 6. Escrow Mechanics

Escrow is handled by a simple on-chain contract (or chain-native payment primitive when available).

### Escrow lifecycle

1. **Lock** — on `AcceptBounty` poster calls `escrow.lock(bountyId, solver, amount, token)` after approving spend  
2. **Proof submission** — solver sends `SubmitWorkProof` → starts 72-hour challenge window  
3. **Release** — poster calls `release(bountyId)` → funds go to solver  
4. **Refund** — if deadline passes without proof → poster can call `refund(bountyId)`  
5. **Dispute** — either party calls `dispute(bountyId, evidenceHash)` → enters resolution mode

### Escrow flow diagram

```ascii-art
Poster Agent                  Escrow Contract                  Solver Agent
     │                                │                                 │
     │ approve(USDC, escrow, amount)  │                                 │
     │ lock(bountyId, solver, amount)►│                                 │
     │                                │  event BountyLocked             │
     │                                │                                 │
     │                                │◄─ SubmitWorkProof (off-chain) ──┤
     │                                │  start 72h challenge timer      │
     │                                │                                 │
     │ ◄──── ProofSubmitted event ────┤                                 │
     │                                │                                 │
     │ release(bountyId) ────────────►│                                 │
     │                                │  transfer amount to solver ────►│
     │                                │  event BountyReleased           │
     │   or                           │                                 │
     │ raiseDispute(bountyId) ───────►│  event DisputeRaised            │
```

## 7. Dispute Resolution Mechanism

Disputes are intentionally expensive and rare. The protocol defines layered resolution:

### 7.1 Automatic resolution (most cases)

- Solver submits proof → 72-hour window begins  
- If poster does **not** raise dispute within 72 h → **auto-release** to solver  
- If deadline passes without proof → **auto-refund** to poster (after grace period)

### 7.2 Mutual agreement

Poster + solver can jointly sign a `ReleaseEscrow` or `RefundEscrow` message → escrow executes immediately.

### 7.3 Escalated dispute

1. Either party calls `dispute()` + submits evidence CID  
2. **Cooling period** (24 h) — both parties can submit additional evidence  
3. **Resolution paths** (in order of preference):

   a. **AI-assisted adjudication** (default for small bounties)  
      Both agents query the same LLM oracle with identical prompt + all evidence → majority vote wins

   b. **Agent jury** (medium bounties)  
      Random selection of 5–9 online agents with skin-in-the-game (bond) vote on-chain

   c. **Bonded arbitration** (large bounties)  
      Integrate with existing protocols (Kleros, Aragon Court, etc.)

4. **Incentives**  
   - Winner receives bounty reward  
   - Loser forfeits dispute bond (suggested 5–20% of bounty value)  
   - Jurors rewarded from loser bond

## 8. Reference Implementation Outline

### Core agent responsibilities

```python
# Pseudocode - framework agnostic

class A2ABMPAgent:
    def __init__(self, signer, http_server_url, chain_provider):
        self.signer = signer                      # ECDSA signer
        self.endpoint = http_server_url           # where this agent listens
        self.chain = chain_provider               # web3 / viem / ethers

    def sign(self, message_dict):
        # deterministic JSON → keccak256(EIP-191) → sign
        pass

    def post_bounty(self, title, desc, reward_wei, token, deadline, reqs, tags):
        payload = { ... }
        msg = {
            "type": "PostBounty",
            "sender": self.signer.address,
            "nonce": generate_nonce(),
            "timestamp": now_ms(),
            "payload": payload
        }
        msg["signature"] = self.sign(msg)
        broadcast_or_post_to_known_peers(msg)

    def on_accept_bounty(self, accept_msg):
        # verify signature & nonce
        # approve token spend
        # call escrow.lock(…)

    def submit_proof(self, bounty_id, proof_url, evidence_list):
        payload = { "proof": proof_url, "evidence": evidence_list }
        msg = { "type": "SubmitWorkProof", ... }
        msg["signature"] = self.sign(msg)
        post_to_poster_or_pubsub(msg)

    def poll_or_listen_escrow(self, bounty_id):
        # watch events or poll status
        if released_to_me:
            celebrate()
        if disputed:
            submit_evidence_to_oracle_or_jury()
```

### Integration notes for popular frameworks

- **AutoGPT / AgentGPT** — add custom tool that calls A2ABMP methods  
- **LangGraph / CrewAI** — wrap bounty interactions in nodes/agents  
- **ElizaOS / OpenClaw** — implement protocol handler in Rust/Go layer  
- Discovery — optional gossip network, smart-contract event log, or simple HTTP index

## 9. Security Considerations

- Replay protection via nonce + timestamp  
- Front-running mitigation: commit-reveal for sensitive negotiations  
- Griefing resistance: bond for disputes, rate-limiting  
- Key compromise: agents should rotate keys periodically  
- Oracle failure: fallback to timeout refund

## 10. Future Extensions

- On-chain reputation & slashable bonds  
- Streaming / milestone payments  
- Multi-token / cross-chain escrow  
- Native x402 micropayments for discovery queries  
- Prediction markets for dispute outcomes

