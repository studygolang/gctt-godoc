e1version: 1.10
## package net

  `import "net"`

## 概述

net 包提供了网络 I/O 的接口，它包括 TCP/IP，UDP，域名解析和 Unix 域套接字。

尽管这个包提供了访问底层原始网络的能力，不过大多数用户只需要 Dial、Listen、Accept 函数和对应的 Conn，Listen 提供的基础接口。crypto/tls 包也是使用本包中的接口和与 Dial，Listen 相似的函数。

使用 Dial 函数连接服务器：

    conn, err := net.Dial("tcp", "golang.org:80")
    if err != nil {
    	// handle error
    }
    fmt.Fprintf(conn, "GET / HTTP/1.0\r\n\r\n")
    status, err := bufio.NewReader(conn).ReadString('\n')
    // ...

使用 Listen 函数创建服务器：

    ln, err := net.Listen("tcp", ":8080")
    if err != nil {
    	// handle error
    }
    for {
    	conn, err := ln.Accept()
    	if err != nil {
    		// handle error
    	}
    	go handleConnection(conn)
    }

域名解析

根据不同的操作系统，解析域名的方法可能是间接使用 Dial 函数也可能是直接使用 LookupHost 和 LookupAddr。

在 Unix 系统中，有两种处理域名的方式。可以通过纯 Go 实现的处理器直接向 /etc/resolv.conf 列出的服务器发送 DNS 请求，也可以通过 cgo 调用 C 路由库（例如：getaddrinfo 和 getnameinfo）。

默认情况下使用纯 Go 实现的处理器，因为阻塞的 DNS 请求只占用一个 goroutine，而一个阻塞的 C 调用却要消耗一个操作系统的线程。当 cgo 可用时，会在以下条件下使用 cgo 的域名处理器：系统不允许程序直接发起 DNS 请求（OS X），设置了 LOCALDOMAIN 环境变量， RES_OPTIONS 或 HOSTALIASES 环境变量非空，ASR_CONFIG 环境变量非空（仅限于 OpenBSD），/etc/resolv.conf 或 /etc/nsswitch.conf 功能在纯 Go 处理器中没有实现，域名以 .local 结尾或是一个 mDNS 域名。

可以通过设置 GODEBUG 环境变量的 netdns 的值（go/cgo）来手动配置：

    export GODEBUG=netdns=go    # 强制使用纯 go 处理器
    export GODEBUG=netdns=cgo   # 强制使用 cgo 处理器

也可以在构建 Go 源码树的时候使用 netgo/netcgo 标签进行指定。

像 GODEBUG=netdns=1 这样当 netdns 是一个数字值的时候，处理器会打印调试信息和处理器类型。强制指定一个处理器并打印调试信息可以将两个配置值用加号连接（GODEBUG=netdns=go+1）。

在 Plan 9 中，处理器访问 /net/cs 和 /net/dns。

在 Windows 中，处理器使用 C 库函数，例如：GetAddrInfo 和 DnsQuery。

## 索引

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func InterfaceAddrs() ([]Addr, error)](#InterfaceAddrs)
- [func Interfaces() ([]Interface, error)](#Interfaces)
- [func JoinHostPort(host, port string) string](#JoinHostPort)
- [func LookupAddr(addr string) (names []string, err error)](#LookupAddr)
- [func LookupCNAME(host string) (cname string, err error)](#LookupCNAME)
- [func LookupHost(host string) (addrs []string, err error)](#LookupHost)
- [func LookupIP(host string) ([]IP, error)](#LookupIP)
- [func LookupMX(name string) ([]*MX, error)](#LookupMX)
- [func LookupNS(name string) ([]*NS, error)](#LookupNS)
- [func LookupPort(network, service string) (port int, err error)](#LookupPort)
- [func LookupSRV(service, proto, name string) (cname string, addrs []*SRV, err error)](#LookupSRV)
- [func LookupTXT(name string) ([]string, error)](#LookupTXT)
- [func SplitHostPort(hostport string) (host, port string, err error)](#SplitHostPort)
- [type Addr](#Addr)
- [type AddrError](#AddrError)
  - [func (e *AddrError) Error() string](#AddrError.Error)
  - [func (e *AddrError) Temporary() bool](#AddrError.Temporary)
  - [func (e *AddrError) Timeout() bool](#AddrError.Timeout)
- [type Buffers](#Buffers)
  - [func (v *Buffers) Read(p []byte) (n int, err error)](#Buffers.Read)
  - [func (v *Buffers) WriteTo(w io.Writer) (n int64, err error)](#Buffers.WriteTo)
- [type Conn](#Conn)
  - [func Dial(network, address string) (Conn, error)](#Dial)
  - [func DialTimeout(network, address string, timeout time.Duration) (Conn, error)](#DialTimeout)
  - [func FileConn(f *os.File) (c Conn, err error)](#FileConn)
  - [func Pipe() (Conn, Conn)](#Pipe)
- [type DNSConfigError](#DNSConfigError)
  - [func (e *DNSConfigError) Error() string](#DNSConfigError.Error)
  - [func (e *DNSConfigError) Temporary() bool](#DNSConfigError.Temporary)
  - [func (e *DNSConfigError) Timeout() bool](#DNSConfigError.Timeout)
- [type DNSError](#DNSError)
  - [func (e *DNSError) Error() string](#DNSError.Error)
  - [func (e *DNSError) Temporary() bool](#DNSError.Temporary)
  - [func (e *DNSError) Timeout() bool](#DNSError.Timeout)
- [type Dialer](#Dialer)
  - [func (d *Dialer) Dial(network, address string) (Conn, error)](#Dialer.Dial)
  - [func (d *Dialer) DialContext(ctx context.Context, network, address string) (Conn, error)](#Dialer.DialContext)
- [type Error](#Error)
- [type Flags](#Flags)
  - [func (f Flags) String() string](#Flags.String)
- [type HardwareAddr](#HardwareAddr)
  - [func ParseMAC(s string) (hw HardwareAddr, err error)](#ParseMAC)
  - [func (a HardwareAddr) String() string](#HardwareAddr.String)
- [type IP](#IP)
  - [func IPv4(a, b, c, d byte) IP](#IPv4)
  - [func ParseCIDR(s string) (IP, *IPNet, error)](#ParseCIDR)
  - [func ParseIP(s string) IP](#ParseIP)
  - [func (ip IP) DefaultMask() IPMask](#IP.DefaultMask)
  - [func (ip IP) Equal(x IP) bool](#IP.Equal)
  - [func (ip IP) IsGlobalUnicast() bool](#IP.IsGlobalUnicast)
  - [func (ip IP) IsInterfaceLocalMulticast() bool](#IP.IsInterfaceLocalMulticast)
  - [func (ip IP) IsLinkLocalMulticast() bool](#IP.IsLinkLocalMulticast)
  - [func (ip IP) IsLinkLocalUnicast() bool](#IP.IsLinkLocalUnicast)
  - [func (ip IP) IsLoopback() bool](#IP.IsLoopback)
  - [func (ip IP) IsMulticast() bool](#IP.IsMulticast)
  - [func (ip IP) IsUnspecified() bool](#IP.IsUnspecified)
  - [func (ip IP) MarshalText() ([]byte, error)](#IP.MarshalText)
  - [func (ip IP) Mask(mask IPMask) IP](#IP.Mask)
  - [func (ip IP) String() string](#IP.String)
  - [func (ip IP) To16() IP](#IP.To16)
  - [func (ip IP) To4() IP](#IP.To4)
  - [func (ip *IP) UnmarshalText(text []byte) error](#IP.UnmarshalText)
- [type IPAddr](#IPAddr)
  - [func ResolveIPAddr(network, address string) (*IPAddr, error)](#ResolveIPAddr)
  - [func (a *IPAddr) Network() string](#IPAddr.Network)
  - [func (a *IPAddr) String() string](#IPAddr.String)
- [type IPConn](#IPConn)
  - [func DialIP(network string, laddr, raddr *IPAddr) (*IPConn, error)](#DialIP)
  - [func ListenIP(network string, laddr *IPAddr) (*IPConn, error)](#ListenIP)
  - [func (c *IPConn) Close() error](#IPConn.Close)
  - [func (c *IPConn) File() (f *os.File, err error)](#IPConn.File)
  - [func (c *IPConn) LocalAddr() Addr](#IPConn.LocalAddr)
  - [func (c *IPConn) Read(b []byte) (int, error)](#IPConn.Read)
  - [func (c *IPConn) ReadFrom(b []byte) (int, Addr, error)](#IPConn.ReadFrom)
  - [func (c *IPConn) ReadFromIP(b []byte) (int, *IPAddr, error)](#IPConn.ReadFromIP)
  - [func (c *IPConn) ReadMsgIP(b, oob []byte) (n, oobn, flags int, addr *IPAddr, err error)](#IPConn.ReadMsgIP)
  - [func (c *IPConn) RemoteAddr() Addr](#IPConn.RemoteAddr)
  - [func (c *IPConn) SetDeadline(t time.Time) error](#IPConn.SetDeadline)
  - [func (c *IPConn) SetReadBuffer(bytes int) error](#IPConn.SetReadBuffer)
  - [func (c *IPConn) SetReadDeadline(t time.Time) error](#IPConn.SetReadDeadline)
  - [func (c *IPConn) SetWriteBuffer(bytes int) error](#IPConn.SetWriteBuffer)
  - [func (c *IPConn) SetWriteDeadline(t time.Time) error](#IPConn.SetWriteDeadline)
  - [func (c *IPConn) SyscallConn() (syscall.RawConn, error)](#IPConn.SyscallConn)
  - [func (c *IPConn) Write(b []byte) (int, error)](#IPConn.Write)
  - [func (c *IPConn) WriteMsgIP(b, oob []byte, addr *IPAddr) (n, oobn int, err error)](#IPConn.WriteMsgIP)
  - [func (c *IPConn) WriteTo(b []byte, addr Addr) (int, error)](#IPConn.WriteTo)
  - [func (c *IPConn) WriteToIP(b []byte, addr *IPAddr) (int, error)](#IPConn.WriteToIP)
- [type IPMask](#IPMask)
  - [func CIDRMask(ones, bits int) IPMask](#CIDRMask)
  - [func IPv4Mask(a, b, c, d byte) IPMask](#IPv4Mask)
  - [func (m IPMask) Size() (ones, bits int)](#IPMask.Size)
  - [func (m IPMask) String() string](#IPMask.String)
- [type IPNet](#IPNet)
  - [func (n *IPNet) Contains(ip IP) bool](#IPNet.Contains)
  - [func (n *IPNet) Network() string](#IPNet.Network)
  - [func (n *IPNet) String() string](#IPNet.String)
- [type Interface](#Interface)
  - [func InterfaceByIndex(index int) (*Interface, error)](#InterfaceByIndex)
  - [func InterfaceByName(name string) (*Interface, error)](#InterfaceByName)
  - [func (ifi *Interface) Addrs() ([]Addr, error)](#Interface.Addrs)
  - [func (ifi *Interface) MulticastAddrs() ([]Addr, error)](#Interface.MulticastAddrs)
- [type InvalidAddrError](#InvalidAddrError)
  - [func (e InvalidAddrError) Error() string](#InvalidAddrError.Error)
  - [func (e InvalidAddrError) Temporary() bool](#InvalidAddrError.Temporary)
  - [func (e InvalidAddrError) Timeout() bool](#InvalidAddrError.Timeout)
- [type Listener](#Listener)
  - [func FileListener(f *os.File) (ln Listener, err error)](#FileListener)
  - [func Listen(network, address string) (Listener, error)](#Listen)
- [type MX](#MX)
- [type NS](#NS)
- [type OpError](#OpError)
  - [func (e *OpError) Error() string](#OpError.Error)
  - [func (e *OpError) Temporary() bool](#OpError.Temporary)
  - [func (e *OpError) Timeout() bool](#OpError.Timeout)
- [type PacketConn](#PacketConn)
  - [func FilePacketConn(f *os.File) (c PacketConn, err error)](#FilePacketConn)
  - [func ListenPacket(network, address string) (PacketConn, error)](#ListenPacket)
- [type ParseError](#ParseError)
  - [func (e *ParseError) Error() string](#ParseError.Error)
- [type Resolver](#Resolver)
  - [func (r *Resolver) LookupAddr(ctx context.Context, addr string) (names []string, err error)](#Resolver.LookupAddr)
  - [func (r *Resolver) LookupCNAME(ctx context.Context, host string) (cname string, err error)](#Resolver.LookupCNAME)
  - [func (r *Resolver) LookupHost(ctx context.Context, host string) (addrs []string, err error)](#Resolver.LookupHost)
  - [func (r *Resolver) LookupIPAddr(ctx context.Context, host string) ([]IPAddr, error)](#Resolver.LookupIPAddr)
  - [func (r *Resolver) LookupMX(ctx context.Context, name string) ([]*MX, error)](#Resolver.LookupMX)
  - [func (r *Resolver) LookupNS(ctx context.Context, name string) ([]*NS, error)](#Resolver.LookupNS)
  - [func (r *Resolver) LookupPort(ctx context.Context, network, service string) (port int, err error)](#Resolver.LookupPort)
  - [func (r *Resolver) LookupSRV(ctx context.Context, service, proto, name string) (cname string, addrs []*SRV, err error)](#Resolver.LookupSRV)
  - [func (r *Resolver) LookupTXT(ctx context.Context, name string) ([]string, error)](#Resolver.LookupTXT)
- [type SRV](#SRV)
- [type TCPAddr](#TCPAddr)
  - [func ResolveTCPAddr(network, address string) (*TCPAddr, error)](#ResolveTCPAddr)
  - [func (a *TCPAddr) Network() string](#TCPAddr.Network)
  - [func (a *TCPAddr) String() string](#TCPAddr.String)
- [type TCPConn](#TCPConn)
  - [func DialTCP(network string, laddr, raddr *TCPAddr) (*TCPConn, error)](#DialTCP)
  - [func (c *TCPConn) Close() error](#TCPConn.Close)
  - [func (c *TCPConn) CloseRead() error](#TCPConn.CloseRead)
  - [func (c *TCPConn) CloseWrite() error](#TCPConn.CloseWrite)
  - [func (c *TCPConn) File() (f *os.File, err error)](#TCPConn.File)
  - [func (c *TCPConn) LocalAddr() Addr](#TCPConn.LocalAddr)
  - [func (c *TCPConn) Read(b []byte) (int, error)](#TCPConn.Read)
  - [func (c *TCPConn) ReadFrom(r io.Reader) (int64, error)](#TCPConn.ReadFrom)
  - [func (c *TCPConn) RemoteAddr() Addr](#TCPConn.RemoteAddr)
  - [func (c *TCPConn) SetDeadline(t time.Time) error](#TCPConn.SetDeadline)
  - [func (c *TCPConn) SetKeepAlive(keepalive bool) error](#TCPConn.SetKeepAlive)
  - [func (c *TCPConn) SetKeepAlivePeriod(d time.Duration) error](#TCPConn.SetKeepAlivePeriod)
  - [func (c *TCPConn) SetLinger(sec int) error](#TCPConn.SetLinger)
  - [func (c *TCPConn) SetNoDelay(noDelay bool) error](#TCPConn.SetNoDelay)
  - [func (c *TCPConn) SetReadBuffer(bytes int) error](#TCPConn.SetReadBuffer)
  - [func (c *TCPConn) SetReadDeadline(t time.Time) error](#TCPConn.SetReadDeadline)
  - [func (c *TCPConn) SetWriteBuffer(bytes int) error](#TCPConn.SetWriteBuffer)
  - [func (c *TCPConn) SetWriteDeadline(t time.Time) error](#TCPConn.SetWriteDeadline)
  - [func (c *TCPConn) SyscallConn() (syscall.RawConn, error)](#TCPConn.SyscallConn)
  - [func (c *TCPConn) Write(b []byte) (int, error)](#TCPConn.Write)
- [type TCPListener](#TCPListener)
  - [func ListenTCP(network string, laddr *TCPAddr) (*TCPListener, error)](#ListenTCP)
  - [func (l *TCPListener) Accept() (Conn, error)](#TCPListener.Accept)
  - [func (l *TCPListener) AcceptTCP() (*TCPConn, error)](#TCPListener.AcceptTCP)
  - [func (l *TCPListener) Addr() Addr](#TCPListener.Addr)
  - [func (l *TCPListener) Close() error](#TCPListener.Close)
  - [func (l *TCPListener) File() (f *os.File, err error)](#TCPListener.File)
  - [func (l *TCPListener) SetDeadline(t time.Time) error](#TCPListener.SetDeadline)
  - [func (l *TCPListener) SyscallConn() (syscall.RawConn, error)](#TCPListener.SyscallConn)
- [type UDPAddr](#UDPAddr)
  - [func ResolveUDPAddr(network, address string) (*UDPAddr, error)](#ResolveUDPAddr)
  - [func (a *UDPAddr) Network() string](#UDPAddr.Network)
  - [func (a *UDPAddr) String() string](#UDPAddr.String)
- [type UDPConn](#UDPConn)
  - [func DialUDP(network string, laddr, raddr *UDPAddr) (*UDPConn, error)](#DialUDP)
  - [func ListenMulticastUDP(network string, ifi *Interface, gaddr *UDPAddr) (*UDPConn, error)](#ListenMulticastUDP)
  - [func ListenUDP(network string, laddr *UDPAddr) (*UDPConn, error)](#ListenUDP)
  - [func (c *UDPConn) Close() error](#UDPConn.Close)
  - [func (c *UDPConn) File() (f *os.File, err error)](#UDPConn.File)
  - [func (c *UDPConn) LocalAddr() Addr](#UDPConn.LocalAddr)
  - [func (c *UDPConn) Read(b []byte) (int, error)](#UDPConn.Read)
  - [func (c *UDPConn) ReadFrom(b []byte) (int, Addr, error)](#UDPConn.ReadFrom)
  - [func (c *UDPConn) ReadFromUDP(b []byte) (int, *UDPAddr, error)](#UDPConn.ReadFromUDP)
  - [func (c *UDPConn) ReadMsgUDP(b, oob []byte) (n, oobn, flags int, addr *UDPAddr, err error)](#UDPConn.ReadMsgUDP)
  - [func (c *UDPConn) RemoteAddr() Addr](#UDPConn.RemoteAddr)
  - [func (c *UDPConn) SetDeadline(t time.Time) error](#UDPConn.SetDeadline)
  - [func (c *UDPConn) SetReadBuffer(bytes int) error](#UDPConn.SetReadBuffer)
  - [func (c *UDPConn) SetReadDeadline(t time.Time) error](#UDPConn.SetReadDeadline)
  - [func (c *UDPConn) SetWriteBuffer(bytes int) error](#UDPConn.SetWriteBuffer)
  - [func (c *UDPConn) SetWriteDeadline(t time.Time) error](#UDPConn.SetWriteDeadline)
  - [func (c *UDPConn) SyscallConn() (syscall.RawConn, error)](#UDPConn.SyscallConn)
  - [func (c *UDPConn) Write(b []byte) (int, error)](#UDPConn.Write)
  - [func (c *UDPConn) WriteMsgUDP(b, oob []byte, addr *UDPAddr) (n, oobn int, err error)](#UDPConn.WriteMsgUDP)
  - [func (c *UDPConn) WriteTo(b []byte, addr Addr) (int, error)](#UDPConn.WriteTo)
  - [func (c *UDPConn) WriteToUDP(b []byte, addr *UDPAddr) (int, error)](#UDPConn.WriteToUDP)
- [type UnixAddr](#UnixAddr)
  - [func ResolveUnixAddr(network, address string) (*UnixAddr, error)](#ResolveUnixAddr)
  - [func (a *UnixAddr) Network() string](#UnixAddr.Network)
  - [func (a *UnixAddr) String() string](#UnixAddr.String)
- [type UnixConn](#UnixConn)
  - [func DialUnix(network string, laddr, raddr *UnixAddr) (*UnixConn, error)](#DialUnix)
  - [func ListenUnixgram(network string, laddr *UnixAddr) (*UnixConn, error)](#ListenUnixgram)
  - [func (c *UnixConn) Close() error](#UnixConn.Close)
  - [func (c *UnixConn) CloseRead() error](#UnixConn.CloseRead)
  - [func (c *UnixConn) CloseWrite() error](#UnixConn.CloseWrite)
  - [func (c *UnixConn) File() (f *os.File, err error)](#UnixConn.File)
  - [func (c *UnixConn) LocalAddr() Addr](#UnixConn.LocalAddr)
  - [func (c *UnixConn) Read(b []byte) (int, error)](#UnixConn.Read)
  - [func (c *UnixConn) ReadFrom(b []byte) (int, Addr, error)](#UnixConn.ReadFrom)
  - [func (c *UnixConn) ReadFromUnix(b []byte) (int, *UnixAddr, error)](#UnixConn.ReadFromUnix)
  - [func (c *UnixConn) ReadMsgUnix(b, oob []byte) (n, oobn, flags int, addr *UnixAddr, err error)](#UnixConn.ReadMsgUnix)
  - [func (c *UnixConn) RemoteAddr() Addr](#UnixConn.RemoteAddr)
  - [func (c *UnixConn) SetDeadline(t time.Time) error](#UnixConn.SetDeadline)
  - [func (c *UnixConn) SetReadBuffer(bytes int) error](#UnixConn.SetReadBuffer)
  - [func (c *UnixConn) SetReadDeadline(t time.Time) error](#UnixConn.SetReadDeadline)
  - [func (c *UnixConn) SetWriteBuffer(bytes int) error](#UnixConn.SetWriteBuffer)
  - [func (c *UnixConn) SetWriteDeadline(t time.Time) error](#UnixConn.SetWriteDeadline)
  - [func (c *UnixConn) SyscallConn() (syscall.RawConn, error)](#UnixConn.SyscallConn)
  - [func (c *UnixConn) Write(b []byte) (int, error)](#UnixConn.Write)
  - [func (c *UnixConn) WriteMsgUnix(b, oob []byte, addr *UnixAddr) (n, oobn int, err error)](#UnixConn.WriteMsgUnix)
  - [func (c *UnixConn) WriteTo(b []byte, addr Addr) (int, error)](#UnixConn.WriteTo)
  - [func (c *UnixConn) WriteToUnix(b []byte, addr *UnixAddr) (int, error)](#UnixConn.WriteToUnix)
- [type UnixListener](#UnixListener)
  - [func ListenUnix(network string, laddr *UnixAddr) (*UnixListener, error)](#ListenUnix)
  - [func (l *UnixListener) Accept() (Conn, error)](#UnixListener.Accept)
  - [func (l *UnixListener) AcceptUnix() (*UnixConn, error)](#UnixListener.AcceptUnix)
  - [func (l *UnixListener) Addr() Addr](#UnixListener.Addr)
  - [func (l *UnixListener) Close() error](#UnixListener.Close)
  - [func (l *UnixListener) File() (f *os.File, err error)](#UnixListener.File)
  - [func (l *UnixListener) SetDeadline(t time.Time) error](#UnixListener.SetDeadline)
  - [func (l *UnixListener) SetUnlinkOnClose(unlink bool)](#UnixListener.SetUnlinkOnClose)
  - [func (l *UnixListener) SyscallConn() (syscall.RawConn, error)](#UnixListener.SyscallConn)
- [type UnknownNetworkError](#UnknownNetworkError)
  - [func (e UnknownNetworkError) Error() string](#UnknownNetworkError.Error)
  - [func (e UnknownNetworkError) Temporary() bool](#UnknownNetworkError.Temporary)
  - [func (e UnknownNetworkError) Timeout() bool](#UnknownNetworkError.Timeout)
- [Bugs](#pkg-note-BUG)

### 例子

- [CIDRMask](#exampleCIDRMask)
- [IP.DefaultMask](#exampleIP_DefaultMask)
- [IP.Mask](#exampleIP_Mask)
- [IPv4](#exampleIPv4)
- [IPv4Mask](#exampleIPv4Mask)
- [Listener](#exampleListener)
- [ParseCIDR](#exampleParseCIDR)
- [ParseIP](#exampleParseIP)

### 文件

 [addrselect.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/addrselect.go) [cgo_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/cgo_linux.go) [cgo_resnew.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/cgo_resnew.go) [cgo_socknew.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/cgo_socknew.go) [cgo_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/cgo_unix.go) [conf.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/conf.go) [dial.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go) [dnsclient.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsclient.go) [dnsclient_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsclient_unix.go) [dnsconfig_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsconfig_unix.go) [dnsmsg.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsmsg.go) [error_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/error_posix.go) [fd_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/fd_unix.go) [file.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/file.go) [file_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/file_unix.go) [hook.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/hook.go) [hook_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/hook_unix.go) [hosts.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/hosts.go) [interface.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go) [interface_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface_linux.go) [ip.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go) [iprawsock.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go) [iprawsock_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock_posix.go) [ipsock.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ipsock.go) [ipsock_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ipsock_posix.go) [lookup.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go) [lookup_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup_unix.go) [mac.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/mac.go) [net.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go) [nss.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/nss.go) [parse.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/parse.go) [pipe.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/pipe.go) [port.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/port.go) [port_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/port_unix.go) [rawconn.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rawconn.go) [sendfile_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sendfile_linux.go) [sock_cloexec.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sock_cloexec.go) [sock_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sock_linux.go) [sock_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sock_posix.go) [sockopt_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sockopt_linux.go) [sockopt_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sockopt_posix.go) [sockoptip_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sockoptip_linux.go) [sockoptip_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sockoptip_posix.go) [tcpsock.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go) [tcpsock_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock_posix.go) [tcpsockopt_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsockopt_posix.go) [tcpsockopt_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsockopt_unix.go) [udpsock.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go) [udpsock_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock_posix.go) [unixsock.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go) [unixsock_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock_posix.go) [writev_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/writev_unix.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="IPv4len">IPv4len</span> = 4
    <span id="IPv6len">IPv6len</span> = 16
)</pre>

IP 地址长度（字节）。

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span id="IPv4bcast">IPv4bcast</span>     = <a href="#IPv4">IPv4</a>(255, 255, 255, 255) <span class="comment">// limited broadcast</span>
    <span id="IPv4allsys">IPv4allsys</span>    = <a href="#IPv4">IPv4</a>(224, 0, 0, 1)       <span class="comment">// all systems</span>
    <span id="IPv4allrouter">IPv4allrouter</span> = <a href="#IPv4">IPv4</a>(224, 0, 0, 2)       <span class="comment">// all routers</span>
    <span id="IPv4zero">IPv4zero</span>      = <a href="#IPv4">IPv4</a>(0, 0, 0, 0)         <span class="comment">// all zeros</span>
)</pre>

预置的 IPv4 地址

<pre>var (
    <span id="IPv6zero">IPv6zero</span>                   = <a href="#IP">IP</a>{0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}
    <span id="IPv6unspecified">IPv6unspecified</span>            = <a href="#IP">IP</a>{0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}
    <span id="IPv6loopback">IPv6loopback</span>               = <a href="#IP">IP</a>{0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1}
    <span id="IPv6interfacelocalallnodes">IPv6interfacelocalallnodes</span> = <a href="#IP">IP</a>{0xff, 0x01, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0x01}
    <span id="IPv6linklocalallnodes">IPv6linklocalallnodes</span>      = <a href="#IP">IP</a>{0xff, 0x02, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0x01}
    <span id="IPv6linklocalallrouters">IPv6linklocalallrouters</span>    = <a href="#IP">IP</a>{0xff, 0x02, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0x02}
)</pre>

预置的 IPv6 地址

<pre>var <span id="DefaultResolver">DefaultResolver</span> = &amp;<a href="#Resolver">Resolver</a>{}</pre>

包中的 Lookup 函数和没有指定处理器的 Dialer 都会使用 DefaultResolver。

<pre>var (
    <span id="ErrWriteToConnected">ErrWriteToConnected</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;use of WriteTo with pre-connected connection&#34;)
)</pre>

OpError 包括很多的错误。

<h2 id="InterfaceAddrs">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L105">InterfaceAddrs</a>
    <a href="#InterfaceAddrs">¶</a></h2>
<pre>func InterfaceAddrs() ([]<a href="#Addr">Addr</a>, <a href="/builtin/#error">error</a>)</pre>

InterfaceAddrs 返回系统的单播接口地址列表。

返回的列表不是对应接口；使用 Interfaces 和 Interface.Addr 来获取更多信息。

<h2 id="Interfaces">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L89">Interfaces</a>
    <a href="#Interfaces">¶</a></h2>
<pre>func Interfaces() ([]<a href="#Interface">Interface</a>, <a href="/builtin/#error">error</a>)</pre>

Interfaces 返回系统的网络接口列表。

<h2 id="JoinHostPort">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ipsock.go#L216">JoinHostPort</a>
    <a href="#JoinHostPort">¶</a></h2>
<pre>func JoinHostPort(host, port <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

JoinHostPort 把 host 和 port 组合成 host:port 格式。如果 host 像 IPv6 地址一样包含冒号，会返回 [host]:port 格式。

了解更多请看 Dial 函数关于 host 和 port 参数的描述。

<h2 id="LookupAddr">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L359">LookupAddr</a>
    <a href="#LookupAddr">¶</a></h2>
<pre>func LookupAddr(addr <a href="/builtin/#string">string</a>) (names []<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupAddr 根据指定地址获取匹配的域名列表。

当使用 C 库处理器时，最多返回一个结果。可以使用自定义的处理器来绕过主机处理器。

<h2 id="LookupCNAME">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L280">LookupCNAME</a>
    <a href="#LookupCNAME">¶</a></h2>
<pre>func LookupCNAME(host <a href="/builtin/#string">string</a>) (cname <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupCNAME 返回给定主机的规范名称。用户不用关心什么是规范的名称，只需知道它可以直接用来调用 LookupHost 或 LookupIP；我们可以把它看作 lookup 操作的一部分。

规范名称是 0 或多条 CNAME 记录的最后一个名称。LookupCNAME 只要能找到 host 的地址记录，即使在主机不包含 DNS CNAME 记录也不会返回错误。

<h2 id="LookupHost">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L132">LookupHost</a>
    <a href="#LookupHost">¶</a></h2>
<pre>func LookupHost(host <a href="/builtin/#string">string</a>) (addrs []<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupHost 使用本地处理器查询指定主机。它会返回一个由主机地址组成的切片。

<h2 id="LookupIP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L152">LookupIP</a>
    <a href="#LookupIP">¶</a></h2>
<pre>func LookupIP(host <a href="/builtin/#string">string</a>) ([]<a href="#IP">IP</a>, <a href="/builtin/#error">error</a>)</pre>

LookupIP 使用本地处理器寻找主机。它返回主机的 IPv4 和 IPv6 地址组成的切片。

<h2 id="LookupMX">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L325">LookupMX</a>
    <a href="#LookupMX">¶</a></h2>
<pre>func LookupMX(name <a href="/builtin/#string">string</a>) ([]*<a href="#MX">MX</a>, <a href="/builtin/#error">error</a>)</pre>

LookupMX 按优先级返回指定域名的 DNS MX 记录。

<h2 id="LookupNS">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L335">LookupNS</a>
    <a href="#LookupNS">¶</a></h2>
<pre>func LookupNS(name <a href="/builtin/#string">string</a>) ([]*<a href="#NS">NS</a>, <a href="/builtin/#error">error</a>)</pre>

LookupNS 返回指定域名的 DNS NS 记录。

<h2 id="LookupPort">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L251">LookupPort</a>
    <a href="#LookupPort">¶</a></h2>
<pre>func LookupPort(network, service <a href="/builtin/#string">string</a>) (port <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupPort 寻找指定网络，服务的端口。

<h2 id="LookupSRV">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L307">LookupSRV</a>
    <a href="#LookupSRV">¶</a></h2>
<pre>func LookupSRV(service, proto, name <a href="/builtin/#string">string</a>) (cname <a href="/builtin/#string">string</a>, addrs []*<a href="#SRV">SRV</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupSRV 尝试处理给定服务，协议和域名的 SRV 查询。proto 可以是 tcp 或 udp。返回的记录按优先级和权重排序。

LookupSRV 根据 RFC 2782 构造 DNS 名称去查看。它会查找 _service._proto.name。为了适应含有非标准名称的 SRV 记录，如果 service 和 proto 为空字符串，LookupSRV 直接按 name 查找。 

<h2 id="LookupTXT">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L345">LookupTXT</a>
    <a href="#LookupTXT">¶</a></h2>
<pre>func LookupTXT(name <a href="/builtin/#string">string</a>) ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

LookupTXT 返回给定域名的 DNS TXT 记录。

<h2 id="SplitHostPort">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ipsock.go#L145">SplitHostPort</a>
    <a href="#SplitHostPort">¶</a></h2>
<pre>func SplitHostPort(hostport <a href="/builtin/#string">string</a>) (host, port <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

SplitHostPort 把 host:port、host%zone:port、[host]:port 或 [host%zone]:port 格式的网络地址分成 host%zone 和 port。

带端口号的 IPv6 地址必须在中括号内，像：[::1]:80、[::1%lo0]:80。

了解更多请看 Dial 函数关于 host 和 port 参数的描述。

<h2 id="Addr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L94">Addr</a>
    <a href="#Addr">¶</a></h2>
<pre>type Addr interface {
    Network() <a href="/builtin/#string">string</a> <span class="comment">// name of the network (for example, &#34;tcp&#34;, &#34;udp&#34;)</span>
    String() <a href="/builtin/#string">string</a>  <span class="comment">// string form of address (for example, &#34;192.0.2.1:25&#34;, &#34;[2001:db8::1]:80&#34;)</span>
}</pre>

Addr 表示一个网络终端地址。

一般来说 Network 和 String 两个方法的返回值可以用做 Dial 的参数。但是字符串的具体格式依赖于具体实现。

<h2 id="AddrError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L502">AddrError</a>
    <a href="#AddrError">¶</a></h2>
<pre>type AddrError struct {
<span id="AddrError.Err"></span>    Err  <a href="/builtin/#string">string</a>
<span id="AddrError.Addr"></span>    Addr <a href="/builtin/#string">string</a>
}</pre>


<h3 id="AddrError.Error">func (*AddrError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L507">Error</a>
    <a href="#AddrError.Error">¶</a></h3>
<pre>func (e *<a href="#AddrError">AddrError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="AddrError.Temporary">func (*AddrError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L519">Temporary</a>
    <a href="#AddrError.Temporary">¶</a></h3>
<pre>func (e *<a href="#AddrError">AddrError</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>


<h3 id="AddrError.Timeout">func (*AddrError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L518">Timeout</a>
    <a href="#AddrError.Timeout">¶</a></h3>
<pre>func (e *<a href="#AddrError">AddrError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>


<h2 id="Buffers">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L619">Buffers</a>
    <a href="#Buffers">¶</a></h2>
<pre>type Buffers [][]<a href="/builtin/#byte">byte</a></pre>

Buffers 包含 0 或多个用于写入的字节。

在某些机器上，对于某些类型的链接，它会优化成特定 OS 的批量写入操作（例如：writev）。

<h3 id="Buffers.Read">func (*Buffers) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L642">Read</a>
    <a href="#Buffers.Read">¶</a></h3>
<pre>func (v *<a href="#Buffers">Buffers</a>) Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h3 id="Buffers.WriteTo">func (*Buffers) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L626">WriteTo</a>
    <a href="#Buffers.WriteTo">¶</a></h3>
<pre>func (v *<a href="#Buffers">Buffers</a>) WriteTo(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Conn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L102">Conn</a>
    <a href="#Conn">¶</a></h2>
<pre>type Conn interface 
    <span class="comment">// Read 从链接中读取数据。</span>
    <span class="comment">// Read 可以设置超时并在固定的时限后返回 Timeout()==true 的错误；请看 SetDeadline 和 SetReadDeadline。</span>
    Read(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
    
    <span class="comment">// Write 向链接写入内容。</span>
    <span class="comment">// Write 可以设置超时并在固定时限后返回 Timeout()==true 的错误；请看 SetDeadline 和 SetWriteDeadline。</span>
    Write(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
    
    <span class="comment">// Close 关闭链接。</span>
    <span class="comment">// 任何阻塞的读或写操作全部解除阻塞并返回错误。</span>
    Close() <a href="/builtin/#error">error</a>
    
    <span class="comment">// LocalAddr 返回本地网络地址。</span>
    LocalAddr() <a href="#Addr">Addr</a>
    
    <span class="comment">// RemoteAddr 返回远程网络地址。</span>
    RemoteAddr() <a href="#Addr">Addr</a>
    
    <span class="comment">// SetDeadline 设置对应链接读与写的截止时间。</span>
    <span class="comment">// 它和同时调用 SetReadDeadline 和 SetWriteDeadline 是一样的。</span>
    <span class="comment">//</span>
    <span class="comment">// 截止时间就是 I/O 操作超时返回时的绝对时间。截止时间对之后调用的读或写都有效。如果超过截止时间，可以通过设置之后的截止时间来刷新链接。</span>
    <span class="comment">//</span>
    <span class="comment">// 空闲超时时间通过在成功读或写后延长截止时间实现。</span>
    <span class="comment">//</span>
    <span class="comment">// 零值代表 I/O 操作没有超时时间。</span>
    SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a>
    
    <span class="comment">// SetReadDeadline 为之后的和当前阻塞的读调用设置截止时间。</span>
    <span class="comment">// 零值意味着 Read 没有超时时间。</span>
    SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a>
    
    <span class="comment">// SetWriteDeadline 为之后的和当前阻塞的读调用设置截止时间。</span>
    <span class="comment">// 即使超时他也可能返回 n > 0，代表已经成功写入数据。</span>
    <span class="comment">// 零值代表 Write 没有超时时间。</span>
    SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a>
}</pre>

Conn 是面向流的通用网络链接。

多个 goroutine 可以同时调用 Conn 的方法。

<h3 id="Dial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L279">Dial</a>
    <a href="#Dial">¶</a></h3>
<pre>func Dial(network, address <a href="/builtin/#string">string</a>) (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

Dial 连接指定网络上的地址。

已经存在的网络包括 `tcp`，`tcp4`（IPv4），`tcp6`（IPv6），`udp`，`udp4`（IPv4），`udp6`（IPv6），`ip4`（IPv4），`ip6`（IPv6），

`unix`，`unixgram` 和  `unixpacket`。

对于 TCP 和 UDP 网络，地址是 `host:port` 格式。 host 必须必须是一个 IP 地址或能够解析成 IP 地址的主机名。port 必须是一个数字端口或服务名称。如果 host 是 IPv6 地址，它必须用中括号包裹，像 `[2001:db8::1]:80` 或 `[fe80::1%zone]:80`。IPv6 的地址范围在 RFC 4007 中详细说明。函数 JoinHostPort 和 SplitHostPort 会把 host 和 port  处理成这种格式。当使用 TCP 时，Dial 会依次尝试连接每个由 host 解析来的 IP 地址直到其中一个连接成功。

例:

    Dial("tcp", "golang.org:http")
    Dial("tcp", "192.0.2.1:http")
    Dial("tcp", "198.51.100.1:80")
    Dial("udp", "[2001:db8::1]:domain")
    Dial("udp", "[fe80::1%lo0]:53")
    Dial("tcp", ":80")

对于 IP 网络，network 必须是 `ip`, `ip4` or `ip6` 后面接一个冒号和协议代号或者协议名，host 格式的地址。host 必须是 IP 地址或者 IPv6 地址空间。它取决于每种操作系统的对私有协议如何编号（例如 `0` 或 `255`）。

例:

    Dial("ip4:1", "192.0.2.1")
    Dial("ip6:ipv6-icmp", "2001:db8::1")
    Dial("ip6:58", "fe80::1%lo0")

对于 TCP，UDP 和 IP 网络，如果 host 为空或者没有指定 IP 地址（例如：`:80`,本地系统  TCP 和 UDP 的 `0.0.0.0:80` 或 `[::]:80`,IP 的  `0.0.0.0` 或 `::`）。

对于 Unix 网络，地址必须是文件系统的路径。

<h3 id="DialTimeout">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L294">DialTimeout</a>
    <a href="#DialTimeout">¶</a></h3>
<pre>func DialTimeout(network, address <a href="/builtin/#string">string</a>, timeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>) (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

DialTimeout 是带超时时间的 Dial。

超时时间包括域名处理的时间（如果域名需要处理的话）。当使用 TCP 并且主机地址参数解析出了多个 IP 地址，会把超时时间分散到每个连续的 dial 中，使得每个 dial 都有适当的连接时间。

Dial 函数文档详细介绍 network 和 address 参数。

<h3 id="FileConn">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/file.go#L11">FileConn</a>
    <a href="#FileConn">¶</a></h3>
<pre>func FileConn(f *<a href="/os/">os</a>.<a href="/os/#File">File</a>) (c <a href="#Conn">Conn</a>, err <a href="/builtin/#error">error</a>)</pre>

FileConn 返回一个打开文件 f 的网络链接。由调用者负责在完成后关闭 f。关闭 c 不会影响 f，关闭 f 也不会影响 c。

<h3 id="Pipe">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/pipe.go#L108">Pipe</a>
    <a href="#Pipe">¶</a></h3>
<pre>func Pipe() (<a href="#Conn">Conn</a>, <a href="#Conn">Conn</a>)</pre>

Pipe 在内存中创建一个同步的双向网络链接；两个返回值都实现了 Conn 接口。从一边写入的数据可以在另一边读取到，它会直接在两个链接间拷贝数据；没有内部的缓存机制。

<h2 id="DNSConfigError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L535">DNSConfigError</a>
    <a href="#DNSConfigError">¶</a></h2>
<pre>type DNSConfigError struct {
<span id="DNSConfigError.Err"></span>    Err <a href="/builtin/#error">error</a>
}</pre>

DNSConfigError 代表读取机器的 DNS 配置出错时返回的错误（已经不再使用，为了兼容性而保留）。

<h3 id="DNSConfigError.Error">func (*DNSConfigError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L539">Error</a>
    <a href="#DNSConfigError.Error">¶</a></h3>
<pre>func (e *<a href="#DNSConfigError">DNSConfigError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="DNSConfigError.Temporary">func (*DNSConfigError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L541">Temporary</a>
    <a href="#DNSConfigError.Temporary">¶</a></h3>
<pre>func (e *<a href="#DNSConfigError">DNSConfigError</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>


<h3 id="DNSConfigError.Timeout">func (*DNSConfigError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L540">Timeout</a>
    <a href="#DNSConfigError.Timeout">¶</a></h3>
<pre>func (e *<a href="#DNSConfigError">DNSConfigError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>


<h2 id="DNSError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L549">DNSError</a>
    <a href="#DNSError">¶</a></h2>
<pre>type DNSError struct {
<span id="DNSError.Err"></span>    Err         <a href="/builtin/#string">string</a> <span class="comment">// description of the error</span>
<span id="DNSError.Name"></span>    Name        <a href="/builtin/#string">string</a> <span class="comment">// name looked for</span>
<span id="DNSError.Server"></span>    Server      <a href="/builtin/#string">string</a> <span class="comment">// server used</span>
<span id="DNSError.IsTimeout"></span>    IsTimeout   <a href="/builtin/#bool">bool</a>   <span class="comment">// if true, timed out; not all timeouts set this</span>
<span id="DNSError.IsTemporary"></span>    IsTemporary <a href="/builtin/#bool">bool</a>   <span class="comment">// if true, error is temporary; not all errors set this</span>
}</pre>

DNSError 代表一个 DNS 查询错误。

<h3 id="DNSError.Error">func (*DNSError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L557">Error</a>
    <a href="#DNSError.Error">¶</a></h3>
<pre>func (e *<a href="#DNSError">DNSError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="DNSError.Temporary">func (*DNSError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L577">Temporary</a>
    <a href="#DNSError.Temporary">¶</a></h3>
<pre>func (e *<a href="#DNSError">DNSError</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>

Temporary 判断 DNS 是不是已知的暂时性错误。不是所有的暂时性错误都是已知的；DNS 查询可能返回暂时性错误但是 Temporary 返回 false。

<h3 id="DNSError.Timeout">func (*DNSError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L572">Timeout</a>
    <a href="#DNSError.Timeout">¶</a></h3>
<pre>func (e *<a href="#DNSError">DNSError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>

Timeout 判断 DNS 查询是不是已知超时错误。并不包含所有超时错误；DNS 查询可能由于超时返回 DNS 错误但是 Timeout 返回 false。

<h2 id="Dialer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L9">Dialer</a>
    <a href="#Dialer">¶</a></h2>
<pre>type Dialer struct {
<span id="Dialer.Timeout"></span>    <span class="comment">// Timeout 是 dial 等待连接完成的最大时间。如果同时设置了 Deadline 会以优先触发的为准。</span>

    <span class="comment">//</span>
    <span class="comment">// 默认没有超时时间。</span>
    <span class="comment">//</span>
    <span class="comment">// 当使用 TCP 并且连接一个对应多 IP 地址的主机名称，超时时间会在它们之间分配。</span>
    <span class="comment">//</span>
    <span class="comment">// 不管有没有设置超时时间，操作系统都会有自己的超时时间。例如 TCP 的超时时间总是 3 分钟左右。</span>
    Timeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Dialer.Deadline"></span>    <span class="comment">// Deadline 是连接失败的绝对时间。</span>

    <span class="comment">// 如果同时设置了 Timeout 会以优先触发的为准。</span>
    <span class="comment">// 零值代表没有截止时间，或和操作系统的超时时间保持一致。
    Deadline <a href="/time/">time</a>.<a href="/time/#Time">Time</a>

<span id="Dialer.LocalAddr"></span>    <span class="comment">// LocalAddr 是连接指定地址时所占用的本地地址。</span>

    <span class="comment">// 地址必须是网络链接兼容的类型。</span>
    <span class="comment">// 如果为 nil，会自动选择本地地址。</span>
    LocalAddr <a href="#Addr">Addr</a>

<span id="Dialer.DualStack"></span>    <span class="comment">// 当连接一个同时包含 IPv4 和 IPv6 地址的 `tcp` 网络主机时，DualStack 控制是否启用 RFC 6555-compliant 的 `Happy Eyeballs` 算法;</span>

    <span class="comment">// 它允许其中的一个网络静默断开。</span>
    DualStack <a href="/builtin/#bool">bool</a>

<span id="Dialer.FallbackDelay"></span>    <span class="comment">// FallbackDelay 在 DualStack 开启时，指定等待产生回退链接的时长。</span>

    <span class="comment">// 如果为零，默认使用 300ms。</span>
    FallbackDelay <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Dialer.KeepAlive"></span>    <span class="comment">// KeepAlive 让网络链接可以保持一段时间。</span>

    <span class="comment">// 如果为 0 代表不启用。</span>
    <span class="comment">// 不支持 keep-alive 的链接会忽略该属性。</span>
    KeepAlive <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Dialer.Resolver"></span>    <span class="comment">// Resolver 可选的指定用来替换的 Resolver。</span>

    Resolver *<a href="#Resolver">Resolver</a>
<span id="Dialer.Cancel"></span>    <span class="comment">// Cancel is an optional channel whose closure indicates that</span>

<span id="Dialer.Cancel"></span>    <span class="comment">// Cancel 是一个可选的 channel，他能在链接需要关闭的时候关闭。不是所有的链接都支持取消。</span>

    <span class="comment">//</span>
    <span class="comment">// 弃用: 使用 DialContext 代替该功能。</span>
    Cancel &lt;-chan struct{}
}</pre>

Dialer 包含连接一个网络地址的可选参数。

字段为零值和没有该字段是一样的。

使用零值 Dialer 和只调用 Dial 函数是一样的。

<h3 id="Dialer.Dial">func (*Dialer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L309">Dial</a>
    <a href="#Dialer.Dial">¶</a></h3>
<pre>func (d *<a href="#Dialer">Dialer</a>) Dial(network, address <a href="/builtin/#string">string</a>) (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

Dial 连接指定网络上的地址。

Dial 函数介绍关于 network 和 address 参数。

<h3 id="Dialer.DialContext">func (*Dialer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L331">DialContext</a>
    <a href="#Dialer.DialContext">¶</a></h3>
<pre>func (d *<a href="#Dialer">Dialer</a>) DialContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, network, address <a href="/builtin/#string">string</a>) (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

DialContext 使用提供的 context 连接指定网络上的地址。

提供的 Context 不能为 nil。如果 context 在连接完成之前过期，会返回一个错误。一旦连接成功，context 是否过期不会影响链接状态。

当使用 TCP 并且 address 参数的主机解析出多个网络地址时，链接的超时时间（来自 ctx 或 d.Timeout）会分配给每个连续的连接上，让每次连接都有适当的时间。例如：一个主机名对应 4 个 IP 地址并且超时时间是一分钟，那么对于单个地址它有 15 秒的时间来完成链接，如果超时就会尝试下一个。

Dial 函数介绍关于 network 和 address 参数。

<h2 id="Error">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L366">Error</a>
    <a href="#Error">¶</a></h2>
<pre>type Error interface {
    <a href="/builtin/#error">error</a>
    Timeout() <a href="/builtin/#bool">bool</a>   <span class="comment">// Is the error a timeout?</span>
    Temporary() <a href="/builtin/#bool">bool</a> <span class="comment">// Is the error temporary?</span>
}</pre>

一个代表网络错误的 Error。

<h2 id="Flags">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L28">Flags</a>
    <a href="#Flags">¶</a></h2>
<pre>type Flags <a href="/builtin/#uint">uint</a></pre>


<pre>const (
    <span id="FlagUp">FlagUp</span>           <a href="#Flags">Flags</a> = 1 &lt;&lt; <a href="/builtin/#iota">iota</a> <span class="comment">// interface 已经启动</span>
    <span id="FlagBroadcast">FlagBroadcast</span>                      <span class="comment">// interface 支持广播。</span>
    <span id="FlagLoopback">FlagLoopback</span>                       <span class="comment">// interface 是环回接口。</span>
    <span id="FlagPointToPoint">FlagPointToPoint</span>                   <span class="comment">// interface 属于点对点链接。</span>
    <span id="FlagMulticast">FlagMulticast</span>                      <span class="comment">// interface 支持多播。</span>
)</pre>


<h3 id="Flags.String">func (Flags) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L46">String</a>
    <a href="#Flags.String">¶</a></h3>
<pre>func (f <a href="#Flags">Flags</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="HardwareAddr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/mac.go#L1">HardwareAddr</a>
    <a href="#HardwareAddr">¶</a></h2>
<pre>type HardwareAddr []<a href="/builtin/#byte">byte</a></pre>

HardwareAddr 代表一个物理硬件地址。

<h3 id="ParseMAC">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/mac.go#L28">ParseMAC</a>
    <a href="#ParseMAC">¶</a></h3>
<pre>func ParseMAC(s <a href="/builtin/#string">string</a>) (hw <a href="#HardwareAddr">HardwareAddr</a>, err <a href="/builtin/#error">error</a>)</pre>

ParseMAC 会依据以下格式把 s 解析成 IEEE 802 MAC-48, EUI-48, EUI-64, 或 a 20-octet IP 无限带宽链路层地址：

    01:23:45:67:89:ab
    01:23:45:67:89:ab:cd:ef
    01:23:45:67:89:ab:cd:ef:00:00:01:23:45:67:89:ab:cd:ef:00:00
    01-23-45-67-89-ab
    01-23-45-67-89-ab-cd-ef
    01-23-45-67-89-ab-cd-ef-00-00-01-23-45-67-89-ab-cd-ef-00-00
    0123.4567.89ab
    0123.4567.89ab.cdef
    0123.4567.89ab.cdef.0000.0123.4567.89ab.cdef.0000

<h3 id="HardwareAddr.String">func (HardwareAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/mac.go#L2">String</a>
    <a href="#HardwareAddr.String">¶</a></h3>
<pre>func (a <a href="#HardwareAddr">HardwareAddr</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="IP">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L22">IP</a>
    <a href="#IP">¶</a></h2>
<pre>type IP []<a href="/builtin/#byte">byte</a></pre>

IP 代表一个 IP 地址，它是一个字节切片。在这个包中的函数接受 4字节（IPv4）或 16 字节（IPv6）切片作为输入。

在这个文档中，将 IP 地址称为 IPv4 地址或 IPv6 地址是地址的语义属性，不仅仅是切片的长度，长度为 16 的字节切片也有可能是 IPv4 地址。

<h3 id="IPv4">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L35">IPv4</a>
    <a href="#IPv4">¶</a></h3>
<pre>func IPv4(a, b, c, d <a href="/builtin/#byte">byte</a>) <a href="#IP">IP</a></pre>

IPv4 返回 IPv4 地址 a.b.c.d 的 IP 地址实例（16 字节格式）。

<a id="exampleIPv4"></a>
例:

    fmt.Println(net.IPv4(8, 8, 8, 8))
    
    // Output:
    // 8.8.8.8

<h3 id="ParseCIDR">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L657">ParseCIDR</a>
    <a href="#ParseCIDR">¶</a></h3>
<pre>func ParseCIDR(s <a href="/builtin/#string">string</a>) (<a href="#IP">IP</a>, *<a href="#IPNet">IPNet</a>, <a href="/builtin/#error">error</a>)</pre>

ParseCIDR 把 s 转换成 CIDR 表示法（ RFC 4632 和 RFC 4291 中定义的 `192.0.2.0/24` 或 `2001:db8::/32`）的 IP 地址和前缀长度。

它会返回 IP 地址和通过前缀长度和 IP 隐含的网络。

例如：ParseCIDR("192.0.2.1/24") 返回 IP 地址 192.0.2.1 和网络 192.0.2.0/24。

<a id="exampleParseCIDR"></a>
例:

    ipv4Addr, ipv4Net, err := net.ParseCIDR("192.0.2.1/24")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(ipv4Addr)
    fmt.Println(ipv4Net)
    
    ipv6Addr, ipv6Net, err := net.ParseCIDR("2001:db8:a0b:12f0::1/32")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(ipv6Addr)
    fmt.Println(ipv6Net)
    
    // Output:
    // 192.0.2.1
    // 192.0.2.0/24
    // 2001:db8:a0b:12f0::1
    // 2001:db8::/32

<h3 id="ParseIP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L636">ParseIP</a>
    <a href="#ParseIP">¶</a></h3>
<pre>func ParseIP(s <a href="/builtin/#string">string</a>) <a href="#IP">IP</a></pre>

ParseIP 将 s 作为一个 IP 地址并返回结果。字符串 s 可以为点小数格式 （`192.0.2.1`） 或 IPv6 格式（`2001:db8::68`）。如果 s 不是有效的 IP 地址字面值会返回 nil。

<a id="exampleParseIP"></a>
例:

    fmt.Println(net.ParseIP("192.0.2.1"))
    fmt.Println(net.ParseIP("2001:db8::68"))
    fmt.Println(net.ParseIP("192.0.2"))
    
    // Output:
    // 192.0.2.1
    // 2001:db8::68
    // <nil>

<h3 id="IP.DefaultMask">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L211">DefaultMask</a>
    <a href="#IP.DefaultMask">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) DefaultMask() <a href="#IPMask">IPMask</a></pre>

DefaultMask 返回 ip 的默认 IP 掩码。只有 IPv4 地址有默认的掩码；DefaultMask 在 ip 不是有效 IPv4 地址时返回 nil。

<a id="exampleIP_DefaultMask"></a>
例:

    ip := net.ParseIP("192.0.2.1")
    fmt.Println(ip.DefaultMask())
    
    // Output:
    // ffffff00

<h3 id="IP.Equal">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L365">Equal</a>
    <a href="#IP.Equal">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) Equal(x <a href="#IP">IP</a>) <a href="/builtin/#bool">bool</a></pre>

Equal 判断 ip 和 s 是不是相同的 IP 地址。IPv4 地址和 IPv6 格式的相同地址也会被认为是相等的。

<h3 id="IP.IsGlobalUnicast">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L155">IsGlobalUnicast</a>
    <a href="#IP.IsGlobalUnicast">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsGlobalUnicast() <a href="/builtin/#bool">bool</a></pre>

IsGlobalUnicast 判断 ip 是不是全局单播地址。

全局单播地址标识使用 RFC 1122, RFC 4632 和 RFC 4291 中定义的地址类型标识符（除了 IPv4 定向广播地址）。如果 ip 在 IPv4 地址私有空间或本地 IPv6 广播地址也会返回 true。

<h3 id="IP.IsInterfaceLocalMulticast">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L125">IsInterfaceLocalMulticast</a>
    <a href="#IP.IsInterfaceLocalMulticast">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsInterfaceLocalMulticast() <a href="/builtin/#bool">bool</a></pre>

IsInterfaceLocalMulticast 判断 ip 是不是本地多播地址。

<h3 id="IP.IsLinkLocalMulticast">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L131">IsLinkLocalMulticast</a>
    <a href="#IP.IsLinkLocalMulticast">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsLinkLocalMulticast() <a href="/builtin/#bool">bool</a></pre>

IsLinkLocalMulticast 判断 ip 是不是本地链路多播地址。

<h3 id="IP.IsLinkLocalUnicast">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L140">IsLinkLocalUnicast</a>
    <a href="#IP.IsLinkLocalUnicast">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsLinkLocalUnicast() <a href="/builtin/#bool">bool</a></pre>

IsLinkLocalUnicast 判断 ip 是不是本地链路单播地址。

<h3 id="IP.IsLoopback">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L108">IsLoopback</a>
    <a href="#IP.IsLoopback">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsLoopback() <a href="/builtin/#bool">bool</a></pre>

IsLoopback 判断 ip 是不是环回地址。

<h3 id="IP.IsMulticast">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L116">IsMulticast</a>
    <a href="#IP.IsMulticast">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsMulticast() <a href="/builtin/#bool">bool</a></pre>

IsMulticast 判断 ip 是不是多播地址。

<h3 id="IP.IsUnspecified">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L103">IsUnspecified</a>
    <a href="#IP.IsUnspecified">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsUnspecified() <a href="/builtin/#bool">bool</a></pre>

IsUnspecified 判断 ip是否为未指定的地址（IPv4 中的 `0.0.0.0` 或 IPv6 中的 `::`）。

<h3 id="IP.MarshalText">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L336">MarshalText</a>
    <a href="#IP.MarshalText">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) MarshalText() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

MarshalText 实现了 encoding.TextMarshaler 接口。该方法和 String 方法返回相同值，只有在 `len(ip)` 为零时它返回一个空切片。

<h3 id="IP.Mask">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L235">Mask</a>
    <a href="#IP.Mask">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) Mask(mask <a href="#IPMask">IPMask</a>) <a href="#IP">IP</a></pre>

Mask 返回 ip 经过掩码处理后的结果。

<a id="exampleIP_Mask"></a>
例:

    ipv4Addr := net.ParseIP("192.0.2.1")
    // This mask corresponds to a /24 subnet for IPv4.
    ipv4Mask := net.CIDRMask(24, 32)
    fmt.Println(ipv4Addr.Mask(ipv4Mask))
    
    ipv6Addr := net.ParseIP("2001:db8:a0b:12f0::1")
    // This mask corresponds to a /32 subnet for IPv6.
    ipv6Mask := net.CIDRMask(32, 128)
    fmt.Println(ipv6Addr.Mask(ipv6Mask))
    
    // Output:
    // 192.0.2.0
    // 2001:db8::

<h3 id="IP.String">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L259">String</a>
    <a href="#IP.String">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) String() <a href="/builtin/#string">string</a></pre>

返回 ip 的字符串形式。它返回以下 4 种格式之一：

    - "<nil>", 如果 ip 长度为 0
    - dotted decimal ("192.0.2.1"), 如果 ip 是 IPv4 地址或 IPv4 对应的 IPv6 地址
    - IPv6 ("2001:db8::1"), 如果 ip 是有效的 IPv6 地址
    - 如果不是之前 3 种格式，会返回没有标点符号的十六进制格式的 ip

<h3 id="IP.To16">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L191">To16</a>
    <a href="#IP.To16">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) To16() <a href="#IP">IP</a></pre>

To16 将 IP 地址 ip 转换成 16 字节的表现形式。如果 ip 不是 IP 地址（非法长度），To16 返回 nil。

<h3 id="IP.To4">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L176">To4</a>
    <a href="#IP.To4">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) To4() <a href="#IP">IP</a></pre>

To4 将 IPv4 地址  ip 转换成 4 字节的表现形式。如果 ip 不是 IPv4 地址，To4 返回 nil。

<h3 id="IP.UnmarshalText">func (*IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L348">UnmarshalText</a>
    <a href="#IP.UnmarshalText">¶</a></h3>
<pre>func (ip *<a href="#IP">IP</a>) UnmarshalText(text []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

UnmarshalText 实现了 encoding.TextUnmarshaler 接口。IP 地址必须是 ParseIP 接受的格式。

<h2 id="IPAddr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L21">IPAddr</a>
    <a href="#IPAddr">¶</a></h2>
<pre>type IPAddr struct {
<span id="IPAddr.IP"></span>    IP   <a href="#IP">IP</a>
<span id="IPAddr.Zone"></span>    Zone <a href="/builtin/#string">string</a> <span class="comment">// IPv6 scoped addressing zone</span>
}</pre>

IPAddr 代表 IP 终端所在的地址。

<h3 id="ResolveIPAddr">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L67">ResolveIPAddr</a>
    <a href="#ResolveIPAddr">¶</a></h3>
<pre>func ResolveIPAddr(network, address <a href="/builtin/#string">string</a>) (*<a href="#IPAddr">IPAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ResolveIPAddr 返回指定网络 IP 终端地址。

network 必须是 IP 网络的名称。

如果 address 中的主机名不是一个 IP 地址，ResolveIPAddr 会解析该地址或得对应的 IP 地址。否则会将其转换成 IP 地址。address 参数可以使用主机名，但是不推荐这么做，因为它只会返回最多一个 IP 地址。

Dial 函数介绍关于 network 和 address 参数。

<h3 id="IPAddr.Network">func (*IPAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L27">Network</a>
    <a href="#IPAddr.Network">¶</a></h3>
<pre>func (a *<a href="#IPAddr">IPAddr</a>) Network() <a href="/builtin/#string">string</a></pre>

Network 返回地址的网络名，`ip`。

<h3 id="IPAddr.String">func (*IPAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L29">String</a>
    <a href="#IPAddr.String">¶</a></h3>
<pre>func (a *<a href="#IPAddr">IPAddr</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="IPConn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L89">IPConn</a>
    <a href="#IPConn">¶</a></h2>
<pre>type IPConn struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

IPConn 是 IP 网络链接对接口 Conn 和 PacketConn 的实现。

<h3 id="DialIP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L201">DialIP</a>
    <a href="#DialIP">¶</a></h3>
<pre>func DialIP(network <a href="/builtin/#string">string</a>, laddr, raddr *<a href="#IPAddr">IPAddr</a>) (*<a href="#IPConn">IPConn</a>, <a href="/builtin/#error">error</a>)</pre>

DialIP 和使用 IP 网络的 Dial 相似。

network 必须是 IP 网络名称，Dial 文档有详细说明。

如果 laddr 为 nil，会自动选择本地地址。如果 raddr 的 IP 字段为 nil 或者不确定的 IP 地址，会默认使用本地网络。

<h3 id="ListenIP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L216">ListenIP</a>
    <a href="#ListenIP">¶</a></h3>
<pre>func ListenIP(network <a href="/builtin/#string">string</a>, laddr *<a href="#IPAddr">IPAddr</a>) (*<a href="#IPConn">IPConn</a>, <a href="/builtin/#error">error</a>)</pre>

ListenIP 和使用 IP 网络的 ListenPacket 相似。

network 必须是 IP 网络名称；Dial 文档有详细说明。

如果 laddr 的 IP 为 nil 或者 不确定的 IP 地址，ListenIP 会监听本地所有除了多播的 IP 地址。

<h3 id="IPConn.Close">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L186">Close</a>
    <a href="#IPConn.Close">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close 关闭链接。

<h3 id="IPConn.File">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L283">File</a>
    <a href="#IPConn.File">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) File() (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

File 设置底层 os.File 为阻塞模式并返回一个拷贝。由调用者负责执行完成后关闭 f。关闭 c 不会影响 f，关闭 f 也不会影响 c。

返回的 os.File 文件描述符和链接是不一样的。改变链接的属性可能不会达到预期效果。

在 Unix 系统中这会导致 SetDeadline 中止执行。

<h3 id="IPConn.LocalAddr">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L200">LocalAddr</a>
    <a href="#IPConn.LocalAddr">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) LocalAddr() <a href="#Addr">Addr</a></pre>

LocalAddr 返回本地网络地址。Addr 是所有调用者所共享的，所以不要改变它。

<h3 id="IPConn.Read">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L162">Read</a>
    <a href="#IPConn.Read">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) Read(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Read 实现了 Conn 的 Read 方法。

<h3 id="IPConn.ReadFrom">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L115">ReadFrom</a>
    <a href="#IPConn.ReadFrom">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) ReadFrom(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="#Addr">Addr</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFrom 实现了 PacketConn 的 ReadFrom 方法。

<h3 id="IPConn.ReadFromIP">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L103">ReadFromIP</a>
    <a href="#IPConn.ReadFromIP">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) ReadFromIP(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, *<a href="#IPAddr">IPAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFromIP 和 ReadFrom 相似只是返回 IPAddr。

<h3 id="IPConn.ReadMsgIP">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L136">ReadMsgIP</a>
    <a href="#IPConn.ReadMsgIP">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) ReadMsgIP(b, oob []<a href="/builtin/#byte">byte</a>) (n, oobn, flags <a href="/builtin/#int">int</a>, addr *<a href="#IPAddr">IPAddr</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadMsgIP 读取 c 中的信息，将载荷拷贝进 b 并且将带外数据复制到 oob。返回拷贝进 b 的字节长度，拷贝进 oob 的字节长度，信息中的参数和信息的源地址。

golang.org/x/net/ipv4 和  golang.org/x/net/ipv6 包可以用来操作 oob 中 IP 层的套接字选项。

<h3 id="IPConn.RemoteAddr">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L210">RemoteAddr</a>
    <a href="#IPConn.RemoteAddr">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) RemoteAddr() <a href="#Addr">Addr</a></pre>

RemoteAddr 返回远程网络地址。Addr 是所有调用者所共享，所以不要改变它。

<h3 id="IPConn.SetDeadline">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L218">SetDeadline</a>
    <a href="#IPConn.SetDeadline">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline 实现了 Conn 的 SetDeadline 方法。

<h3 id="IPConn.SetReadBuffer">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L252">SetReadBuffer</a>
    <a href="#IPConn.SetReadBuffer">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) SetReadBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetReadBuffer 设置链接对应的操作系统的接收缓存大小。

<h3 id="IPConn.SetReadDeadline">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L229">SetReadDeadline</a>
    <a href="#IPConn.SetReadDeadline">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetReadDeadline 实现了 Conn 的 SetReadDeadline 方法。

<h3 id="IPConn.SetWriteBuffer">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L264">SetWriteBuffer</a>
    <a href="#IPConn.SetWriteBuffer">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) SetWriteBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetWriteBuffer 设置链接对应的操作系统的发送缓存大小。

<h3 id="IPConn.SetWriteDeadline">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L240">SetWriteDeadline</a>
    <a href="#IPConn.SetWriteDeadline">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetWriteDeadline 实现了 Conn 接口的 SetWriteDeadline 方法。

<h3 id="IPConn.SyscallConn">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L95">SyscallConn</a>
    <a href="#IPConn.SyscallConn">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) SyscallConn() (<a href="/syscall/">syscall</a>.<a href="/syscall/#RawConn">RawConn</a>, <a href="/builtin/#error">error</a>)</pre>

SyscallConn 返回原生的网络链接。它实现了 syscall.Conn 接口。

<h3 id="IPConn.Write">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L174">Write</a>
    <a href="#IPConn.Write">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) Write(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write 实现了 Conn 接口的 Write 方法。

<h3 id="IPConn.WriteMsgIP">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L181">WriteMsgIP</a>
    <a href="#IPConn.WriteMsgIP">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) WriteMsgIP(b, oob []<a href="/builtin/#byte">byte</a>, addr *<a href="#IPAddr">IPAddr</a>) (n, oobn <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteMsgIP 通过 c 向 addr 写入消息，从 b 中拷贝载荷 ，从 oob 拷贝带外数据。它返回写入的载荷和带外数据的字节数。

golang.org/x/net/ipv4 和 golang.org/x/net/ipv6 包可以用来操作 oob 中 IP 层的套接字选项。

<h3 id="IPConn.WriteTo">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L160">WriteTo</a>
    <a href="#IPConn.WriteTo">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) WriteTo(b []<a href="/builtin/#byte">byte</a>, addr <a href="#Addr">Addr</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteTo 实现了 PacketConn 接口的 WriteTo 方法。

<h3 id="IPConn.WriteToIP">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L148">WriteToIP</a>
    <a href="#IPConn.WriteToIP">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) WriteToIP(b []<a href="/builtin/#byte">byte</a>, addr *<a href="#IPAddr">IPAddr</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteToIP 和 WriteTo 相似只是需要提供一个 IPAddr。

<h2 id="IPMask">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L25">IPMask</a>
    <a href="#IPMask">¶</a></h2>
<pre>type IPMask []<a href="/builtin/#byte">byte</a></pre>

IP 掩码是一个 IP 地址。

<h3 id="CIDRMask">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L61">CIDRMask</a>
    <a href="#CIDRMask">¶</a></h3>
<pre>func CIDRMask(ones, bits <a href="/builtin/#int">int</a>) <a href="#IPMask">IPMask</a></pre>

CIDRMask 返回一个 IPMask，由 ones 个 1 后跟 bits 个 0 组成。对于这个格式的掩码，CIDRMask 与 IPMask.Size 相反。

<a id="exampleCIDRMask"></a>
例:

    // This mask corresponds to a /31 subnet for IPv4.
    fmt.Println(net.CIDRMask(31, 32))
    
    // This mask corresponds to a /64 subnet for IPv6.
    fmt.Println(net.CIDRMask(64, 128))
    
    // Output:
    // fffffffe
    // ffffffffffffffff0000000000000000

<h3 id="IPv4Mask">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L49">IPv4Mask</a>
    <a href="#IPv4Mask">¶</a></h3>
<pre>func IPv4Mask(a, b, c, d <a href="/builtin/#byte">byte</a>) <a href="#IPMask">IPMask</a></pre>

IPv4Mask 返回 IPv4 掩码 `a.b.c.d`（4 字节格式）。

<a id="exampleIPv4Mask"></a>
例:

    fmt.Println(net.IPv4Mask(255, 255, 255, 0))
    
    // Output:
    // ffffff00

<h3 id="IPMask.Size">func (IPMask) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L418">Size</a>
    <a href="#IPMask.Size">¶</a></h3>
<pre>func (m <a href="#IPMask">IPMask</a>) Size() (ones, bits <a href="/builtin/#int">int</a>)</pre>

Size 返回前开头的数字和掩码总的比特数。如果掩码不是规范格式（1 后面跟 0），Size 返回 0，0。

<h3 id="IPMask.String">func (IPMask) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L427">String</a>
    <a href="#IPMask.String">¶</a></h3>
<pre>func (m <a href="#IPMask">IPMask</a>) String() <a href="/builtin/#string">string</a></pre>

String 返回 m 无标点符号的十六进制格式字符串。

<h2 id="IPNet">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L28">IPNet</a>
    <a href="#IPNet">¶</a></h2>
<pre>type IPNet struct {
<span id="IPNet.IP"></span>    IP   <a href="#IP">IP</a>     <span class="comment">// network number</span>
<span id="IPNet.Mask"></span>    Mask <a href="#IPMask">IPMask</a> <span class="comment">// network mask</span>
}</pre>

IPNet 代表一个 IP 网络。

<h3 id="IPNet.Contains">func (*IPNet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L458">Contains</a>
    <a href="#IPNet.Contains">¶</a></h3>
<pre>func (n *<a href="#IPNet">IPNet</a>) Contains(ip <a href="#IP">IP</a>) <a href="/builtin/#bool">bool</a></pre>

Contains 判断网络中是否包含 ip。

<h3 id="IPNet.Network">func (*IPNet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L476">Network</a>
    <a href="#IPNet.Network">¶</a></h3>
<pre>func (n *<a href="#IPNet">IPNet</a>) Network() <a href="/builtin/#string">string</a></pre>

Network 返回地址的网络名称。

<h3 id="IPNet.String">func (*IPNet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L484">String</a>
    <a href="#IPNet.String">¶</a></h3>
<pre>func (n *<a href="#IPNet">IPNet</a>) String() <a href="/builtin/#string">string</a></pre>

String 返回 n 的 CIDR 标记，像 RFC 4632 和 RFC 4291 定义的 `192.0.2.1/24` 或 `2001:db8::/48`。如果掩码不是规范格式，它会作为 IP 地址后跟下划线和十六进制的无标点符号掩码返回（例：`198.51.100.1/c000ff00`）。

<h2 id="Interface">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L20">Interface</a>
    <a href="#Interface">¶</a></h2>
<pre>type Interface struct {
<span id="Interface.Index"></span>    Index        <a href="/builtin/#int">int</a>          <span class="comment">// 从 1 开始的正整数永远不为 0。</span>

<span id="Interface.MTU"></span>    MTU          <a href="/builtin/#int">int</a>          <span class="comment">// 最大传输单位。</span>
<span id="Interface.Name"></span>    Name         <a href="/builtin/#string">string</a>       <span class="comment">// e.g., &#34;en0&#34;, &#34;lo0&#34;, &#34;eth0.100&#34;</span>
<span id="Interface.HardwareAddr"></span>    HardwareAddr <a href="#HardwareAddr">HardwareAddr</a> <span class="comment">// IEEE MAC-48, EUI-48 and EUI-64 格式。</span>
<span id="Interface.Flags"></span>    Flags        <a href="#Flags">Flags</a>        <span class="comment">// e.g., FlagUp, FlagLoopback, FlagMulticast</span>
}</pre>

Interface 代表网络接口名和索引对照表，他也代表网络接口的设施信息。

<h3 id="InterfaceByIndex">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L118">InterfaceByIndex</a>
    <a href="#InterfaceByIndex">¶</a></h3>
<pre>func InterfaceByIndex(index <a href="/builtin/#int">int</a>) (*<a href="#Interface">Interface</a>, <a href="/builtin/#error">error</a>)</pre>

InterfaceByIndex 返回指定索引对应的网络接口。

在 Solaris 平台，它会返回共享逻辑数据链接的逻辑网络接口之一；如果想要更精确可以使用 InterfaceByName。

<h3 id="InterfaceByName">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L143">InterfaceByName</a>
    <a href="#InterfaceByName">¶</a></h3>
<pre>func InterfaceByName(name <a href="/builtin/#string">string</a>) (*<a href="#Interface">Interface</a>, <a href="/builtin/#error">error</a>)</pre>

InterfaceByName 返回指定名称的网络接口。

<h3 id="Interface.Addrs">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L64">Addrs</a>
    <a href="#Interface.Addrs">¶</a></h3>
<pre>func (ifi *<a href="#Interface">Interface</a>) Addrs() ([]<a href="#Addr">Addr</a>, <a href="/builtin/#error">error</a>)</pre>

Addrs 返回指定网络接口的单播网络地址列表。

<h3 id="Interface.MulticastAddrs">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L77">MulticastAddrs</a>
    <a href="#Interface.MulticastAddrs">¶</a></h3>
<pre>func (ifi *<a href="#Interface">Interface</a>) MulticastAddrs() ([]<a href="#Addr">Addr</a>, <a href="/builtin/#error">error</a>)</pre>

MulticastAddrs 返回指定网络接口的多播网络地址列表。

<h2 id="InvalidAddrError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L527">InvalidAddrError</a>
    <a href="#InvalidAddrError">¶</a></h2>
<pre>type InvalidAddrError <a href="/builtin/#string">string</a></pre>


<h3 id="InvalidAddrError.Error">func (InvalidAddrError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L529">Error</a>
    <a href="#InvalidAddrError.Error">¶</a></h3>
<pre>func (e <a href="#InvalidAddrError">InvalidAddrError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="InvalidAddrError.Temporary">func (InvalidAddrError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L531">Temporary</a>
    <a href="#InvalidAddrError.Temporary">¶</a></h3>
<pre>func (e <a href="#InvalidAddrError">InvalidAddrError</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>


<h3 id="InvalidAddrError.Timeout">func (InvalidAddrError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L530">Timeout</a>
    <a href="#InvalidAddrError.Timeout">¶</a></h3>
<pre>func (e <a href="#InvalidAddrError">InvalidAddrError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>


<h2 id="Listener">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L353">Listener</a>
    <a href="#Listener">¶</a></h2>
<pre>type Listener interface {
    <span class="comment">// Accept 等待并返回下一个链接。</span>
    Accept() (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)
    
    <span class="comment">// Close 关闭 Listener</span>
    <span class="comment">// 任何阻塞的接受操作都会解除阻塞并返回错误。</span>
    Close() <a href="/builtin/#error">error</a>
    
    <span class="comment">// Addr 返回 Listener 的网络地址。</span>
    Addr() <a href="#Addr">Addr</a>
}</pre>

Listener 是一个面向流协议的通用网络监听器。

Listener 的方法可以在多个 goroutine 中同时调用。

<a id="exampleListener"></a>
例:

    // 监听所有有效单播地址和本地系统的 IP 地址的 2000 端口。
    l, err := net.Listen("tcp", ":2000")
    if err != nil {
        log.Fatal(err)
    }
    defer l.Close()
    for {
        // 等待链接
        conn, err := l.Accept()
        if err != nil {
            log.Fatal(err)
        }
        // 在新 goroutine 中处理链接
        // 循环继续等待新链接， 这样就可以并发处理请求
        go func(c net.Conn) {
            // 打印收到的数据
            io.Copy(c, c)
            // 关闭链接
            c.Close()
        }(conn)
    }

<h3 id="FileListener">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/file.go#L23">FileListener</a>
    <a href="#FileListener">¶</a></h3>
<pre>func FileListener(f *<a href="/os/">os</a>.<a href="/os/#File">File</a>) (ln <a href="#Listener">Listener</a>, err <a href="/builtin/#error">error</a>)</pre>

FileListener 返回文件 f 对应的网络监听器拷贝。由调用者负责在使用完后关闭它。关闭 In 不会影响 f，关闭 f 不会影响 In。

<h3 id="Listen">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L574">Listen</a>
    <a href="#Listen">¶</a></h3>
<pre>func Listen(network, address <a href="/builtin/#string">string</a>) (<a href="#Listener">Listener</a>, <a href="/builtin/#error">error</a>)</pre>

Listen 通知本地网络地址。

network 必须是 `tcp`，`tcp4`，`tcp6`，`unix` 或 `unixpacket`。

对于 TCP 网络，如果 address 参数的主机名为空或不确定的 IP 地址，Listen 监听所有有效的单播地址和本地系统的

 IP 地址。使用 `tcp4` 就会只使用 IPv4。地址可以使用主机名，但不推荐这么做，因为这样可能会同时监听多个 IP 地址。

如果 address 参数的端口部分是空或者 0，就像 `127.0.0.1:` 或 `[::1]:0`，会自动选者端口号。Listener 的 Addr 方法可以用作发现指定端口。

network 和 address 参数请看 Dial 函数的描述。

<h2 id="MX">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsclient.go#L239">MX</a>
    <a href="#MX">¶</a></h2>
<pre>type MX struct {
<span id="MX.Host"></span>    Host <a href="/builtin/#string">string</a>
<span id="MX.Pref"></span>    Pref <a href="/builtin/#uint16">uint16</a>
}</pre>

MX 代表一条 DNS MX 记录。

<h2 id="NS">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsclient.go#L261">NS</a>
    <a href="#NS">¶</a></h2>
<pre>type NS struct {
<span id="NS.Host"></span>    Host <a href="/builtin/#string">string</a>
}</pre>

NS 代表一条 DNS NS 记录。

<h2 id="OpError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L404">OpError</a>
    <a href="#OpError">¶</a></h2>
<pre>type OpError struct {
<span id="OpError.Op"></span>    <span class="comment">// Op 代表导致错误的操作，例如： `read` 或 `write`。</span>
    Op <a href="/builtin/#string">string</a>
    

<span id="OpError.Net"></span>    <span class="comment">// Net 发生错误的网络类型，例如：`tcp` 或 `udp6`。</span>
    Net <a href="/builtin/#string">string</a>
    
    <span class="comment">// 对于调用远程网络链接的操作，像：Dial，Read 或 Write，Source 是对应的本地网络地址。</span>
<span id="OpError.Source"></span>    Source <a href="#Addr">Addr</a>

<span id="OpError.Addr"></span>    <span class="comment">// Addr 是导致该错误的网络地址。</span>
    <span class="comment">// 对于本地操作，像 Listen 或 SetDeadline，Addr 是被操作的本地地址。</span>
    <span class="comment">// 对于调用远程网络链接的操作，像 Dial，Read 或 Write，Addr 是远程链接的地址。</span>
    Addr <a href="#Addr">Addr</a>

<span id="OpError.Err"></span>    <span class="comment">// Err 是操作中发生的错误。</span>
    Err <a href="/builtin/#error">error</a>
}</pre>

OpError 是 net 包经常返回的错误类型。它描述产生错误时的操作，网络类型和地址。

<h3 id="OpError.Error">func (*OpError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L430">Error</a>
    <a href="#OpError.Error">¶</a></h3>
<pre>func (e *<a href="#OpError">OpError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="OpError.Temporary">func (*OpError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L481">Temporary</a>
    <a href="#OpError.Temporary">¶</a></h3>
<pre>func (e *<a href="#OpError">OpError</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>


<h3 id="OpError.Timeout">func (*OpError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L468">Timeout</a>
    <a href="#OpError.Timeout">¶</a></h3>
<pre>func (e *<a href="#OpError">OpError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>


<h2 id="PacketConn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L294">PacketConn</a>
    <a href="#PacketConn">¶</a></h2>
<pre>type PacketConn interface {
    <span class="comment">// ReadFrom 从链接读取一个包，将载荷拷贝进 b。它返回拷贝进 b 的字节数和包的网络地址。</span>
    <span class="comment">// ReadFrom 可以设置超时，在固定时限后返回 Timeout() == true 的错误。</span>
    <span class="comment">// 具体请看 SetDeadline 和 SetReadDeadline。</span>
    ReadFrom(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, addr <a href="#Addr">Addr</a>, err <a href="/builtin/#error">error</a>)
    
    <span class="comment">// WriteTo 将 b 中数据写入 addr。</span>
    <span class="comment">// WriteTo 可以设置超时，在固定时限后返回 Timeout() == true 的错误。</span>
    <span class="comment">// 具体请看 SetDeadline 和 SetWriteDeadline。</span>
    <span class="comment">// 在面向数据包的链接中，写入超时很少见。</span>
    WriteTo(b []<a href="/builtin/#byte">byte</a>, addr <a href="#Addr">Addr</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)
    
    <span class="comment">// Close 关闭链接。</span>
    <span class="comment">// 任何阻塞的 ReadFrom 和 WriteTo 将会返回错误。</span>
    Close() <a href="/builtin/#error">error</a>
    
    <span class="comment">// LocalAddr 返回本地网络地址。</span>
    LocalAddr() <a href="#Addr">Addr</a>
    
    <span class="comment">// SetDeadline 会设置对应链接的 read 和 write 的截止时间。它和同时调用 SetReadDeadline 和 SetWriteDeadline 是一样的。</span>
    <span class="comment">//</span>
    <span class="comment">// 截止时间是 I/O 操作取消阻塞失败后的绝对时间。截止时间对当前阻塞和之后的 I/O 操作生效，而不仅仅是下次调用 ReadFrom 和 WriteTo。在触发截止时间之后，链接的截止时间会刷新。</span>
    <span class="comment">//</span>
    <span class="comment">// 在 ReadFrom 或 WriteTo 成功调用之后会立即延长截止时间。</span>
    <span class="comment">//</span>
    <span class="comment">// t 的零值表示 I/O 操作不会超时。</span>
    SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a>
    
    <span class="comment">// SetReadDeadline 设置当前阻塞和之后 ReadFrom 调用的截止时间。</span>
    <span class="comment">// t 为零值表示 ReadFrom 不会超时。</span>
    SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a>
    
    <span class="comment">// SetWriteDeadline 设置当前阻塞和之后 WriteTo 调用的截止时间。</span>
    <span class="comment">// 即使写入超时, 也可能返回 n > 0，代表已经成功写入的数据。</span>
    <span class="comment">// t 的零值表示 WriteTo 不会超时。</span>
    SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a>
}</pre>

PacketConn 是面向包的通用网络链接。

PacketConn 的方法可以在多个 goroutine 中同时调用。

<h3 id="FilePacketConn">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/file.go#L35">FilePacketConn</a>
    <a href="#FilePacketConn">¶</a></h3>
<pre>func FilePacketConn(f *<a href="/os/">os</a>.<a href="/os/#File">File</a>) (c <a href="#PacketConn">PacketConn</a>, err <a href="/builtin/#error">error</a>)</pre>

FilePacketConn 返回对应文件 f 的包网络链接拷贝。由调用者负责在使用完成后关闭 f。关闭 c 不会影响 f，关闭 f 不会影响 c。

<h3 id="ListenPacket">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L616">ListenPacket</a>
    <a href="#ListenPacket">¶</a></h3>
<pre>func ListenPacket(network, address <a href="/builtin/#string">string</a>) (<a href="#PacketConn">PacketConn</a>, <a href="/builtin/#error">error</a>)</pre>

ListenPacket 在本地网络地址监听。

network 必须是 `udp`，`udp4`，`udp6`，`unixgram` 或一个 IP 传输。IP 传输为 `ip`，`ip4` 或 `ip6` 紧跟着冒号和协议数字或协议名称，就像 `ip:1` 或 `ip:icmp`。

对于 UDP 和 IP 网络，address 参数的主机名为空或者不确定地址时，ListenPacket 监听本地系统除了多播的所有有效的 IP 地址。使用 `udp4` 或 `ip4:proto` 会只用 IPv4 地址。address 可以使用主机名，但不推荐这么做，因为它有可能同时监听多个主机 IP 地址。如果 address 中的端口号为空或者为 0，就像 `127.0.0.1:` 或 `[::1]:0`，会自动选者端口号。PacketConn 的 LocalAddr 方法可以用来发现指定端口。

关于 network 和 address 参数的描述请看 Dial 函数文档。

<h2 id="ParseError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L491">ParseError</a>
    <a href="#ParseError">¶</a></h2>
<pre>type ParseError struct {
<span id="ParseError.Type"></span>    <span class="comment">// Type 是希望的类型字符串例如 `IP address`，`CIDR address`。</span>
    Type <a href="/builtin/#string">string</a>

<span id="ParseError.Text"></span>    <span class="comment">// Text 是异常描述文本。</span>
    Text <a href="/builtin/#string">string</a>
}</pre>

ParseError 是网络地址解析的错误类型。

<h3 id="ParseError.Error">func (*ParseError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L500">Error</a>
    <a href="#ParseError.Error">¶</a></h3>
<pre>func (e *<a href="#ParseError">ParseError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Resolver">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L97">Resolver</a>
    <a href="#Resolver">¶</a></h2>
<pre>type Resolver struct {
<span id="Resolver.PreferGo"></span>    <span class="comment">// PreferGo 控制在平台上是否优先使用 Go 内置的 DNS 解析器。它和设置 GODEBUG=netdns=go 一样，不过只影响这个解析器。</span>

    PreferGo <a href="/builtin/#bool">bool</a>

<span id="Resolver.StrictErrors"></span>    <span class="comment">// StrictErrors 控制在使用 Go 内置解析器时产生的暂时性错误的行为（包括超时，套接字错误和 SERVFAIL）。</span>

    <span class="comment">// 对于一个由多个子查询组成的查询（例如：查找 A+AAAA 地址或遍历 DNS 搜索列表），这个选项引起的错误会中断查询而不是返回部分结果。它默认不启用因为它可能影响兼容性导致错误解析 AAAA 查询。</span>
    StrictErrors <a href="/builtin/#bool">bool</a>

<span id="Resolver.Dial"></span>    <span class="comment">// Dial 可选的指定 Go

 内置 DNS 解析器创建 TCP 和 UDP 链接连接 DNS 服务时调用的函数。</span>

    <span class="comment">// address 参数的主机名总是一个 IP 地址而不是主机名，并且 address 的端口号必须是数字而不是服务名称。</span>
    <span class="comment">// 如果返回的 Conn 也是一个 PacketConn, 那么发送和接收的 DNS 信息必须遵循 RFC 1035 第 4.2.1 章节，"UDP usage"。</span>
    <span class="comment">// 否则, DNS 信息的传输规则必须遵循 RFC 7766 第 5 章节，"Transport Protocol Selection"。</span>
    <span class="comment">// 如果为 nil，使用默认链接器。</span>
    Dial func(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, network, address <a href="/builtin/#string">string</a>) (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)
}</pre>

Resolver 查询名称和数字。

一个 nil *Resolver 等于 Resolver 的零值。

<h3 id="Resolver.LookupAddr">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L365">LookupAddr</a>
    <a href="#Resolver.LookupAddr">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupAddr(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, addr <a href="/builtin/#string">string</a>) (names []<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupAddr 对给定地址进行反向查找，返回映射到该地址的名称列表。

<h3 id="Resolver.LookupCNAME">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L294">LookupCNAME</a>
    <a href="#Resolver.LookupCNAME">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupCNAME(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, host <a href="/builtin/#string">string</a>) (cname <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupCNAME 返回给定主机规范名称。调用者不用关心这个可以直接调用 LookupHost 或 LookupIP的规范名称是什么；它们都是把解析权威名称作为查询的一部分。

规范名称是零或多条 CNAME 记录中的最后一个名称。LookupCNAME 会会在主机名不包含 DNS `CNAME` 记录时返回错误，而是直接解析主机名的地址记录。

<h3 id="Resolver.LookupHost">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L138">LookupHost</a>
    <a href="#Resolver.LookupHost">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupHost(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, host <a href="/builtin/#string">string</a>) (addrs []<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupHost 使用本地解析器查找给定主机名。它返回主机名对应的地址切片。

<h3 id="Resolver.LookupIPAddr">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L166">LookupIPAddr</a>
    <a href="#Resolver.LookupIPAddr">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupIPAddr(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, host <a href="/builtin/#string">string</a>) ([]<a href="#IPAddr">IPAddr</a>, <a href="/builtin/#error">error</a>)</pre>

LookupIPAddr 使用本地解析器查找主机名。它返回主机的 `IPv4` 和 `IPv6` 地址。

<h3 id="Resolver.LookupMX">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L330">LookupMX</a>
    <a href="#Resolver.LookupMX">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupMX(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, name <a href="/builtin/#string">string</a>) ([]*<a href="#MX">MX</a>, <a href="/builtin/#error">error</a>)</pre>

LookupMX 按优先级返回给定域名的 DNS MX 记录。

<h3 id="Resolver.LookupNS">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L340">LookupNS</a>
    <a href="#Resolver.LookupNS">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupNS(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, name <a href="/builtin/#string">string</a>) ([]*<a href="#NS">NS</a>, <a href="/builtin/#error">error</a>)</pre>

LookupNS 返回指定域名的 DNS NS 记录。

<h3 id="Resolver.LookupPort">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L256">LookupPort</a>
    <a href="#Resolver.LookupPort">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupPort(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, network, service <a href="/builtin/#string">string</a>) (port <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupPort 查找给定网络和服务的端口号。

<h3 id="Resolver.LookupSRV">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L320">LookupSRV</a>
    <a href="#Resolver.LookupSRV">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupSRV(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, service, proto, name <a href="/builtin/#string">string</a>) (cname <a href="/builtin/#string">string</a>, addrs []*<a href="#SRV">SRV</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupSRV 尝试解析给定服务，协议和域名的 SRV 查询。协议可以是 `tcp` 或 `udp`。返回的记录按优先级和权重排序。

LookupSRV 遵循 RFC 2782 构造 DNS 名字用做查询。也就是说它查找 service.proto.name。为了适应以非标准名称发布的 SRV 记录，如果服务和协议都为空字符串，LookupSRV 会直接查找名称。

<h3 id="Resolver.LookupTXT">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L350">LookupTXT</a>
    <a href="#Resolver.LookupTXT">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupTXT(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, name <a href="/builtin/#string">string</a>) ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

LookupTXT 返回给定域名的 DNS TXT 记录。 

<h2 id="SRV">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsclient.go#L185">SRV</a>
    <a href="#SRV">¶</a></h2>
<pre>type SRV struct {
<span id="SRV.Target"></span>    Target   <a href="/builtin/#string">string</a>
<span id="SRV.Port"></span>    Port     <a href="/builtin/#uint16">uint16</a>
<span id="SRV.Priority"></span>    Priority <a href="/builtin/#uint16">uint16</a>
<span id="SRV.Weight"></span>    Weight   <a href="/builtin/#uint16">uint16</a>
}</pre>

SRV 代表一条 DNS SRV 记录。

<h2 id="TCPAddr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L9">TCPAddr</a>
    <a href="#TCPAddr">¶</a></h2>
<pre>type TCPAddr struct {
<span id="TCPAddr.IP"></span>    IP   <a href="#IP">IP</a>
<span id="TCPAddr.Port"></span>    Port <a href="/builtin/#int">int</a>
<span id="TCPAddr.Zone"></span>    Zone <a href="/builtin/#string">string</a> <span class="comment">// IPv6 scoped addressing zone</span>
}</pre>

TCPAddr 代表 TCP 终端地址。

<h3 id="ResolveTCPAddr">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L58">ResolveTCPAddr</a>
    <a href="#ResolveTCPAddr">¶</a></h3>
<pre>func ResolveTCPAddr(network, address <a href="/builtin/#string">string</a>) (*<a href="#TCPAddr">TCPAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ResolveTCPAddr 返回 address 对应的  TCP 端点。

network 必须是一个 TCP 网络名称。

如果 address 参数的 IP 地址或端口号不是数字，ResolveTCPAddr 会把地址处理成 TCP 端点的地址。否则它将地址解析成 IP 地址和端口号的组合。address 参数可以使用主机名，但是不推荐这么做，因为有可能主机名对应多个 IP 地址。

关于更多 network 和 address 参数说明请看 Dial 函数文档。

<h3 id="TCPAddr.Network">func (*TCPAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L16">Network</a>
    <a href="#TCPAddr.Network">¶</a></h3>
<pre>func (a *<a href="#TCPAddr">TCPAddr</a>) Network() <a href="/builtin/#string">string</a></pre>

Network 返回地址的网络名称，`tcp`。

<h3 id="TCPAddr.String">func (*TCPAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L18">String</a>
    <a href="#TCPAddr.String">¶</a></h3>
<pre>func (a *<a href="#TCPAddr">TCPAddr</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="TCPConn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L75">TCPConn</a>
    <a href="#TCPConn">¶</a></h2>
<pre>type TCPConn struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

TCPConn 是 TCP 网络连接对 Conn 接口的实现。

<h3 id="DialTCP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L196">DialTCP</a>
    <a href="#DialTCP">¶</a></h3>
<pre>func DialTCP(network <a href="/builtin/#string">string</a>, laddr, raddr *<a href="#TCPAddr">TCPAddr</a>) (*<a href="#TCPConn">TCPConn</a>, <a href="/builtin/#error">error</a>)</pre>

DialTCP 是针对 TCP 网络的 Dial 函数。

network 必须是一个 TCP 网络名称；关于更多请看 Dial 函数文档。

如果 Iaddr 为 nil，会自动选择本地地址。如果 raddr 的 IP 字段为 nil 或者不确定的 IP 地址，默认使用本地网络。

<h3 id="TCPConn.Close">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L186">Close</a>
    <a href="#TCPConn.Close">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close 关闭链接。

<h3 id="TCPConn.CloseRead">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L102">CloseRead</a>
    <a href="#TCPConn.CloseRead">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) CloseRead() <a href="/builtin/#error">error</a></pre>

CloseRead 关闭 TCP 链接的读取端，大多数调用者只需使用 Close。

<h3 id="TCPConn.CloseWrite">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L114">CloseWrite</a>
    <a href="#TCPConn.CloseWrite">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) CloseWrite() <a href="/builtin/#error">error</a></pre>

CloseWrite 关闭 TCP 链接的写入端，大多数调用者只需使用 Close。

<h3 id="TCPConn.File">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L283">File</a>
    <a href="#TCPConn.File">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) File() (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

File 设置底层 os.File 文件为阻塞模式并返回拷贝。由调用者负责使用完成后关闭 f。关闭 c 不会影响 f，关闭 f 也不会影响 c。

os.File 的文件描述符和链接返回的不同。

改变 os.File 的文件描述符属性可能不会有预期的结果。

在 Unix 系统中这会导致 SetDeadline 方法停止工作。

<h3 id="TCPConn.LocalAddr">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L200">LocalAddr</a>
    <a href="#TCPConn.LocalAddr">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) LocalAddr() <a href="#Addr">Addr</a></pre>

LocalAddr 返回本地网络地址。返回的 Addr 是调用者之间共享的，所以不能改变它。

<h3 id="TCPConn.Read">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L162">Read</a>
    <a href="#TCPConn.Read">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) Read(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Read 实现了 Conn 接口的 Read 方法。

<h3 id="TCPConn.ReadFrom">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L89">ReadFrom</a>
    <a href="#TCPConn.ReadFrom">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) ReadFrom(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFrom 实现了 io.ReaderFrom 接口的 ReadFrom 方法。

<h3 id="TCPConn.RemoteAddr">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L210">RemoteAddr</a>
    <a href="#TCPConn.RemoteAddr">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) RemoteAddr() <a href="#Addr">Addr</a></pre>

RemoteAddr 返回远程网络地址。返回的 Addr 是调用者之间共享的，所以不能改变它。

<h3 id="TCPConn.SetDeadline">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L218">SetDeadline</a>
    <a href="#TCPConn.SetDeadline">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline 实现了 Conn 接口的 SetDeadline 方法。

<h3 id="TCPConn.SetKeepAlive">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L148">SetKeepAlive</a>
    <a href="#TCPConn.SetKeepAlive">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetKeepAlive(keepalive <a href="/builtin/#bool">bool</a>) <a href="/builtin/#error">error</a></pre>

SetKeepAlive 设置操作系统是否应该在链接中发送 keepalive 信息。

<h3 id="TCPConn.SetKeepAlivePeriod">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L159">SetKeepAlivePeriod</a>
    <a href="#TCPConn.SetKeepAlivePeriod">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetKeepAlivePeriod(d <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>) <a href="/builtin/#error">error</a></pre>

SetKeepAlivePeriod 设置保持链接的时长。

<h3 id="TCPConn.SetLinger">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L136">SetLinger</a>
    <a href="#TCPConn.SetLinger">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetLinger(sec <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetLinger 设置仍然有数据等待发送或确认的链接的关闭行为。

如果 sec < 0，操作系统会在后台完成数据发送。

如果 sec 等于 0，操作系统会丢弃所有还没发送或确认的数据。

如果 sec > 0，和 sec < 0 一样，会在后台发送数据。在一些操作系统中在 sec 秒后未发送的数据可能被丢弃。

<h3 id="TCPConn.SetNoDelay">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L173">SetNoDelay</a>
    <a href="#TCPConn.SetNoDelay">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetNoDelay(noDelay <a href="/builtin/#bool">bool</a>) <a href="/builtin/#error">error</a></pre>

SetNoDelay 控制操作在需要发送的包较少时是否应该延迟包的发送（Nagle 算法）。默认为 true（不延迟），说明数据会在写入之后直接发送。

<h3 id="TCPConn.SetReadBuffer">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L252">SetReadBuffer</a>
    <a href="#TCPConn.SetReadBuffer">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetReadBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetReadBuffer 设置链接对应的操作系统接受缓存的大小。

<h3 id="TCPConn.SetReadDeadline">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L229">SetReadDeadline</a>
    <a href="#TCPConn.SetReadDeadline">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetReadDeadline 实现了 Conn 接口的 SetReadDeadline 方法。

<h3 id="TCPConn.SetWriteBuffer">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L264">SetWriteBuffer</a>
    <a href="#TCPConn.SetWriteBuffer">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetWriteBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetWriteBuffer 设置链接对应的操作系统发送缓存大小。

<h3 id="TCPConn.SetWriteDeadline">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L240">SetWriteDeadline</a>
    <a href="#TCPConn.SetWriteDeadline">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetWriteDeadline 实现了 Conn 接口的 SetWriteDeadline 方法。

<h3 id="TCPConn.SyscallConn">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L81">SyscallConn</a>
    <a href="#TCPConn.SyscallConn">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SyscallConn() (<a href="/syscall/">syscall</a>.<a href="/syscall/#RawConn">RawConn</a>, <a href="/builtin/#error">error</a>)</pre>

SyscallConn returns a raw network connection. This implements the syscall.Conn
interface.

SyscallConn 返回一个原生的网络链接。它实现了 syscall.Conn 接口。

<h3 id="TCPConn.Write">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L174">Write</a>
    <a href="#TCPConn.Write">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) Write(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write 实现了 Conn 接口的 Write 方法。

<h2 id="TCPListener">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L214">TCPListener</a>
    <a href="#TCPListener">¶</a></h2>
<pre>type TCPListener struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

TCPListener 是 TCP 网络监听器。客户端应该 Listener 类型的变量而不是 TCPListener。

<h3 id="ListenTCP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L312">ListenTCP</a>
    <a href="#ListenTCP">¶</a></h3>
<pre>func ListenTCP(network <a href="/builtin/#string">string</a>, laddr *<a href="#TCPAddr">TCPAddr</a>) (*<a href="#TCPListener">TCPListener</a>, <a href="/builtin/#error">error</a>)</pre>

ListenTCP 是针对 TCP 网络的 Listen 。

network 必须是一个 TCP 网络名称；查看 Dial 函数文档了解更多。

如果 Iaddr 的 IP 字段是不确定 IP 地址，ListenTCP 会监听本地系统所有单播和任播地址。如果 Iaddr 的 Port 字段为 0，端口号会自动选择。

<h3 id="TCPListener.Accept">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L245">Accept</a>
    <a href="#TCPListener.Accept">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) Accept() (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

Accept 实现了 Listener 接口的 Accept 方法；他会等待下次调用并返回通用的 Conn。

<h3 id="TCPListener.AcceptTCP">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L232">AcceptTCP</a>
    <a href="#TCPListener.AcceptTCP">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) AcceptTCP() (*<a href="#TCPConn">TCPConn</a>, <a href="/builtin/#error">error</a>)</pre>

AcceptTCP 接受到来的请求并返回新的链接。

<h3 id="TCPListener.Addr">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L271">Addr</a>
    <a href="#TCPListener.Addr">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) Addr() <a href="#Addr">Addr</a></pre>

Addr 返回 Listener 的网络地址，是一个 *TCPAddr 类型。返回的 Addr 是所有调用者共享的，所以不要跟改它。

<h3 id="TCPListener.Close">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L258">Close</a>
    <a href="#TCPListener.Close">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) Close() <a href="/builtin/#error">error</a></pre>

Close 停止监听 TCP 网络地址。已经接受的链接不会关闭。

<h3 id="TCPListener.File">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L292">File</a>
    <a href="#TCPListener.File">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) File() (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

File 返回底层 os.File 拷贝，设置为阻塞模式。由调用者负责在使用完成后关闭 f。关闭 l 不会影响 f，关闭 f 也不会影响 l。

返回的 os.File 文件描述符和链接的文件描述符不同，使用这个副本来改变属性可能不会达到预期效果。

<h3 id="TCPListener.SetDeadline">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L275">SetDeadline</a>
    <a href="#TCPListener.SetDeadline">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline 设置监听器对应的截止时间。 t 为零值表示禁用截止日期。

<h3 id="TCPListener.SyscallConn">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L223">SyscallConn</a>
    <a href="#TCPListener.SyscallConn">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) SyscallConn() (<a href="/syscall/">syscall</a>.<a href="/syscall/#RawConn">RawConn</a>, <a href="/builtin/#error">error</a>)</pre>

SyscallConn 返回原生的网络链接。它实现了 syscall.Conn 接口。

返回的  RawConn 只支持调用 Control。读和写都会返回错误。

<h2 id="UDPAddr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L12">UDPAddr</a>
    <a href="#UDPAddr">¶</a></h2>
<pre>type UDPAddr struct {
<span id="UDPAddr.IP"></span>    IP   <a href="#IP">IP</a>
<span id="UDPAddr.Port"></span>    Port <a href="/builtin/#int">int</a>
<span id="UDPAddr.Zone"></span>    Zone <a href="/builtin/#string">string</a> <span class="comment">// IPv6 scoped addressing zone</span>
}</pre>

UDPAddr 代表一个 UDP 端点地址。

<h3 id="ResolveUDPAddr">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L61">ResolveUDPAddr</a>
    <a href="#ResolveUDPAddr">¶</a></h3>
<pre>func ResolveUDPAddr(network, address <a href="/builtin/#string">string</a>) (*<a href="#UDPAddr">UDPAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ResolveUDPAddr 返回 UDP 终端地址。

network 必须是 UDP 网络名称。

如果 address 参数的主机不是 IP 地址或端口不是数字，ResolveUDPAddr 会将地址处理成 UDP 端点地址。否则它会解析出 address 中的 IP 地址和端口号。address 参数可以使用主机名，但是不推荐这么做。因为它可能返回不只一个 IP 地址。

关于 network 和 address 参数请看 Dial 函数文档。

<h3 id="UDPAddr.Network">func (*UDPAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L19">Network</a>
    <a href="#UDPAddr.Network">¶</a></h3>
<pre>func (a *<a href="#UDPAddr">UDPAddr</a>) Network() <a href="/builtin/#string">string</a></pre>

Network 返回 address 的网络名称，`udp`。

<h3 id="UDPAddr.String">func (*UDPAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L21">String</a>
    <a href="#UDPAddr.String">¶</a></h3>
<pre>func (a *<a href="#UDPAddr">UDPAddr</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="UDPConn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L78">UDPConn</a>
    <a href="#UDPConn">¶</a></h2>
<pre>type UDPConn struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

UDPConn UDP 网络链接对 Conn 和 PacketConn 接口的实现。

<h3 id="DialUDP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L192">DialUDP</a>
    <a href="#DialUDP">¶</a></h3>
<pre>func DialUDP(network <a href="/builtin/#string">string</a>, laddr, raddr *<a href="#UDPAddr">UDPAddr</a>) (*<a href="#UDPConn">UDPConn</a>, <a href="/builtin/#error">error</a>)</pre>

DialUDP 是针对 UDP 网络的 Dial 函数。

network 必须是一个 UDP 网络名称；更多详细信息请看 Dial。

如果 laddr 为 nil，会自动选择本地地址。如果 raddr 的 IP 字段为 nil 或者不确定 IP 地址，默认使用本地系统。

<h3 id="ListenMulticastUDP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L250">ListenMulticastUDP</a>
    <a href="#ListenMulticastUDP">¶</a></h3>
<pre>func ListenMulticastUDP(network <a href="/builtin/#string">string</a>, ifi *<a href="#Interface">Interface</a>, gaddr *<a href="#UDPAddr">UDPAddr</a>) (*<a href="#UDPConn">UDPConn</a>, <a href="/builtin/#error">error</a>)</pre>

ListenMulticastUDP 和 UDP 网络的 ListenPacket 相似，只是会监听指定网络接口的多个地址。

network 必须是一个 UDP 网络名称；更多详情请看 Dial 函数文档。

ListenMulticastUDP 会监听所有包括组，和组播的本地系统地址。如果 ifi 为 nil，ListenMulticastUDP 会使用系统分配的组播地址。不过不推荐这样做，因为这个分配策略依赖平台的实现，并且有时需要路由配置。如果 gaddr 的 Port 字段为 0，会自动选择端口号。

ListenMulticastUDP 只是为了应用使用便捷，golang.org/x/net/ipv4 和 golang.org/x/net/ipv6 包中也有更通用的用法。

<h3 id="ListenUDP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L217">ListenUDP</a>
    <a href="#ListenUDP">¶</a></h3>
<pre>func ListenUDP(network <a href="/builtin/#string">string</a>, laddr *<a href="#UDPAddr">UDPAddr</a>) (*<a href="#UDPConn">UDPConn</a>, <a href="/builtin/#error">error</a>)</pre>

ListenUDP 是针对 UDP 网络的 ListenPacket。

network 必须是一个 UDP 网络名称；更多详情请看 Dial 函数文档。

如果 laddr 的 IP 字段为 nil 或不确定的 IP 地址，ListenUDP 会监听本地系统除了多播以外的所有 IP 地址。如果 laddr 的 Port 字段为 0，会自动选者端口号。

<h3 id="UDPConn.Close">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L186">Close</a>
    <a href="#UDPConn.Close">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close 关闭链接。

<h3 id="UDPConn.File">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L283">File</a>
    <a href="#UDPConn.File">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) File() (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

File 把底层 os.File 文件设置成阻塞模式并返回拷贝。由调用者负责在使用完成后关闭 f。关闭 c 不会影响 f，关闭 f 不会影响 c。

返回的 os.File 文件描述符和链接是不同的。修改部分的属性可能不会达到预期的效果。

在 Unix 系统中这会导致 SetDeadline 方法停止工作。

<h3 id="UDPConn.LocalAddr">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L200">LocalAddr</a>
    <a href="#UDPConn.LocalAddr">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) LocalAddr() <a href="#Addr">Addr</a></pre>

LocalAddr 返回贝蒂网络地址。返回的 Addr 被所有调用者所共享，所以不要改变它。

<h3 id="UDPConn.Read">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L162">Read</a>
    <a href="#UDPConn.Read">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) Read(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Read 实现了 Conn 接口的 Read 方法。

<h3 id="UDPConn.ReadFrom">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L104">ReadFrom</a>
    <a href="#UDPConn.ReadFrom">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) ReadFrom(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="#Addr">Addr</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFrom 实现了 PacketConn 接口的 ReadFrom 方法。

<h3 id="UDPConn.ReadFromUDP">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L92">ReadFromUDP</a>
    <a href="#UDPConn.ReadFromUDP">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) ReadFromUDP(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, *<a href="#UDPAddr">UDPAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFromUDP 与 ReadFrom 相似只是返回 UDPAddr。

<h3 id="UDPConn.ReadMsgUDP">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L125">ReadMsgUDP</a>
    <a href="#UDPConn.ReadMsgUDP">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) ReadMsgUDP(b, oob []<a href="/builtin/#byte">byte</a>) (n, oobn, flags <a href="/builtin/#int">int</a>, addr *<a href="#UDPAddr">UDPAddr</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadMsgUDP 从 c 中读取信息，将载荷拷贝进 b 并将带外数据拷贝进 oob。它会返回拷贝进 b 的字节数，拷贝进 oob 的字节数，消息的参数和消息的源地址。

golang.org/x/net/ipv4 和 golang.org/x/net/ipv6 包可以用来操作 oob 中 IP 层的套接字选项。

<h3 id="UDPConn.RemoteAddr">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L210">RemoteAddr</a>
    <a href="#UDPConn.RemoteAddr">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) RemoteAddr() <a href="#Addr">Addr</a></pre>

RemoteAddr 返回远程网络地址。返回的 Addr 被所有调用者所共享，所以不要改变它。

<h3 id="UDPConn.SetDeadline">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L218">SetDeadline</a>
    <a href="#UDPConn.SetDeadline">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline 实现了 Conn 接口的 SetDeadline 方法。

<h3 id="UDPConn.SetReadBuffer">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L252">SetReadBuffer</a>
    <a href="#UDPConn.SetReadBuffer">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) SetReadBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetReadBuffer 设置链接对应的操作系统接收缓存大小。

<h3 id="UDPConn.SetReadDeadline">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L229">SetReadDeadline</a>
    <a href="#UDPConn.SetReadDeadline">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetReadDeadline 实现了 Conn 接口的 SetReadDeadline 方法。

<h3 id="UDPConn.SetWriteBuffer">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L264">SetWriteBuffer</a>
    <a href="#UDPConn.SetWriteBuffer">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) SetWriteBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetWriteBuffer 设置链接对应操作系统的发送缓存大小。

<h3 id="UDPConn.SetWriteDeadline">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L240">SetWriteDeadline</a>
    <a href="#UDPConn.SetWriteDeadline">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetWriteDeadline 实现了 Conn 接口的 SetWriteDeadline 方法。

<h3 id="UDPConn.SyscallConn">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L84">SyscallConn</a>
    <a href="#UDPConn.SyscallConn">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) SyscallConn() (<a href="/syscall/">syscall</a>.<a href="/syscall/#RawConn">RawConn</a>, <a href="/builtin/#error">error</a>)</pre>

SyscallConn 返回原生的网络链接。它实现了 syscall.Conn 接口。

<h3 id="UDPConn.Write">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L174">Write</a>
    <a href="#UDPConn.Write">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) Write(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write 实现了 Conn 的 Write 方法。

<h3 id="UDPConn.WriteMsgUDP">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L172">WriteMsgUDP</a>
    <a href="#UDPConn.WriteMsgUDP">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) WriteMsgUDP(b, oob []<a href="/builtin/#byte">byte</a>, addr *<a href="#UDPAddr">UDPAddr</a>) (n, oobn <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

如果 c 没有连接 WriteMsgUDP 通过 c 向 addr 写入消息，如果 c 已经连接，会把消息发送到的 c 的远程地址。从 b 中拷贝载荷，并且从 oob 中拷贝对应的带外数据。它会返回写入成功的载荷和带外数据字节数。

golang.org/x/net/ipv4 和 golang.org/x/net/ipv6 包可以用来操作 oob 中 IP 层套接字选项。

<h3 id="UDPConn.WriteTo">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L149">WriteTo</a>
    <a href="#UDPConn.WriteTo">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) WriteTo(b []<a href="/builtin/#byte">byte</a>, addr <a href="#Addr">Addr</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteTo 实现了 PacketConn 接口的 WriteTo 方法。

<h3 id="UDPConn.WriteToUDP">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L137">WriteToUDP</a>
    <a href="#UDPConn.WriteToUDP">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) WriteToUDP(b []<a href="/builtin/#byte">byte</a>, addr *<a href="#UDPAddr">UDPAddr</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteToUDP 与 WriteTo 像是只是接受 UDPAddr 参数。

<h2 id="UnixAddr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L6">UnixAddr</a>
    <a href="#UnixAddr">¶</a></h2>
<pre>type UnixAddr struct {
<span id="UnixAddr.Name"></span>    Name <a href="/builtin/#string">string</a>
<span id="UnixAddr.Net"></span>    Net  <a href="/builtin/#string">string</a>
}</pre>

UnixAddr 代表 Unix 域套接字端点地址。

<h3 id="ResolveUnixAddr">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L41">ResolveUnixAddr</a>
    <a href="#ResolveUnixAddr">¶</a></h3>
<pre>func ResolveUnixAddr(network, address <a href="/builtin/#string">string</a>) (*<a href="#UnixAddr">UnixAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ResolveUnixAddr 返回 Unix 域套接字端点地址。

network 必须必须是 Unix 网络名称。

关于 network 和 address 参数请看 Dial 函数文档。

<h3 id="UnixAddr.Network">func (*UnixAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L13">Network</a>
    <a href="#UnixAddr.Network">¶</a></h3>
<pre>func (a *<a href="#UnixAddr">UnixAddr</a>) Network() <a href="/builtin/#string">string</a></pre>

Network 返回地址的网络名称，`unix`, `unixgram` 或 `unixpacket`。

<h3 id="UnixAddr.String">func (*UnixAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L17">String</a>
    <a href="#UnixAddr.String">¶</a></h3>
<pre>func (a *<a href="#UnixAddr">UnixAddr</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="UnixConn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L52">UnixConn</a>
    <a href="#UnixConn">¶</a></h2>
<pre>type UnixConn struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

UnixConn 是 Unix 域套接字对 Conn 接口的实现。

<h3 id="DialUnix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L187">DialUnix</a>
    <a href="#DialUnix">¶</a></h3>
<pre>func DialUnix(network <a href="/builtin/#string">string</a>, laddr, raddr *<a href="#UnixAddr">UnixAddr</a>) (*<a href="#UnixConn">UnixConn</a>, <a href="/builtin/#error">error</a>)</pre>

DialUnix 是针对 Unix 网络的 Dial。

网络名必须是 Unix 网络名；关于更多请看 Dial 函数。

如果 laddr 是非 nil，会使用链接的本地地址。

<h3 id="ListenUnixgram">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L319">ListenUnixgram</a>
    <a href="#ListenUnixgram">¶</a></h3>
<pre>func ListenUnixgram(network <a href="/builtin/#string">string</a>, laddr *<a href="#UnixAddr">UnixAddr</a>) (*<a href="#UnixConn">UnixConn</a>, <a href="/builtin/#error">error</a>)</pre>

ListenUnixgram 是针对 Unix 网络的 ListenPacket。

网络必须为 `unixgram`。

<h3 id="UnixConn.Close">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L186">Close</a>
    <a href="#UnixConn.Close">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close 关闭链接。

<h3 id="UnixConn.CloseRead">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L67">CloseRead</a>
    <a href="#UnixConn.CloseRead">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) CloseRead() <a href="/builtin/#error">error</a></pre>

CloseRead 会关闭 Unix 域链接的读取端。大多数调用者只需要使用 Close。

<h3 id="UnixConn.CloseWrite">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L79">CloseWrite</a>
    <a href="#UnixConn.CloseWrite">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) CloseWrite() <a href="/builtin/#error">error</a></pre>

CloseWrite 关闭 Unix 域链接的写入端。大多数调用者只需要使用 Close。

<h3 id="UnixConn.File">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L283">File</a>
    <a href="#UnixConn.File">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) File() (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

File 设置底层的 os.File 为阻塞模式并返回拷贝。由调用者负责在使用完成后关闭 f。关闭 c 不会影响 f，关闭 f 不会影响 c。

返回的 os.File 文件描述符和链接是不同的。修改副本的属性可能不会有预期结果。

在 Unix 系统值会导致 SetDeadline 方法停止工作。

<h3 id="UnixConn.LocalAddr">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L200">LocalAddr</a>
    <a href="#UnixConn.LocalAddr">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) LocalAddr() <a href="#Addr">Addr</a></pre>

LocalAddr 返回本地网络地址。返回的 Addr 被所有调用者共享，所以不要改变它。

<h3 id="UnixConn.Read">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L162">Read</a>
    <a href="#UnixConn.Read">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) Read(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Read 实现了 Conn 接口的 Read 方法。

<h3 id="UnixConn.ReadFrom">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L102">ReadFrom</a>
    <a href="#UnixConn.ReadFrom">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) ReadFrom(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="#Addr">Addr</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFrom 实现了 PacketConn 接口的 ReadFrom 方法。

<h3 id="UnixConn.ReadFromUnix">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L90">ReadFromUnix</a>
    <a href="#UnixConn.ReadFromUnix">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) ReadFromUnix(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, *<a href="#UnixAddr">UnixAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFromUnix 和 ReadFrom 相似只是返回 UnixAddr。

<h3 id="UnixConn.ReadMsgUnix">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L123">ReadMsgUnix</a>
    <a href="#UnixConn.ReadMsgUnix">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) ReadMsgUnix(b, oob []<a href="/builtin/#byte">byte</a>) (n, oobn, flags <a href="/builtin/#int">int</a>, addr *<a href="#UnixAddr">UnixAddr</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadMsgUnix 从 c 中读取信息，将载荷拷贝进 b，将带外数据拷贝进 oob。它会返回拷贝进 b 的字节数，拷贝进 oob 字节数，消息的参数和消息的源地址。

注意如果 `len(b)==0` 并且 `len(oob)>0`，这个函数仍然会从链接读取并丢弃一字节。

<h3 id="UnixConn.RemoteAddr">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L210">RemoteAddr</a>
    <a href="#UnixConn.RemoteAddr">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) RemoteAddr() <a href="#Addr">Addr</a></pre>

RemoteAddr 返回远程网络地址。返回的 Addr 被所有调用者共享，所以不要改变它。

<h3 id="UnixConn.SetDeadline">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L218">SetDeadline</a>
    <a href="#UnixConn.SetDeadline">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline 实现了 Conn 的 SetDeadline 方法。

<h3 id="UnixConn.SetReadBuffer">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L252">SetReadBuffer</a>
    <a href="#UnixConn.SetReadBuffer">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) SetReadBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetReadBuffer 设置链接对应的操作系统接收缓存的大小。

<h3 id="UnixConn.SetReadDeadline">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L229">SetReadDeadline</a>
    <a href="#UnixConn.SetReadDeadline">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetReadDeadline 实现了 Conn 接口的 SetReadDeadline 方法。

<h3 id="UnixConn.SetWriteBuffer">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L264">SetWriteBuffer</a>
    <a href="#UnixConn.SetWriteBuffer">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) SetWriteBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetWriteBuffer 设置链接对应的操作系统发送缓存大小。

<h3 id="UnixConn.SetWriteDeadline">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L240">SetWriteDeadline</a>
    <a href="#UnixConn.SetWriteDeadline">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetWriteDeadline 实现了 Conn 接口的 SetWriteDeadline 方法。

<h3 id="UnixConn.SyscallConn">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L58">SyscallConn</a>
    <a href="#UnixConn.SyscallConn">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) SyscallConn() (<a href="/syscall/">syscall</a>.<a href="/syscall/#RawConn">RawConn</a>, <a href="/builtin/#error">error</a>)</pre>

SyscallConn 返回原生的网络链接。它实现了 syscall.Conn 接口。

<h3 id="UnixConn.Write">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L174">Write</a>
    <a href="#UnixConn.Write">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) Write(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write 实现了 Conn 接口的 Write 方法。

<h3 id="UnixConn.WriteMsgUnix">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L168">WriteMsgUnix</a>
    <a href="#UnixConn.WriteMsgUnix">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) WriteMsgUnix(b, oob []<a href="/builtin/#byte">byte</a>, addr *<a href="#UnixAddr">UnixAddr</a>) (n, oobn <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteMsgUnix 通过 c 向 addr 发送信息，从 b 中拷贝载荷，从 oob 中拷贝带外数据。它会返回成功写入载荷和带外数据的字节数。

如果 `len(b) == 0` 并且`len(oob)>0`，这个函数仍然会向链接写入 1 字节。

<h3 id="UnixConn.WriteTo">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L147">WriteTo</a>
    <a href="#UnixConn.WriteTo">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) WriteTo(b []<a href="/builtin/#byte">byte</a>, addr <a href="#Addr">Addr</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteTo 实现了 PacketConn 接口的 WriteTo 方法。

<h3 id="UnixConn.WriteToUnix">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L135">WriteToUnix</a>
    <a href="#UnixConn.WriteToUnix">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) WriteToUnix(b []<a href="/builtin/#byte">byte</a>, addr *<a href="#UnixAddr">UnixAddr</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteToUnix 与 WriteTo 相似只是接受 UnixAddr。

<h2 id="UnixListener">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L203">UnixListener</a>
    <a href="#UnixListener">¶</a></h2>
<pre>type UnixListener struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

UnixListener 是 Unix 域套接字监听器。用户应该使用 Listener 类型而不是 Unix 域套接字类型。

<h3 id="ListenUnix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L300">ListenUnix</a>
    <a href="#ListenUnix">¶</a></h3>
<pre>func ListenUnix(network <a href="/builtin/#string">string</a>, laddr *<a href="#UnixAddr">UnixAddr</a>) (*<a href="#UnixListener">UnixListener</a>, <a href="/builtin/#error">error</a>)</pre>

ListenUnix 是针对 Unix 网络的 Listen。

network 必须是 unix 或 unixpacket。

<h3 id="UnixListener.Accept">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L239">Accept</a>
    <a href="#UnixListener.Accept">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) Accept() (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

Accept 实现了 Listener 接口的 Accept 方法。返回的链接类型为 *UnixConn。

<h3 id="UnixListener.AcceptUnix">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L226">AcceptUnix</a>
    <a href="#UnixListener.AcceptUnix">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) AcceptUnix() (*<a href="#UnixConn">UnixConn</a>, <a href="/builtin/#error">error</a>)</pre>

AcceptUnix 会接受下次调用并返回新链接。

<h3 id="UnixListener.Addr">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L265">Addr</a>
    <a href="#UnixListener.Addr">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) Addr() <a href="#Addr">Addr</a></pre>

Addr 返回监听器的网络地址。返回的 Addr 被所有调用者所共享，所以不要改变它。

<h3 id="UnixListener.Close">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L252">Close</a>
    <a href="#UnixListener.Close">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) Close() <a href="/builtin/#error">error</a></pre>

Close 停止监听 Unix 地址，已经接收到的链接不会关闭。

<h3 id="UnixListener.File">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L286">File</a>
    <a href="#UnixListener.File">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) File() (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

File 返回底层 os.File 的拷贝，设置阻塞模式。由调用者负责在使用后关闭 f。关闭 l 不会影响 f，关闭 f 不会影响 l。

返回的 os.File 文件描述符和链接是不同的。修改副本属性可能不会达到预期效果。

<h3 id="UnixListener.SetDeadline">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L269">SetDeadline</a>
    <a href="#UnixListener.SetDeadline">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline 设置监听器对应的截止信息。t 的零值表示禁用截止时间。

<h3 id="UnixListener.SetUnlinkOnClose">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock_posix.go#L195">SetUnlinkOnClose</a>
    <a href="#UnixListener.SetUnlinkOnClose">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) SetUnlinkOnClose(unlink <a href="/builtin/#bool">bool</a>)</pre>

SetUnlinkOnClose 设置在监听器关闭时底层套接字文件是否删除。

默认只在是由 net 包创建的时才会 unlink 太接字文件。当底层套接字文件由 Listen 或 ListenUnix 创建时，默认会移除该套接字文件。但是如果监听器是 FileListener 使用一个已经存在的套接字文件，监听器在关闭时默认不会移除套接字文件。

<h3 id="UnixListener.SyscallConn">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L217">SyscallConn</a>
    <a href="#UnixListener.SyscallConn">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) SyscallConn() (<a href="/syscall/">syscall</a>.<a href="/syscall/#RawConn">RawConn</a>, <a href="/builtin/#error">error</a>)</pre>

SyscallConn 返回原生的网络链接。它实现了 syscall.Conn 接口。

返回的 RawConn 只支持调用 Control。读和写操作会返回错误。

<h2 id="UnknownNetworkError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L521">UnknownNetworkError</a>
    <a href="#UnknownNetworkError">¶</a></h2>
<pre>type UnknownNetworkError <a href="/builtin/#string">string</a></pre>


<h3 id="UnknownNetworkError.Error">func (UnknownNetworkError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L523">Error</a>
    <a href="#UnknownNetworkError.Error">¶</a></h3>
<pre>func (e <a href="#UnknownNetworkError">UnknownNetworkError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="UnknownNetworkError.Temporary">func (UnknownNetworkError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L525">Temporary</a>
    <a href="#UnknownNetworkError.Temporary">¶</a></h3>
<pre>func (e <a href="#UnknownNetworkError">UnknownNetworkError</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>


<h3 id="UnknownNetworkError.Timeout">func (UnknownNetworkError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L524">Timeout</a>
    <a href="#UnknownNetworkError.Timeout">¶</a></h3>
<pre>func (e <a href="#UnknownNetworkError">UnknownNetworkError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>


<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/file.go#L1)  在 NaCl 和 Windows 平台, 没有实现 FileConn, FileListener and FilePacketConn 函数。

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L3)  在 NaCl 平台, 没有实现方法和函数的相关接口。

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L6)  在 DragonFly BSD, NetBSD, OpenBSD, Plan 9 和 Solaris 平台,  没有实现接口的 MulticastAddrs。

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L2)  在每个 POSIX 平台, 使用 ReadFrom 或 ReadFromIP 方法从 \`ip4` 网络读取数据可能不会返回完整的 IPv4 包包括包的头部，即使空间足够。即使在 Read 或 ReadMsgIP 返回完整包也有可能出现这种情况。因为这个原因，如果你在接收一个非常重要的完整包。

  Go 1 的兼容性导致无法改变这些方法的行为; 所以使用 Read 或 ReadMsgIP 作为替代。

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L14)  在 NaCl 和 Plan 9 平台, IPConn 的 ReadMsgIP 和 WriteMsgIP 方法没有实现。 

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L17)  在 Windows 平台, IPConn 的 File 方法没有实现。

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ipsock.go#L2)  在 DragonFly BSD 和 OpenBSD 平台,  "tcp" and "udp" 网络不能同时监听 IPv4 和 IPv6 地址。这是由于 IPv4 没有路由到 IPv6 套接字。如果需要支持两种地址族需要两种套接字。更多详情请看 inet6（4）。

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rawconn.go#L2)  在 Windows 平台, syscall.RawConn 的读和写没有实现。 

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rawconn.go#L5)  在 NaCl 和 Plan 9 平台, syscall.RawConn 的 Read ，Write 和 Control 都没有实现。

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L5)  在 Windows 平台，TCPListener 的 File 方法没有实现。

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L2)  在 NaCl 和 Plan 9 平台, UDPConn 的 ReadMsgUDP 和 WriteMsgUDP 方法没有实现。

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L5)  在 Windows 平台, UDPConn 的 File 方法没有实现。

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L8)  在 NaCl 平台, ListenMulticastUDP 函数没有实现。

## Subdirectories
- [..](..)
- [http](http/)
- [mail](mail/)
- [rpc](rpc/)
- [smtp](smtp/)
- [textproto](textproto/)
- [url](url/)
