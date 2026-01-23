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