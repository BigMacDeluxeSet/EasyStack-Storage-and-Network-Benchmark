# Cloud Storage and Network Benchmarking (OpenStack-Based Environment)

> **Disclaimer**  
>This repository documents a **sanitized, vendor-neutral benchmarking workflow** derived from real-world production experience.  
>All sensitive details (credentials, internal URLs, environment-specific access methods) have been intentionally omitted or generalized.

---

## Overview
This project demonstrates how to perform **storage** and **network performance benchmarking** in an **OpenStack-based private cloud** environment.

The goal is to showcase:
- Practical performance validation methodology
- Network throughput testing using `iperf3`
- Structured, repeatable operational documentation
- Awareness of security and environment isolation constraints

This repository is intended as a **portfolio demonstration**, not a drop-in production automation.

---

## What This Covers

- Storage benchmarking using a prebuilt benchmark image
- Network benchmarking between instances on different compute hosts
- Result collection and validation
- Handling restricted / private environments

---

## Assumptions

- You are working in an **OpenStack-compatible cloud** (private or lab)
- You can upload custom images to the cloud
- You have SSH access to instances
- Benchmark artifacts (images/scripts) are prepared separately

---

## Storage Benchmarking

### Objective
Measure storage performance characteristics such as:
- IOPS
- Latency
- Throughput

### High-Level Workflow

1. Upload a **storage benchmark image** to the cloud image catalog
2. Deploy an instance using an automation or orchestration workflow
3. Allow the benchmark to execute automatically on boot
4. Collect results from the underlying compute or management layer

### Notes
- Results are typically written to log or CSV files on the host executing the benchmark
- Exact file paths depend on the platform and are intentionally not documented here

---

## Network Benchmarking

### Objective
Measure east–west network performance between compute nodes.

### Architecture

- **Instance A**: iperf3 server
- **Instance B**: iperf3 client
- Instances must be scheduled on **different physical hosts**

---

### Step 1: Create Test Instances

- Launch two instances from the same network benchmark image
- Ensure:
  - Different compute hosts
  - Network allows required traffic (e.g. iperf3 default port 5201)

---

### Step 2: Start iperf3 Server (Instance A)

```bash
nohup iperf3 -s &
```

---

### Step 3: Run Automated Network Test (Instance B)

1. Edit the benchmark script:

```bash
vim /root/network-autotest.sh
```

2. Update the server IP parameter:

```bash
IPSERVERS=<IP_OF_INSTANCE_A>
```

3. Execute the script:

```bash
nohup sh -x /root/network-autotest.sh &
```

4. The test runs automatically and writes output to log and CSV files

---

### Step 4: Validate Completion

- Output is redirected to `nohup.out`
- Successful completion is indicated by a terminal status such as:

```text
DONE
```

---

### Step 5: Collect Results

- Results are stored on the client instance
- Typical output format:
  - `.csv` files containing throughput and performance metrics

---

## Expected Outputs

| Benchmark Type | Output Format | Metrics |
|---------------|--------------|--------|
| Storage | Log / CSV | IOPS, latency, throughput |
| Network | CSV | Bandwidth, stability |

---

## Security & Sanitization Notes

- No credentials, internal URLs, or management-plane access details are included
- Environment-specific access (OTP, bastion hosts, management nodes) is intentionally abstracted
- This reflects **security-conscious documentation practices** in real environments

---

## Why This Matters

This project demonstrates:
- Real-world cloud operations experience
- Performance validation methodology
- Safe documentation practices
- Ability to abstract internal systems into reusable processes

---

## License

This repository is shared for **educational and portfolio purposes**.

---

If you have questions about adapting this workflow to your own OpenStack environment, feel free to reach out.

