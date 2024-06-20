sequenceDiagram
participant U as User
participant F1 as Follower 1
participant F2 as Follower 2
participant L as Leader
participant SSAL
participant R as Rollup
participant E as Ethereum
U->>F1: encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
F1->>L: encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
L->>L: verify zk-proof, <br/> order encrypted tx, <br/> sign pre-confirmation
L->>F2: encrypted tx, <br/>pre-confirmation, <br/> signature,
L->>F1: encrypted tx, <br/>pre-confirmation, <br/> signature,
F1-->>U: pre-confirmation, <br/> signature
L->>L: solve time-lock puzzle, <br/> obtain key, <br/> and decrypt tx
R->>SSAL: get current leader
SSAL->>SSAL: (block #) % (sequencers #)
SSAL-->>R: current leader address
R->>L: get block
L->>L: make block commitment
L->>E: post block commitment
F1->>F1: make block commitment
F2->>F2: make block commitment
F1->>E: post block commitment
F2->>E: post block commitment
E->>E: compare followers' commitments <br/> with leader's commitment
opt if commitments match
E->>E: reward sequencers
end
