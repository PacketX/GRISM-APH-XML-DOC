# GTP-U Inner Filtering

This case study shows how to filter traffic using **inner (tunneled)** inside GTP-U, and forward matched packets to an output port.

## Flow

![GTP-U Inner Filtering Flow](/.gitbook/assets/case/test/gtpu-inner-filtering.svg)

## GRISM XML Script

The example below matches DNS traffic to/from `8.8.8.8` by checking **inner IP** and **inner UDP port** fields.

- **Forward direction** (match): `inner.ip.dst == 8.8.8.8` AND `inner.udp.dstport == 53`
- **Reverse direction** (notmatch -> match): `inner.ip.src == 8.8.8.8` AND `inner.udp.srcport == 53`

```xml
<run>
  <filter id="1" >
    <or>
      <find name="inner.ip.dst" relation="==" content="8.8.8.8" />
    </or>
  </filter>
  <filter id="2" >
    <or>
      <find name="inner.udp.dstport" relation="==" content="53" />
    </or>
  </filter>
  <filter id="3" >
    <or>
      <find name="inner.ip.src" relation="==" content="8.8.8.8" />
    </or>
  </filter>
  <filter id="4" >
    <or>
      <find name="inner.udp.srcport" relation="==" content="53" />
    </or>
  </filter>
  <chain>
    <in>S1.1</in>
    <fid type="and">F1,F2</fid>
    <out>S2.1</out>
    <next type="notmatch">
      <fid type="and">F3,F4</fid>
      <out>S2.1</out>
    </next>
  </chain>
</run>
```

## Behavior

- **Input**: Packets enter from port `S1.1`.
- **Match (F1 & F2)**: If inner destination is `8.8.8.8` and inner UDP destination port is `53`, forward to `S2.1`.
- **Not match path**: If not matched, apply **reverse direction** condition (F3 & F4).
- **Result**: DNS traffic to/from `8.8.8.8` is forwarded to `S2.1` based on inner.

## Operational Notes

- This case assumes packets contain an inner IP/UDP (for example, traffic carried inside GTP-U).
- If you need to ensure only GTP-U packets are evaluated, combine this with a GTP-U filter (for example, `gtp.data`) before applying inner conditions.

## Validation Checklist

- [ ] Confirm `S1.1` is receiving GTP-U traffic with inner IP/UDP.
- [ ] Verify matched packets arrive on `S2.1` and show inner DNS (UDP/53) in capture.
- [ ] Validate both directions (to 8.8.8.8 and from 8.8.8.8) are forwarded as intended.
