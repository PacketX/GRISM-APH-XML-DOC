---
description: Example cases for filtering GTP traffic (GTP-C and GTP-U) and forwarding matched packets to a destination port.
---

# GTP Filtering Case

## Overview

This case shows how to filter GTP traffic and forward only the matched packets to a specific destination port. It includes two examples:

- **GTP-C (control-plane)** filtering using `gtp.cp`
- **GTP-U (user-plane)** filtering using `gtp.data`

## Topology

- **Source Port**: `P0` (incoming traffic)
- **Matched Output Port**: `P1` (connected to analyzer / recorder)
- **Unmatched Output Port**: `P2` (optional, for drop or other handling)

## XML Walkthrough

1. Define a `<filter>` under `<run>` using `<or>`.
2. Use `<find>` with `name="gtp.cp"` (GTP-C) or `name="gtp.data"` (GTP-U).
3. In the `<chain>`, reference the filter using `<fid>F#</fid>`.
4. Use `<next type="notmatch">` to define where unmatched packets go.

### Example 1: Filter GTP-C (GTP control-plane)

```xml
<run>
	<!-- Filter: GTP-C only -->
	<filter id="1">
		<or>
			<find name="gtp.cp" relation="==" content="" />
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

### Example 2: Filter GTP-U (GTP user-plane)

```xml
<run>
	<!-- Filter: GTP-U only -->
	<filter id="2">
		<or>
			<find name="gtp.data" relation="==" content="" />
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

- `gtp.cp` and `gtp.data` are treated as boolean-style filters here, so `content` is left empty (`content=""`).
- If you don’t need to handle unmatched packets, you can omit the `<next type="notmatch">...</next>` block.
- If you need to combine GTP filtering with inner (tunneled) header fields, add more `<find>` items under the same `<or>` block.

## Validation Checklist

- [ ] Confirm `P0` is receiving expected traffic.
- [ ] Verify the `<fid>` value matches the `<filter id>` (e.g., `id="2"` -> `F2`).
- [ ] Check that matched packets arrive on `P1` (analyzer counters / capture confirms GTP-C or GTP-U).
- [ ] If configured, confirm unmatched packets are handled as intended on `P2`.

