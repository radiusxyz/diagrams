sequenceDiagram
%% participant S3 as Sequencer 3 <br/> (Follower)
participant S2 as Sequencer 2 <br/> (Follower)
participant S1 as Sequencer 1 <br/> (Leader)
participant L1 as SSAL(L1)
participant R as Rollup
participant A as Aggregator
L1->>S1: for every block creation event(that event gives ssal block number)
L1->>S2: for every block creation event(that event gives ssal block number)
L1->>R: for every block creation event(that event gives ssal block number)
S1->>L1: get the sequencer list based on that number
S2->>L1: get the sequencer list based on that number
R->>L1: get the sequencer list based on that number
R->>S1: build_block(L1 block number, rollup block number)
S1->>S2: propagate the message
S1->>S1: checks whether the current ssal block number and current ssal block number sent by rollup are withing the margin
S2->>S2: checks whether the current ssal block number and current ssal block number sent by rollup are withing the margin
S1->>S1: calculate the next leader by getting the remainder <br/>from dividing the rollup block number <br/>mod sequencer list from L1
%% the main motivation behind margin is to force rollup to update ssal block number so that the sequencer list is updated so that there is fair distribution of rewards
%% so that we can handle registering and deregegistering the sequencer
