---
description: Example case for aggregating specific traffic from multiple sources into a single destination port.
---

# Aggregating Traffic Case

## Overview

This case shows how to forward traffic from multiple source ports into a single destination port. Use it when you need to consolidate mirrored traffic for recording, analysis, or inspection—e.g., sending packets from various edges into one analyzer input.

## Topology

- **Source Ports**: `P0`, `P1` (mirrored feeds from distinct edges)
- **Aggregation Port**: `P2` (connected to the packet analyzer)

## XML Walkthrough

1. Build one `<chain>` per source port and point its `<out>` to the aggregation port.
2. To add more sources, duplicate a chain and change only the `<in>` port.
3. If filtering is needed, insert `<fid>` + `<filter>` blocks as described in the filter documentation, but it is omitted in this base case.

```xml
<run>
	<!-- Aggregate P0 into P2 -->
	<chain>
		<in>P0</in>
		<out>P2</out>
	</chain>

	<!-- Aggregate P1 into P2 -->
	<chain>
		<in>P1</in>
		<out>P2</out>
	</chain>
</run>
```

## Operational Notes

- Each chain can have only one `<in>`, so create multiple chains for multi-source aggregation.
- Add `type="loadBalance"` on `<out>` if you must distribute load or forward to additional analyzers; omit it to keep the default duplication behavior.
- If you need to restrict traffic, insert a `<fid>` element in each chain and reference a dedicated `<filter>` definition.
- Ensure the destination port `P2` connects to a high-capacity recorder or IDS to avoid introducing a bottleneck.

## Validation Checklist

- [ ] Verify the mirrored sources are configured properly and are delivering expected packets.
- [ ] Reference only existing chain `id`s inside the `run` block.
- [ ] Use external monitoring (e.g., analyzer counters or span receivers) to confirm the aggregated packets reach the destination.

Use this template for other aggregation scenarios by adjusting the number of chains and, if needed, inserting filters specific to the traffic you want to keep.

