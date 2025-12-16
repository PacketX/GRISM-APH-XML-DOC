# Element \<filter>

In filter, must start at \<or>\</or> or \<and>\</and>, then put \<find/> into there like

And filter id=1 -> F1, refer to Example

```xml
<filter id="1" >
    <or>
	<find name="tcp.port" relation="==" content="443" />
	<find name="udp.port" relation="==" content="53" />
    </or>
</filter>
```

### \<or>

Defines all finds conjunct with or. It has a start tag \<or> and an end tag \</or>.

### \<and>

Defines all finds conjunct with and. It has a start tag \<and> and an end tag \</and>.

### \<find>

Please refer to [Element - \<find](filter-find.md)>

## Attribute

<table><thead><tr><th width="188">Attribute</th><th width="387">Description</th><th width="150">Type</th><th width="154">Default (* must have)</th></tr></thead><tbody><tr><td>id</td><td>Specifies a unique id for an element</td><td>Interger</td><td>*</td></tr><tr><td>name</td><td>Specifies a name for an element</td><td>String</td><td></td></tr></tbody></table>

## Example

filter dns port and server

```xml
<run>
<filter id="1" sessionBase="no">
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
