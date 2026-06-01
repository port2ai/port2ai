# Proxmox SDN/BGP Integration Submission Plan

## AI-Staff Work Disclaimer

Greg Focaccio (GF) will use AI-Staff in a supervised and controlled manner to help develop the work plan, solution approaches, documentation, validation checklists, and implementation support materials. GF will execute the remote access discovery, production implementation, configuration changes, testing, validation, and client-facing technical decisions.

## 1. Further Clarification Questions

Before implementation, GF would confirm the following so the work can be completed safely and without avoidable routing disruption:

- **1.1.** What Proxmox VE version is running on each node, and is this a single cluster or multiple clusters?
- **1.2.** How many Proxmox nodes are in scope, and what are their management, storage, migration, and uplink interfaces?
- **1.3.** Is the desired design plain BGP route advertisement, EVPN/VXLAN overlay networking, or both?
- **1.4.** What router, firewall, or switch platform will Proxmox peer with for BGP?
- **1.5.** Will BGP use private ASNs only, or are public ASN/prefix announcements involved?
- **1.6.** What prefixes, VNets, VLANs, or tenant networks must be advertised?
- **1.7.** Are exit nodes required for north-south traffic from SDN networks to the physical network or internet?
- **1.8.** Should routing be active/active, active/passive, or tied to specific Proxmox nodes?
- **1.9.** Are there existing FRRouting, OPNsense, pfSense, MikroTik, Cisco, Juniper, Arista, VyOS, or Linux router configurations to preserve?
- **1.10.** What firewall/security policy should apply between SDN networks, host management, storage, and public access?
- **1.11.** Is there a maintenance window, acceptable downtime, and rollback requirement?
- **1.12.** What monitoring or validation tools are already in place, such as Zabbix, Grafana, LibreNMS, Prometheus, or syslog?
- **1.13.** Is remote access available through VPN or bastion host, and will console/out-of-band access be available during changes?
- **1.14.** Should the final deliverable include only configuration, or also diagrams, runbooks, automation, and knowledge transfer?

### 1.15. Timeline, Urgency, and Schedule Tolerance

- **1.15.1.** What is the required completion date or target go-live date for this work?
- **1.15.2.** Is this project urgent because of an outage, pending migration, security exposure, customer commitment, billing issue, or other business deadline?
- **1.15.3.** What happens if the schedule slips by 1 day, 3 days, or 1 week?
- **1.15.4.** Is the priority speed, risk reduction, documentation quality, or a balanced delivery?
- **1.15.5.** Are there fixed maintenance windows, blackout periods, business hours, or change-control approvals that constrain when GF can implement and test changes?
- **1.15.6.** If discovery reveals additional network risk or missing access, should the timeline pause for approval or proceed with a best-effort mitigation path?

## 2. Proposed Submission Message

Hello,

We can help design and implement the Proxmox SDN/BGP integration with a controlled delivery process: discovery, architecture, configuration, validation, rollback planning, and final documentation.

Our approach is to first review the current Proxmox cluster, physical network, routing design, ASN/prefix plan, and security requirements. From there we would produce a proposed SDN/BGP design and confirm the change plan with you. GF would then execute the approved implementation during an agreed window, perform testing and validation of route exchange and guest connectivity, and provide a final runbook so the setup can be maintained confidently.

Relevant work areas include Proxmox VE networking, SDN zones and VNets, EVPN/VXLAN, FRRouting, BGP peering, VLAN/bridge design, firewall policy review, monitoring checks, and production-safe rollback planning.

## 3. Delivery Goals

- **3.1.** We will develop a stable, documented, and supportable Proxmox SDN/BGP integration plan.
- **3.2.** GF will execute the approved configuration so SDN VNets or EVPN zones route correctly through the intended upstream network.
- **3.3.** We will design controls to avoid route leaks, asymmetric routing, management-plane exposure, or downtime caused by unclear network boundaries.
- **3.4.** GF will provide validation evidence showing BGP sessions, learned routes, advertised routes, VM/LXC connectivity, and failover behavior.
- **3.5.** We will leave the client with clear diagrams, final configuration notes, and operational handoff documentation.

## 4. Requirements Coverage Plan

### 4.1. Proxmox SDN Review

- **4.1.1.** GF will confirm Proxmox VE version, cluster health, node membership, and SDN package readiness.
- **4.1.2.** GF will review existing Linux bridges, bonds, VLAN-aware bridges, MTU settings, and physical uplinks.
- **4.1.3.** GF will inspect current SDN configuration for zones, VNets, subnets, IPAM, DNS, and firewall integration.
- **4.1.4.** We will identify whether the right target is Simple, VLAN, VXLAN, or EVPN zone design.

### 4.2. BGP/EVPN Design

- **4.2.1.** We will define local and remote ASNs, router IDs, peer addresses, update source, timers, and authentication if supported.
- **4.2.2.** We will decide whether BGP is used for underlay routing, EVPN overlay control plane, external route advertisement, or all of these.
- **4.2.3.** We will define route targets, VNIs, L3VNI/L2VNI mapping, and any import/export requirements for EVPN.
- **4.2.4.** We will confirm route filtering so only approved prefixes are advertised.
- **4.2.5.** We will plan failover behavior for multi-node exit, active/active routing, or primary exit node design.

### 4.3. Security and Risk Controls

- **4.3.1.** We will preserve management-plane isolation from tenant/guest SDN networks in the solution approach.
- **4.3.2.** GF will apply explicit route filters, prefix lists, and BGP policy where supported.
- **4.3.3.** GF will confirm firewall behavior at the Proxmox, router, and guest-network boundaries.
- **4.3.4.** We will avoid public route announcement unless ownership, ASN, RPKI/IRR, and provider approval are confirmed.
- **4.3.5.** We will prepare rollback steps before production changes begin.

### 4.4. Implementation

- **4.4.1.** GF will back up current Proxmox networking and SDN configuration.
- **4.4.2.** GF will stage SDN controller, zone, VNet, subnet, and BGP/EVPN settings.
- **4.4.3.** GF will configure or coordinate required upstream router BGP peer settings.
- **4.4.4.** GF will apply changes in a maintenance window if production workloads are affected.
- **4.4.5.** GF will reload SDN configuration and verify generated FRRouting/network state.
- **4.4.6.** GF will test guest connectivity, route propagation, failover, and firewall boundaries.

### 4.5. Validation

- **4.5.1.** GF will confirm Proxmox cluster and node health after changes.
- **4.5.2.** GF will verify BGP neighbor state reaches `Established`.
- **4.5.3.** GF will verify advertised and received prefixes match the approved route plan.
- **4.5.4.** GF will confirm SDN VNets are reachable from intended networks only.
- **4.5.5.** GF will test VM/LXC traffic across nodes, across VNets if required, and through exit nodes.
- **4.5.6.** GF will simulate node or peer failure where safe and confirm expected route convergence.
- **4.5.7.** GF will check logs for FRR, Proxmox SDN, kernel networking, and firewall drops.

### 4.6. Documentation and Handoff

- **4.6.1.** We will provide a current-state summary and final-state design.
- **4.6.2.** We will include a topology diagram with nodes, uplinks, routers, ASNs, VNIs, VLANs, and routed prefixes.
- **4.6.3.** We will document key Proxmox SDN objects and upstream BGP configuration.
- **4.6.4.** We will provide validation commands and expected outputs.
- **4.6.5.** We will provide rollback instructions.
- **4.6.6.** We will provide maintenance notes for adding future VNets, nodes, prefixes, or BGP peers.

## 5. Work Phases

### 5.1. Phase 1: Discovery and Access Review

Estimated time: 2-4 hours

Deliverables:

- **5.1.1.** GF will complete access confirmation
- **5.1.2.** GF will complete the current-state checklist
- **5.1.3.** We will produce risk notes
- **5.1.4.** We will produce the missing information list

Client inputs needed:

- **5.1.5.** Proxmox UI/API or SSH access
- **5.1.6.** Router/firewall read-only configuration
- **5.1.7.** Network diagram if available
- **5.1.8.** Prefix, ASN, VLAN, and VNet requirements

### 5.2. Phase 2: Architecture and Change Plan

Estimated time: 3-6 hours

Deliverables:

- **5.2.1.** We will produce the target SDN/BGP design
- **5.2.2.** We will produce the change sequence
- **5.2.3.** We will produce the rollback plan
- **5.2.4.** We will produce the validation checklist

Client approval needed:

- **5.2.5.** Approved prefixes and route policy
- **5.2.6.** Approved maintenance window
- **5.2.7.** Confirmation of downtime tolerance

### 5.3. Phase 3: Configuration and Integration

Estimated time: 4-10 hours depending on cluster size and router complexity

Deliverables:

- **5.3.1.** GF will configure Proxmox SDN controller, zone, VNet, and subnet settings
- **5.3.2.** GF will configure BGP/EVPN peer settings
- **5.3.3.** GF will handle router-side coordination or config recommendations
- **5.3.4.** GF will apply route filtering and firewall controls

### 5.4. Phase 4: Testing and Stabilization

Estimated time: 2-5 hours

Deliverables:

- **5.4.1.** GF will perform BGP session verification
- **5.4.2.** GF will perform route advertisement verification
- **5.4.3.** GF will perform VM/LXC connectivity tests
- **5.4.4.** GF will perform failover or convergence checks where safe
- **5.4.5.** GF will perform log review and cleanup

### 5.5. Phase 5: Final Documentation and Handoff

Estimated time: 2-4 hours

Deliverables:

- **5.5.1.** We will deliver the final runbook
- **5.5.2.** We will deliver the network diagram
- **5.5.3.** We will deliver the config summary
- **5.5.4.** GF will provide validation evidence from testing
- **5.5.5.** We will deliver operational notes and recommended next steps

## 6. Estimated Timeline

For a small to medium Proxmox cluster, this can likely be completed in 2-4 working days after access and requirements are confirmed.

- **6.1.** Day 1: GF-led discovery, current-state review, clarification, and joint target design.
- **6.2.** Day 2: Change plan approval and GF-led staged configuration.
- **6.3.** Day 3: GF-led implementation, validation, troubleshooting, and stabilization.
- **6.4.** Day 4: Joint documentation, handoff, and optional knowledge-transfer session.

Complex multi-site, public ASN, DDoS, carrier, or EVPN multi-cluster requirements may require additional time.

## 7. Access Needed

- **7.1.** Proxmox administrative access or a role with SDN/network visibility and change permissions.
- **7.2.** SSH access for GF to Proxmox nodes for network and FRR validation.
- **7.3.** Router/firewall access or a network contact who can apply BGP peer configuration.
- **7.4.** Existing diagrams, IPAM exports, prefix lists, ASN details, VLAN lists, and firewall policy notes.
- **7.5.** Maintenance window and rollback contact.

## 8. Acceptance Criteria

The project should be considered complete when:

- **8.1.** BGP peers are established and stable.
- **8.2.** Approved SDN prefixes are advertised to the intended peers.
- **8.3.** Unapproved prefixes are not advertised.
- **8.4.** VM/LXC workloads attached to target VNets have expected connectivity.
- **8.5.** Management, storage, and migration networks remain isolated.
- **8.6.** Failover behavior is tested or documented if testing is too risky.
- **8.7.** Final configuration and runbook are delivered.

## 9. Risks and Mitigations

- **9.1.** Risk: Route leaks or incorrect prefix advertisement.
  Mitigation: Use explicit prefix filters, peer policy, and pre-approved route lists.

- **9.2.** Risk: Loss of management access during network changes.
  Mitigation: Require console/out-of-band path, staged changes, and rollback commands.

- **9.3.** Risk: MTU mismatch with VXLAN/EVPN overlays.
  Mitigation: Review physical MTU, bridge MTU, VNet MTU, and guest MTU before rollout.

- **9.4.** Risk: Existing FRR or manual network configuration conflicts with Proxmox-generated SDN configuration.
  Mitigation: Inventory current configuration and decide whether Proxmox SDN or manual FRR owns each routing function.

- **9.5.** Risk: Multi-exit routing loops or asymmetric traffic.
  Mitigation: Define exit-node policy, route preference, local routing behavior, and test traffic paths before completion.

## 10. Optional Enhancements

- **10.1.** Convert the final design into Ansible or Terraform where the environment supports it.
- **10.2.** Add monitoring for BGP neighbor state, route count, interface state, and SDN zone health.
- **10.3.** Add periodic configuration backups.
- **10.4.** Add diagrams for tenant/customer networks.
- **10.5.** Add a post-implementation security review for Proxmox firewall, host hardening, and access control.

## 11. Suggested Fixed-Scope Milestones

### 11.1. Milestone 1: Discovery and Design

Output: GF-led current-state review plus jointly developed clarification answers, target architecture, implementation plan, and rollback plan.

### 11.2. Milestone 2: Implementation and Validation

Output: GF-configured Proxmox SDN/BGP integration, tested route exchange, guest connectivity validation, and troubleshooting.

### 11.3. Milestone 3: Documentation and Handoff

Output: Jointly prepared final runbook, topology diagram, configuration summary, and maintenance instructions.

## 12. Notes for Our Internal Use

- **12.1.** Do not accept production implementation without a discovery milestone.
- **12.2.** Ask for topology, version, ASN, and prefix details before committing to a final price.
- **12.3.** Avoid promising public BGP announcements unless provider authorization, prefix ownership, RPKI/IRR status, and router access are confirmed.
- **12.4.** Position this as careful network engineering plus documentation, not a quick one-click Proxmox task.
- **12.5.** Keep AI-Staff assistance scoped to supervised planning, documentation, solution analysis, and support materials; GF owns remote access discovery, production implementation, testing, and validation.
