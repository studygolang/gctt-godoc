version: 1.10
## package flate

  `import "compress/flate"`

## Overview

Package flate implements the DEFLATE compressed data format, described in RFC
1951. The gzip and zlib packages implement access to DEFLATE-based file formats.

<a id="example_dictionary"></a>
Example:

    // The dictionary is a string of bytes. When compressing some input data,
    // the compressor will attempt to substitute substrings with matches found
    // in the dictionary. As such, the dictionary should only contain substrings
    // that are expected to be found in the actual data stream.
    const dict = `<?xml version="1.0"?>` + `<book>` + `<data>` + `<meta name="` + `" content="`

    // The data to compress should (but is not required to) contain frequent
    // substrings that match those in the dictionary.
    const data = `<?xml version="1.0"?>
    <book>
    	<meta name="title" content="The Go Programming Language"/>
    	<meta name="authors" content="Alan Donovan and Brian Kernighan"/>
    	<meta name="published" content="2015-10-26"/>
    	<meta name="isbn" content="978-0134190440"/>
    	<data>...</data>
    </book>
    `

    var b bytes.Buffer

    // Compress the data using the specially crafted dictionary.
    zw, err := flate.NewWriterDict(&b, flate.DefaultCompression, []byte(dict))
    if err != nil {
        log.Fatal(err)
    }
    if _, err := io.Copy(zw, strings.NewReader(data)); err != nil {
        log.Fatal(err)
    }
    if err := zw.Close(); err != nil {
        log.Fatal(err)
    }

    // The decompressor must use the same dictionary as the compressor.
    // Otherwise, the input may appear as corrupted.
    fmt.Println("Decompressed output using the dictionary:")
    zr := flate.NewReaderDict(bytes.NewReader(b.Bytes()), []byte(dict))
    if _, err := io.Copy(os.Stdout, zr); err != nil {
        log.Fatal(err)
    }
    if err := zr.Close(); err != nil {
        log.Fatal(err)
    }

    fmt.Println()

    // Substitute all of the bytes in the dictionary with a '#' to visually
    // demonstrate the approximate effectiveness of using a preset dictionary.
    fmt.Println("Substrings matched by the dictionary are marked with #:")
    hashDict := []byte(dict)
    for i := range hashDict {
        hashDict[i] = '#'
    }
    zr = flate.NewReaderDict(&b, hashDict)
    if _, err := io.Copy(os.Stdout, zr); err != nil {
        log.Fatal(err)
    }
    if err := zr.Close(); err != nil {
        log.Fatal(err)
    }

    // Output:
    // Decompressed output using the dictionary:
    // <?xml version="1.0"?>
    // <book>
    // 	<meta name="title" content="The Go Programming Language"/>
    // 	<meta name="authors" content="Alan Donovan and Brian Kernighan"/>
    // 	<meta name="published" content="2015-10-26"/>
    // 	<meta name="isbn" content="978-0134190440"/>
    // 	<data>...</data>
    // </book>
    //
    // Substrings matched by the dictionary are marked with #:
    // #####################
    // ######
    // 	############title###########The Go Programming Language"/#
    // 	############authors###########Alan Donovan and Brian Kernighan"/#
    // 	############published###########2015-10-26"/#
    // 	############isbn###########978-0134190440"/#
    // 	######...</#####
    // </#####


<a id="example_reset"></a>
Example:

    proverbs := []string{
        "Don't communicate by sharing memory, share memory by communicating.\n",
        "Concurrency is not parallelism.\n",
        "The bigger the interface, the weaker the abstraction.\n",
        "Documentation is for users.\n",
    }

    var r strings.Reader
    var b bytes.Buffer
    buf := make([]byte, 32<<10)

    zw, err := flate.NewWriter(nil, flate.DefaultCompression)
    if err != nil {
        log.Fatal(err)
    }
    zr := flate.NewReader(nil)

    for _, s := range proverbs {
        r.Reset(s)
        b.Reset()

        // Reset the compressor and encode from some input stream.
        zw.Reset(&b)
        if _, err := io.CopyBuffer(zw, &r, buf); err != nil {
            log.Fatal(err)
        }
        if err := zw.Close(); err != nil {
            log.Fatal(err)
        }

        // Reset the decompressor and decode to some output stream.
        if err := zr.(flate.Resetter).Reset(&b, nil); err != nil {
            log.Fatal(err)
        }
        if _, err := io.CopyBuffer(os.Stdout, zr, buf); err != nil {
            log.Fatal(err)
        }
        if err := zr.Close(); err != nil {
            log.Fatal(err)
        }
    }

    // Output:
    // Don't communicate by sharing memory, share memory by communicating.
    // Concurrency is not parallelism.
    // The bigger the interface, the weaker the abstraction.
    // Documentation is for users.


<a id="example_synchronization"></a>
Example:

    var wg sync.WaitGroup
    defer wg.Wait()

    // Use io.Pipe to simulate a network connection.
    // A real network application should take care to properly close the
    // underlying connection.
    rp, wp := io.Pipe()

    // Start a goroutine to act as the transmitter.
    wg.Add(1)
    go func() {
        defer wg.Done()

        zw, err := flate.NewWriter(wp, flate.BestSpeed)
        if err != nil {
            log.Fatal(err)
        }

        b := make([]byte, 256)
        for _, m := range strings.Fields("A long time ago in a galaxy far, far away...") {
            // We use a simple framing format where the first byte is the
            // message length, followed the message itself.
            b[0] = uint8(copy(b[1:], m))

            if _, err := zw.Write(b[:1+len(m)]); err != nil {
                log.Fatal(err)
            }

            // Flush ensures that the receiver can read all data sent so far.
            if err := zw.Flush(); err != nil {
                log.Fatal(err)
            }
        }

        if err := zw.Close(); err != nil {
            log.Fatal(err)
        }
    }()

    // Start a goroutine to act as the receiver.
    wg.Add(1)
    go func() {
        defer wg.Done()

        zr := flate.NewReader(rp)

        b := make([]byte, 256)
        for {
            // Read the message length.
            // This is guaranteed to return for every corresponding
            // Flush and Close on the transmitter side.
            if _, err := io.ReadFull(zr, b[:1]); err != nil {
                if err == io.EOF {
                    break // The transmitter closed the stream
                }
                log.Fatal(err)
            }

            // Read the message content.
            n := int(b[0])
            if _, err := io.ReadFull(zr, b[:n]); err != nil {
                log.Fatal(err)
            }

            fmt.Printf("Received %d bytes: %s\n", n, b[:n])
        }
        fmt.Println()

        if err := zr.Close(); err != nil {
            log.Fatal(err)
        }
    }()

    // Output:
    // Received 1 bytes: A
    // Received 4 bytes: long
    // Received 4 bytes: time
    // Received 3 bytes: ago
    // Received 2 bytes: in
    // Received 1 bytes: a
    // Received 6 bytes: galaxy
    // Received 4 bytes: far,
    // Received 3 bytes: far
    // Received 7 bytes: away...

## Index

- [Constants](#pkg-constants)
- [func NewReader(r io.Reader) io.ReadCloser](#NewReader)
- [func NewReaderDict(r io.Reader, dict []byte) io.ReadCloser](#NewReaderDict)
- [type CorruptInputError](#CorruptInputError)
  - [func (e CorruptInputError) Error() string](#CorruptInputError.Error)
- [type InternalError](#InternalError)
  - [func (e InternalError) Error() string](#InternalError.Error)
- [type ReadError](#ReadError)
  - [func (e *ReadError) Error() string](#ReadError.Error)
- [type Reader](#Reader)
- [type Resetter](#Resetter)
- [type WriteError](#WriteError)
  - [func (e *WriteError) Error() string](#WriteError.Error)
- [type Writer](#Writer)
  - [func NewWriter(w io.Writer, level int) (*Writer, error)](#NewWriter)
  - [func NewWriterDict(w io.Writer, level int, dict []byte) (*Writer, error)](#NewWriterDict)
  - [func (w *Writer) Close() error](#Writer.Close)
  - [func (w *Writer) Flush() error](#Writer.Flush)
  - [func (w *Writer) Reset(dst io.Writer)](#Writer.Reset)
  - [func (w *Writer) Write(data []byte) (n int, err error)](#Writer.Write)

### Examples

- [Package (Dictionary)](#example_dictionary)
- [Package (Reset)](#example_reset)
- [Package (Synchronization)](#example_synchronization)

### Package files
 [deflate.go](//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/deflate.go) [deflatefast.go](//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/deflatefast.go) [dict_decoder.go](//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/dict_decoder.go) [huffman_bit_writer.go](//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/huffman_bit_writer.go) [huffman_code.go](//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/huffman_code.go) [inflate.go](//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go) [token.go](//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/token.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="NoCompression">NoCompression</span>      = 0
    <span id="BestSpeed">BestSpeed</span>          = 1
    <span id="BestCompression">BestCompression</span>    = 9
    <span id="DefaultCompression">DefaultCompression</span> = -1

    <span class="comment">// HuffmanOnly disables Lempel-Ziv match searching and only performs Huffman</span>
    <span class="comment">// entropy encoding. This mode is useful in compressing data that has</span>
    <span class="comment">// already been compressed with an LZ style algorithm (e.g. Snappy or LZ4)</span>
    <span class="comment">// that lacks an entropy encoder. Compression gains are achieved when</span>
    <span class="comment">// certain bytes in the input stream occur more frequently than others.</span>
    <span class="comment">//</span>
    <span class="comment">// Note that HuffmanOnly produces a compressed output that is</span>
    <span class="comment">// RFC 1951 compliant. That is, any valid DEFLATE decompressor will</span>
    <span class="comment">// continue to be able to decompress this output.</span>
    <span id="HuffmanOnly">HuffmanOnly</span> = -2
)</pre>


<h2 id="NewReader">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go#L775">NewReader</a>
    <a href="#NewReader">¶</a></h2>
<pre>func NewReader(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a></pre>

NewReader returns a new ReadCloser that can be used to read the uncompressed
version of r. If r does not also implement io.ByteReader, the decompressor may
read more data than necessary from r. It is the caller's responsibility to call
Close on the ReadCloser when finished reading.

The ReadCloser returned by NewReader also implements Resetter.

<h2 id="NewReaderDict">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go#L794">NewReaderDict</a>
    <a href="#NewReaderDict">¶</a></h2>
<pre>func NewReaderDict(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, dict []<a href="/builtin/#byte">byte</a>) <a href="/io/">io</a>.<a href="/io/#ReadCloser">ReadCloser</a></pre>

NewReaderDict is like NewReader but initializes the reader with a preset
dictionary. The returned Reader behaves as if the uncompressed data stream
started with the given dictionary, which has already been read. NewReaderDict is
typically used to read data compressed by NewWriterDict.

The ReadCloser returned by NewReader also implements Resetter.

<h2 id="CorruptInputError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go#L23">CorruptInputError</a>
    <a href="#CorruptInputError">¶</a></h2>
<pre>type CorruptInputError <a href="/builtin/#int64">int64</a></pre>

A CorruptInputError reports the presence of corrupt input at a given offset.

<h3 id="CorruptInputError.Error">func (CorruptInputError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go#L25">Error</a>
    <a href="#CorruptInputError.Error">¶</a></h3>
<pre>func (e <a href="#CorruptInputError">CorruptInputError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="InternalError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go#L30">InternalError</a>
    <a href="#InternalError">¶</a></h2>
<pre>type InternalError <a href="/builtin/#string">string</a></pre>

An InternalError reports an error in the flate code itself.

<h3 id="InternalError.Error">func (InternalError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go#L32">Error</a>
    <a href="#InternalError.Error">¶</a></h3>
<pre>func (e <a href="#InternalError">InternalError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="ReadError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go#L37">ReadError</a>
    <a href="#ReadError">¶</a></h2>
<pre>type ReadError struct {
<span id="ReadError.Offset"></span>    Offset <a href="/builtin/#int64">int64</a> <span class="comment">// byte offset where error occurred</span>
<span id="ReadError.Err"></span>    Err    <a href="/builtin/#error">error</a> <span class="comment">// error returned by underlying Read</span>
}</pre>

A ReadError reports an error encountered while reading input.

Deprecated: No longer returned.

<h3 id="ReadError.Error">func (*ReadError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go#L42">Error</a>
    <a href="#ReadError.Error">¶</a></h3>
<pre>func (e *<a href="#ReadError">ReadError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Reader">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go#L251">Reader</a>
    <a href="#Reader">¶</a></h2>
<pre>type Reader interface {
    <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>
    <a href="/io/">io</a>.<a href="/io/#ByteReader">ByteReader</a>
}</pre>

The actual read interface needed by NewReader. If the passed in io.Reader does
not also have ReadByte, the NewReader will introduce its own buffering.

<h2 id="Resetter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go#L61">Resetter</a>
    <a href="#Resetter">¶</a></h2>
<pre>type Resetter interface {
    <span class="comment">// Reset discards any buffered data and resets the Resetter as if it was</span>
    <span class="comment">// newly initialized with the given reader.</span>
    Reset(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>, dict []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a>
}</pre>

Resetter resets a ReadCloser returned by NewReader or NewReaderDict to to switch
to a new underlying Reader. This permits reusing a ReadCloser instead of
allocating a new one.

<h2 id="WriteError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go#L49">WriteError</a>
    <a href="#WriteError">¶</a></h2>
<pre>type WriteError struct {
<span id="WriteError.Offset"></span>    Offset <a href="/builtin/#int64">int64</a> <span class="comment">// byte offset where error occurred</span>
<span id="WriteError.Err"></span>    Err    <a href="/builtin/#error">error</a> <span class="comment">// error returned by underlying Write</span>
}</pre>

A WriteError reports an error encountered while writing output.

Deprecated: No longer returned.

<h3 id="WriteError.Error">func (*WriteError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/inflate.go#L54">Error</a>
    <a href="#WriteError.Error">¶</a></h3>
<pre>func (e *<a href="#WriteError">WriteError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Writer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/deflate.go#L691">Writer</a>
    <a href="#Writer">¶</a></h2>
<pre>type Writer struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Writer takes data written to it and writes the compressed form of that data to
an underlying writer (see NewWriter).

<h3 id="NewWriter">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/deflate.go#L656">NewWriter</a>
    <a href="#NewWriter">¶</a></h3>
<pre>func NewWriter(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, level <a href="/builtin/#int">int</a>) (*<a href="#Writer">Writer</a>, <a href="/builtin/#error">error</a>)</pre>

NewWriter returns a new Writer compressing data at the given level. Following
zlib, levels range from 1 (BestSpeed) to 9 (BestCompression); higher levels
typically run slower but compress more. Level 0 (NoCompression) does not attempt
any compression; it only adds the necessary DEFLATE framing. Level -1
(DefaultCompression) uses the default compression level. Level -2 (HuffmanOnly)
will use Huffman compression only, giving a very fast compression for all types
of input, but sacrificing considerable compression efficiency.

If level is in the range [-2, 9] then the error returned will be nil. Otherwise
the error returned will be non-nil.

<h3 id="NewWriterDict">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/deflate.go#L670">NewWriterDict</a>
    <a href="#NewWriterDict">¶</a></h3>
<pre>func NewWriterDict(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>, level <a href="/builtin/#int">int</a>, dict []<a href="/builtin/#byte">byte</a>) (*<a href="#Writer">Writer</a>, <a href="/builtin/#error">error</a>)</pre>

NewWriterDict is like NewWriter but initializes the new Writer with a preset
dictionary. The returned Writer behaves as if the dictionary had been written to
it without producing any compressed output. The compressed data written to w can
only be decompressed by a Reader initialized with the same dictionary.

<h3 id="Writer.Close">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/deflate.go#L718">Close</a>
    <a href="#Writer.Close">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Close() <a href="/builtin/#error">error</a></pre>

Close flushes and closes the writer.

<h3 id="Writer.Flush">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/deflate.go#L711">Flush</a>
    <a href="#Writer.Flush">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Flush() <a href="/builtin/#error">error</a></pre>

Flush flushes any pending data to the underlying writer. It is useful mainly in
compressed network protocols, to ensure that a remote reader has enough data to
reconstruct a packet. Flush does not return until the data has been written.
Calling Flush when there is no pending data still causes the Writer to emit a
sync marker of at least 4 bytes. If the underlying writer returns an error,
Flush returns that error.

In the terminology of the zlib library, Flush is equivalent to Z_SYNC_FLUSH.

<h3 id="Writer.Reset">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/deflate.go#L725">Reset</a>
    <a href="#Writer.Reset">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Reset(dst <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>)</pre>

Reset discards the writer's state and makes it equivalent to the result of
NewWriter or NewWriterDict called with dst and w's level and dictionary.

<h3 id="Writer.Write">func (*Writer) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/compress/flate/deflate.go#L698">Write</a>
    <a href="#Writer.Write">¶</a></h3>
<pre>func (w *<a href="#Writer">Writer</a>) Write(data []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

Write writes data to w, which will eventually write the compressed form of data
to its underlying writer.


