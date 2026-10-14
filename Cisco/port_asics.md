# ASIC distribution on the WS-C3850-12X48U

Cisco switches use custom Application Specific Integrated Circuits (ASICs) for the </br> switch ports. Depending on the switch, there can be one or </br> multiple ASICs. On a Catalyst 6800 I had a customer with a 16-port 10G module. They only had 6 IDFs and they patched them into ports 1-6. There were two ASICs, ports 1-8 and 9-16, and the ASIC for ports 1-8 failed. I was able to move all uplinks to 9-14. Since then I always look at the ASIC boundaries and distribute critical services across the ASICs.

----------------------------------------------------------------

The full mapping on the 12X48U:

| ASIC | Ports |
|---|---|
| 0 | Te1/0/43-44 |
| 1 | Gi1/0/33-36, Te1/0/41-42, Te1/0/45-48 |
| 2 | Gi1/0/17-32 |
| 3 | Gi1/0/1-16, Te1/0/37-40 |

The mGig ports Te1/0/37-48 are spread across three ASICs:

```text
show platform software fed switch 1 ifm mappings
Interface                 IF_ID    Inst Asic Core Port SubPort Mac  Cntx LPN  GPN  Type Active
TenGigabitEthernet1/0/37  0x2b       3   3   0    1      0      1    0    37   37   NIF  Y
TenGigabitEthernet1/0/38  0x2c       3   3   0    0      0      0    0    38   38   NIF  Y
TenGigabitEthernet1/0/39  0x2d       3   3   0    3      0      3    0    39   39   NIF  Y
TenGigabitEthernet1/0/40  0x2e       3   3   0    2      0      2    0    40   40   NIF  Y
TenGigabitEthernet1/0/41  0x2f       1   1   0    4      0      4    0    41   41   NIF  Y
TenGigabitEthernet1/0/42  0x30       1   1   0    5      0      5    0    42   42   NIF  Y
TenGigabitEthernet1/0/43  0x31       0   0   0    7      0      7    0    43   43   NIF  Y
TenGigabitEthernet1/0/44  0x32       0   0   0    6      0      6    0    44   44   NIF  Y
TenGigabitEthernet1/0/45  0x33       1   1   0    1      0      1    0    45   45   NIF  Y
TenGigabitEthernet1/0/46  0x34       1   1   0    0      0      0    0    46   46   NIF  Y
TenGigabitEthernet1/0/47  0x35       1   1   0    3      0      3    0    47   47   NIF  Y
TenGigabitEthernet1/0/48  0x36       1   1   0    2      0      2    0    48   48   NIF  Y
```

I have 8 APs with 2.5G ports, so I will patch them as follows:

- ASIC 3: Te1/0/37, 38, 39
- ASIC 1: Te1/0/41, 42, 45
- ASIC 0: Te1/0/43, 44

Te1/0/47 (SonicWall) and Te1/0/48 (trunk uplink to the voice switch) are both on ASIC 1.
