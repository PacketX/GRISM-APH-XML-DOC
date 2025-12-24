# L2 - L4 Composite Filtering

This case study shows how to combine multiple L2/L3/L4 conditions using filters and forward packets when **any** condition matches.

## Flow

![L2 - L4 Composite Filtering Flow](/.gitbook/assets/case/test/l2-l4-composite-filtering.svg)

## GRISM XML Script

The example below forwards packets to `S2.1` when any of the following matches:

- IPv4 address match: `8.8.8.8` or `8.8.4.4`
- IPv6 address match: `2001:b000:5e00:ff00::1`
- MAC address match: `aa:bb:cc:dd:ee:ff`
- VLAN ID match: `1234` or `1235`

```xml
<run>
	<filter id="1" >
		<or>
			<find name="ip.addr" relation="==" content="8.8.8.8" />
			<find name="ip.addr" relation="==" content="8.8.4.4" />
		</or>
	</filter>
	<filter id="2" >
		<or>
			<find name="ip.addr" relation="==" content="2001:b000:5e00:ff00::1" />
		</or>
	</filter>
	<filter id="3" >
		<or>
			<find name="eth.addr" relation="==" content="aa:bb:cc:dd:ee:ff" />
		</or>
	</filter>
	<filter id="4" >
		<or>
			<find name="vlan.id" relation="==" content="1234" />
			<find name="vlan.id" relation="==" content="1235" />
		</or>
	</filter>
	<chain>
		<in>S1.1</in>
		<fid type="or">F1,F2,F3,F4</fid>
		<out>S2.1</out>
	</chain>
</run>
```

## Behavior

- **Input**: Packets enter from port `S1.1`.
- **Match (OR)**: If any filter matches (F1 or F2 or F3 or F4), the packet is forwarded.
- **Result**: Matched packets are forwarded to `S2.1`.

## Operational Notes

- Filter `F1` and `F4` use multiple `<find>` entries under `<or>` to match one of several values.
- This example shows both IPv4 and IPv6 address matching using `ip.addr` (behavior depends on platform parsing).

## Validation Checklist

- [ ] Confirm `S1.1` is receiving traffic with expected IP/MAC/VLAN fields.
- [ ] Verify packets matching any rule are forwarded to `S2.1`.
- [ ] Confirm non-matching packets are not forwarded (or are handled by other configured chains).
