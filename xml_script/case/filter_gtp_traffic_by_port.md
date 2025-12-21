---
description: Example cases for filtering GTP traffic by UDP port (GTP-C and GTP-U) and forwarding matched packets to a destination port.
---

# GTP Filtering Case (by UDP Port)

## Overview

This case shows how to filter GTP traffic by UDP port and forward only the matched packets to a specific destination port. It includes two examples:

- **GTP-C (control-plane)** filtering using `udp.port == 2123`
- **GTP-U (user-plane)** filtering using `udp.port == 2152`

## Topology

- **Source Port**: `P0` (incoming traffic)
- **Matched Output Port**: `P1` (connected to analyzer / recorder)
- **Unmatched Output Port**: `P2` (optional, for drop or other handling)

## XML Walkthrough

1. Define a `<filter>` under `<run>` using `<or>`.
2. Use `<find>` with `name="udp.port"` and the target port number.
3. In the `<chain>`, reference the filter using `<fid>F#</fid>`.
4. Use `<next type="notmatch">` to define where unmatched packets go.

### Example 1: Filter GTP-C by UDP port (2123)

```xml
<run>
	<!-- Filter: UDP 2123 (commonly GTP-C) -->
	<filter id="1">
		<or>
			<find name="udp.port" relation="==" content="2123" />
		</or>
	</filter>

	<!-- Chain: matched -> P1, not matched -> P2 -->
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

### Example 2: Filter GTP-U by UDP port (2152)

```xml
<run>
	<!-- Filter: UDP 2152 (commonly GTP-U) -->
	<filter id="2">
		<or>
			<find name="udp.port" relation="==" content="2152" />
		</or>
	</filter>

	<!-- Chain: matched -> P1, not matched -> P2 -->
	<chain>
		<in>P0</in>
		<fid>F2</fid>
		<out>P1</out>
		<next type="notmatch">
			<out>P2</out>
		</next>
	</chain>
</run>
```

## Operational Notes

- These UDP ports are common defaults for GTP, but may differ depending on your network; adjust `content` accordingly.
- If you don’t need to handle unmatched packets, you can omit the `<next type="notmatch">...</next>` block.
- If you need higher accuracy, prefer protocol-aware filters like `gtp.cp` / `gtp.data` when available.

## Validation Checklist

- [ ] Confirm `P0` is receiving expected traffic.
- [ ] Verify the `<fid>` value matches the `<filter id>` (e.g., `id="2"` -> `F2`).
- [ ] Check that matched packets arrive on `P1` (capture confirms UDP 2123 / 2152 traffic).
- [ ] If configured, confirm unmatched packets are handled as intended on `P2`.

