sequenceDiagram
participant S3 as Sequencer 3 <br/> (Follower)
participant S2 as Sequencer 2 <br/> (Follower)
participant S1 as Sequencer 1 <br/> (Leader)
participant L1 as SSAL(L1)
participant R as Rollup
participant A as Aggregator
R->>S1: close block
S1->>S2: close block
S1-->>R: block
S1->>L1: block commitment
L1->>A: block commitment event
S2->>A: validate
A->>L1: finalize validation
L1->>S1: reward
