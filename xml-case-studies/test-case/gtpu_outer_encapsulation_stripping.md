---
description: Example case for stripping the outer encapsulation of GTP-U packets and forwarding the decapsulated payload.
---

# GTP-U Outer Encapsulation Stripping

## Overview

This case shows how to strip the outer encapsulation of **GTP-U (user-plane)** packets when forwarding traffic to an output port.

## Topology

- **Source Port**: `S1.1` (incoming traffic)
- **Output Port**: `S2.1` (decapsulated traffic)

## Flow

![GTP-U Outer Encapsulation Stripping Flow](/.gitbook/assets/case/test/gtpu-outer-encapsulation-stripping.svg)

## GRISM XML Script

Set `gtpType="stripping"` on the `<out>` element in the `<chain>`.

```xml
<run>
    <chain>
        <in>S1.1</in>
        <out gtpType="stripping">S2.1</out>
    </chain>
</run>
```

## Behavior

- **Input**: Packets enter from port `S1.1`.
- **GTP-U stripping**: For GTP-U packets, the device strips the outer encapsulation.
- **Forwarding**: The resulting (decapsulated) packet is forwarded to `S2.1`.

## Operational Notes

- This case is intended for **GTP-U** traffic; non-GTP-U behavior depends on platform handling.
- Combine this with filtering (for example, `gtp.data`) if you want to ensure only GTP-U packets are processed.

## Validation Checklist

- [ ] Confirm `S1.1` is receiving GTP-U traffic.
- [ ] Verify `S2.1` receives decapsulated packets (inner headers visible in capture).
