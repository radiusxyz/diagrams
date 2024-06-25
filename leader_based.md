sequenceDiagram
participant S3 as Sequencer 3 <br/> (Follower)
participant S2 as Sequencer 2 <br/> (Follower)
participant S1 as Sequencer 1 <br/> (Leader)
participant R as Rollup

S2->>S2: 1. verify zk-proof
S2->>S1: 2. forward encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
S1->>S1: 3. verify zk-proof
S1->>S1: 4. order encrypted tx
S1->>S1: 5. make pre-confirmation
S1->>S1: 6. sign pre-confirmation
S1->>S2: 7. encrypted tx, <br/>time-lock puzzle, <br/>pre-confirmation, <br/> signature
S1->>S3: 7. encrypted tx, <br/>time-lock puzzle, <br/>pre-confirmation, <br/>signature
S1->>S1: 8. solve time-lock puzzles
S1->>S1: 9. decrypt transactions
R->>S1: 10. request block
S1-->>R: 11. submit block
S1->>S1: 12. generate Merkle proof
S1->>S2: 13.Merkle proof
S1->>S3: 13.Merkle proof
