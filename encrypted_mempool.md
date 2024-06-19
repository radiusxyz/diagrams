sequenceDiagram
participant U as User
participant F1 as Follower 1
participant F2 as Follower 2
participant L as Leader
participant SSAL
participant R as Rollup
participant E as Ethereum
U->>U: generate transaction
U->>U: generate symmetric encryption <br/>via time-lock puzzle
U->>U: encrypt transaction
U->>U: generate zk-SNARK proof
U->>F1: encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
F1->>L: encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
L->>L: verify zk-proof
L->>L: order encrypted tx
L->>L: sign pre-confirmation
L->>F2: encrypted tx, <br/>pre-confirmation, <br/> signature,
L->>F1: encrypted tx, <br/>pre-confirmation, <br/> signature,
F1-->>U: pre-confirmation, <br/> signature
alt If provides decryption key
U->>F1: decryption key
F1->>L: decryption key
else otherwise
L->>L: solve time-lock puzzle <br/> to get decryption key
end
L->>L: decrypt tx

sequenceDiagram
participant U as User
participant F1 as Follower 1
participant F2 as Follower 2
participant L as Leader
participant SSAL
participant R as Rollup
participant E as Ethereum
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

sequenceDiagram
participant U as User
participant S as Sequencing layer
participant R as Rollup
participant E as Ethereum
U->>U: generate transaction
U->>U: generate symmetric encryption <br/>via time-lock puzzle
U->>U: encrypt transaction
U->>U: generate zk-SNARK proof
U->>S: encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
S->>S: verify zk-proof
S->>S: order encrypted tx
S->>S: sign pre-confirmation
S-->>U: pre-confirmation, <br/> signature
alt if provides decryption key
U->>S: decryption key
else otherwise
S->>S: solve time-lock puzzle <br/> to get decryption key
end
S->>S: decrypt tx
S->>S: generate block commitment
S->>S: generate vector commitment
S->>S: generate pointproof
S->>E: store vector commitment on Ethereum
R->>S: request block
S-->>R: submit block
U-->>S: request pointproof
S-->>U: send pointproof, <br/> signature
opt verify inclusion and order
U->>E: submit tx, order, proof, signatures
end
