# Packet Tracer Build Guide — Small Business Network Design

**Why there is no .pkt file in this folder yet:** Cisco Packet Tracer saves labs in its own encrypted .pkt format, with no supported way to generate a valid .pkt outside Packet Tracer itself. Packet Tracer is not installed on this machine, and it is distributed through Cisco Networking Academy. A fake .pkt file made by hand would not open in Packet Tracer and would hurt credibility if a recruiter tried it.

**What IS in this folder, and what to do with it:** Complete, paste-ready Cisco IOS configs for every device in the design, plus this step-by-step build guide. Follow it in Packet Tracer on your PC, paste the configs, test, and then save a genuine .pkt file from Packet Tracer. That genuine .pkt is the file to upload to GitHub next.

## Devices to place (Packet Tracer)
- 1x Router 2911 — Edge Router
- 1x Switch 3560 — Core Switch (multilayer, Layer 3)
- 2x Switch 2960 — Access Switches (one Staff, one Guest/Servers)
- 1x Wireless Access Point or Wireless Router in AP mode — Guest and Staff wireless (see wireless notes below)
- 2x PC — one Staff PC, one Management PC (for testing)
- 1x Server — in Servers VLAN, for testing Staff-to-Server access
- 1x Laptop — Guest wireless client, for testing Guest isolation

## Cabling
1. Edge Router Gig0/0 → Cloud / ISP (Packet Tracer Cloud-PT, or a second router as ISP in a lab)
2. Edge Router Gig0/1 → Core Switch Gig0/1 (use the 192.168.1.0/30 link in the configs)
3. Core Switch Gig0/2 → Access Switch 1 Gig0/1 (trunk, VLANs 10, 20, 40)
4. Core Switch Gig0/3 → Access Switch 2 Gig0/1 (trunk, VLANs 10, 30, 40)
5. Access Switch 1 Fa0/2 → Staff PC
6. Access Switch 2 Fa0/3 → Server
7. Access Switch 2 → Wireless AP, then Laptop connects wirelessly to the Guest SSID

## Paste the configs, in this order
1. Core Switch first: paste `configs/core-switch.txt` in CLI, Config mode. Check `show vlan brief` and `show ip interface brief`.
2. Access Switches: paste `configs/access-switch.txt`, then change the access VLAN on each port to match its role (Staff = 20, Server = 40, AP uplink = 30 where noted in the file comments).
3. Edge Router last: paste `configs/edge-router.txt`. Check `show ip nat translations` after a test ping to the internet / Cloud.

## Wireless (GUI, not CLI)
- Staff SSID: WPA3 where Packet Tracer supports it, otherwise WPA2-Personal, VLAN 20, password set in the AP GUI.
- Guest SSID: separate SSID, VLAN 30, internet only. Do not bridge Guest to the internal network.
- If Packet Tracer in your version does not support WPA3 on the chosen AP, use WPA2-Personal and note that in the lab — the design document already says WPA3 where supported, WPA2 fallback where a device cannot support it.

## Test, in this order — all must pass before saving
1. Staff PC → Server: ping 192.168.40.x — should succeed.
2. Staff PC → Management gateway 192.168.10.1 — should fail (ACL, except from an admin host).
3. Guest Laptop → Internet / Cloud — should succeed.
4. Guest Laptop → Staff 192.168.20.x and Server 192.168.40.x — should fail (Guest ACL, internet only).
5. Management PC → Core Switch management 192.168.10.1 — should succeed.
6. From Core Switch: `show access-lists`, `show ip route`, `show vlan brief` — save the output in your lab notes if you want it in the GitHub README later.

## Save and upload
1. In Packet Tracer: File → Save As → `Small_Business_Network_Design.pkt`
2. Test reopening that .pkt file once, before uploading — if it does not reopen cleanly, do not upload it.
3. Upload that genuine .pkt file to GitHub, alongside the README, PDF, topology, and these configs. Do not upload a .pkt file that was not saved from Packet Tracer.
