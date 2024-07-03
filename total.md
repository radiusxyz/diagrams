<!-- Encrypted Mempool -->

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

<!-- Before order-commitment -->

sequenceDiagram
autonumber
participant U as User
box rgb(42, 48, 80) Rollup
participant P as Proposer
end
U->>U: generate transaction
U->>U: generate symmetric encryption <br/>via time-lock puzzle
U->>U: encrypt transaction
U->>U: generate zk-SNARK proof
U->>P: encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
P-->>U: order-commitment, <br/> signature, <br/> partial Merkle Proof
opt
U->>P: decryption key
end

<!-- After order-commitment before claim -->

sequenceDiagram
participant U as User
box rgb(42, 48, 80) Rollup
participant P as Proposer
participant FN as Full Node
end
participant E as Ethereum
U->>P: encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
P->>P: verify zk-proof
P->>P: order encrypted tx
P-->P: commit to order by <br/> generating partial Merkle Proof
P->>P: sign pre-confirmation
P-->>U: pre-confirmation, <br/> signature
alt if decryption key is provided
U->>P: decryption key
else otherwise
P->>P: solve time-lock puzzle <br/> to get decryption key
end
P->>P: decrypt tx
FN->>P: request block
P-->>FN: submit block
P->>P: generate block commitment
P->>P: generate Merkle proof
P->>P: generate Merkle root
P->>E: store Merkle root
U->>P: request Merkle proof
P-->>U: Merkle proof, <br/> signature
opt verify inclusion and order
U->>E: call verify function with arguments: <br/> partial Merkle proof, <br/> signature, <br/> Merkle Proof, <br/> order, <br/> root, <br/> signature
end

<!-- Claim -->

sequenceDiagram
participant U as User
box rgb(42, 48, 80) Rollup
participant P as Proposer
end
participant E as Ethereum
U->>P: encrypted tx, <br/> zk-proof, <br/> time-lock puzzle
P-->>U: order-commitment:<br/>[order, block number, partial Merkle proof, signature]
P->>E: store Merkle root
U->>P: request final Merkle proof
P-->>U: Merkle proof, <br/> signature
opt verify inclusion and order
U->>E: call verify function with arguments: <br/> partial Merkle proof, <br/> signature, <br/> Merkle Proof, <br/> order, <br/> root, <br/> signature
end

<!-- After order-commitment after claim -->

sequenceDiagram
participant U as User
box rgb(42, 48, 80) Rollup
participant P as Proposer
participant FN as Full Node
end
participant E as Ethereum
U->>P: encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
P->>P: verify zk-proof
P->>P: order encrypted tx
P-->P: commit to order by <br/> generating partial Merkle Proof
P->>P: sign order-commitment
P-->>U: order-commitment, signature
alt if decryption key is provided
U->>P: decryption key
else otherwise
P->>P: solve time-lock puzzle to get decryption key
end
P->>P: decrypt tx
FN->>P: request block
P-->>FN: submit block
P->>P: generate block commitment
P->>P: generate Merkle proof
P->>P: generate Merkle root
P->>E: store Merkle root
U->>P: request Merkle proof
P-->>U: Merkle proof, <br/> signature
opt verify inclusion and order
U->>E: call verify function with arguments: <br/> partial Merkle proof, <br/> signature, <br/> Merkle Proof, <br/> order, <br/> root, <br/> signature
end

<!-- Leader-based -->

sequenceDiagram

box rgb(42, 48, 80) Proposer
participant F2 as Follower
participant F1 as Follower
participant L as Leader
end
participant R as Full Node

F1->>F1: 1. verify zk-proof
F1->>L: 2. forward encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
L->>L: 3. verify zk-proof
L->>L: 4. order encrypted tx
L->>L: 5. make pre-confirmation
L->>L: 6. sign pre-confirmation
L->>F1: 7. encrypted tx, <br/>time-lock puzzle, <br/>pre-confirmation, <br/> signature
L->>F2: 7. encrypted tx, <br/>time-lock puzzle, <br/>pre-confirmation, <br/>signature
L->>L: 8. solve time-lock puzzles
L->>L: 9. decrypt transactions
R->>L: 10. request block
L-->>R: 11. submit block
L->>L: 12. generate Merkle proof
L->>F1: 13.Merkle proof
L->>F2: 13.Merkle proof

<!-- Block validation -->

sequenceDiagram
box rgb(42, 48, 80) Rollup
participant F as Follower
participant L as Leader
participant FN as Full Node
end
participant A as Aggregator
participant L1 as L1
FN->>L: build block
par
L->>F: build block
L->>L: close the current block
L->>L: start building the closed block
L->>L: generate block commitment
L->>L1: block commitment
L1->>L: block commitment event
FN->>L: get the built block
L-->>FN: send built block
and
F->>F: close the current block
F->>F: generate block commitment
F->>F: start building the closed block
L1->>F: block commitment event
F->>A: block commitment
end

A->>A: aggregate commitments
A->>L1: finalize validation
opt majority coincidence
L1->>L: reward
end
