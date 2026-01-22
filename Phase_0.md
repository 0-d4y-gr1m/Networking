## Phase 0 – Establish shared ground truth (non-negotiable)

Before we go anywhere, we need to align on **how packets actually move inside Linux**, because everything else (firewalls, hypervisors, routers, vendors) is layered on top of this.

### Phase 0 objective

Build an *observable*, minimal Linux network where:

* You can predict packet behavior before running commands
* You can prove your prediction using kernel-visible data

No vendors. No VMs yet. No NAT yet.