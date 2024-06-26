sequenceDiagram
%% participant S3 as Sequencer 3 <br/> (Follower)
participant S2 as Sequencer 2 <br/> (Follower)
participant S1 as Sequencer 1 <br/> (Leader)
participant L1 as SSAL(L1)
participant R as Rollup
participant A as Aggregator
R->>S1: 1. build block
S1->>S1: 2. close the current block
S1->>S1: 2. start building the closed block
S1->>S1: 2. generate block commitment
S1->>L1: 3. block commitment
S1->>S2: 1. build block
R->>S1: 4. get the built block
S1-->>R: built block
S2->>S2: close the current block
S2->>S2: generate block commitment
S2->>S2: start building the closed block
L1->>S2: block commitment event
S2->>A: block commitment
A->>A: aggregate commitments
A->>L1: finalize validation
opt majority coincidence
L1->>S1: reward
end
