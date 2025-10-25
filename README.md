# Project Title:

**In-NAND Fast Cache Architecture for DRAM-Less AI Datacenter SSDs**

---

## 1. Objective

To design and implement a **DRAM-less NAND flash architecture** optimized for **AI datacenter workloads**, using a **dedicated SLC-based fast-access region** within NAND for handling:

* Flash Translation Layer (FTL) mapping tables
* Write coalescing
* Metadata and journaling
* Hot data caching

This approach eliminates the need for external DRAM while maintaining **near-DRAM performance characteristics** for high-frequency AI I/O workloads such as large model weight access, vector databases, and distributed training pipelines.

---

## 2. Background

Conventional SSD architectures rely on **external DRAM** for:

* FTL mapping lookups (L2P, P2L tables)
* I/O caching and buffering
* Metadata handling

However, in **AI datacenters**, DRAM:

* Adds cost and power overhead.
* Introduces design complexity and limits scalability.
* Consumes precious board space.

At the same time, **AI models** (e.g., LLMs, vision transformers, embeddings) demand:

* High random read throughput.
* Low latency for model parameter loading.
* Predictable I/O behavior under massive concurrent access.

Thus, eliminating DRAM while maintaining performance could **revolutionize AI storage density and efficiency**.

---

## 3. Core Idea

We propose a **“pseudo-DRAM layer”** built directly inside the NAND array  using **a controlled SLC flash partition** managed by the firmware.

### Key Principle:

> Replace external DRAM with an internal, NAND-based SLC cache tier, tuned for low-latency control operations (FTL, hot-data buffers, etc.) without adding discrete memory modules.

This “in-NAND fast cache” acts as a **firmware-managed scratchpad** for all control and metadata operations, allowing:

* DRAM-like access patterns.
* Lower power and cost footprint.
* Higher system integration density.

---

## 4. System Architecture Overview

### a. Physical Layout

| Component               | Role                           | Memory Type                           |
| ----------------------- | ------------------------------ | ------------------------------------- |
| **SLC Fast Zone**       | FTL tables, hot data, metadata | High-speed NAND partition (SLC mode)  |
| **SLC Main Zone**       | User data                      | Enterprise-grade NAND (SLC-only)      |
| **Controller Firmware** | Cache manager + allocator      | Embedded microcontroller or FPGA core |

### b. Firmware Subsystems

* **FTL Engine:** optimized for in-place mapping updates using fast-zone storage.
* **Wear-Leveling Controller:** ensures balanced writes across SLC blocks.
* **Garbage Collector:** aware of cache and data zones; preserves low-latency zones.
* **Prefetch Engine (AI-Aware):** anticipates access patterns based on model workload traces.

---

## 5. Research Objectives

1. **Develop a firmware-controlled in-NAND cache subsystem** capable of sustaining near-DRAM access latency for metadata and hot data.
2. **Eliminate external DRAM** entirely from SSD controller design without sacrificing throughput.
3. **Optimize FTL performance** by dynamically maintaining mapping tables in the SLC fast zone.
4. **Benchmark performance** against DRAM-based enterprise SSDs under AI workloads (e.g., large tensor access, embedding table lookups).
5. **Evaluate scalability and energy efficiency** for large multi-rack AI systems.

---

## 6. Technical Goals

| Metric                 | Target                                      |
| ---------------------- | ------------------------------------------- |
| FTL lookup latency     | < 5 µs                                      |
| Metadata write latency | < 10 µs                                     |
| Sequential throughput  | > 6 GB/s                                    |
| Random read IOPS       | > 1M                                        |
| Power consumption      | 20–30% lower vs DRAM-based SSDs             |
| Endurance              | 30K+ P/E cycles (industrial SLC-grade NAND) |

---

## 7. Experimental Design

1. **Simulation Phase**

   * Model NAND timing, latency distribution, and internal bus contention.
   * Simulate different fast-zone sizes (1%, 5%, 10% of NAND).

2. **Prototype Phase**

   * Implement firmware in FPGA or RISC-based controller.
   * Emulate in-NAND cache behavior and benchmark FTL performance.

3. **Evaluation Phase**

   * Test against DRAM-equipped enterprise SSDs under AI-oriented I/O traces (PyTorch data loader, HuggingFace model loading, etc.).
   * Measure total access latency, energy efficiency, and thermal profile.

---

## 8. Potential Impact

| Area                   | Benefit                                         |
| ---------------------- | ----------------------------------------------- |
| **Cost Efficiency**    | Removes external DRAM; simplifies PCB.          |
| **Scalability**        | Easier to integrate many drives in dense racks. |
| **Power Optimization** | Reduces DRAM refresh and I/O power draw.        |
| **AI Performance**     | Faster model weight fetch and batch loading.    |
| **Reliability**        | Fewer components : lower failure rate.          |

---

## 9. Future Directions

* Integration with **CXL-based storage-class memory** systems.
* Firmware-level compression for model tensors in fast-zone.
* AI-optimized prefetch engine using temporal access prediction.
* Multi-drive coordination (FTL table sharing between drives for distributed AI).

---

## 10. Conclusion

This project introduces a **new storage paradigm** for AI infrastructure  replacing external DRAM with an **in-NAND SLC-based caching layer**.
It combines:

* Enterprise-grade performance,
* Simplified hardware design, and
* Firmware-driven intelligence.

By leveraging the raw speed potential of SLC and intelligent caching mechanisms, this design enables **AI datacenters** to achieve **faster data throughput, lower latency, and greater energy efficiency** without DRAM dependency.
