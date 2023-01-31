# PCAP-based Network Interface

For development and testing, we can use pcap files as network interfaces.  It
avoids the need to have physical interfaces and also allows us to generate
traffic for testing.

## Generate pcap file

[pcap.rs](https://github.com/utah-scs/monster/blob/main/common/src/pcap.rs) in
common provides a function to generate pcap file from a list of packets in
/tmp/out.pcap.

```bash
cd common
cargo test
```

## Server: Use pcap files as interface

```bash
cd server
cargo run --features debug
```

For now the configuration assumes that the RX interface is using
/tmp/input.pcap and the TX interface is using /tmp/out.pcap.

So you can generate the pcap file using the above command and use it as input to
the server. Copy the same file to /tmp/out.pcap and use it as output or create a
new pcap file with a pcap header.

## Analyze pcap file using tcpdump
```bash
sudo tcpdump -nvvvX -r /tmp/input.pcap
```