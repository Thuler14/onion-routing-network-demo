# Packet-Level Verification

The project can be inspected at two points in the simulated route:

```text
h1 → r1 → r2 → r3 → h2
```

The intermediate capture demonstrates that the application message remains encrypted after the first router. The destination capture demonstrates that plaintext appears only after the final onion layer is removed.

## Start the Inspection Environment

Start the topology with xterms:

```bash
sudo python3 net.py --xterms
```

## Intermediate Hop

From the `r1` terminal, capture traffic forwarded to `r2`:

```bash
tcpdump -n -A -s 0 -i r1-eth1 tcp port 9002
```

At this point the application payload remains inside an AES-GCM wrapper, so the original plaintext should not appear in the capture.

With Wireshark:

```bash
wireshark -k -i r1-eth1 &
```

Display filter:

```text
tcp.dstport == 9002 && tcp.len > 0
```

Preserved evidence:

- [`wireshark-encrypted-intermediate-hop.png`](../evidence/wireshark-encrypted-intermediate-hop.png)
- [`encrypted-intermediate-hop.png`](../evidence/encrypted-intermediate-hop.png)

## Destination Hop

From the `h2` terminal, capture traffic delivered by `r3`:

```bash
tcpdump -n -A -s 0 -i h2-eth0 tcp port 9009
```

After the final onion layer is removed, the plaintext application payload is visible on this link.

With Wireshark:

```bash
wireshark -k -i h2-eth0 &
```

Display filter:

```text
tcp.dstport == 9009 && tcp.len > 0
```

Preserved evidence:

- [`wireshark-decrypted-destination-hop.png`](../evidence/wireshark-decrypted-destination-hop.png)
- [`decrypted-destination-hop.png`](../evidence/decrypted-destination-hop.png)

## Additional Evidence

- [`onion-routing-demo.mp4`](../evidence/onion-routing-demo.mp4) — end-to-end demonstration
- [`demo-success.png`](../evidence/demo-success.png) — client execution
- [`demo-log.txt`](../evidence/demo-log.txt) — router and server execution log
- [`environment.txt`](../evidence/environment.txt) — verified test environment
