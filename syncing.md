sequenceDiagram
%% participant S3 as Sequencer 3 <br/> (Follower)
participant S2 as Sequencer 2 <br/> (Follower)
participant S1 as Sequencer 1 <br/> (Leader)
participant L1 as SSAL(L1)
participant R as Rollup
participant A as Aggregator
S1->

%% leader propagates to all followers concurrently
%% random another follower when leader does not respond
%% always sync to the status of the one who received the request from the rollup
