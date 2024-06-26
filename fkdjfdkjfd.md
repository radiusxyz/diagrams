sequenceDiagram
%% participant S3 as Sequencer 3 <br/> (Follower)
participant S2 as Sequencer 2 <br/> (Follower)
participant S1 as Sequencer 1 <br/> (Leader)
participant L1 as SSAL(L1)
participant R as Rollup
participant A as Aggregator

%% leader propagates to all followers concurrently
%% random another follower when leader does not respond
%% always sync to the status of the one who received the request from the rollup
%% when/if the leader revives he asks any of the followers about the whether
%%the rollup asked or not, and new leaders syncs to that followers(new leaders)
%% block-height, meaning the enc tx and order commitment.
%% The follower submits the block-commitment.
