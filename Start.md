## Phase 0 – Establish shared ground truth (non-negotiable)

Before we go anywhere, we need to align on **how packets actually move inside Linux**, because everything else (firewalls, hypervisors, routers, vendors) is layered on top of this.

### Phase 0 objective

Build an *observable*, minimal Linux network where:

* You can predict packet behavior before running commands
* You can prove your prediction using kernel-visible data

No vendors. No VMs yet. No NAT yet.

---

## Phase 1 – Canonical base lab (very small, very strict)

### Problem this lab solves

Most network confusion comes from **not knowing where routing ends and filtering begins**, and **not knowing when state is created**.

This lab makes that explicit.

### Architecture (conceptual)

```
[ns_client] ---- veth ---- [root namespace] ---- veth ---- [ns_server]
```

* Three network namespaces
* Pure L3 routing
* No firewall rules initially
* No NAT
* No bridge

### What we will prove

* Routing happens **before** firewalling
* Reverse traffic relies on routing symmetry, not magic
* The kernel does exactly what you tell it, nothing more

### Failure modes we will induce later

* Asymmetric routing
* Missing return path
* Silent drops vs explicit rejects

---

## What I need from you before proceeding

Confirm **one** of the following so I can proceed correctly:

1. You are comfortable creating Linux network namespaces and veth pairs manually
   **OR**
2. You want me to drive Phase 1 step-by-step with verification checkpoints

Reply with **only**:

* `Proceed with Phase 1 (guided)`
* **or**
* `Proceed with Phase 1 (I’ll execute, you verify)`

Once confirmed, we start Phase 1 and do not jump ahead.