participant User as U
participant Sequencer1 as S1
participant Sequencer2 as S2
participant Leader as L
participant Rollup as R
U->S1: TX1
U->S2: TX2
S1->L: forward TX1
S2->L: forward TX2
L->L: randomly select \n sequencer
L->S2: forward TX1
S2->S2: order TX1, sig
S2->L: forward TX1 oc, sig
L->S1: forward TX1 oc, sig
L->S1: fowrard TX2
S1->S1: order TX2, sig
S1->L: forward TX2 oc, sig
L->S2: forward TX2 oc, sig
S1->U: TX1 oc, sig
S2->U: TX2 oc, sig
L-->R: block
R->L: get block
