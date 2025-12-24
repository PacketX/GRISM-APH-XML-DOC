---
description: Defines the chain. It has a start tag <chain> and an end tag </chain>.
---

# Element \<chain>

## Elements in chain

### \<in>

Defines input ports. It has a start tag \<in> and an end tag \</in>.

#### Note

Only one input port is allowed in a chain.

#### Example

```xml
<in>P0</in>
```

### \<out>

Defines output ports. It has a start tag \<out> and an end tag \</out>.


#### Attribute

| Attribute | Description | Value | Type |
|-----------|-------------|-------|------|
| type      | Output mode: `duplicate` (send a copy to every port listed in `<out>`) or `loadBalance` (select one port from the `<out>` port list per packet/flow). | loadBalance | String |
| lbtype    | Effective only when `type="loadBalance"`. Session/flow hashing method used for load balance. Supported value: `5thash` (computes hash by 5-tuple: src/dst IP, src/dst port, L4 protocol) so packets in the same session/flow are consistently forwarded to the same output port. | 5thash | String |
| vlantype  | VLAN handling. Supported values: `stripping` (remove VLAN tag) or `tagging` (add VLAN tag). | tagging / stripping | String |
| vlanid    | VLAN ID used when `vlantype="tagging"` (typical range: 1~4094). | 1 | Integer |
| gtptype   | GTP handling. Supported value: `stripping` (remove GTP header). | stripping | String |

#### Example

```xml
<!--duplication to P1 and P2 -->
<out>P0,P1</out>
<!-- load Balance to P0,P1 by 5-tuple -->
<out type="loadBalance" lbtype="5thash">P0,P1</out>
<!-- VLAN tagging with VLAN ID 100 -->
<out vlantype="tagging" vlanid="100">P0</out>
<!-- GTP stripping -->
<out gtptype="stripping">P0</out>
```

### \<fid>

Defines packets pass through filter id. It has a start tag \<fid> and an end tag \</fid>.

#### Attribute

| Attribute | Description                                       | Type   | Default (* must have) |
|-----------|---------------------------------------------------|--------|------------------------|
| type      | and/or                                            | String | or                     |

#### Example

```xml
<fid>F1</fid>
<!--if F1 or F2 -->
<fid type="or">F1,F2</fid>

<!--if F1 and F2 -->
<fid type="and">F1,F2</fid>
```

### \<next>

Defines going next if packet notmatch filter. It has a next tag \<next> and an end tag \</next>.

#### Attribute

| Attribute | Description                                       | Type   | Default (* must have)  |
|-----------|---------------------------------------------------|--------|------------------------|
| type      | notmatch                                    | String | match                  |

#### Example

```xml
<run>
  <!-- packet from P0, if matched F1 send to P1 else send to P2 -->
  <chain>
    <in>P0</in> 
    <fid>F1</fid>
    <out>P1</out>
    <next type="notmatch">
      <out>P2</out>
    </next>
  </chain>
</run>
```
