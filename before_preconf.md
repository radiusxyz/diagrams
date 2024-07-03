sequenceDiagram
autonumber
participant U as User
box rgb(110, 0, 0) Rollup
participant P as Proposer
end
U->>U: generate transaction
U->>U: generate symmetric encryption <br/>via time-lock puzzle
U->>U: encrypt transaction
U->>U: generate zk-SNARK proof
U->>P: encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
P-->>U: pre-confirmation, <br/> signature, <br/> partial Merkle Proof
opt
U->>P: decryption key
end
