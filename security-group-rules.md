# Security group rules — verification required

The screenshots show two inbound rules (SSH and All ICMP - IPv4) in the App-A and App-B security group edit screens. However, the displayed source addresses appear to be host-specific `/32` values and may not correspond to the intended source for each test.

Before publishing a definitive rule table, inspect each EC2 instance's attached security group and verify:

- Bastion: SSH (TCP 22) should be restricted to your current trusted public IP `/32`.
- App-A: SSH (TCP 22) should be allowed only from the Bastion security group or Bastion private IP as required by the lab. ICMP should allow the intended peer test source.
- App-B: SSH (TCP 22) should be allowed only from the Bastion security group or Bastion private IP as required by the lab. ICMP should allow the intended peer test source.
- Avoid broad `0.0.0.0/0` inbound access for SSH or ICMP.

Do not treat the screenshots as proof of the final corrected configuration until you verify the live rules.
