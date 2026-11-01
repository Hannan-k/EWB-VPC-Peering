# Route table configuration

The screenshots show these named route tables:

- `ewb-rt-a-public` — VPC A
- `ewb-rt-a-private` — VPC A
- `ewb-rt-b-private` — VPC B

Required peering routes:

| Route table | Destination CIDR | Target |
|---|---|---|
| ewb-rt-a-public | `10.20.0.0/16` | `pcx-03049e1619553aed5` |
| ewb-rt-a-private | `10.20.0.0/16` | `pcx-03049e1619553aed5` |
| ewb-rt-b-private | `10.10.0.0/16` | `pcx-03049e1619553aed5` |

The captured VPC A private route-table edit screen showed a duplicate `10.20.0.0/16` entry and an error that the route already exists. The later route-table list shows the table but does not prove the final route state. Re-open `ewb-rt-a-private` in AWS and confirm exactly one active peering route before submission.

Additional evidence note: `vpc-a-private-route-edit.png` and `vpc-b-private-route-edit.png` show route-edit screens from the configuration process, including duplicate-route attempts. They are not proof by themselves that the final saved state is correct. Verify the final route entries in the AWS console.
