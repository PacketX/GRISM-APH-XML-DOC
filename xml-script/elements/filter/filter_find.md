---
description: Defines the find(f). It has a start tag <find> or <f>
---

# \<find>

## Attribute

| Attribute | Description                                      | Type     | Default (\* must have)  |
|----------|---------------------------------------------------|----------|-------------------------|
| name     | refer to wireshark filter function, but less item | String   | \*                      |
| relation | Equal                                             | ==       | \*                      |
| content  | content of name, could be empty                   | String   | \*                      |

### Attribute-name

| name              | type                      | Description                                         | Example           |
|-------------------|---------------------------|-----------------------------------------------------|-------------------|
| eth.addr          | String                    | Source or Destination MAC Address                   | 00:11:22:33:44:55 |
| eth.src           | String                    | Source MAC Address                                  | 00:11:22:33:44:55 |
| eth.dst           | String                    | Destination MAC Address                             | 00:11:22:33:44:55 |
| ip.addr           | String                    | Source or Destination Address                       | 8.8.8.8           |
| ip.src            | String                    | Source Address                                      | 8.8.8.8           |
| ip.dst            | String                    | Destination Address                                 | 8.8.8.8           |
| ip.proto          | Unsigned integer, 1 byte  | Protocol                                            | 6 (TCP)           |
| tcp.port          | Unsigned integer, 2 bytes | Source or Destination Port                          | 443               |
| tcp.srcport       | Unsigned integer, 2 bytes | Source Port                                         | 443               |
| tcp.dstport       | Unsigned integer, 2 bytes | Destination Port                                    | 443               |
| udp.port          | Unsigned integer, 2 bytes | Source or Destination Port                          | 53                |
| udp.srcport       | Unsigned integer, 2 bytes | Source Port                                         | 53                |
| udp.dstport       | Unsigned integer, 2 bytes | Destination Port                                    | 53                |
| gtp.cp            | Boolean                   | Matches GTP-C (control-plane) packets               |                   |
| gtp.data          | Boolean                   | Matches GTP-U (user-plane) packets                  |                   |
| inner.ip.addr     | IPv4 address              | Inner (tunneled) Source or Destination Address      | 8.8.8.8           |
| inner.ip.src      | IPv4 address              | Inner (tunneled) Source Address                     | 8.8.8.8           |
| inner.ip.dst      | IPv4 address              | Inner (tunneled) Destination Address                | 8.8.8.8           |
| inner.proto       | Unsigned integer, 1 byte  | Inner (tunneled) Protocol                           | 6 (TCP)           |
| inner.tcp.port    | Unsigned integer, 2 bytes | Inner (tunneled) TCP Source or Destination Port     | 443               |
| inner.tcp.srcport | Unsigned integer, 2 bytes | Inner (tunneled) TCP Source Port                    | 443               |
| inner.tcp.dstport | Unsigned integer, 2 bytes | Inner (tunneled) TCP Destination Port               | 443               |
| inner.udp.port    | Unsigned integer, 2 bytes | Inner (tunneled) UDP Source or Destination Port     | 53                |
| inner.udp.srcport | Unsigned integer, 2 bytes | Inner (tunneled) UDP Source Port                    | 53                |
| inner.udp.dstport | Unsigned integer, 2 bytes | Inner (tunneled) UDP Destination Port               | 53                |


## Example

```xml
<filter id="1">
  <or>
    <find id="1" name="ip.addr" relation="==" content="8.8.8.8" />
    <find id="2" name="ip.addr" relation="==" content="2.2.2.2" />
  </or>
</filter>
```

```xml
<filter id="1">
  <or>
    <f n="ip.addr" r="==" c="8.8.8.8" />
    <f n="ip.addr" r="==" c="2.2.2.2" />
  </or>
</filter>
```
