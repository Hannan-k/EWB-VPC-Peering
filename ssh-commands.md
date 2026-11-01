# SSH commands

## Local machine → Bastion

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
chmod 400 ~/.ssh/ewb-lab.pem
ssh -i ~/.ssh/ewb-lab.pem ec2-user@32.199.138.134
```

## Local machine → Bastion with agent forwarding

```bash
ssh -A -i ~/.ssh/ewb-lab.pem ec2-user@32.199.138.134
```

## Bastion → private instances

```bash
ssh ec2-user@10.10.2.10
ssh ec2-user@10.20.1.10
```

Agent forwarding requires the private key to be loaded into the local SSH agent and the corresponding public key to be authorized on the target instances. Never copy the private key to the Bastion or commit it to GitHub.
