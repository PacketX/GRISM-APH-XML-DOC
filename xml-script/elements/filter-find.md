---
description: Defines the find(f). It has a start tag <find> or <f>
---

# Element \<find>

## Attribute

<table><thead><tr><th width="150">Attribute</th><th width="150">Alternative</th><th width="215">Description</th><th width="156">Type</th><th>Default (* must have)</th></tr></thead><tbody><tr><td>id</td><td></td><td>Specifies a unique id for an element</td><td>Interger</td><td></td></tr><tr><td>name</td><td>n</td><td>refer to wireshark filter function, but less item</td><td>String</td><td>*</td></tr><tr><td>relation</td><td>r</td><td>Equal or Not equal</td><td><p>==/!=</p><p>>=/&#x3C;= (v3.9)</p></td><td>*</td></tr><tr><td>content</td><td>c</td><td>content of name, could be empty</td><td>String</td><td>*</td></tr></tbody></table>

### Attribute -name

<table><thead><tr><th width="151.32440213813035">name</th><th width="72" data-type="number">id</th><th width="177">type</th><th width="166.61725067385444">Description</th><th width="156">Example</th><th>Support</th></tr></thead><tbody>
<tr><td>ip</td><td>7</td><td></td><td>is IPv4</td><td>ip ==</td><td></td></tr>
<tr><td>ip.addr</td><td>8</td><td>IPv4 address</td><td>Source or Destination Address</td><td>ip.addr == 8.8.8.8</td><td></td></tr>
<tr><td>ip.src</td><td>9</td><td>IPv4 address</td><td>Source Address</td><td>ip.src == 8.8.8.8</td><td></td></tr>
<tr><td>ip.dst</td><td>10</td><td>IPv4 address</td><td>Destination Address</td><td>ip.dst == 8.8.8.8</td><td></td></tr>
<tr><td>ip.proto</td><td>11</td><td>Unsigned integer, 1 byte</td><td>Protocol</td><td>ip.proto == 6 (TCP)</td><td></td></tr>
<tr><td>ip.fragment</td><td>12</td><td></td><td>is IPv4 Fragment</td><td>ip.fragment ==</td><td></td></tr>
<tr><td>ip.flags.df</td><td>90</td><td>Unsigned integer, 1 byte</td><td>is IP don't fragment</td><td>ip.flags.df == 1</td><td>v3.9</td></tr>
<tr><td>ip.flags.mf</td><td>91</td><td>Unsigned integer, 1 byte</td><td>is IP more fragment</td><td>ip.flags.mf == 1</td><td>v3.9</td></tr>
<tr><td>ip.dsfield</td><td>13</td><td>Unsigned integer, 1 byte</td><td>Differentiated Services Field</td><td>ip.dsfield == 1</td><td></td></tr>
<tr><td>ipv6</td><td>14</td><td></td><td>is IPv6</td><td>ipv6 ==</td><td></td></tr>
<tr><td>ipv6.addr</td><td>15</td><td>IPv6 address</td><td>Source or Destination Address</td><td>ipv6.addr == 2001:0db8:86a3:08d3:1319:8a2e:0370:7344</td><td></td></tr>
<tr><td>ipv6.src</td><td>16</td><td>IPv6 address</td><td>Source Address</td><td>ipv6.src == 2001:0db8:86a3:08d3:1319:8a2e:0370:7344</td><td></td></tr>
<tr><td>ipv6.dst</td><td>17</td><td>IPv6 address</td><td>Destination Address</td><td>ipv6.dst == 2001:0db8:86a3:08d3:1319:8a2e:0370:7344</td><td></td></tr>
<tr><td>ipv6.nxt</td><td>18</td><td>Unsigned integer, 1 byte</td><td>Next Header</td><td></td><td></td></tr>
<tr><td>tcp.port</td><td>20</td><td>Unsigned integer, 2 bytes</td><td>Source or Destination Port</td><td>tcp.port == 443</td><td></td></tr>
<tr><td>tcp.srcport</td><td>21</td><td>Unsigned integer, 2 bytes</td><td>Source Port</td><td>tcp.srcport == 443</td><td></td></tr>
<tr><td>tcp.dstport</td><td>22</td><td>Unsigned integer, 2 bytes</td><td>Destination Port</td><td>tcp.dstport == 443</td><td></td></tr>
<tr><td>udp.port</td><td>28</td><td>Unsigned integer, 2 bytes</td><td>Source or Destination Port</td><td>udp.port == 53</td><td></td></tr>
<tr><td>udp.srcport</td><td>29</td><td>Unsigned integer, 2 bytes</td><td>Source Port</td><td>udp.srcport == 53</td><td></td></tr>
<tr><td>udp.dstport</td><td>30</td><td>Unsigned integer, 2 bytes</td><td>Destination Port</td><td>udp.dstport == 53</td><td></td></tr>
<tr><td>sctp.port</td><td>32</td><td>Unsigned integer, 2 bytes</td><td>Source or Destination Port</td><td>sctp.port == 2906</td><td></td></tr>
<tr><td>sctp.srcport</td><td>33</td><td>Unsigned integer, 2 bytes</td><td>Source Port</td><td>sctp.srcport == 2906</td><td></td></tr>
<tr><td>sctp.dstport</td><td>34</td><td>Unsigned integer, 2 bytes</td><td>Destination Port</td><td>sctp.dstport == 2906</td><td></td></tr>
<tr><td>5-tuple</td><td>35</td><td>5 Tuple, - means don't care</td><td>Source IP Address, Destination IP Address, Protocol, Source Port, Destination Port</td><td>5-tuple == - 192.168.1.203 - - 443</td><td></td></tr>
</tbody></table>

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
