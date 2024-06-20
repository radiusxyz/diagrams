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
S->>S: generate vector Merkle root
S->>S: generate Merkle proof
S->>E: store vector Merkle root
R->>S: request block
S-->>R: submit block
U-->>S: request Merkle proof
S-->>U: send Merkle proof, <br/> signature
opt verify inclusion and order
U->>E: submit tx, order, proof, signatures
end
