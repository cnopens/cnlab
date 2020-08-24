#java-LZ4-compression-HC-decompresssion.md

##Lz4 compression

LZ4 Java

Build Status

LZ4 compression for Java, based on Yann Collet's work available at http://code.google.com/p/lz4/.

This library provides access to two compression methods that both generate a valid LZ4 stream:

    fast scan (LZ4):
        low memory footprint (~ 16 KB),
        very fast (fast scan with skipping heuristics in case the input looks incompressible),
        reasonable compression ratio (depending on the redundancy of the input).
    high compression (LZ4 HC):
        medium memory footprint (~ 256 KB),
        rather slow (~ 10 times slower than LZ4),
        good compression ratio (depending on the size and the redundancy of the input).

The streams produced by those 2 compression algorithms use the same compression format, are very fast to decompress and can be decompressed by the same decompressor instance.
Implementations

For LZ4 compressors, LZ4 HC compressors and decompressors, 3 implementations are available:

    JNI bindings to the original C implementation by Yann Collet,
    a pure Java port of the compression and decompression algorithms,
    a Java port that uses the sun.misc.Unsafe API in order to achieve compression and decompression speeds close to the C implementation.

Have a look at LZ4Factory for more information.
Compatibility notes

    Compressors and decompressors are interchangeable: it is perfectly correct to compress with the JNI bindings and to decompress with a Java port, or the other way around.

    Compressors might not generate the same compressed streams on all platforms, especially if CPU endianness differs, but the compressed streams can be safely decompressed by any decompressor implementation on any platform.

Examples

```
LZ4Factory factory = LZ4Factory.fastestInstance();

byte[] data = "12345345234572".getBytes("UTF-8");
final int decompressedLength = data.length;

// compress data
LZ4Compressor compressor = factory.fastCompressor();
int maxCompressedLength = compressor.maxCompressedLength(decompressedLength);
byte[] compressed = new byte[maxCompressedLength];
int compressedLength = compressor.compress(data, 0, decompressedLength, compressed, 0, maxCompressedLength);

// decompress data
// - method 1: when the decompressed length is known
LZ4FastDecompressor decompressor = factory.fastDecompressor();
byte[] restored = new byte[decompressedLength];
int compressedLength2 = decompressor.decompress(compressed, 0, restored, 0, decompressedLength);
// compressedLength == compressedLength2

// - method 2: when the compressed length is known (a little slower)
// the destination buffer needs to be over-sized
LZ4SafeDecompressor decompressor2 = factory.safeDecompressor();
int decompressedLength2 = decompressor2.decompress(compressed, 0, compressedLength, restored, 0);
// decompressedLength == decompressedLength2

byte[] data = "12345345234572".getBytes("UTF-8");
final int decompressedLength = data.length;

LZ4FrameOutputStream outStream = new LZ4FrameOutputStream(new FileOutputStream(new File("test.lz4")));
outStream.write(data);
outStream.close();

byte[] restored = new byte[decompressedLength];
LZ4FrameInputStream inStream = new LZ4FrameInputStream(new FileInputStream(new File("test.lz4")));
inStream.read(restored);
inStream.close();
```



## References 

https://github.com/lz4/lz4-java