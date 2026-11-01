# Connectivity test commands

Run the tests from the indicated EC2 host.

## From App-A (10.10.2.10) to App-B (10.20.1.10)

```bash
ping -c 4 10.20.1.10
```

## From App-B (10.20.1.10) to App-A (10.10.2.10)

```bash
ping -c 4 10.10.2.10
```

A successful test should report 4 packets transmitted, 4 received, and 0% packet loss. The supplied screenshots include successful results in both directions and an earlier failed ping. Capture the corresponding security-group rule change if demonstrating controlled ICMP failure/recovery.
