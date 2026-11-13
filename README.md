# Secured Multi-VLAN Network Lab (Cisco Packet Tracer)

A multi-tier **Secured Multi-VLAN Campus Network** implemented in **Cisco Packet Tracer**, demonstrating Layer 2 redundancy, Spanning Tree hardening, port-level access security, centralized VLAN management, Router-on-a-Stick inter-VLAN routing, automated DHCP provisioning, and encrypted remote administration.

<img width="1485" height="640" alt="topology png" src="https://github.com/user-attachments/assets/f08bdbe8-c003-484d-aa57-9ce3b53ab88c" />

---

## 📌 Project Architecture & Topology

The network follows a structured hierarchical design separating routing, core switching, access distribution, and server hosting:

* **Routing & Edge Layer:** `Router0` (Cisco 1941) handling Inter-VLAN Routing (Router-on-a-Stick), centralized DHCP pools, and the dedicated Servers subnet (`Gig0/1`).
* **Core Layer:** `Core-SW` (Cisco Catalyst 3560 Multilayer Switch) acting as the VTP Server, Spanning Tree Root Bridge, and aggregation point via LACP EtherChannel.
* **Access Layer:** Three Cisco Catalyst 2960 switches (`Switch0`, `Switch1`, `Switch2`) serving isolated user VLANs with Layer 2 security hardening, plus `Switch3` hosting the Server segment.

---

## 🚀 Key Implemented Features

1. **VLAN Segmentation & IP Addressing:**
   * **VLAN 10 (Left Segment):** `192.168.10.0/24` (Gateway: `192.168.10.1`)
   * **VLAN 20 (Center Segment):** `192.168.20.0/24` (Gateway: `192.168.20.1`)
   * **VLAN 30 (Right Segment):** `192.168.30.0/24` (Gateway: `192.168.30.1`)
   * **Servers Network:** `192.168.40.0/24` (Gateway: `192.168.40.1` | `Server0`: `.10`, `Server1`: `.11`)
2. **Inter-VLAN Routing (Router-on-a-Stick):**
   * Configured 802.1Q sub-interfaces (`Gig0/0.10`, `Gig0/0.20`, `Gig0/0.30`) on `Router0` connected to `Core-SW` (`Fa0/7` trunk).
3. **EtherChannel (Link Aggregation):**
   * Configured three active **LACP** bundles (`Po1`, `Po2`, `Po3`) between `Core-SW` and `Switch0`, `Switch1`, and `Switch2` for aggregated bandwidth and link redundancy.
4. **Spanning Tree Protocol (STP) & Root Guard:**
   * Forced deterministic Root Bridge election on `Core-SW` (`spanning-tree vlan 1,10,20,30 priority 4096`).
   * Enabled `spanning-tree guard root` on `Port-channel 1`, `Port-channel 2`, and `Port-channel 3` on `Core-SW` to prevent rogue root bridge takeovers from downstream switches.
5. **VTP (VLAN Trunking Protocol):**
   * Centralized VLAN administration with `Core-SW` as **VTP Server** and `Switch0–2` as **VTP Clients** with domain and password authentication.
6. **Layer 2 Access Security (BPDU Guard & Port Security):**
   * **Switch0 (VLAN 10):** Enabled `spanning-tree portfast` and `spanning-tree bpduguard enable` on host access ports (`Fa0/3 - 5`).
   * **Switch1 (VLAN 20):** Enabled `switchport port-security` with `maximum 1`, `mac-address sticky`, and `violation shutdown` on host access ports (`Fa0/3 - 5`).
   * **Switch2 (VLAN 30):** Enabled `spanning-tree portfast` on host access ports (`Fa0/3 - 5`).
7. **Automated IP Allocation (DHCP Server):**
   * Configured three DHCP pools (`VLAN10`, `VLAN20`, `VLAN30`) on `Router0` with `.1`–`.10` excluded per subnet and `192.168.40.10` assigned as DNS server.
8. **Management Security (SSHv2):**
   * Configured local user authentication (`privilege 15`), 1024-bit RSA encryption keys, and `transport input ssh` on `line vty` for secure remote CLI access.

---

## 🔐 Credentials & Device Access

For testing and verification in Cisco Packet Tracer:

* **Enable Secret Password:** `cisco123`
* **SSH Credentials:**
  * **Username:** `admin`
  * **Password:** `cisco123`
* **VTP Domain:** `cisco`
* **VTP Password:** `cisco`

---

## 🛠️ Verification & Testing

* ✅ **Inter-VLAN & Server Connectivity:** Verified end-to-end reachability across all VLANs and to `192.168.40.10` (`0% loss`).
* ✅ **EtherChannel Status:** Validated bundled active LACP links (`Po1`, `Po2`, `Po3`) in `(SU)` state via `show etherchannel summary`.
* ✅ **STP Root & Guard:** Confirmed `Core-SW` as Root Bridge via `show spanning-tree vlan 10` and `show spanning-tree summary`.
* ✅ **Port Security:** Verified `Secure-up` and `Shutdown` violation mode on `Switch1` via `show port-security interface fa0/3`.
* ✅ **DHCP & SSH Access:** Confirmed dynamic IP leases via `show ip dhcp binding` and authenticated remote login via `ssh -l admin 192.168.10.1`.

---

**Simulated in:** Cisco Packet Tracer  
**Author:** Mohamed Naeem
