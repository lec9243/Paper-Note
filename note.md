## Concurrent Entanglement Routing for Quantum Networks: Model and Designs
#### 2023.01.06

-----------------------------
### Goal and Terminology
**safety:** it is ok to have untrusted nodes; entanglement swap doesn't let repeaters know thew data bits

**concurrent:** multiple source-destination pairs

**network topology** G = <V,E,C> (V: nodes, E: edges, C: channels)

**success rate** p_c = e^(-alpha*L)

-----------------------------
### Time slot
* phase 1: all nodes receive information of S-D pairs 
* phase 2: find paths between S-D pairs (which is what QAM can simulate), assign qubits to channels
* phase 3: each nodes knows its own link states via classical communications with its neighbors
* phase 4: nodes perform entanglement swapping to establish long-distance quantum entanglement using successful entanglement
  + phase 3 and 4 should be short, node doesn't know global but just k-hops.
  + short entanglement persistent time T sets the limit that t_2+t_3+t_4<T (1.46s for discohere, 165us for entanglement establishment)
  + in P2, a link successfullu built is q; in P4, a swapping succeeds in probability
  
-----------------------------
### Routing Algorithm
* path computation based on global tolopogy and path recovery based on local link states

**routing metric/expected throughput(EXT)** E_t = q^h * \sum_{i=1}i* P_h^i
  + Sum of node distances
  + Creation rate (success rate)
  + Bottleneck capacity

#### QPass
**Core idea** pre-compute potential good paths, then every node uses an online algorithm to make qubit-to-channel assignments and make local swapping

Two steps of algorithm:
1. paths computed from offline algorithm for S-D pairs are put into a priority queue, ordered by the selected routing cost metric, also check the capacity. Ends until no paths can be fully satisfied. (Major paths)
2. compute partial paths (recovery paths)

Disadvantages:
* compute candidate paths for n(n-1)/2 pairs
* candidate paths exhibit a low utilization rate

#### QCast
contention-free on qubits and channels, output are highly overlapped
1. using Extended Dijkstra's algorithm to find best path in terms of oruting metric EXT
2. futher selects path with highest EXT and reserve the resources of this path, network topology is updated to the residual graph. Greedy EDA.
  + bound the path length, set upper bound threshold h_m for the hopcount of major paths
  
**Fairness** because aim to maximize throughput, each time slot is considered totally separately --> multiplied a factor to EXT

**Prioritized routing** add priority classes to S-D pairs to first select paths for high priority pairs.
  
  
