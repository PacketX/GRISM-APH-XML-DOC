# Real Task 1: Split GTP-C and GTP-U Outputs

This case study shows how to split **GTP-C** and **GTP-U** traffic into different output paths, while also copying both traffic types to `S3.1`.

## Flow

![Real Task 1 Flow](/.gitbook/assets/case/test/real-task-1.svg)

## GRISM XML Script

- **GTP-C path**: match `gtp.cp` and forward to `S3.1` and `S3.2`.
- **GTP-U path**: if not matched, match `gtp.data` and load-balance to `S2.1,S2.2` with `5thash`.
- **Copy to `S3.1`**: `S3.1` receives both GTP-C and GTP-U (GTP-C via `<out>S3.1,S3.2</out>`, and GTP-U via `output="1"` -> `<output id="1"><port>S3.1</port></output>`).

```xml
<run>
	<filter id="1" >
		<or>
			<find name="gtp.cp" relation="==" content="" />
		</or>
	</filter>
	<filter id="2" >
		<or>
			<find name="gtp.data" relation="==" content="" />
		</or>
	</filter>
	<output id="1" >
		<port>S3.1</port>
	</output>
	<chain>
		<in>S1.1</in>
		<fid>F1</fid>
		<out>S3.1,S3.2</out>
		<next type="notmatch">
			<fid>F2</fid>
			<out type="loadBalance" lbtype="5thash" output="1">S2.1,S2.2</out>
		</next>
	</chain>
</run>
```

## Behavior

- **Input**: Packets enter from port `S1.1`.
- **Match (F1 / `gtp.cp`)**: Forward packets to `S3.1` and `S3.2`.
- **Not match**: If not matched, evaluate `F2` in `<next type="notmatch">`.
- **Match in `<next>` (F2 / `gtp.data`)**: Load-balance packets to `S2.1,S2.2` using `5thash`, and copy the packets to `S3.1`.

## Operational Notes

- `gtp.cp` and `gtp.data` are used as boolean-style matches in this example (`content=""`).
- The `output="1"` attribute references `<output id="1">` and is used here to copy the GTP-U path to port `S3.1`.

## Validation Checklist

- [ ] Confirm `S1.1` is receiving both GTP-C and GTP-U traffic.
- [ ] Verify GTP-C packets are forwarded to `S3.1` and `S3.2`.
- [ ] Verify GTP-U packets are load-balanced across `S2.1` and `S2.2` and remain flow-consistent.
