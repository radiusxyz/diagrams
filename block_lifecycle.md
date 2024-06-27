sequenceDiagram
%% participant S3 as Sequencer 3 <br/> (Follower)
participant S2 as Sequencer 2 <br/> (Follower)
participant S1 as Sequencer 1 <br/> (Leader)
participant L1 as L1
participant R as Rollup
participant A as Aggregator
R->>S1: build block
par
S1->>S2: build block
S1->>S1: close the current block
S1->>S1: start building the closed block
S1->>S1: generate block commitment
S1->>L1: block commitment
L1->>S1: block commitment event
R->>S1: get the built block
S1-->>R: built block
and
S2->>S2: close the current block
S2->>S2: generate block commitment
S2->>S2: start building the closed block
L1->>S2: block commitment event
S2->>A: block commitment
end

A->>A: aggregate commitments
A->>L1: finalize validation
opt majority coincidence
L1->>S1: reward
end
