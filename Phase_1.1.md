# 1) **Three network namespaces**

## What a network namespace actually is (Linux truth)

A **network namespace** is a **fully isolated copy of the Linux networking stack**.

Each namespace has its **own**:

* Interfaces (`lo`, `eth0`, `vethX`, etc.)
* Routing table
* ARP / neighbor table
* Firewall rules (nftables / iptables)
* Connection tracking table
* TCP/UDP socket state
* IP addresses and ports

They **do not see each other** unless you explicitly connect them.

> Think of a network namespace as “a whole Linux machine’s networking, without the machine.”

This is not virtualization theory — this is exactly how containers work.

---

## What “three” means in practice

When I say **three network namespaces**, I mean:

1. **Client namespace**

   * Represents a workload / endpoint
   * Has an IP and sends traffic

2. **Root namespace**

   * The default namespace on the host
   * Acts as a **router**
   * Makes forwarding decisions

3. **Server namespace**

   * Represents the destination workload
   * Receives traffic and replies

They are not special. They are peers. The only difference is **what role we assign them**.

---

## Why three is important (production reason)

If you only use **two namespaces**, you hide routing behavior.

Three forces you to understand:

* Forwarding vs local delivery
* Return path correctness
* Where state actually lives

This mirrors real systems:

* VM → hypervisor → VM
* Pod → node → pod
* Client → firewall → server

---

## **What breaks if you misunderstand namespaces**

* You think firewall rules apply globally (they don’t)
* You assume packets “just come back” (they don’t)
* You debug the wrong machine in outages
* You misplace NAT and conntrack

---

## How you observe namespaces

At any time, you can ask:

* `ip addr`
* `ip route`
* `ss -tn`
* `conntrack -L`

And the answer will be **different per namespace**.

That difference is the lesson.

---

# 2) **Pure L3 routing**

## What “L3” means here

Layer 3 = **IP routing decisions**.

No:

* Bridges
* Switch behavior
* MAC learning
* Broadcast domains
* ARP relaying tricks

Only:

* Source IP
* Destination IP
* Routing table lookup
* Next hop decision

---

## What “pure” means (this is critical)

**Pure L3 routing means:**

* Each interface is a routed interface
* Each link is its own IP subnet
* Packets are forwarded because of routes, not because of MAC flooding

There is **no bridge** in the middle doing L2 magic.

---

## Why this matters in production

Most people accidentally learn networking through:

* Docker bridges
* Hypervisor switches
* Cloud abstractions

Those hide routing failures.

Pure L3 exposes:

* Missing routes
* Asymmetric paths
* TTL behavior
* Policy routing consequences

---

## What breaks when L3 is wrong

* Traffic leaves but never returns
* ICMP works, TCP fails
* NAT “fixes” things accidentally
* Firewalls appear broken but aren’t

---

## How Linux actually handles pure L3

When a packet arrives:

1. Kernel checks: **Is this IP mine?**
2. If yes → local stack
3. If no → routing table lookup
4. Forward out selected interface
5. Decrement TTL
6. Apply forwarding firewall rules

No bridge. No guessing.

---

## How these two ideas connect

**Three namespaces + pure L3 routing** gives us:

* Explicit packet paths
* Explicit failure modes
* No hidden behavior

Which is why this is the **foundation lab**.

---

### Verification checkpoint (do not skip)

Before we move on, answer **yes or no** to each:

1. A network namespace has its own routing table and firewall rules.
2. Packets do not magically cross namespaces.
3. Pure L3 routing means forwarding decisions are made only by IP routes, not bridges.

Reply with:

```
1) yes/no
2) yes/no
3) yes/no
```

Only after this we proceed to Phase 1 execution.
