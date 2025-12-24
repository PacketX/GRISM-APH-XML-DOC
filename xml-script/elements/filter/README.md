# \<filter>

In a `<filter>` element, all matching conditions must be defined inside a single `<or>...</or>` block.  
Each condition is specified using a self-closing `<find />` tag placed within this `<or>` element.

The `id` attribute of the `<filter>` is referenced later in the `<chain>` using the corresponding `<fid>` value.  
For example, `id="1"` in `<filter id="1">` is referenced as `F1` in `<fid>F1</fid>`, as shown in the example below.

```xml
<filter id="1" >
  <or>
    <find name="tcp.port" relation="==" content="443" />
    <find name="tcp.port" relation="==" content="53" />
  </or>
</filter>
```

### \<or>

Defines all finds conjunct with or. It has a start tag \<or> and an end tag \</or>.

### \<find>

Please refer to [Element - \<find>](filter_find.md)

## Attribute

| Attribute | Description                         | Type    | Default (* must have)  |
|----------|--------------------------------------|---------|------------------------|
| id       | Specifies a unique id for an element | Integer | *                      |
| name     | Specifies a name for an element      | String  |                        |

## Example

Documents the filter element, explaining how it is used to include or exclude traffic based on specified IP addresses.

```xml
<run>
  <filter id="1">
    <or>
      <find name="ip.addr" relation="==" content="8.8.8.8" />
      <find name="ip.addr" relation="==" content="8.8.4.4" />
      <find name="ip.addr" relation="==" content="168.95.1.1" />
      <find name="ip.addr" relation="==" content="168.95.100.1" />
    </or>
  </filter>
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
