sequenceDiagram
participant U as User
participant S as Sequencer Set
participant R as Rollup
participant E as Ethereum
U->>S: request Merkle proof
S-->>U: Merkle proof, <br/> signature
opt verify inclusion and order
U->>E: call verify function with arguments: <br/> partial Merkle proof, <br/> signature, <br/> Merkle Proof, <br/> order, <br/> root, <br/> signature
end
