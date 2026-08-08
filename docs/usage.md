# Usage

## Requirements

- Linux environment with `sudo` access
- Python 3.10+
- Mininet
- `python3-cryptography`
- Optional: `xterm`, Wireshark, or `tcpdump` for traffic inspection

The preserved test environment used:

- Linux Mint 22.2
- Python 3.12.3
- Mininet 2.3.0-1.1
- cryptography 41.0.7

See [`../evidence/environment.txt`](../evidence/environment.txt) for the recorded environment information.

## Setup

Clone the repository:

```bash
git clone https://github.com/Thuler14/onion-routing-network-demo.git
cd onion-routing-network-demo
```

Install the required packages:

```bash
sudo apt update
sudo apt install mininet python3-cryptography
```

Optional, for the xterm-based inspection workflow:

```bash
sudo apt install xterm
```

Start the topology:

```bash
sudo python3 net.py
```

Or start it with xterms for the routers, client, and server:

```bash
sudo python3 net.py --xterms
```

The script configures the topology, generates routing data and per-router keys, launches the router processes and destination server, and opens the Mininet CLI.

Generated keys, routing data, logs, and onion payloads are stored under `runtime/`, which is excluded from version control.

## Sending a Message

From the Mininet CLI:

```bash
h1 python3 client.py --message "HELLO_WORLD"
```

A successful transmission prints the elapsed time and server response:

```text
[CLIENT] elapsed ...
[CLIENT] Server reply: OK
```

To send the default payload:

```bash
h1 python3 client.py
```

The default message is:

```text
HELLO_FROM_CLIENT_VIA_ONION
```

## Build and Send Separately

Build an onion payload:

```bash
h1 python3 onion.py --message "HELLO_WORLD"
```

Then reuse the generated payload:

```bash
h1 python3 client.py --reuse --onion-file runtime/onion.out
```
