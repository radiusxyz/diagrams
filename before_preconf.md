sequenceDiagram
autonumber
participant U as User
participant S as Sequencer Set
U->>U: generate transaction
U->>U: generate symmetric encryption <br/>via time-lock puzzle
U->>U: encrypt transaction
U->>U: generate zk-SNARK proof
U->>S: encrypted tx, <br/>zk-proof, <br/>time-lock puzzle
S-->>U: pre-confirmation, <br/> signature, <br/> partial Merkle Proof
opt
U->>S: decryption key
end
