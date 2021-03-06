## Compression

Compression algorithms used with STTP can be either stateful, i.e., the compressor maintains its state through all compressed records, or stateless, i.e., the compressor compresses each data packet independently.

Stateful compression algorithms provide the best possible compression for STTP data transmission but require use of a reliable transport protocol, e.g., TCP. Stateless compression methods are required when a lossy transport protocol is being used for data transmission, e.g., UDP.

### Compression Algorithms

STTP is designed so that new compression algorithms can be implemented and used without requiring revisions to the specification. To accommodate this, compression algorithms are negotiated, by text name and numeric version, after a connection is established along with other operational modes for the session. See [session negotiation](Commands.md#negotiate-session-command) for more details.

Compression algorithms must be declared with a `DataPointEncoding` type, defined as follows, of either `VariableSize` or `FixedSize` which is chosen to best accommodate the compression.

```C
enum {
  VariableSize = 0, // Encode data points with `DataPoint` structure
  FixedSize = 1     // Encode data points with `PaddedDataPoint` structure
}
DataPointEncoding;
```

The `VariableSize` encoding will serialize data using the [`DataPoint`](DataPointStructure.md) structure and the `FixedSize` encoding will serialize data using the [`PaddedDataPoint`](PaddedDataPointStructure.md)  structure.

The compression algorithm encodings need to be known by and configured the publisher and subscriber; these encodings are used during _compression_ and _decompression_ stages to the proper data point encoding can be used. The `AlgorithmEncoding` structure, defined as follows, could be used to track these mappings:

```C
struct {
  CompressionAlgorithm algorithm;
  DataPointEncoding encoding;
}
AlgorithmEncoding;

struct {
  uint16 count;
  AlgorithmEncoding[] items;
}
AlgorithmEncodings;
```

The session negotiation process specifies both the stateful compression algorithm to use as well as the stateless compression algorithm, when applicable.  

The following compression algorithms should always be available for STTP implementations such that a minimal set of compression algorithms are always be available for a publisher/subscription connection session negotiation.

> :wrench: TLS includes options to allow for payload level compression algorithms. When STTP implementations are configured with TLS security and an STTP defined compression option, the implementations should not allow TLS compression options to also be enabled.

Any compression algorithms used by STTP may not increase the content length by more than `1,024` bytes. If the decompression function is used with a compressed data packet that would decompress to a length greater than `16,384` bytes, it must report a decompression failure error.

> :information_source: The `2^14` upper limit for decompressed buffers is the maximum payload size allowed by STTP.

#### No Compression

It is always be possible for STTP to send data packets in their native binary format without any specified compression.

For the purposes of session negotiation the text name for selecting no compression is `NONE` and its version number is `0.0`.

#### Deflate Compression

Deflate is an industry-standard algorithm that is used for lossless compression and decompression. The Deflate compression algorithm is defined in RFC 1951 <sup>[[19](References.md#user-content-ref19)]</sup>. This algorithm was chosen as a safe default option for an always available compression option in STTP since implementations of the protocol are widely available on various operating systems and programming languages.

The Deflate algorithm is technically a stateful algorithm, however, it is defined for use within STTP in either stateful or stateless modes. In a stateless mode, the Deflate algorithm is simply applied to each data packet with its default initial state. The Deflate algorithm is the only default compression option that is specified for STTP stateless compression.

For the purposes of session negotiation the text name for the Deflate algorithm is `DEFLATE` and its version number is `1.0`.

#### Time-series Special Compression

The Time-series Special Compression algorithm (TSSC) is an algorithm specifically designed for STTP that is used to quickly compress streaming time-series data with very good compression ratios. The TSSC algorithm is defined in [Appendix E - TSSC Algorithm](TSSCAlgorithm.md).

TSSC is a stateful compression algorithm that is specifically designed to work with STTP streaming data packets. As such it is only designed for use in STTP when the data channel functions are established with a reliable transport protocol, e.g., TCP.

For the purposes of session negotiation the text name for the Time-series Special Compression algorithm is `TSSC` and its version number is `1.0`.
