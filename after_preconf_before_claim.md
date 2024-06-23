sequenceDiagram
participant U as User
participant S as Sequencer Set
participant R as Rollup
participant E as Ethereum
U->>S: 1. encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
S->>S: 2. verify zk-proof
S->>S: 3. order encrypted tx
S->>S: 4. commit to order by <br/> generating partial Merkle Proof
S->>S: 5. sign pre-confirmation
S-->>U: 6. pre-confirmation, <br/> signature
alt if decryption key is provided
U->>S: decryption key
else otherwise
S->>S: 7. solve time-lock puzzle <br/> to get decryption key
end
S->>S: 8. decrypt tx
R->>S: 9. request block
S-->>R: 10. submit block
%% S->>S: generate block commitment
S->>S: 11. generate Merkle proof
S->>S: 12. generate Merkle root
S->>E: 13. store Merkle root
