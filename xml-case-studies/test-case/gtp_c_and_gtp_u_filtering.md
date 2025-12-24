# GTP-C and GTP-U Filtering

This case study shows how to separate GTP control-plane and user-plane traffic using UDP ports, then forward packets to different destinations.

## Flow

![GTP-C and GTP-U Filtering Flow](/.gitbook/assets/case/test/gtp-c-and-gtp-u-filtering.svg)

## GRISM XML Script

This example uses UDP port matching:

- **GTP-C**: UDP port `2123`
- **GTP-U**: UDP port `2152`

```xml
<run>
	<filter id="1" >
		<or>
			<find name="udp.port" relation="==" content="2123" />
		</or>
	</filter>
	<filter id="2" >
		<or>
			<find name="udp.port" relation="==" content="2152" />
		</or>
	</filter>
	<chain>
		<in>S1.1</in>
		<fid>F1</fid>
		<out>S3.1</out>
		<next type="notmatch">
			<fid>F2</fid>
			<out type="loadBalance" lbtype="5thash">S2.1,S2.2,S2.3,S2.4</out>
		</next>
	</chain>
</run>
```

## Behavior

- **Input**: Packets enter from port `S1.1`.
- **Match (F1 / UDP 2123)**: Packets are forwarded to `S3.1`.
- **Not match**: If not matched, evaluate `F2` (UDP 2152).
- **Match in `<next>` (F2 / UDP 2152)**: Packets are load-balanced to one of `S2.1,S2.2,S2.3,S2.4` using `5thash`.

## Operational Notes

- `udp.port` matches either source or destination UDP port.
- This example separates traffic by UDP ports (2123/2152). If your deployment requires protocol-aware parsing, consider using GTP-specific fields when available.

## Validation Checklist

- [ ] Confirm `S1.1` is receiving both UDP/2123 and UDP/2152 traffic.
- [ ] Verify UDP/2123 packets arrive on `S3.1`.
- [ ] Verify UDP/2152 packets are distributed across `S2.1` to `S2.4` and remain flow-consistent.
