sequenceDiagram
%% participant S3 as Sequencer 3 <br/> (Follower)
participant S2 as Sequencer 2 <br/> (Follower)
participant S1 as Sequencer 1 <br/> (Leader)
participant L1 as SSAL(L1)
participant R as Rollup
participant A as Aggregator
R->>S1: build_block(L1 block number, rollup block number)
S1->>S2: propagate the message
S1->>S1: calculate the next leader by getting the remainder <br/>from dividing the rollup block number <br/>mod sequencer list from L1
