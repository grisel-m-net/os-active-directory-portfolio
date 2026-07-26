# VLAN Routing Troubleshooting Log

*Cisco Packet Tracer Lab — Core Router / VLAN10 & VLAN20 Switches*

## 1. Issue Summary

The VLAN10-Switch was not routing properly. In the logical topology, the link between the Core Router (Cisco 2911) and the 2960-24TT VLAN10-Switch displayed a red down-arrow in Packet Tracer, while all other links in the topology showed green (up).

## 2. Environment

- **Core Router:** Cisco 2911, subinterfaces configured for router-on-a-stick (Gi0/0.10 = 172.31.10.1/24, Gi0/0.20 = 172.31.20.1/24)
- **VLAN10-Switch:** Cisco WS-C2960-24TT-L, IOS 15.0(2)SE4
- **VLAN20-Switch:** Cisco WS-C2960-24TT-L, IOS 15.0(2)SE4
- **End devices:** Server-PT (off VLAN10-Switch), PC-Client-1 and PC-Client-2 (off VLAN20-Switch)

## 3. Diagnostic Steps and Findings

| Step | Symptom / Command | Finding |
|---|---|---|
| 1 | Visual inspection of topology (Packet Tracer link status arrows) | Red down-arrow on the link between Core Router and VLAN10-Switch; all other links green. |
| 2 | `show interfaces fastethernet 0/24 status` (VLAN10-Switch) | Port Fa0/24 showed status `notconnect` — a Layer 1 issue, not a config issue. |
| 3 | `show ip interface brief` (Core Router) | GigabitEthernet0/0 and subinterfaces .10 / .20 up/up — router side healthy. Confirmed problem was downstream of the router. |
| 4 | Physical/port-status panel on Core Router | Only one physical link existed from the router at all, terminating on VLAN20-Switch (Gi0/1) — no direct router-to-VLAN10-Switch cable was ever present. |
| 5 | Port table / `show running-config` on VLAN20-Switch | GigabitEthernet0/1 (link to router) had no trunk configuration — passing only default VLAN 1. |
| 6 | `%CDP-4-NATIVE_VLAN_MISMATCH` console message | An unintended cable connected VLAN20-Switch Fa0/4 (native VLAN 20) to VLAN10-Switch Fa0/1 (native VLAN 10) — leftover/incorrect link, not part of the intended design. |

## 4. Root Causes Identified

- **Configuration error:** an early attempt to trunk VLAN10-Switch's uplink port used invalid syntax (`switchport trunk allowed 10,20` instead of `switchport trunk allowed vlan 10,20`), so the command was rejected and never applied.
- **Design/cabling gap:** no direct physical link ever existed between the Core Router and VLAN10-Switch. The intended design routes VLAN10-Switch's traffic through VLAN20-Switch, which is the only switch physically connected to the router.
- **Missing trunk configuration:** VLAN20-Switch's GigabitEthernet0/1 (the link to the router) had never been configured for trunking and was only passing default VLAN 1 traffic.
- **Miscabled link:** VLAN10-Switch Fa0/1 (access port, VLAN 10) was connected to VLAN20-Switch Fa0/4 (access port, VLAN 20), triggering a CDP native VLAN mismatch — this link was not part of the intended trunk design.

## 5. Resolution

### 5.1 Corrected trunk syntax on VLAN10-Switch (Fa0/24)

```
Switch(config)#interface fastethernet 0/24
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed vlan 10,20
```

### 5.2 Added trunk configuration on VLAN20-Switch (Gi0/1, link to router)

```
Switch(config)#interface gigabitethernet 0/1
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed vlan 10,20
```

### 5.3 Rebuilt the switch-to-switch link on the correct ports

Removed the miscabled link and reconnected VLAN10-Switch Fa0/24 to VLAN20-Switch Fa0/24 using a Copper Straight-Through cable, matching the trunk configuration on both ends.

### 5.4 Verification

Confirmed trunking status on both switches:

```
Switch#show interfaces trunk
```

Expected result: Fa0/24 (and Gi0/1 on VLAN20-Switch) listed as trunk ports allowing VLANs 1, 10, 20.

## 6. Final Topology

- Core Router (Gi0/1) ↔ VLAN20-Switch (Fa0/1) — existing link, now trunking VLANs 10 and 20.
- VLAN20-Switch (Fa0/24) ↔ VLAN10-Switch (Fa0/24) — new trunk link carrying VLAN10 traffic to and from the router.
- VLAN10 traffic reaches the Core Router by hopping through VLAN20-Switch rather than via a direct connection, since only one physical uplink to the router exists in this design.

## 7. Lessons Learned

- Packet Tracer's red/green link arrows are a fast first indicator of link status but don't explain the cause — always follow up with `show interfaces` and `show interfaces trunk`.
- A port showing `notconnect` is a Layer 1 symptom (no cable, wrong port, or wrong cable type) and should be checked before revisiting Layer 2 configuration.
- CDP native VLAN mismatch messages are a reliable way to catch unintended or miscabled links between switches.
- Always confirm actual physical connectivity in the topology before assuming a configuration-only fix will resolve a routing issue.
