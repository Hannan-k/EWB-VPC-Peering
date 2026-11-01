# AWS VPC Peering — EWB Lab

> **Project status:** Core connectivity was demonstrated in the supplied terminal evidence. Review the verification notes below before treating every submission checklist item as complete.

## Overview

This project demonstrates private network connectivity between two Amazon VPCs using VPC Peering. A Bastion EC2 instance provides SSH access into the lab environment. App-A and App-B communicate over private IPv4 addresses through the peering connection.

## Architecture

![AWS VPC Peering architecture](architecture/vpc-architecture.png)

### Observed resources

| Resource | Observed value |
|---|---|
| AWS Region | US East (N. Virginia), `us-east-1` |
| VPC A | `ewb-vpc-a` |
| VPC A CIDR | `10.10.0.0/16` |
| VPC B | `ewb-vpc-b` |
| VPC B CIDR | `10.20.0.0/16` |
| Peering connection | `pcx-03049e1619553aed5` |
| Peering status | Active in captured console evidence |
| Bastion public IPv4 | `32.199.138.134` |
| App-A private IPv4 | `10.10.2.10` |
| App-B private IPv4 | `10.20.1.10` |

**Note:** The screenshots show subnet identifiers truncated in at least one console view. Add the exact subnet IDs and subnet CIDRs from the AWS Subnets page before final submission. Do not infer subnet CIDRs from the EC2 private IP addresses.

## AWS services used

- Amazon VPC
- EC2 instances
- VPC subnets
- VPC route tables
- VPC Peering
- Security Groups
- SSH and ICMP for connectivity testing

## Network design and traffic flow

1. The administrator connects to the Bastion host using SSH over its public IPv4 address.
2. From the Bastion, SSH access is used to reach the private EC2 instances.
3. App-A (`10.10.2.10`) sends traffic to App-B (`10.20.1.10`) using the VPC peering connection.
4. App-B sends return traffic to App-A using the reverse route through the same peering connection.
5. Each VPC route table must contain a route for the other VPC's CIDR with the peering connection as target.

### Required route entries

| Route table | Destination | Target |
|---|---|---|
| VPC A public route table | `10.20.0.0/16` | `pcx-03049e1619553aed5` |
| VPC A private route table | `10.20.0.0/16` | `pcx-03049e1619553aed5` |
| VPC B private route table | `10.10.0.0/16` | `pcx-03049e1619553aed5` |

The VPC A public route table screenshot visibly shows the peering route. The VPC B private route table screenshot visibly shows the reverse peering route. Confirm the VPC A private route table contains its peering route and that each table is associated with the intended subnet before submission.

## Connectivity verification

### SSH to Bastion

From the local Linux machine, the captured session used:

```bash
chmod 400 ~/.ssh/ewb-lab.pem
ssh -i ~/.ssh/ewb-lab.pem ec2-user@32.199.138.134
```

### SSH agent forwarding from Bastion

The captured workflow used agent forwarding:

```bash
ssh -A -i ~/.ssh/ewb-lab.pem ec2-user@32.199.138.134
```

From the Bastion shell, SSH to App-A and App-B using their private IP addresses:

```bash
ssh ec2-user@10.10.2.10
ssh ec2-user@10.20.1.10
```

Agent forwarding and the relevant authorized public keys must be configured for these SSH hops. Do not upload the private key.

### ICMP tests

From App-A:

```bash
ping -c 4 10.20.1.10
```

From App-B:

```bash
ping -c 4 10.10.2.10
```

The supplied screenshots include a 100% packet-loss test and later successful ping evidence in both directions. The successful tests show 4 packets received and 0% packet loss.

## Controlled ICMP failure and recovery

A captured terminal screenshot shows App-A pinging App-B with 100% packet loss. Later screenshots show successful connectivity. To fully satisfy a controlled-failure experiment, include separate evidence showing:

1. The exact ICMP security-group rule before removal.
2. The rule removed or restricted.
3. The resulting failed ping.
4. The rule restored.
5. The successful ping after recovery.

The currently supplied screenshots show failure and later success, but do not conclusively prove the exact rule-removal action. Add those missing console captures if the instructor requires proof of a deliberate controlled change.

## Evidence

See [`screenshots/`](screenshots/) for the original AWS console and terminal captures. The App-B security-group files are two captures of its rule-edit screen, not separate App-A/App-B proof. The private-route edit captures include duplicate-route attempts; verify the saved route tables in the live console before describing them as final. The PDF report is included at the repository root.

## Security and cleanup

- Never commit `.pem` private keys, passwords, access keys, tokens, or local SSH configuration.
- Review screenshots for account identifiers or other details you do not want to publish.
- After evaluation, terminate EC2 instances and remove any lab-only resources only when they are no longer needed and you have confirmed the instructor does not require them to remain available.
- Remove unused Elastic IPs and other billable resources where applicable.
- Keep the VPCs/peering resources only if the lab or evaluator requires them to remain.

## Before submission — final verification

- [ ] Add your name, batch, project title, AWS Region and completion date to the PDF.
- [ ] Confirm exact subnet IDs, subnet CIDRs and all three route-table associations.
- [ ] Verify the VPC A private route table has the peering route.
- [ ] Verify final inbound rules for all security groups. The included security-group captures show the Bastion group and two App-B rule-edit captures; they do not establish a separate final App-A security-group capture.
- [ ] Capture controlled ICMP rule removal and recovery evidence.
- [ ] Add cleanup evidence or state clearly which resources are intentionally retained.
- [ ] Confirm the repository contains no private keys or credentials.
