# b91jp
Base91 json-printable encoding.


Similar to [Z85 encoding](https://rfc.zeromq.org/spec:32/Z85/) (a derivative of [Ascii85](https://en.wikipedia.org/wiki/Ascii85)). 


An encoding format for representing binary data as printable ASCII characters _and_ safe to use as a JSON string value without escaping. The `b91jp` alphabet does not include `\` (backslashes) or `"` (quotes). 


Encoded size overhead:

| Format | Data size increase |
|---|---|
| b91jp | ~23% |
| base64 | ~33% |


This format allows determining the byte size of a `stringified` JSON wrapped payload prior to encoding, using only the input byte length. 


Example:
```ts
// Binary data buffer to encoded.
const data: Buffer = Buffer.from(...);

// Determine the byte length of the encoded output string.
// Note: this byte length assumes the result string will be encoded as utf8 or ascii.
const encodedByteLength = b91p.getByteLength(data.byteLength);

// Example of a JSON object structure that the encoded data payload is wrapped in. 
const payloadWrapper = {
  // placeholder for the b91jp encoded input
  "encodedData": "",
  // placeholder another entry with a deterministic byte size
  "signature": ""
}

// Get the byte length of the `stringified` JSON overhead.
// This example wrapper is 33 bytes, i.e. `'{"encodedData":"","signature":""}'.length == 33`
const wrapperByteLength = JSON.stringify(payloadWrapper).length;

// The example `signature` string is always 64 bytes in length.
const signatureByteLength = 64;

const payloadByteLength: number = payloadWrapper + encodedByteLength + signatureByteLength;

```

