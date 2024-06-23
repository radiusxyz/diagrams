sequenceDiagram
autonumber
participant U as User
participant S as Sequencer Set
participant R as Rollup
participant E as Ethereum
U->>S: encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
S->>S: verify zk-proof
S->>S: order encrypted tx
S-->S: commit to order by <br/> generating partial Merkle Proof
S->>S: sign pre-confirmation
S-->>U: pre-confirmation, <br/> signature
alt if decryption key is provided
U->>S: decryption key
else otherwise
S->>S: solve time-lock puzzle <br/> to get decryption key
end
S->>S: decrypt tx
R->>S: request block
S-->>R: submit block
S->>S: generate block commitment
S->>S: generate Merkle proof
S->>S: generate Merkle root
S->>E: store Merkle root
U-->>S: request Merkle proof
S-->>U: send Merkle proof, <br/> signature
opt verify inclusion and order
U->>E: submit tx, order, proof, signatures
end
