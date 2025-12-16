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

<table><thead><tr><th width="150">Attribute</th><th width="249.7142857142857">Description</th><th width="150">Type</th><th>Default (* must have)</th></tr></thead><tbody><tr><td>type</td><td>duplicate or loadBalance</td><td>String</td><td>duplicate</td></tr><tr><td>lbtype</td><td>Load Balance type, includes session, 5thash</td><td>String</td><td>session</td></tr></tbody></table>

#### Example

```xml
<!--duplication to P1 and P2 -->
<out>P0,P1</out>
<!-- load Balance to P0,P1 by 5-tuple -->
<out type="loadBalance" lbtype="5thash">P0,P1</out>
```

### \<fid>

Defines packets pass through filter id. It has a start tag \<fid> and an end tag \</fid>.

#### Attribute

<table><thead><tr><th width="150">Attribute</th><th width="150">Description</th><th>Type</th><th>Default (* must have)</th></tr></thead><tbody><tr><td>type</td><td>and/or</td><td>String</td><td>or</td></tr></tbody></table>

#### Example

```xml
  <fid>F1</fid>
<!--if F1 or F2 -->
  <fid type="or">F1,F2</fid>

<!--if F1 and F2 -->
  <fid type="and">F1,F2</fid>
```

### \<next>

Defines going next if packet match/not match filter. It has a next tag \<next> and an end tag \</next>.

#### Attribute

<table><thead><tr><th width="150">Attribute</th><th>Description</th><th>Type</th><th>Default (* must have)</th></tr></thead><tbody><tr><td>type</td><td>match/notmatch</td><td>String</td><td>match</td></tr></tbody></table>

#### Example

```xml
<!-- packet from P0, if matched F1 send to P1 else send to P2 -->
<chain>
  <in>P0</in> 
  <fid>F1</fid>
  <out>P1</out>
  <next type="notmatch">
    <out>P2</out>
  </next>
</chain>
```
