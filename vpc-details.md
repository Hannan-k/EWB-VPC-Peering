# Observed VPC details

| Field | VPC A | VPC B |
|---|---|---|
| Name | ewb-vpc-a | ewb-vpc-b |
| VPC ID | vpc-0e5116b5f007c9638 | vpc-0bdd28402ddc17b0a |
| IPv4 CIDR | 10.10.0.0/16 | 10.20.0.0/16 |
| Region | us-east-1 | us-east-1 |

Peering connection: `pcx-03049e1619553aed5` (shown Active in console capture).

## EC2 private/public addresses observed

- Bastion public IPv4: `32.199.138.134`
- App-A private IPv4: `10.10.2.10`
- App-B private IPv4: `10.20.1.10`

Confirm all IDs and addresses against the live AWS console before publishing. The account/owner identifier is intentionally omitted.
