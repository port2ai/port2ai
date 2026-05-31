# Proxmox SDN/BGP Integration Submission Plan

## Further Clarification Questions

Before implementation, I would confirm the following so the work can be completed safely and without avoidable routing disruption:

- What Proxmox VE version is running on each node, and is this a single cluster or multiple clusters?
- How many Proxmox nodes are in scope, and what are their management, storage, migration, and uplink interfaces?
- Is the desired design plain BGP route advertisement, EVPN/VXLAN overlay networking, or both?
- What router, firewall, or switch platform will Proxmox peer with for BGP?
- Will BGP use private ASNs only, or are public ASN/prefix announcements involved?
- What prefixes, VNets, VLANs, or tenant networks must be advertised?
- Are exit nodes required for north-south traffic from SDN networks to the physical network or internet?
- Should routing be active/active, active/passive, or tied to specific Proxmox nodes?
- Are there existing FRRouting, OPNsense, pfSense, MikroTik, Cisco, Juniper, Arista, VyOS, or Linux router configurations to preserve?
- What firewall/security policy should apply between SDN networks, host management, storage, and public access?
- Is there a maintenance window, acceptable downtime, and rollback requirement?
- What monitoring or validation tools are already in place, such as Zabbix, Grafana, LibreNMS, Prometheus, or syslog?
- Is remote access available through VPN or bastion host, and will console/out-of-band access be available during changes?
- Should the final deliverable include only configuration, or also diagrams, runbooks, automation, and knowledge transfer?

## Proposed Submission Message

Hello,

I can help design and implement the Proxmox SDN/BGP integration with a controlled delivery process: discovery, architecture, configuration, validation, rollback planning, and final documentation.

My approach is to first review the current Proxmox cluster, physical network, routing design, ASN/prefix plan, and security requirements. From there I would produce a proposed SDN/BGP design, confirm the change plan with you, implement during an agreed window, validate route exchange and guest connectivity, then provide a final runbook so the setup can be maintained confidently.

Relevant work areas include Proxmox VE networking, SDN zones and VNets, EVPN/VXLAN, FRRouting, BGP peering, VLAN/bridge design, firewall policy review, monitoring checks, and production-safe rollback planning.

## Delivery Goals

- Integrate Proxmox SDN with BGP in a stable, documented, and supportable way.
- Ensure SDN VNets or EVPN zones route correctly through the intended upstream network.
- Avoid route leaks, asymmetric routing, management-plane exposure, or downtime caused by unclear network boundaries.
- Provide validation evidence showing BGP sessions, learned routes, advertised routes, VM/LXC connectivity, and failover behavior.
- Leave the client with clear diagrams, final configuration notes, and operational handoff documentation.

## Requirements Coverage Plan

### Proxmox SDN Review

- Confirm Proxmox VE version, cluster health, node membership, and SDN package readiness.
- Review existing Linux bridges, bonds, VLAN-aware bridges, MTU settings, and physical uplinks.
- Inspect current SDN configuration for zones, VNets, subnets, IPAM, DNS, and firewall integration.
- Identify whether the right target is Simple, VLAN, VXLAN, or EVPN zone design.

### BGP/EVPN Design

- Define local and remote ASNs, router IDs, peer addresses, update source, timers, and authentication if supported.
- Decide whether BGP is used for underlay routing, EVPN overlay control plane, external route advertisement, or all of these.
- Define route targets, VNIs, L3VNI/L2VNI mapping, and any import/export requirements for EVPN.
- Confirm route filtering so only approved prefixes are advertised.
- Plan failover behavior for multi-node exit, active/active routing, or primary exit node design.

### Security and Risk Controls

- Preserve management-plane isolation from tenant/guest SDN networks.
- Apply explicit route filters, prefix lists, and BGP policy where supported.
- Confirm firewall behavior at the Proxmox, router, and guest-network boundaries.
- Avoid public route announcement unless ownership, ASN, RPKI/IRR, and provider approval are confirmed.
- Prepare rollback steps before production changes begin.

### Implementation

- Back up current Proxmox networking and SDN configuration.
- Stage SDN controller, zone, VNet, subnet, and BGP/EVPN settings.
- Configure or coordinate required upstream router BGP peer settings.
- Apply changes in a maintenance window if production workloads are affected.
- Reload SDN configuration and verify generated FRRouting/network state.
- Test guest connectivity, route propagation, failover, and firewall boundaries.

### Validation

- Confirm Proxmox cluster and node health after changes.
- Verify BGP neighbor state reaches `Established`.
- Verify advertised and received prefixes match the approved route plan.
- Confirm SDN VNets are reachable from intended networks only.
- Test VM/LXC traffic across nodes, across VNets if required, and through exit nodes.
- Simulate node or peer failure where safe and confirm expected route convergence.
- Check logs for FRR, Proxmox SDN, kernel networking, and firewall drops.

### Documentation and Handoff

- Provide a current-state summary and final-state design.
- Include a topology diagram with nodes, uplinks, routers, ASNs, VNIs, VLANs, and routed prefixes.
- Document key Proxmox SDN objects and upstream BGP configuration.
- Provide validation commands and expected outputs.
- Provide rollback instructions.
- Provide maintenance notes for adding future VNets, nodes, prefixes, or BGP peers.

## Work Phases

### Phase 1: Discovery and Access Review

Estimated time: 2-4 hours

Deliverables:

- Access confirmation
- Current-state checklist
- Risk notes
- Missing information list

Client inputs needed:

- Proxmox UI/API or SSH access
- Router/firewall read-only configuration
- Network diagram if available
- Prefix, ASN, VLAN, and VNet requirements

### Phase 2: Architecture and Change Plan

Estimated time: 3-6 hours

Deliverables:

- Target SDN/BGP design
- Change sequence
- Rollback plan
- Validation checklist

Client approval needed:

- Approved prefixes and route policy
- Approved maintenance window
- Confirmation of downtime tolerance

### Phase 3: Configuration and Integration

Estimated time: 4-10 hours depending on cluster size and router complexity

Deliverables:

- Proxmox SDN controller, zone, VNet, and subnet configuration
- BGP/EVPN peer configuration
- Router-side coordination or config recommendations
- Applied route filtering and firewall controls

### Phase 4: Testing and Stabilization

Estimated time: 2-5 hours

Deliverables:

- BGP session verification
- Route advertisement verification
- VM/LXC connectivity tests
- Failover or convergence checks where safe
- Log review and cleanup

### Phase 5: Final Documentation and Handoff

Estimated time: 2-4 hours

Deliverables:

- Final runbook
- Network diagram
- Config summary
- Validation evidence
- Operational notes and recommended next steps

## Estimated Timeline

For a small to medium Proxmox cluster, this can likely be completed in 2-4 working days after access and requirements are confirmed.

- Day 1: Discovery, current-state review, clarification, and target design.
- Day 2: Change plan approval and staged configuration.
- Day 3: Implementation, validation, troubleshooting, and stabilization.
- Day 4: Documentation, handoff, and optional knowledge-transfer session.

Complex multi-site, public ASN, DDoS, carrier, or EVPN multi-cluster requirements may require additional time.

## Access Needed

- Proxmox administrative access or a role with SDN/network visibility and change permissions.
- SSH access to Proxmox nodes for network and FRR validation.
- Router/firewall access or a network contact who can apply BGP peer configuration.
- Existing diagrams, IPAM exports, prefix lists, ASN details, VLAN lists, and firewall policy notes.
- Maintenance window and rollback contact.

## Acceptance Criteria

The project should be considered complete when:

- BGP peers are established and stable.
- Approved SDN prefixes are advertised to the intended peers.
- Unapproved prefixes are not advertised.
- VM/LXC workloads attached to target VNets have expected connectivity.
- Management, storage, and migration networks remain isolated.
- Failover behavior is tested or documented if testing is too risky.
- Final configuration and runbook are delivered.

## Risks and Mitigations

- Risk: Route leaks or incorrect prefix advertisement.
  Mitigation: Use explicit prefix filters, peer policy, and pre-approved route lists.

- Risk: Loss of management access during network changes.
  Mitigation: Require console/out-of-band path, staged changes, and rollback commands.

- Risk: MTU mismatch with VXLAN/EVPN overlays.
  Mitigation: Review physical MTU, bridge MTU, VNet MTU, and guest MTU before rollout.

- Risk: Existing FRR or manual network configuration conflicts with Proxmox-generated SDN configuration.
  Mitigation: Inventory current configuration and decide whether Proxmox SDN or manual FRR owns each routing function.

- Risk: Multi-exit routing loops or asymmetric traffic.
  Mitigation: Define exit-node policy, route preference, local routing behavior, and test traffic paths before completion.

## Optional Enhancements

- Convert the final design into Ansible or Terraform where the environment supports it.
- Add monitoring for BGP neighbor state, route count, interface state, and SDN zone health.
- Add periodic configuration backups.
- Add diagrams for tenant/customer networks.
- Add a post-implementation security review for Proxmox firewall, host hardening, and access control.

## Suggested Fixed-Scope Milestones

### Milestone 1: Discovery and Design

Output: Current-state review, clarification answers, target architecture, implementation plan, and rollback plan.

### Milestone 2: Implementation and Validation

Output: Configured Proxmox SDN/BGP integration, tested route exchange, guest connectivity validation, and troubleshooting.

### Milestone 3: Documentation and Handoff

Output: Final runbook, topology diagram, configuration summary, and maintenance instructions.

## Notes for Our Internal Use

- Do not accept production implementation without a discovery milestone.
- Ask for topology, version, ASN, and prefix details before committing to a final price.
- Avoid promising public BGP announcements unless provider authorization, prefix ownership, RPKI/IRR status, and router access are confirmed.
- Position this as careful network engineering plus documentation, not a quick one-click Proxmox task.
