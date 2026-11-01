# Network Architecture and Design Decisions

## Segmentation model

* **Trusted (VLAN 10):** Primary computers and personal devices. Designed to reach selected IoT services.
* **IoT (VLAN 20):** Smart-home and embedded devices. Restricted from initiating access to Trusted, Default, and Management networks by configured LAN-to-LAN deny rules.
* **Guest (VLAN 30):** Visitor devices. Restricted from Trusted, IoT, Default, and Management networks by configured LAN-to-LAN deny rules.
* **Management (VLAN 99):** Intended network for managed infrastructure; AP confirmed on this VLAN.
* **Default (VLAN 1):** Retained for some devices and controller connectivity during migration.

## Design rationale

IoT devices can receive infrequent security updates and often depend on cloud services. Separating them from trusted devices reduces unnecessary reachability. A dedicated guest network reduces exposure to personal devices. Management traffic is separated where practical to reduce the administrative attack surface.

## Important policy nuance

A router may distinguish **LAN-to-LAN** forwarded traffic from **LAN-to-LOCAL** traffic destined for its own services. Testing showed that configured inter-VLAN denies did not fully prevent access to the router's management interface from the IoT network. This is an open security finding, not a resolved control.

## Operational safeguards

* Avoid changing the native VLAN of an active uplink without a verified recovery path.
* Maintain a known-good administrative connection when changing management VLANs.
* Document and test ACL changes incrementally.
* Keep controller and device backups private, not in a public repository.

## Physical topology note

The ER605 router connects to the ES208G managed switch through a configured VLAN trunk. Additional physical port assignments are intentionally omitted from this public documentation.

