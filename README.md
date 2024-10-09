# Visual Guide to Consensus Algorithms

This guide provides visual representations of some of the most widely used consensus algorithms using Mermaid charts. Consensus algorithms are crucial for achieving agreement among distributed nodes in a network, ensuring consistency, availability, and fault tolerance. Each algorithm follows a unique mechanism to reach consensus among multiple nodes or replicas.

## 1. Paxos Consensus Algorithm

Paxos is a fault-tolerant distributed consensus algorithm that coordinates agreement among multiple nodes on a single value.

```mermaid
graph TD
    subgraph Paxos_Algorithm
        A[Start] --> B["Proposer selects a value"]
        B --> C["Proposer sends prepare request<br/>with proposal number"]
        C --> D["Acceptors check proposal number"]
        D --> E{"Proposal number >= max seen?"}
        E -->|Yes| F["Accept proposal and send<br/>promise to Proposer"]
        E -->|No| G["Reject proposal"]
        F --> H["Proposer sends accept request<br/>with value"]
        H --> I{"Proposal number == promised?"}
        I -->|Yes| J["Acceptors send<br/>acknowledgment"]
        I -->|No| K["Ignore request"]
        J --> L["Value is accepted"]
        L --> M[End]
    end
    
    subgraph Nodes
        P["Proposer"]
        Q["Acceptor 1"]
        R["Acceptor 2"]
        S["Acceptor 3"]
    end
    
    B -.-> P
    C -.-> Q
    C -.-> R
    C -.-> S
    J -.-> P
    
    style P fill:#bbf,stroke:#333,stroke-width:4px
    style Q fill:#f9f,stroke:#333,stroke-width:2px
    style R fill:#fbf,stroke:#333,stroke-width:2px
    style S fill:#f9f,stroke:#333,stroke-width:2px
```

## 2. Raft Consensus Algorithm

Raft is a leader-based consensus algorithm that is simpler to understand than Paxos. It divides the problem into leader election, log replication, and safety.

```mermaid
graph TD
    subgraph Raft_Algorithm
        A[Start] --> B["Leader election"]
        B --> C{"Is leader elected?"}
        C -->|Yes| D["Leader sends heartbeats<br/>to maintain authority"]
        C -->|No| E["Hold election"]
        E --> C
        D --> F["Follower appends entries<br/>from leader"]
        F --> G["Leader commits entries"]
        G --> H[End]
    end
    
    subgraph Nodes
        I["Leader"]
        J["Follower 1"]
        K["Follower 2"]
        L["Follower 3"]
    end
    
    D -.-> I
    F -.-> J
    F -.-> K
    F -.-> L

    style I fill:#bbf,stroke:#333,stroke-width:4px
    style J fill:#f9f,stroke:#333,stroke-width:2px
    style K fill:#fbf,stroke:#333,stroke-width:2px
    style L fill:#f9f,stroke:#333,stroke-width:2px
```

## 3. Zookeeper Atomic Broadcast (ZAB)

Zookeeper Atomic Broadcast (ZAB) is a consensus protocol used in Zookeeper to manage leader election and broadcast state changes.

```mermaid
graph TD
    subgraph ZAB_Algorithm
        A[Start] --> B["Leader election"]
        B --> C{"Leader elected?"}
        C -->|Yes| D["Leader broadcasts state changes<br/>to followers"]
        C -->|No| E["Hold new election"]
        E --> C
        D --> F{"Followers confirm state"}
        F --> G["Leader commits state"]
        G --> H[End]
    end
    
    subgraph Nodes
        I["Leader"]
        J["Follower 1"]
        K["Follower 2"]
        L["Follower 3"]
    end
    
    D -.-> J
    D -.-> K
    D -.-> L
    F -.-> I
    
    style I fill:#bbf,stroke:#333,stroke-width:4px
    style J fill:#f9f,stroke:#333,stroke-width:2px
    style K fill:#fbf,stroke:#333,stroke-width:2px
    style L fill:#f9f,stroke:#333,stroke-width:2px
```

## 4. Byzantine Fault Tolerance (PBFT)

Practical Byzantine Fault Tolerance (PBFT) is a consensus algorithm designed to tolerate Byzantine faults, where nodes can fail or act maliciously.

```mermaid
graph TD
    subgraph PBFT_Algorithm
        A[Start] --> B["Client sends request to primary"]
        B --> C["Primary node sends pre-prepare message"]
        C --> D["Replica nodes send prepare message<br/>to each other"]
        D --> E{"Consensus reached?"}
        E -->|Yes| F["Replicas send commit message"]
        F --> G{"Majority commit received?"}
        G -->|Yes| H["Replicas execute and<br/>send response to client"]
        G -->|No| I["Abort consensus"]
        H --> J[End]
    end
    
    subgraph Nodes
        K["Primary"]
        L["Replica 1"]
        M["Replica 2"]
        N["Replica 3"]
    end
    
    C -.-> L
    C -.-> M
    C -.-> N
    F -.-> L
    F -.-> M
    F -.-> N

    style K fill:#bbf,stroke:#333,stroke-width:4px
    style L fill:#f9f,stroke:#333,stroke-width:2px
    style M fill:#fbf,stroke:#333,stroke-width:2px
    style N fill:#f9f,stroke:#333,stroke-width:2px
```

## 5. Two-Phase Commit (2PC)

Two-phase commit (2PC) is a distributed consensus algorithm used in database systems to ensure atomic transactions.

```mermaid
graph TD
    subgraph 2PC_Algorithm
        A[Start] --> B["Coordinator sends<br/>prepare request"]
        B --> C{"All participants ready?"}
        C -->|Yes| D["Participants send<br/>vote commit"]
        D --> E["Coordinator sends<br/>commit message"]
        E --> F["All participants commit"]
        F --> G[End]
        C -->|No| H["Coordinator sends<br/>abort message"]
        H --> I["All participants abort"]
        I --> G
    end
    
    subgraph Nodes
        J["Coordinator"]
        K["Participant 1"]
        L["Participant 2"]
        M["Participant 3"]
    end
    
    B -.-> K
    B -.-> L
    B -.-> M
    D -.-> K
    D -.-> L
    D -.-> M

    style J fill:#bbf,stroke:#333,stroke-width:4px
    style K fill:#f9f,stroke:#333,stroke-width:2px
    style L fill:#fbf,stroke:#333,stroke-width:2px
    style M fill:#f9f,stroke:#333,stroke-width:2px
```

## 6. Three-Phase Commit (3PC)

Three-phase commit (3PC) enhances 2PC to prevent blocking in case of a coordinator failure.

```mermaid
graph TD
    subgraph 3PC_Algorithm
        A[Start] --> B["Coordinator sends<br/>prepare request"]
        B --> C{"All participants ready?"}
        C -->|Yes| D["Participants send<br/>pre-commit"]
        D --> E["Coordinator sends<br/>pre-commit message"]
        E --> F["Participants prepare to commit"]
        F --> G["Coordinator sends<br/>commit message"]
        G --> H["Participants commit"]
        H --> I[End]
        C -->|No| J["Coordinator sends<br/>abort message"]
        J --> K["All participants abort"]
        K --> I
    end
    
    subgraph Nodes
        L["Coordinator"]
        M["Participant 1"]
        N["Participant 2"]
        O["Participant 3"]
    end
    
    B -.-> M
    B -.-> N
    B -.-> O
    D -.-> M
    D -.-> N
    D -.-> O

    style L fill:#bbf,stroke:#333,stroke-width:4px
    style M fill:#f9f,stroke:#333,stroke-width:2px
    style N fill:#fbf,stroke:#333,stroke-width:2px
    style O fill:#f9f,stroke:#333,stroke-width:2px
```

This guide covers essential consensus algorithms used in distributed systems, highlighting the mechanisms that ensure data consistency and fault tolerance across multiple nodes.
