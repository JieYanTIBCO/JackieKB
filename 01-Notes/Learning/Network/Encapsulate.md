MAC Header:
  Destination MAC: FF:FF:FF:FF:FF:FF
  Source MAC: 00:1A:2B:3C:4D:5E
  EtherType: 0x0800 (IPv4)

IP Header:
  Version: 4
  Header Length: 20 bytes
  Total Length: 40 bytes
  TTL: 64
  Protocol: 6 (TCP)
  Source IP: 192.168.1.10
  Destination IP: 8.8.8.8
  Checksum: [Calculated Value]

TCP Header:
  Source Port: 5000
  Destination Port: 25
  Sequence Number: 12345
  Acknowledgment Number: 0
  Flags: SYN
  Checksum: [Calculated Value]

Data: "Hello, World!"

MAC Trailer:
  FCS: [Calculated CRC Value]