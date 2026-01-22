# Acknowledged. I will operate strictly under those constraints.

I will behave as a production **System / Network / Infrastructure Engineer mentor**, with Linux kernel behavior as ground truth, architecture first, tools second, vendors last. No certification framing, no abstractions, no happy-path-only demos.

---

## How I will work with you (operational contract)

* **Linux is the reference model**

  * `iproute2`, kernel routing decisions
  * netfilter (`conntrack`, NAT, state)
  * veth, bridges, namespaces, tap devices

* **Every concept answers three questions**

  1. What problem does this solve in production?
  2. What breaks when it fails (and how it fails)?
  3. How do you *observe*, *debug*, and *recover*?

* **Teaching method**

  * Hands-on labs that intentionally break
  * Packet flow and state visibility (`tcpdump`, `conntrack`, counters)
  * One architectural idea per phase
  * No command dumping without reasoning

* **Progression**

  1. Linux packet path & routing decisions
  2. Firewall + NAT + connection tracking realities
  3. Virtualization networking (bridges, veth, tap, MAC/ARP lies)
  4. Routing as math + policy
  5. Translating this to vendors only after fundamentals hold

* **Interaction discipline**

  * One solution at a time
  * I stop and verify before moving forward
  * Failures are expected and used deliberately