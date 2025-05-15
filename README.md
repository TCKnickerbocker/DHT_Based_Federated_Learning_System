# DHT-based Federated Learning System  
### Jun-Ting Hsu, Thomas Knickerbocker  

---

## 🧠 Overview

This project implements a **federated learning system** built atop a **Chord Distributed Hash Table (DHT)**. It enables a decentralized collection of compute nodes to collaboratively train machine learning models on partitioned data without requiring a centralized parameter server. Nodes are organized using the Chord DHT protocol to ensure scalable, fault-tolerant peer-to-peer communication and routing.

---

## ⚙️ Core Components

### 1. **Supernode**
- Acts as the bootstrap node.
- Handles new compute node registration and assigns unique node IDs.
- Whitelists nodes based on `compute_nodes.txt`.
- Provides initial connection info to join the DHT ring.

### 2. **Compute Nodes**
- Participate in the Chord ring using the Chord protocol.
- Perform local model training on assigned data.
- Return model updates to the client.
- Maintain a finger table, predecessor, and successor for routing and ring stabilization.

### 3. **Client**
- Connects to the network via the supernode.
- Distributes training data among compute nodes.
- Collects and averages model parameters.
- Validates the aggregated model against a validation dataset.

---

## ✅ Validity and Design Considerations

- **Scalability**: Chord routing ensures O(log N) efficiency.
- **Fault Tolerance**: Nodes maintain successor/predecessor links to handle failures.
- **Decentralization**: No central server or parameter aggregator needed.
- **Dynamic Join/Leave**: Nodes can join/exit during training.
- **Security**: Basic access control enforced by the supernode.

---

## 🔍 Use Cases

- **Privacy-Preserving ML**: Sensitive data remains local to each node.
- **Edge or Resource-Limited Networks**: Ideal for IoT, volunteer computing, etc.
- **Distributed ML Research**: Useful for academic experimentation with federated learning and DHTs.
- **Dynamic Topologies**: Adaptable to networks with unstable or transient nodes.

---

## 🏗️ System Architecture

<img src="./DHT%20Diagram.png" alt="System Diagram" title="System Diagram" length="1200" width="500" />

---

## 🔄 How It Works

### 🧭 Node Bootstrapping
- The **supernode** initializes the network and grants access to eligible compute nodes.
- **Compute nodes**:
  - Receive a node ID and connection point.
  - Join the ring using Chord stabilization.
  - Store and train on assigned data.

### 🤝 Client Workflow
- The **client**:
  - Connects to the Chord ring via the supernode.
  - Partitions and sends training data.
  - Collects model outputs.
  - Aggregates the results via averaging.
  - Validates the final model.

---

## 🚀 Getting Started
**First time only:**
```bash
thrift --gen py service.thrift
```
1. Start supernode:
`python3 supernode.py`
2. Start compute nodes:
`python3 compute_node.py <por_no>`
repeat for each node, i.e.:
```bash
python3 compute_node.py 8091
python3 compute_node.py 8092
python3 compute_node.py 8093
python3 compute_node.py 8094 
```
3. Start client:
```bash
python3 client.py ./letters validate_letters.txt
```


