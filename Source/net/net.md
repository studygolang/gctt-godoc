version: 1.9.2
## package net

  `import "net"`

## Overview

Package net provides a portable interface for network I/O, including TCP/IP,
UDP, domain name resolution, and Unix domain sockets.

Although the package provides access to low-level networking primitives, most
clients will need only the basic interface provided by the Dial, Listen, and
Accept functions and the associated Conn and Listener interfaces. The crypto/tls
package uses the same interfaces and similar Dial and Listen functions.

The Dial function connects to a server:

    conn, err := net.Dial("tcp", "golang.org:80")
    if err != nil {
    	// handle error
    }
    fmt.Fprintf(conn, "GET / HTTP/1.0\r\n\r\n")
    status, err := bufio.NewReader(conn).ReadString('\n')
    // ...

The Listen function creates servers:

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


Name Resolution

The method for resolving domain names, whether indirectly with functions like
Dial or directly with functions like LookupHost and LookupAddr, varies by
operating system.

On Unix systems, the resolver has two options for resolving names. It can use a
pure Go resolver that sends DNS requests directly to the servers listed in
/etc/resolv.conf, or it can use a cgo-based resolver that calls C library
routines such as getaddrinfo and getnameinfo.

By default the pure Go resolver is used, because a blocked DNS request consumes
only a goroutine, while a blocked C call consumes an operating system thread.
When cgo is available, the cgo-based resolver is used instead under a variety of
conditions: on systems that do not let programs make direct DNS requests (OS X),
when the LOCALDOMAIN environment variable is present (even if empty), when the
RES_OPTIONS or HOSTALIASES environment variable is non-empty, when the
ASR_CONFIG environment variable is non-empty (OpenBSD only), when
/etc/resolv.conf or /etc/nsswitch.conf specify the use of features that the Go
resolver does not implement, and when the name being looked up ends in .local or
is an mDNS name.

The resolver decision can be overridden by setting the netdns value of the
GODEBUG environment variable (see package runtime) to go or cgo, as in:

    export GODEBUG=netdns=go    # force pure Go resolver
    export GODEBUG=netdns=cgo   # force cgo resolver

The decision can also be forced while building the Go source tree by setting the
netgo or netcgo build tag.

A numeric netdns setting, as in GODEBUG=netdns=1, causes the resolver to print
debugging information about its decisions. To force a particular resolver while
also printing debugging information, join the two settings by a plus sign, as in
GODEBUG=netdns=go+1.

On Plan 9, the resolver always accesses /net/cs and /net/dns.

On Windows, the resolver always uses C library functions, such as GetAddrInfo
and DnsQuery.

## Index

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
- [type UnknownNetworkError](#UnknownNetworkError)
  - [func (e UnknownNetworkError) Error() string](#UnknownNetworkError.Error)
  - [func (e UnknownNetworkError) Temporary() bool](#UnknownNetworkError.Temporary)
  - [func (e UnknownNetworkError) Timeout() bool](#UnknownNetworkError.Timeout)
- [Bugs](#pkg-note-BUG)

### Examples

- [CIDRMask](#example_CIDRMask)
- [IP.DefaultMask](#example_IP_DefaultMask)
- [IP.Mask](#example_IP_Mask)
- [IPv4](#example_IPv4)
- [IPv4Mask](#example_IPv4Mask)
- [Listener](#example_Listener)
- [ParseCIDR](#example_ParseCIDR)
- [ParseIP](#example_ParseIP)

### Package files
 [addrselect.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/addrselect.go) [cgo_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/cgo_linux.go) [cgo_resnew.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/cgo_resnew.go) [cgo_socknew.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/cgo_socknew.go) [cgo_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/cgo_unix.go) [conf.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/conf.go) [dial.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go) [dnsclient.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsclient.go) [dnsclient_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsclient_unix.go) [dnsconfig_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsconfig_unix.go) [dnsmsg.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsmsg.go) [error_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/error_posix.go) [fd_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/fd_unix.go) [file.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/file.go) [file_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/file_unix.go) [hook.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/hook.go) [hook_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/hook_unix.go) [hosts.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/hosts.go) [interface.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go) [interface_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface_linux.go) [ip.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go) [iprawsock.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go) [iprawsock_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock_posix.go) [ipsock.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ipsock.go) [ipsock_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ipsock_posix.go) [lookup.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go) [lookup_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup_unix.go) [mac.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/mac.go) [net.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go) [nss.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/nss.go) [parse.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/parse.go) [pipe.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/pipe.go) [port.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/port.go) [port_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/port_unix.go) [rawconn.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rawconn.go) [sendfile_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sendfile_linux.go) [sock_cloexec.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sock_cloexec.go) [sock_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sock_linux.go) [sock_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sock_posix.go) [sockopt_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sockopt_linux.go) [sockopt_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sockopt_posix.go) [sockoptip_linux.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sockoptip_linux.go) [sockoptip_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/sockoptip_posix.go) [tcpsock.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go) [tcpsock_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock_posix.go) [tcpsockopt_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsockopt_posix.go) [tcpsockopt_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsockopt_unix.go) [udpsock.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go) [udpsock_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock_posix.go) [unixsock.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go) [unixsock_posix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock_posix.go) [writev_unix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/writev_unix.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="IPv4len">IPv4len</span> = 4
    <span id="IPv6len">IPv6len</span> = 16
)</pre>

IP address lengths (bytes).

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span id="IPv4bcast">IPv4bcast</span>     = <a href="#IPv4">IPv4</a>(255, 255, 255, 255) <span class="comment">// limited broadcast</span>
    <span id="IPv4allsys">IPv4allsys</span>    = <a href="#IPv4">IPv4</a>(224, 0, 0, 1)       <span class="comment">// all systems</span>
    <span id="IPv4allrouter">IPv4allrouter</span> = <a href="#IPv4">IPv4</a>(224, 0, 0, 2)       <span class="comment">// all routers</span>
    <span id="IPv4zero">IPv4zero</span>      = <a href="#IPv4">IPv4</a>(0, 0, 0, 0)         <span class="comment">// all zeros</span>
)</pre>

Well-known IPv4 addresses

<pre>var (
    <span id="IPv6zero">IPv6zero</span>                   = <a href="#IP">IP</a>{0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}
    <span id="IPv6unspecified">IPv6unspecified</span>            = <a href="#IP">IP</a>{0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}
    <span id="IPv6loopback">IPv6loopback</span>               = <a href="#IP">IP</a>{0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1}
    <span id="IPv6interfacelocalallnodes">IPv6interfacelocalallnodes</span> = <a href="#IP">IP</a>{0xff, 0x01, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0x01}
    <span id="IPv6linklocalallnodes">IPv6linklocalallnodes</span>      = <a href="#IP">IP</a>{0xff, 0x02, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0x01}
    <span id="IPv6linklocalallrouters">IPv6linklocalallrouters</span>    = <a href="#IP">IP</a>{0xff, 0x02, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0x02}
)</pre>

Well-known IPv6 addresses

<pre>var <span id="DefaultResolver">DefaultResolver</span> = &amp;<a href="#Resolver">Resolver</a>{}</pre>

DefaultResolver is the resolver used by the package-level Lookup functions and
by Dialers without a specified Resolver.

<pre>var (
    <span id="ErrWriteToConnected">ErrWriteToConnected</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;use of WriteTo with pre-connected connection&#34;)
)</pre>

Various errors contained in OpError.

<h2 id="InterfaceAddrs">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L105">InterfaceAddrs</a>
    <a href="#InterfaceAddrs">¶</a></h2>
<pre>func InterfaceAddrs() ([]<a href="#Addr">Addr</a>, <a href="/builtin/#error">error</a>)</pre>

InterfaceAddrs returns a list of the system's unicast interface addresses.

The returned list does not identify the associated interface; use Interfaces and
Interface.Addrs for more detail.

<h2 id="Interfaces">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L89">Interfaces</a>
    <a href="#Interfaces">¶</a></h2>
<pre>func Interfaces() ([]<a href="#Interface">Interface</a>, <a href="/builtin/#error">error</a>)</pre>

Interfaces returns a list of the system's network interfaces.

<h2 id="JoinHostPort">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ipsock.go#L216">JoinHostPort</a>
    <a href="#JoinHostPort">¶</a></h2>
<pre>func JoinHostPort(host, port <a href="/builtin/#string">string</a>) <a href="/builtin/#string">string</a></pre>

JoinHostPort combines host and port into a network address of the form
"host:port". If host contains a colon, as found in literal IPv6 addresses, then
JoinHostPort returns "[host]:port".

See func Dial for a description of the host and port parameters.

<h2 id="LookupAddr">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L349">LookupAddr</a>
    <a href="#LookupAddr">¶</a></h2>
<pre>func LookupAddr(addr <a href="/builtin/#string">string</a>) (names []<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupAddr performs a reverse lookup for the given address, returning a list of
names mapping to that address.

When using the host C library resolver, at most one result will be returned. To
bypass the host resolver, use a custom Resolver.

<h2 id="LookupCNAME">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L270">LookupCNAME</a>
    <a href="#LookupCNAME">¶</a></h2>
<pre>func LookupCNAME(host <a href="/builtin/#string">string</a>) (cname <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupCNAME returns the canonical name for the given host. Callers that do not
care about the canonical name can call LookupHost or LookupIP directly; both
take care of resolving the canonical name as part of the lookup.

A canonical name is the final name after following zero or more CNAME records.
LookupCNAME does not return an error if host does not contain DNS "CNAME"
records, as long as host resolves to address records.

<h2 id="LookupHost">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L127">LookupHost</a>
    <a href="#LookupHost">¶</a></h2>
<pre>func LookupHost(host <a href="/builtin/#string">string</a>) (addrs []<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupHost looks up the given host using the local resolver. It returns a slice
of that host's addresses.

<h2 id="LookupIP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L147">LookupIP</a>
    <a href="#LookupIP">¶</a></h2>
<pre>func LookupIP(host <a href="/builtin/#string">string</a>) ([]<a href="#IP">IP</a>, <a href="/builtin/#error">error</a>)</pre>

LookupIP looks up host using the local resolver. It returns a slice of that
host's IPv4 and IPv6 addresses.

<h2 id="LookupMX">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L315">LookupMX</a>
    <a href="#LookupMX">¶</a></h2>
<pre>func LookupMX(name <a href="/builtin/#string">string</a>) ([]*<a href="#MX">MX</a>, <a href="/builtin/#error">error</a>)</pre>

LookupMX returns the DNS MX records for the given domain name sorted by
preference.

<h2 id="LookupNS">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L325">LookupNS</a>
    <a href="#LookupNS">¶</a></h2>
<pre>func LookupNS(name <a href="/builtin/#string">string</a>) ([]*<a href="#NS">NS</a>, <a href="/builtin/#error">error</a>)</pre>

LookupNS returns the DNS NS records for the given domain name.

<h2 id="LookupPort">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L241">LookupPort</a>
    <a href="#LookupPort">¶</a></h2>
<pre>func LookupPort(network, service <a href="/builtin/#string">string</a>) (port <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupPort looks up the port for the given network and service.

<h2 id="LookupSRV">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L297">LookupSRV</a>
    <a href="#LookupSRV">¶</a></h2>
<pre>func LookupSRV(service, proto, name <a href="/builtin/#string">string</a>) (cname <a href="/builtin/#string">string</a>, addrs []*<a href="#SRV">SRV</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupSRV tries to resolve an SRV query of the given service, protocol, and
domain name. The proto is "tcp" or "udp". The returned records are sorted by
priority and randomized by weight within a priority.

LookupSRV constructs the DNS name to look up following RFC 2782. That is, it
looks up _service._proto.name. To accommodate services publishing SRV records
under non-standard names, if both service and proto are empty strings, LookupSRV
looks up name directly.

<h2 id="LookupTXT">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L335">LookupTXT</a>
    <a href="#LookupTXT">¶</a></h2>
<pre>func LookupTXT(name <a href="/builtin/#string">string</a>) ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

LookupTXT returns the DNS TXT records for the given domain name.

<h2 id="SplitHostPort">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ipsock.go#L145">SplitHostPort</a>
    <a href="#SplitHostPort">¶</a></h2>
<pre>func SplitHostPort(hostport <a href="/builtin/#string">string</a>) (host, port <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

SplitHostPort splits a network address of the form "host:port",
"host%zone:port", "[host]:port" or "[host%zone]:port" into host or host%zone and
port.

A literal IPv6 address in hostport must be enclosed in square brackets, as in
"[::1]:80", "[::1%lo0]:80".

See func Dial for a description of the hostport parameter, and host and port
results.

<h2 id="Addr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L94">Addr</a>
    <a href="#Addr">¶</a></h2>
<pre>type Addr interface {
    Network() <a href="/builtin/#string">string</a> <span class="comment">// name of the network (for example, &#34;tcp&#34;, &#34;udp&#34;)</span>
    String() <a href="/builtin/#string">string</a>  <span class="comment">// string form of address (for example, &#34;192.0.2.1:25&#34;, &#34;[2001:db8::1]:80&#34;)</span>
}</pre>

Addr represents a network end point address.

The two methods Network and String conventionally return strings that can be
passed as the arguments to Dial, but the exact form and meaning of the strings
is up to the implementation.

<h2 id="AddrError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L500">AddrError</a>
    <a href="#AddrError">¶</a></h2>
<pre>type AddrError struct {
<span id="AddrError.Err"></span>    Err  <a href="/builtin/#string">string</a>
<span id="AddrError.Addr"></span>    Addr <a href="/builtin/#string">string</a>
}</pre>


<h3 id="AddrError.Error">func (*AddrError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L505">Error</a>
    <a href="#AddrError.Error">¶</a></h3>
<pre>func (e *<a href="#AddrError">AddrError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="AddrError.Temporary">func (*AddrError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L517">Temporary</a>
    <a href="#AddrError.Temporary">¶</a></h3>
<pre>func (e *<a href="#AddrError">AddrError</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>


<h3 id="AddrError.Timeout">func (*AddrError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L516">Timeout</a>
    <a href="#AddrError.Timeout">¶</a></h3>
<pre>func (e *<a href="#AddrError">AddrError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>


<h2 id="Buffers">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L617">Buffers</a>
    <a href="#Buffers">¶</a></h2>
<pre>type Buffers [][]<a href="/builtin/#byte">byte</a></pre>

Buffers contains zero or more runs of bytes to write.

On certain machines, for certain types of connections, this is optimized into an
OS-specific batch write operation (such as "writev").

<h3 id="Buffers.Read">func (*Buffers) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L640">Read</a>
    <a href="#Buffers.Read">¶</a></h3>
<pre>func (v *<a href="#Buffers">Buffers</a>) Read(p []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>


<h3 id="Buffers.WriteTo">func (*Buffers) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L624">WriteTo</a>
    <a href="#Buffers.WriteTo">¶</a></h3>
<pre>func (v *<a href="#Buffers">Buffers</a>) WriteTo(w <a href="/io/">io</a>.<a href="/io/#Writer">Writer</a>) (n <a href="/builtin/#int64">int64</a>, err <a href="/builtin/#error">error</a>)</pre>


<h2 id="Conn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L102">Conn</a>
    <a href="#Conn">¶</a></h2>
<pre>type Conn interface {
    <span class="comment">// Read reads data from the connection.</span>
    <span class="comment">// Read can be made to time out and return an Error with Timeout() == true</span>
    <span class="comment">// after a fixed time limit; see SetDeadline and SetReadDeadline.</span>
    Read(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)

    <span class="comment">// Write writes data to the connection.</span>
    <span class="comment">// Write can be made to time out and return an Error with Timeout() == true</span>
    <span class="comment">// after a fixed time limit; see SetDeadline and SetWriteDeadline.</span>
    Write(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)

    <span class="comment">// Close closes the connection.</span>
    <span class="comment">// Any blocked Read or Write operations will be unblocked and return errors.</span>
    Close() <a href="/builtin/#error">error</a>

    <span class="comment">// LocalAddr returns the local network address.</span>
    LocalAddr() <a href="#Addr">Addr</a>

    <span class="comment">// RemoteAddr returns the remote network address.</span>
    RemoteAddr() <a href="#Addr">Addr</a>

    <span class="comment">// SetDeadline sets the read and write deadlines associated</span>
    <span class="comment">// with the connection. It is equivalent to calling both</span>
    <span class="comment">// SetReadDeadline and SetWriteDeadline.</span>
    <span class="comment">//</span>
    <span class="comment">// A deadline is an absolute time after which I/O operations</span>
    <span class="comment">// fail with a timeout (see type Error) instead of</span>
    <span class="comment">// blocking. The deadline applies to all future and pending</span>
    <span class="comment">// I/O, not just the immediately following call to Read or</span>
    <span class="comment">// Write. After a deadline has been exceeded, the connection</span>
    <span class="comment">// can be refreshed by setting a deadline in the future.</span>
    <span class="comment">//</span>
    <span class="comment">// An idle timeout can be implemented by repeatedly extending</span>
    <span class="comment">// the deadline after successful Read or Write calls.</span>
    <span class="comment">//</span>
    <span class="comment">// A zero value for t means I/O operations will not time out.</span>
    SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a>

    <span class="comment">// SetReadDeadline sets the deadline for future Read calls</span>
    <span class="comment">// and any currently-blocked Read call.</span>
    <span class="comment">// A zero value for t means Read will not time out.</span>
    SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a>

    <span class="comment">// SetWriteDeadline sets the deadline for future Write calls</span>
    <span class="comment">// and any currently-blocked Write call.</span>
    <span class="comment">// Even if write times out, it may return n &gt; 0, indicating that</span>
    <span class="comment">// some of the data was successfully written.</span>
    <span class="comment">// A zero value for t means Write will not time out.</span>
    SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a>
}</pre>

Conn is a generic stream-oriented network connection.

Multiple goroutines may invoke methods on a Conn simultaneously.

<h3 id="Dial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L279">Dial</a>
    <a href="#Dial">¶</a></h3>
<pre>func Dial(network, address <a href="/builtin/#string">string</a>) (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

Dial connects to the address on the named network.

Known networks are "tcp", "tcp4" (IPv4-only), "tcp6" (IPv6-only), "udp", "udp4"
(IPv4-only), "udp6" (IPv6-only), "ip", "ip4" (IPv4-only), "ip6" (IPv6-only),
"unix", "unixgram" and "unixpacket".

For TCP and UDP networks, the address has the form "host:port". The host must be
a literal IP address, or a host name that can be resolved to IP addresses. The
port must be a literal port number or a service name. If the host is a literal
IPv6 address it must be enclosed in square brackets, as in "[2001:db8::1]:80" or
"[fe80::1%zone]:80". The zone specifies the scope of the literal IPv6 address as
defined in RFC 4007. The functions JoinHostPort and SplitHostPort manipulate a
pair of host and port in this form. When using TCP, and the host resolves to
multiple IP addresses, Dial will try each IP address in order until one
succeeds.

Examples:

    Dial("tcp", "golang.org:http")
    Dial("tcp", "192.0.2.1:http")
    Dial("tcp", "198.51.100.1:80")
    Dial("udp", "[2001:db8::1]:domain")
    Dial("udp", "[fe80::1%lo0]:53")
    Dial("tcp", ":80")

For IP networks, the network must be "ip", "ip4" or "ip6" followed by a colon
and a literal protocol number or a protocol name, and the address has the form
"host". The host must be a literal IP address or a literal IPv6 address with
zone. It depends on each operating system how the operating system behaves with
a non-well known protocol number such as "0" or "255".

Examples:

    Dial("ip4:1", "192.0.2.1")
    Dial("ip6:ipv6-icmp", "2001:db8::1")
    Dial("ip6:58", "fe80::1%lo0")

For TCP, UDP and IP networks, if the host is empty or a literal unspecified IP
address, as in ":80", "0.0.0.0:80" or "[::]:80" for TCP and UDP, "", "0.0.0.0"
or "::" for IP, the local system is assumed.

For Unix networks, the address must be a file system path.

<h3 id="DialTimeout">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L294">DialTimeout</a>
    <a href="#DialTimeout">¶</a></h3>
<pre>func DialTimeout(network, address <a href="/builtin/#string">string</a>, timeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>) (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

DialTimeout acts like Dial but takes a timeout.

The timeout includes name resolution, if required. When using TCP, and the host
in the address parameter resolves to multiple IP addresses, the timeout is
spread over each consecutive dial, such that each is given an appropriate
fraction of the time to connect.

See func Dial for a description of the network and address parameters.

<h3 id="FileConn">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/file.go#L11">FileConn</a>
    <a href="#FileConn">¶</a></h3>
<pre>func FileConn(f *<a href="/os/">os</a>.<a href="/os/#File">File</a>) (c <a href="#Conn">Conn</a>, err <a href="/builtin/#error">error</a>)</pre>

FileConn returns a copy of the network connection corresponding to the open file
f. It is the caller's responsibility to close f when finished. Closing c does
not affect f, and closing f does not affect c.

<h3 id="Pipe">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/pipe.go#L8">Pipe</a>
    <a href="#Pipe">¶</a></h3>
<pre>func Pipe() (<a href="#Conn">Conn</a>, <a href="#Conn">Conn</a>)</pre>

Pipe creates a synchronous, in-memory, full duplex network connection; both ends
implement the Conn interface. Reads on one end are matched with writes on the
other, copying data directly between the two; there is no internal buffering.

<h2 id="DNSConfigError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L533">DNSConfigError</a>
    <a href="#DNSConfigError">¶</a></h2>
<pre>type DNSConfigError struct {
<span id="DNSConfigError.Err"></span>    Err <a href="/builtin/#error">error</a>
}</pre>

DNSConfigError represents an error reading the machine's DNS configuration. (No
longer used; kept for compatibility.)

<h3 id="DNSConfigError.Error">func (*DNSConfigError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L537">Error</a>
    <a href="#DNSConfigError.Error">¶</a></h3>
<pre>func (e *<a href="#DNSConfigError">DNSConfigError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="DNSConfigError.Temporary">func (*DNSConfigError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L539">Temporary</a>
    <a href="#DNSConfigError.Temporary">¶</a></h3>
<pre>func (e *<a href="#DNSConfigError">DNSConfigError</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>


<h3 id="DNSConfigError.Timeout">func (*DNSConfigError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L538">Timeout</a>
    <a href="#DNSConfigError.Timeout">¶</a></h3>
<pre>func (e *<a href="#DNSConfigError">DNSConfigError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>


<h2 id="DNSError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L547">DNSError</a>
    <a href="#DNSError">¶</a></h2>
<pre>type DNSError struct {
<span id="DNSError.Err"></span>    Err         <a href="/builtin/#string">string</a> <span class="comment">// description of the error</span>
<span id="DNSError.Name"></span>    Name        <a href="/builtin/#string">string</a> <span class="comment">// name looked for</span>
<span id="DNSError.Server"></span>    Server      <a href="/builtin/#string">string</a> <span class="comment">// server used</span>
<span id="DNSError.IsTimeout"></span>    IsTimeout   <a href="/builtin/#bool">bool</a>   <span class="comment">// if true, timed out; not all timeouts set this</span>
<span id="DNSError.IsTemporary"></span>    IsTemporary <a href="/builtin/#bool">bool</a>   <span class="comment">// if true, error is temporary; not all errors set this</span>
}</pre>

DNSError represents a DNS lookup error.

<h3 id="DNSError.Error">func (*DNSError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L555">Error</a>
    <a href="#DNSError.Error">¶</a></h3>
<pre>func (e *<a href="#DNSError">DNSError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="DNSError.Temporary">func (*DNSError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L575">Temporary</a>
    <a href="#DNSError.Temporary">¶</a></h3>
<pre>func (e *<a href="#DNSError">DNSError</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>

Temporary reports whether the DNS error is known to be temporary. This is not
always known; a DNS lookup may fail due to a temporary error and return a
DNSError for which Temporary returns false.

<h3 id="DNSError.Timeout">func (*DNSError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L570">Timeout</a>
    <a href="#DNSError.Timeout">¶</a></h3>
<pre>func (e *<a href="#DNSError">DNSError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>

Timeout reports whether the DNS lookup is known to have timed out. This is not
always known; a DNS lookup may fail due to a timeout and return a DNSError for
which Timeout returns false.

<h2 id="Dialer">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L9">Dialer</a>
    <a href="#Dialer">¶</a></h2>
<pre>type Dialer struct {
<span id="Dialer.Timeout"></span>    <span class="comment">// Timeout is the maximum amount of time a dial will wait for</span>
    <span class="comment">// a connect to complete. If Deadline is also set, it may fail</span>
    <span class="comment">// earlier.</span>
    <span class="comment">//</span>
    <span class="comment">// The default is no timeout.</span>
    <span class="comment">//</span>
    <span class="comment">// When using TCP and dialing a host name with multiple IP</span>
    <span class="comment">// addresses, the timeout may be divided between them.</span>
    <span class="comment">//</span>
    <span class="comment">// With or without a timeout, the operating system may impose</span>
    <span class="comment">// its own earlier timeout. For instance, TCP timeouts are</span>
    <span class="comment">// often around 3 minutes.</span>
    Timeout <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Dialer.Deadline"></span>    <span class="comment">// Deadline is the absolute point in time after which dials</span>
    <span class="comment">// will fail. If Timeout is set, it may fail earlier.</span>
    <span class="comment">// Zero means no deadline, or dependent on the operating system</span>
    <span class="comment">// as with the Timeout option.</span>
    Deadline <a href="/time/">time</a>.<a href="/time/#Time">Time</a>

<span id="Dialer.LocalAddr"></span>    <span class="comment">// LocalAddr is the local address to use when dialing an</span>
    <span class="comment">// address. The address must be of a compatible type for the</span>
    <span class="comment">// network being dialed.</span>
    <span class="comment">// If nil, a local address is automatically chosen.</span>
    LocalAddr <a href="#Addr">Addr</a>

<span id="Dialer.DualStack"></span>    <span class="comment">// DualStack enables RFC 6555-compliant &#34;Happy Eyeballs&#34;</span>
    <span class="comment">// dialing when the network is &#34;tcp&#34; and the host in the</span>
    <span class="comment">// address parameter resolves to both IPv4 and IPv6 addresses.</span>
    <span class="comment">// This allows a client to tolerate networks where one address</span>
    <span class="comment">// family is silently broken.</span>
    DualStack <a href="/builtin/#bool">bool</a>

<span id="Dialer.FallbackDelay"></span>    <span class="comment">// FallbackDelay specifies the length of time to wait before</span>
    <span class="comment">// spawning a fallback connection, when DualStack is enabled.</span>
    <span class="comment">// If zero, a default delay of 300ms is used.</span>
    FallbackDelay <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Dialer.KeepAlive"></span>    <span class="comment">// KeepAlive specifies the keep-alive period for an active</span>
    <span class="comment">// network connection.</span>
    <span class="comment">// If zero, keep-alives are not enabled. Network protocols</span>
    <span class="comment">// that do not support keep-alives ignore this field.</span>
    KeepAlive <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>

<span id="Dialer.Resolver"></span>    <span class="comment">// Resolver optionally specifies an alternate resolver to use.</span>
    Resolver *<a href="#Resolver">Resolver</a>

<span id="Dialer.Cancel"></span>    <span class="comment">// Cancel is an optional channel whose closure indicates that</span>
    <span class="comment">// the dial should be canceled. Not all types of dials support</span>
    <span class="comment">// cancelation.</span>
    <span class="comment">//</span>
    <span class="comment">// Deprecated: Use DialContext instead.</span>
    Cancel &lt;-chan struct{}
}</pre>

A Dialer contains options for connecting to an address.

The zero value for each field is equivalent to dialing without that option.
Dialing with the zero value of Dialer is therefore equivalent to just calling
the Dial function.

<h3 id="Dialer.Dial">func (*Dialer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L309">Dial</a>
    <a href="#Dialer.Dial">¶</a></h3>
<pre>func (d *<a href="#Dialer">Dialer</a>) Dial(network, address <a href="/builtin/#string">string</a>) (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

Dial connects to the address on the named network.

See func Dial for a description of the network and address parameters.

<h3 id="Dialer.DialContext">func (*Dialer) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L331">DialContext</a>
    <a href="#Dialer.DialContext">¶</a></h3>
<pre>func (d *<a href="#Dialer">Dialer</a>) DialContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, network, address <a href="/builtin/#string">string</a>) (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

DialContext connects to the address on the named network using the provided
context.

The provided Context must be non-nil. If the context expires before the
connection is complete, an error is returned. Once successfully connected, any
expiration of the context will not affect the connection.

When using TCP, and the host in the address parameter resolves to multiple
network addresses, any dial timeout (from d.Timeout or ctx) is spread over each
consecutive dial, such that each is given an appropriate fraction of the time to
connect. For example, if a host has 4 IP addresses and the timeout is 1 minute,
the connect to each single address will be given 15 seconds to complete before
trying the next one.

See func Dial for a description of the network and address parameters.

<h2 id="Error">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L364">Error</a>
    <a href="#Error">¶</a></h2>
<pre>type Error interface {
    <a href="/builtin/#error">error</a>
    Timeout() <a href="/builtin/#bool">bool</a>   <span class="comment">// Is the error a timeout?</span>
    Temporary() <a href="/builtin/#bool">bool</a> <span class="comment">// Is the error temporary?</span>
}</pre>

An Error represents a network error.

<h2 id="Flags">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L28">Flags</a>
    <a href="#Flags">¶</a></h2>
<pre>type Flags <a href="/builtin/#uint">uint</a></pre>


<pre>const (
    <span id="FlagUp">FlagUp</span>           <a href="#Flags">Flags</a> = 1 &lt;&lt; <a href="/builtin/#iota">iota</a> <span class="comment">// interface is up</span>
    <span id="FlagBroadcast">FlagBroadcast</span>                      <span class="comment">// interface supports broadcast access capability</span>
    <span id="FlagLoopback">FlagLoopback</span>                       <span class="comment">// interface is a loopback interface</span>
    <span id="FlagPointToPoint">FlagPointToPoint</span>                   <span class="comment">// interface belongs to a point-to-point link</span>
    <span id="FlagMulticast">FlagMulticast</span>                      <span class="comment">// interface supports multicast access capability</span>
)</pre>


<h3 id="Flags.String">func (Flags) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L46">String</a>
    <a href="#Flags.String">¶</a></h3>
<pre>func (f <a href="#Flags">Flags</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="HardwareAddr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/mac.go#L1">HardwareAddr</a>
    <a href="#HardwareAddr">¶</a></h2>
<pre>type HardwareAddr []<a href="/builtin/#byte">byte</a></pre>

A HardwareAddr represents a physical hardware address.

<h3 id="ParseMAC">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/mac.go#L28">ParseMAC</a>
    <a href="#ParseMAC">¶</a></h3>
<pre>func ParseMAC(s <a href="/builtin/#string">string</a>) (hw <a href="#HardwareAddr">HardwareAddr</a>, err <a href="/builtin/#error">error</a>)</pre>

ParseMAC parses s as an IEEE 802 MAC-48, EUI-48, EUI-64, or a 20-octet IP over
InfiniBand link-layer address using one of the following formats:

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

An IP is a single IP address, a slice of bytes. Functions in this package accept
either 4-byte (IPv4) or 16-byte (IPv6) slices as input.

Note that in this documentation, referring to an IP address as an IPv4 address
or an IPv6 address is a semantic property of the address, not just the length of
the byte slice: a 16-byte slice can still be an IPv4 address.

<h3 id="IPv4">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L35">IPv4</a>
    <a href="#IPv4">¶</a></h3>
<pre>func IPv4(a, b, c, d <a href="/builtin/#byte">byte</a>) <a href="#IP">IP</a></pre>

IPv4 returns the IP address (in 16-byte form) of the IPv4 address a.b.c.d.

<a id="example_IPv4"></a>
Example:

    fmt.Println(net.IPv4(8, 8, 8, 8))

    // Output:
    // 8.8.8.8

<h3 id="ParseCIDR">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L657">ParseCIDR</a>
    <a href="#ParseCIDR">¶</a></h3>
<pre>func ParseCIDR(s <a href="/builtin/#string">string</a>) (<a href="#IP">IP</a>, *<a href="#IPNet">IPNet</a>, <a href="/builtin/#error">error</a>)</pre>

ParseCIDR parses s as a CIDR notation IP address and prefix length, like
"192.0.2.0/24" or "2001:db8::/32", as defined in RFC 4632 and RFC 4291.

It returns the IP address and the network implied by the IP and prefix length.
For example, ParseCIDR("192.0.2.1/24") returns the IP address 192.0.2.1 and the
network 192.0.2.0/24.

<a id="example_ParseCIDR"></a>
Example:

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

ParseIP parses s as an IP address, returning the result. The string s can be in
dotted decimal ("192.0.2.1") or IPv6 ("2001:db8::68") form. If s is not a valid
textual representation of an IP address, ParseIP returns nil.

<a id="example_ParseIP"></a>
Example:

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

DefaultMask returns the default IP mask for the IP address ip. Only IPv4
addresses have default masks; DefaultMask returns nil if ip is not a valid IPv4
address.

<a id="example_IP_DefaultMask"></a>
Example:

    ip := net.ParseIP("192.0.2.1")
    fmt.Println(ip.DefaultMask())

    // Output:
    // ffffff00

<h3 id="IP.Equal">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L365">Equal</a>
    <a href="#IP.Equal">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) Equal(x <a href="#IP">IP</a>) <a href="/builtin/#bool">bool</a></pre>

Equal reports whether ip and x are the same IP address. An IPv4 address and that
same address in IPv6 form are considered to be equal.

<h3 id="IP.IsGlobalUnicast">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L155">IsGlobalUnicast</a>
    <a href="#IP.IsGlobalUnicast">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsGlobalUnicast() <a href="/builtin/#bool">bool</a></pre>

IsGlobalUnicast reports whether ip is a global unicast address.

The identification of global unicast addresses uses address type identification
as defined in RFC 1122, RFC 4632 and RFC 4291 with the exception of IPv4
directed broadcast addresses. It returns true even if ip is in IPv4 private
address space or local IPv6 unicast address space.

<h3 id="IP.IsInterfaceLocalMulticast">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L125">IsInterfaceLocalMulticast</a>
    <a href="#IP.IsInterfaceLocalMulticast">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsInterfaceLocalMulticast() <a href="/builtin/#bool">bool</a></pre>

IsInterfaceLocalMulticast reports whether ip is an interface-local multicast
address.

<h3 id="IP.IsLinkLocalMulticast">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L131">IsLinkLocalMulticast</a>
    <a href="#IP.IsLinkLocalMulticast">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsLinkLocalMulticast() <a href="/builtin/#bool">bool</a></pre>

IsLinkLocalMulticast reports whether ip is a link-local multicast address.

<h3 id="IP.IsLinkLocalUnicast">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L140">IsLinkLocalUnicast</a>
    <a href="#IP.IsLinkLocalUnicast">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsLinkLocalUnicast() <a href="/builtin/#bool">bool</a></pre>

IsLinkLocalUnicast reports whether ip is a link-local unicast address.

<h3 id="IP.IsLoopback">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L108">IsLoopback</a>
    <a href="#IP.IsLoopback">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsLoopback() <a href="/builtin/#bool">bool</a></pre>

IsLoopback reports whether ip is a loopback address.

<h3 id="IP.IsMulticast">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L116">IsMulticast</a>
    <a href="#IP.IsMulticast">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsMulticast() <a href="/builtin/#bool">bool</a></pre>

IsMulticast reports whether ip is a multicast address.

<h3 id="IP.IsUnspecified">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L103">IsUnspecified</a>
    <a href="#IP.IsUnspecified">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) IsUnspecified() <a href="/builtin/#bool">bool</a></pre>

IsUnspecified reports whether ip is an unspecified address, either the IPv4
address "0.0.0.0" or the IPv6 address "::".

<h3 id="IP.MarshalText">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L336">MarshalText</a>
    <a href="#IP.MarshalText">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) MarshalText() ([]<a href="/builtin/#byte">byte</a>, <a href="/builtin/#error">error</a>)</pre>

MarshalText implements the encoding.TextMarshaler interface. The encoding is the
same as returned by String, with one exception: When len(ip) is zero, it returns
an empty slice.

<h3 id="IP.Mask">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L235">Mask</a>
    <a href="#IP.Mask">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) Mask(mask <a href="#IPMask">IPMask</a>) <a href="#IP">IP</a></pre>

Mask returns the result of masking the IP address ip with mask.

<a id="example_IP_Mask"></a>
Example:

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

String returns the string form of the IP address ip. It returns one of 4 forms:

    - "<nil>", if ip has length 0
    - dotted decimal ("192.0.2.1"), if ip is an IPv4 or IP4-mapped IPv6 address
    - IPv6 ("2001:db8::1"), if ip is a valid IPv6 address
    - the hexadecimal form of ip, without punctuation, if no other cases apply

<h3 id="IP.To16">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L191">To16</a>
    <a href="#IP.To16">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) To16() <a href="#IP">IP</a></pre>

To16 converts the IP address ip to a 16-byte representation. If ip is not an IP
address (it is the wrong length), To16 returns nil.

<h3 id="IP.To4">func (IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L176">To4</a>
    <a href="#IP.To4">¶</a></h3>
<pre>func (ip <a href="#IP">IP</a>) To4() <a href="#IP">IP</a></pre>

To4 converts the IPv4 address ip to a 4-byte representation. If ip is not an
IPv4 address, To4 returns nil.

<h3 id="IP.UnmarshalText">func (*IP) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L348">UnmarshalText</a>
    <a href="#IP.UnmarshalText">¶</a></h3>
<pre>func (ip *<a href="#IP">IP</a>) UnmarshalText(text []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

UnmarshalText implements the encoding.TextUnmarshaler interface. The IP address
is expected in a form accepted by ParseIP.

<h2 id="IPAddr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L21">IPAddr</a>
    <a href="#IPAddr">¶</a></h2>
<pre>type IPAddr struct {
<span id="IPAddr.IP"></span>    IP   <a href="#IP">IP</a>
<span id="IPAddr.Zone"></span>    Zone <a href="/builtin/#string">string</a> <span class="comment">// IPv6 scoped addressing zone</span>
}</pre>

IPAddr represents the address of an IP end point.

<h3 id="ResolveIPAddr">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L67">ResolveIPAddr</a>
    <a href="#ResolveIPAddr">¶</a></h3>
<pre>func ResolveIPAddr(network, address <a href="/builtin/#string">string</a>) (*<a href="#IPAddr">IPAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ResolveIPAddr returns an address of IP end point.

The network must be an IP network name.

If the host in the address parameter is not a literal IP address, ResolveIPAddr
resolves the address to an address of IP end point. Otherwise, it parses the
address as a literal IP address. The address parameter can use a host name, but
this is not recommended, because it will return at most one of the host name's
IP addresses.

See func Dial for a description of the network and address parameters.

<h3 id="IPAddr.Network">func (*IPAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L27">Network</a>
    <a href="#IPAddr.Network">¶</a></h3>
<pre>func (a *<a href="#IPAddr">IPAddr</a>) Network() <a href="/builtin/#string">string</a></pre>

Network returns the address's network name, "ip".

<h3 id="IPAddr.String">func (*IPAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L29">String</a>
    <a href="#IPAddr.String">¶</a></h3>
<pre>func (a *<a href="#IPAddr">IPAddr</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="IPConn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L89">IPConn</a>
    <a href="#IPConn">¶</a></h2>
<pre>type IPConn struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

IPConn is the implementation of the Conn and PacketConn interfaces for IP
network connections.

<h3 id="DialIP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L201">DialIP</a>
    <a href="#DialIP">¶</a></h3>
<pre>func DialIP(network <a href="/builtin/#string">string</a>, laddr, raddr *<a href="#IPAddr">IPAddr</a>) (*<a href="#IPConn">IPConn</a>, <a href="/builtin/#error">error</a>)</pre>

DialIP acts like Dial for IP networks.

The network must be an IP network name; see func Dial for details.

If laddr is nil, a local address is automatically chosen. If the IP field of
raddr is nil or an unspecified IP address, the local system is assumed.

<h3 id="ListenIP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L216">ListenIP</a>
    <a href="#ListenIP">¶</a></h3>
<pre>func ListenIP(network <a href="/builtin/#string">string</a>, laddr *<a href="#IPAddr">IPAddr</a>) (*<a href="#IPConn">IPConn</a>, <a href="/builtin/#error">error</a>)</pre>

ListenIP acts like ListenPacket for IP networks.

The network must be an IP network name; see func Dial for details.

If the IP field of laddr is nil or an unspecified IP address, ListenIP listens
on all available IP addresses of the local system except multicast IP addresses.

<h3 id="IPConn.Close">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L186">Close</a>
    <a href="#IPConn.Close">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the connection.

<h3 id="IPConn.File">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L281">File</a>
    <a href="#IPConn.File">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) File() (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

File sets the underlying os.File to blocking mode and returns a copy. It is the
caller's responsibility to close f when finished. Closing c does not affect f,
and closing f does not affect c.

The returned os.File's file descriptor is different from the connection's.
Attempting to change properties of the original using this duplicate may or may
not have the desired effect.

<h3 id="IPConn.LocalAddr">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L200">LocalAddr</a>
    <a href="#IPConn.LocalAddr">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) LocalAddr() <a href="#Addr">Addr</a></pre>

LocalAddr returns the local network address. The Addr returned is shared by all
invocations of LocalAddr, so do not modify it.

<h3 id="IPConn.Read">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L162">Read</a>
    <a href="#IPConn.Read">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) Read(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Read implements the Conn Read method.

<h3 id="IPConn.ReadFrom">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L115">ReadFrom</a>
    <a href="#IPConn.ReadFrom">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) ReadFrom(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="#Addr">Addr</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFrom implements the PacketConn ReadFrom method.

<h3 id="IPConn.ReadFromIP">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L103">ReadFromIP</a>
    <a href="#IPConn.ReadFromIP">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) ReadFromIP(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, *<a href="#IPAddr">IPAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFromIP acts like ReadFrom but returns an IPAddr.

<h3 id="IPConn.ReadMsgIP">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L136">ReadMsgIP</a>
    <a href="#IPConn.ReadMsgIP">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) ReadMsgIP(b, oob []<a href="/builtin/#byte">byte</a>) (n, oobn, flags <a href="/builtin/#int">int</a>, addr *<a href="#IPAddr">IPAddr</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadMsgIP reads a message from c, copying the payload into b and the associated
out-of-band data into oob. It returns the number of bytes copied into b, the
number of bytes copied into oob, the flags that were set on the message and the
source address of the message.

The packages golang.org/x/net/ipv4 and golang.org/x/net/ipv6 can be used to
manipulate IP-level socket options in oob.

<h3 id="IPConn.RemoteAddr">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L210">RemoteAddr</a>
    <a href="#IPConn.RemoteAddr">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) RemoteAddr() <a href="#Addr">Addr</a></pre>

RemoteAddr returns the remote network address. The Addr returned is shared by
all invocations of RemoteAddr, so do not modify it.

<h3 id="IPConn.SetDeadline">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L218">SetDeadline</a>
    <a href="#IPConn.SetDeadline">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline implements the Conn SetDeadline method.

<h3 id="IPConn.SetReadBuffer">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L252">SetReadBuffer</a>
    <a href="#IPConn.SetReadBuffer">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) SetReadBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetReadBuffer sets the size of the operating system's receive buffer associated
with the connection.

<h3 id="IPConn.SetReadDeadline">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L229">SetReadDeadline</a>
    <a href="#IPConn.SetReadDeadline">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetReadDeadline implements the Conn SetReadDeadline method.

<h3 id="IPConn.SetWriteBuffer">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L264">SetWriteBuffer</a>
    <a href="#IPConn.SetWriteBuffer">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) SetWriteBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetWriteBuffer sets the size of the operating system's transmit buffer
associated with the connection.

<h3 id="IPConn.SetWriteDeadline">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L240">SetWriteDeadline</a>
    <a href="#IPConn.SetWriteDeadline">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetWriteDeadline implements the Conn SetWriteDeadline method.

<h3 id="IPConn.SyscallConn">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L95">SyscallConn</a>
    <a href="#IPConn.SyscallConn">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) SyscallConn() (<a href="/syscall/">syscall</a>.<a href="/syscall/#RawConn">RawConn</a>, <a href="/builtin/#error">error</a>)</pre>

SyscallConn returns a raw network connection. This implements the syscall.Conn
interface.

<h3 id="IPConn.Write">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L174">Write</a>
    <a href="#IPConn.Write">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) Write(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write implements the Conn Write method.

<h3 id="IPConn.WriteMsgIP">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L181">WriteMsgIP</a>
    <a href="#IPConn.WriteMsgIP">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) WriteMsgIP(b, oob []<a href="/builtin/#byte">byte</a>, addr *<a href="#IPAddr">IPAddr</a>) (n, oobn <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteMsgIP writes a message to addr via c, copying the payload from b and the
associated out-of-band data from oob. It returns the number of payload and
out-of-band bytes written.

The packages golang.org/x/net/ipv4 and golang.org/x/net/ipv6 can be used to
manipulate IP-level socket options in oob.

<h3 id="IPConn.WriteTo">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L160">WriteTo</a>
    <a href="#IPConn.WriteTo">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) WriteTo(b []<a href="/builtin/#byte">byte</a>, addr <a href="#Addr">Addr</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteTo implements the PacketConn WriteTo method.

<h3 id="IPConn.WriteToIP">func (*IPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L148">WriteToIP</a>
    <a href="#IPConn.WriteToIP">¶</a></h3>
<pre>func (c *<a href="#IPConn">IPConn</a>) WriteToIP(b []<a href="/builtin/#byte">byte</a>, addr *<a href="#IPAddr">IPAddr</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteToIP acts like WriteTo but takes an IPAddr.

<h2 id="IPMask">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L25">IPMask</a>
    <a href="#IPMask">¶</a></h2>
<pre>type IPMask []<a href="/builtin/#byte">byte</a></pre>

An IP mask is an IP address.

<h3 id="CIDRMask">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L61">CIDRMask</a>
    <a href="#CIDRMask">¶</a></h3>
<pre>func CIDRMask(ones, bits <a href="/builtin/#int">int</a>) <a href="#IPMask">IPMask</a></pre>

CIDRMask returns an IPMask consisting of `ones' 1 bits followed by 0s up to a
total length of `bits' bits. For a mask of this form, CIDRMask is the inverse of
IPMask.Size.

<a id="example_CIDRMask"></a>
Example:

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

IPv4Mask returns the IP mask (in 4-byte form) of the IPv4 mask a.b.c.d.

<a id="example_IPv4Mask"></a>
Example:

    fmt.Println(net.IPv4Mask(255, 255, 255, 0))

    // Output:
    // ffffff00

<h3 id="IPMask.Size">func (IPMask) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L418">Size</a>
    <a href="#IPMask.Size">¶</a></h3>
<pre>func (m <a href="#IPMask">IPMask</a>) Size() (ones, bits <a href="/builtin/#int">int</a>)</pre>

Size returns the number of leading ones and total bits in the mask. If the mask
is not in the canonical form--ones followed by zeros--then Size returns 0, 0.

<h3 id="IPMask.String">func (IPMask) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L427">String</a>
    <a href="#IPMask.String">¶</a></h3>
<pre>func (m <a href="#IPMask">IPMask</a>) String() <a href="/builtin/#string">string</a></pre>

String returns the hexadecimal form of m, with no punctuation.

<h2 id="IPNet">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L28">IPNet</a>
    <a href="#IPNet">¶</a></h2>
<pre>type IPNet struct {
<span id="IPNet.IP"></span>    IP   <a href="#IP">IP</a>     <span class="comment">// network number</span>
<span id="IPNet.Mask"></span>    Mask <a href="#IPMask">IPMask</a> <span class="comment">// network mask</span>
}</pre>

An IPNet represents an IP network.

<h3 id="IPNet.Contains">func (*IPNet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L458">Contains</a>
    <a href="#IPNet.Contains">¶</a></h3>
<pre>func (n *<a href="#IPNet">IPNet</a>) Contains(ip <a href="#IP">IP</a>) <a href="/builtin/#bool">bool</a></pre>

Contains reports whether the network includes ip.

<h3 id="IPNet.Network">func (*IPNet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L476">Network</a>
    <a href="#IPNet.Network">¶</a></h3>
<pre>func (n *<a href="#IPNet">IPNet</a>) Network() <a href="/builtin/#string">string</a></pre>

Network returns the address's network name, "ip+net".

<h3 id="IPNet.String">func (*IPNet) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ip.go#L484">String</a>
    <a href="#IPNet.String">¶</a></h3>
<pre>func (n *<a href="#IPNet">IPNet</a>) String() <a href="/builtin/#string">string</a></pre>

String returns the CIDR notation of n like "192.0.2.1/24" or "2001:db8::/48" as
defined in RFC 4632 and RFC 4291. If the mask is not in the canonical form, it
returns the string which consists of an IP address, followed by a slash
character and a mask expressed as hexadecimal form with no punctuation like
"198.51.100.1/c000ff00".

<h2 id="Interface">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L20">Interface</a>
    <a href="#Interface">¶</a></h2>
<pre>type Interface struct {
<span id="Interface.Index"></span>    Index        <a href="/builtin/#int">int</a>          <span class="comment">// positive integer that starts at one, zero is never used</span>
<span id="Interface.MTU"></span>    MTU          <a href="/builtin/#int">int</a>          <span class="comment">// maximum transmission unit</span>
<span id="Interface.Name"></span>    Name         <a href="/builtin/#string">string</a>       <span class="comment">// e.g., &#34;en0&#34;, &#34;lo0&#34;, &#34;eth0.100&#34;</span>
<span id="Interface.HardwareAddr"></span>    HardwareAddr <a href="#HardwareAddr">HardwareAddr</a> <span class="comment">// IEEE MAC-48, EUI-48 and EUI-64 form</span>
<span id="Interface.Flags"></span>    Flags        <a href="#Flags">Flags</a>        <span class="comment">// e.g., FlagUp, FlagLoopback, FlagMulticast</span>
}</pre>

Interface represents a mapping between network interface name and index. It also
represents network interface facility information.

<h3 id="InterfaceByIndex">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L118">InterfaceByIndex</a>
    <a href="#InterfaceByIndex">¶</a></h3>
<pre>func InterfaceByIndex(index <a href="/builtin/#int">int</a>) (*<a href="#Interface">Interface</a>, <a href="/builtin/#error">error</a>)</pre>

InterfaceByIndex returns the interface specified by index.

On Solaris, it returns one of the logical network interfaces sharing the logical
data link; for more precision use InterfaceByName.

<h3 id="InterfaceByName">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L143">InterfaceByName</a>
    <a href="#InterfaceByName">¶</a></h3>
<pre>func InterfaceByName(name <a href="/builtin/#string">string</a>) (*<a href="#Interface">Interface</a>, <a href="/builtin/#error">error</a>)</pre>

InterfaceByName returns the interface specified by name.

<h3 id="Interface.Addrs">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L64">Addrs</a>
    <a href="#Interface.Addrs">¶</a></h3>
<pre>func (ifi *<a href="#Interface">Interface</a>) Addrs() ([]<a href="#Addr">Addr</a>, <a href="/builtin/#error">error</a>)</pre>

Addrs returns a list of unicast interface addresses for a specific interface.

<h3 id="Interface.MulticastAddrs">func (*Interface) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L77">MulticastAddrs</a>
    <a href="#Interface.MulticastAddrs">¶</a></h3>
<pre>func (ifi *<a href="#Interface">Interface</a>) MulticastAddrs() ([]<a href="#Addr">Addr</a>, <a href="/builtin/#error">error</a>)</pre>

MulticastAddrs returns a list of multicast, joined group addresses for a
specific interface.

<h2 id="InvalidAddrError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L525">InvalidAddrError</a>
    <a href="#InvalidAddrError">¶</a></h2>
<pre>type InvalidAddrError <a href="/builtin/#string">string</a></pre>


<h3 id="InvalidAddrError.Error">func (InvalidAddrError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L527">Error</a>
    <a href="#InvalidAddrError.Error">¶</a></h3>
<pre>func (e <a href="#InvalidAddrError">InvalidAddrError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="InvalidAddrError.Temporary">func (InvalidAddrError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L529">Temporary</a>
    <a href="#InvalidAddrError.Temporary">¶</a></h3>
<pre>func (e <a href="#InvalidAddrError">InvalidAddrError</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>


<h3 id="InvalidAddrError.Timeout">func (InvalidAddrError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L528">Timeout</a>
    <a href="#InvalidAddrError.Timeout">¶</a></h3>
<pre>func (e <a href="#InvalidAddrError">InvalidAddrError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>


<h2 id="Listener">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L351">Listener</a>
    <a href="#Listener">¶</a></h2>
<pre>type Listener interface {
    <span class="comment">// Accept waits for and returns the next connection to the listener.</span>
    Accept() (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)

    <span class="comment">// Close closes the listener.</span>
    <span class="comment">// Any blocked Accept operations will be unblocked and return errors.</span>
    Close() <a href="/builtin/#error">error</a>

    <span class="comment">// Addr returns the listener&#39;s network address.</span>
    Addr() <a href="#Addr">Addr</a>
}</pre>

A Listener is a generic network listener for stream-oriented protocols.

Multiple goroutines may invoke methods on a Listener simultaneously.

<a id="example_Listener"></a>
Example:

    // Listen on TCP port 2000 on all available unicast and
    // anycast IP addresses of the local system.
    l, err := net.Listen("tcp", ":2000")
    if err != nil {
        log.Fatal(err)
    }
    defer l.Close()
    for {
        // Wait for a connection.
        conn, err := l.Accept()
        if err != nil {
            log.Fatal(err)
        }
        // Handle the connection in a new goroutine.
        // The loop then returns to accepting, so that
        // multiple connections may be served concurrently.
        go func(c net.Conn) {
            // Echo all incoming data.
            io.Copy(c, c)
            // Shut down the connection.
            c.Close()
        }(conn)
    }

<h3 id="FileListener">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/file.go#L23">FileListener</a>
    <a href="#FileListener">¶</a></h3>
<pre>func FileListener(f *<a href="/os/">os</a>.<a href="/os/#File">File</a>) (ln <a href="#Listener">Listener</a>, err <a href="/builtin/#error">error</a>)</pre>

FileListener returns a copy of the network listener corresponding to the open
file f. It is the caller's responsibility to close ln when finished. Closing ln
does not affect f, and closing f does not affect ln.

<h3 id="Listen">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L574">Listen</a>
    <a href="#Listen">¶</a></h3>
<pre>func Listen(network, address <a href="/builtin/#string">string</a>) (<a href="#Listener">Listener</a>, <a href="/builtin/#error">error</a>)</pre>

Listen announces on the local network address.

The network must be "tcp", "tcp4", "tcp6", "unix" or "unixpacket".

For TCP networks, if the host in the address parameter is empty or a literal
unspecified IP address, Listen listens on all available unicast and anycast IP
addresses of the local system. To only use IPv4, use network "tcp4". The address
can use a host name, but this is not recommended, because it will create a
listener for at most one of the host's IP addresses. If the port in the address
parameter is empty or "0", as in "127.0.0.1:" or "[::1]:0", a port number is
automatically chosen. The Addr method of Listener can be used to discover the
chosen port.

See func Dial for a description of the network and address parameters.

<h2 id="MX">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsclient.go#L239">MX</a>
    <a href="#MX">¶</a></h2>
<pre>type MX struct {
<span id="MX.Host"></span>    Host <a href="/builtin/#string">string</a>
<span id="MX.Pref"></span>    Pref <a href="/builtin/#uint16">uint16</a>
}</pre>

An MX represents a single DNS MX record.

<h2 id="NS">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsclient.go#L261">NS</a>
    <a href="#NS">¶</a></h2>
<pre>type NS struct {
<span id="NS.Host"></span>    Host <a href="/builtin/#string">string</a>
}</pre>

An NS represents a single DNS NS record.

<h2 id="OpError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L402">OpError</a>
    <a href="#OpError">¶</a></h2>
<pre>type OpError struct {
<span id="OpError.Op"></span>    <span class="comment">// Op is the operation which caused the error, such as</span>
    <span class="comment">// &#34;read&#34; or &#34;write&#34;.</span>
    Op <a href="/builtin/#string">string</a>

<span id="OpError.Net"></span>    <span class="comment">// Net is the network type on which this error occurred,</span>
    <span class="comment">// such as &#34;tcp&#34; or &#34;udp6&#34;.</span>
    Net <a href="/builtin/#string">string</a>

    <span class="comment">// For operations involving a remote network connection, like</span>
    <span class="comment">// Dial, Read, or Write, Source is the corresponding local</span>
    <span class="comment">// network address.</span>
<span id="OpError.Source"></span>    Source <a href="#Addr">Addr</a>

<span id="OpError.Addr"></span>    <span class="comment">// Addr is the network address for which this error occurred.</span>
    <span class="comment">// For local operations, like Listen or SetDeadline, Addr is</span>
    <span class="comment">// the address of the local endpoint being manipulated.</span>
    <span class="comment">// For operations involving a remote network connection, like</span>
    <span class="comment">// Dial, Read, or Write, Addr is the remote address of that</span>
    <span class="comment">// connection.</span>
    Addr <a href="#Addr">Addr</a>

<span id="OpError.Err"></span>    <span class="comment">// Err is the error that occurred during the operation.</span>
    Err <a href="/builtin/#error">error</a>
}</pre>

OpError is the error type usually returned by functions in the net package. It
describes the operation, network type, and address of an error.

<h3 id="OpError.Error">func (*OpError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L428">Error</a>
    <a href="#OpError.Error">¶</a></h3>
<pre>func (e *<a href="#OpError">OpError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="OpError.Temporary">func (*OpError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L479">Temporary</a>
    <a href="#OpError.Temporary">¶</a></h3>
<pre>func (e *<a href="#OpError">OpError</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>


<h3 id="OpError.Timeout">func (*OpError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L466">Timeout</a>
    <a href="#OpError.Timeout">¶</a></h3>
<pre>func (e *<a href="#OpError">OpError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>


<h2 id="PacketConn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L292">PacketConn</a>
    <a href="#PacketConn">¶</a></h2>
<pre>type PacketConn interface {
    <span class="comment">// ReadFrom reads a packet from the connection,</span>
    <span class="comment">// copying the payload into b. It returns the number of</span>
    <span class="comment">// bytes copied into b and the return address that</span>
    <span class="comment">// was on the packet.</span>
    <span class="comment">// ReadFrom can be made to time out and return</span>
    <span class="comment">// an Error with Timeout() == true after a fixed time limit;</span>
    <span class="comment">// see SetDeadline and SetReadDeadline.</span>
    ReadFrom(b []<a href="/builtin/#byte">byte</a>) (n <a href="/builtin/#int">int</a>, addr <a href="#Addr">Addr</a>, err <a href="/builtin/#error">error</a>)

    <span class="comment">// WriteTo writes a packet with payload b to addr.</span>
    <span class="comment">// WriteTo can be made to time out and return</span>
    <span class="comment">// an Error with Timeout() == true after a fixed time limit;</span>
    <span class="comment">// see SetDeadline and SetWriteDeadline.</span>
    <span class="comment">// On packet-oriented connections, write timeouts are rare.</span>
    WriteTo(b []<a href="/builtin/#byte">byte</a>, addr <a href="#Addr">Addr</a>) (n <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)

    <span class="comment">// Close closes the connection.</span>
    <span class="comment">// Any blocked ReadFrom or WriteTo operations will be unblocked and return errors.</span>
    Close() <a href="/builtin/#error">error</a>

    <span class="comment">// LocalAddr returns the local network address.</span>
    LocalAddr() <a href="#Addr">Addr</a>

    <span class="comment">// SetDeadline sets the read and write deadlines associated</span>
    <span class="comment">// with the connection. It is equivalent to calling both</span>
    <span class="comment">// SetReadDeadline and SetWriteDeadline.</span>
    <span class="comment">//</span>
    <span class="comment">// A deadline is an absolute time after which I/O operations</span>
    <span class="comment">// fail with a timeout (see type Error) instead of</span>
    <span class="comment">// blocking. The deadline applies to all future and pending</span>
    <span class="comment">// I/O, not just the immediately following call to ReadFrom or</span>
    <span class="comment">// WriteTo. After a deadline has been exceeded, the connection</span>
    <span class="comment">// can be refreshed by setting a deadline in the future.</span>
    <span class="comment">//</span>
    <span class="comment">// An idle timeout can be implemented by repeatedly extending</span>
    <span class="comment">// the deadline after successful ReadFrom or WriteTo calls.</span>
    <span class="comment">//</span>
    <span class="comment">// A zero value for t means I/O operations will not time out.</span>
    SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a>

    <span class="comment">// SetReadDeadline sets the deadline for future ReadFrom calls</span>
    <span class="comment">// and any currently-blocked ReadFrom call.</span>
    <span class="comment">// A zero value for t means ReadFrom will not time out.</span>
    SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a>

    <span class="comment">// SetWriteDeadline sets the deadline for future WriteTo calls</span>
    <span class="comment">// and any currently-blocked WriteTo call.</span>
    <span class="comment">// Even if write times out, it may return n &gt; 0, indicating that</span>
    <span class="comment">// some of the data was successfully written.</span>
    <span class="comment">// A zero value for t means WriteTo will not time out.</span>
    SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a>
}</pre>

PacketConn is a generic packet-oriented network connection.

Multiple goroutines may invoke methods on a PacketConn simultaneously.

<h3 id="FilePacketConn">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/file.go#L35">FilePacketConn</a>
    <a href="#FilePacketConn">¶</a></h3>
<pre>func FilePacketConn(f *<a href="/os/">os</a>.<a href="/os/#File">File</a>) (c <a href="#PacketConn">PacketConn</a>, err <a href="/builtin/#error">error</a>)</pre>

FilePacketConn returns a copy of the packet network connection corresponding to
the open file f. It is the caller's responsibility to close f when finished.
Closing c does not affect f, and closing f does not affect c.

<h3 id="ListenPacket">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dial.go#L616">ListenPacket</a>
    <a href="#ListenPacket">¶</a></h3>
<pre>func ListenPacket(network, address <a href="/builtin/#string">string</a>) (<a href="#PacketConn">PacketConn</a>, <a href="/builtin/#error">error</a>)</pre>

ListenPacket announces on the local network address.

The network must be "udp", "udp4", "udp6", "unixgram", or an IP transport. The
IP transports are "ip", "ip4", or "ip6" followed by a colon and a literal
protocol number or a protocol name, as in "ip:1" or "ip:icmp".

For UDP and IP networks, if the host in the address parameter is empty or a
literal unspecified IP address, ListenPacket listens on all available IP
addresses of the local system except multicast IP addresses. To only use IPv4,
use network "udp4" or "ip4:proto". The address can use a host name, but this is
not recommended, because it will create a listener for at most one of the host's
IP addresses. If the port in the address parameter is empty or "0", as in
"127.0.0.1:" or "[::1]:0", a port number is automatically chosen. The LocalAddr
method of PacketConn can be used to discover the chosen port.

See func Dial for a description of the network and address parameters.

<h2 id="ParseError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L489">ParseError</a>
    <a href="#ParseError">¶</a></h2>
<pre>type ParseError struct {
<span id="ParseError.Type"></span>    <span class="comment">// Type is the type of string that was expected, such as</span>
    <span class="comment">// &#34;IP address&#34;, &#34;CIDR address&#34;.</span>
    Type <a href="/builtin/#string">string</a>

<span id="ParseError.Text"></span>    <span class="comment">// Text is the malformed text string.</span>
    Text <a href="/builtin/#string">string</a>
}</pre>

A ParseError is the error type of literal network address parsers.

<h3 id="ParseError.Error">func (*ParseError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L498">Error</a>
    <a href="#ParseError.Error">¶</a></h3>
<pre>func (e *<a href="#ParseError">ParseError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="Resolver">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L92">Resolver</a>
    <a href="#Resolver">¶</a></h2>
<pre>type Resolver struct {
<span id="Resolver.PreferGo"></span>    <span class="comment">// PreferGo controls whether Go&#39;s built-in DNS resolver is preferred</span>
    <span class="comment">// on platforms where it&#39;s available. It is equivalent to setting</span>
    <span class="comment">// GODEBUG=netdns=go, but scoped to just this resolver.</span>
    PreferGo <a href="/builtin/#bool">bool</a>

<span id="Resolver.StrictErrors"></span>    <span class="comment">// StrictErrors controls the behavior of temporary errors</span>
    <span class="comment">// (including timeout, socket errors, and SERVFAIL) when using</span>
    <span class="comment">// Go&#39;s built-in resolver. For a query composed of multiple</span>
    <span class="comment">// sub-queries (such as an A+AAAA address lookup, or walking the</span>
    <span class="comment">// DNS search list), this option causes such errors to abort the</span>
    <span class="comment">// whole query instead of returning a partial result. This is</span>
    <span class="comment">// not enabled by default because it may affect compatibility</span>
    <span class="comment">// with resolvers that process AAAA queries incorrectly.</span>
    StrictErrors <a href="/builtin/#bool">bool</a>

<span id="Resolver.Dial"></span>    <span class="comment">// Dial optionally specifies an alternate dialer for use by</span>
    <span class="comment">// Go&#39;s built-in DNS resolver to make TCP and UDP connections</span>
    <span class="comment">// to DNS services. The host in the address parameter will</span>
    <span class="comment">// always be a literal IP address and not a host name, and the</span>
    <span class="comment">// port in the address parameter will be a literal port number</span>
    <span class="comment">// and not a service name.</span>
    <span class="comment">// If the Conn returned is also a PacketConn, sent and received DNS</span>
    <span class="comment">// messages must adhere to RFC 1035 section 4.2.1, &#34;UDP usage&#34;.</span>
    <span class="comment">// Otherwise, DNS messages transmitted over Conn must adhere</span>
    <span class="comment">// to RFC 7766 section 5, &#34;Transport Protocol Selection&#34;.</span>
    <span class="comment">// If nil, the default dialer is used.</span>
    Dial func(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, network, address <a href="/builtin/#string">string</a>) (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)
}</pre>

A Resolver looks up names and numbers.

A nil *Resolver is equivalent to a zero Resolver.

<h3 id="Resolver.LookupAddr">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L355">LookupAddr</a>
    <a href="#Resolver.LookupAddr">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupAddr(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, addr <a href="/builtin/#string">string</a>) (names []<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupAddr performs a reverse lookup for the given address, returning a list of
names mapping to that address.

<h3 id="Resolver.LookupCNAME">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L284">LookupCNAME</a>
    <a href="#Resolver.LookupCNAME">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupCNAME(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, host <a href="/builtin/#string">string</a>) (cname <a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupCNAME returns the canonical name for the given host. Callers that do not
care about the canonical name can call LookupHost or LookupIP directly; both
take care of resolving the canonical name as part of the lookup.

A canonical name is the final name after following zero or more CNAME records.
LookupCNAME does not return an error if host does not contain DNS "CNAME"
records, as long as host resolves to address records.

<h3 id="Resolver.LookupHost">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L133">LookupHost</a>
    <a href="#Resolver.LookupHost">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupHost(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, host <a href="/builtin/#string">string</a>) (addrs []<a href="/builtin/#string">string</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupHost looks up the given host using the local resolver. It returns a slice
of that host's addresses.

<h3 id="Resolver.LookupIPAddr">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L161">LookupIPAddr</a>
    <a href="#Resolver.LookupIPAddr">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupIPAddr(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, host <a href="/builtin/#string">string</a>) ([]<a href="#IPAddr">IPAddr</a>, <a href="/builtin/#error">error</a>)</pre>

LookupIPAddr looks up host using the local resolver. It returns a slice of that
host's IPv4 and IPv6 addresses.

<h3 id="Resolver.LookupMX">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L320">LookupMX</a>
    <a href="#Resolver.LookupMX">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupMX(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, name <a href="/builtin/#string">string</a>) ([]*<a href="#MX">MX</a>, <a href="/builtin/#error">error</a>)</pre>

LookupMX returns the DNS MX records for the given domain name sorted by
preference.

<h3 id="Resolver.LookupNS">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L330">LookupNS</a>
    <a href="#Resolver.LookupNS">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupNS(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, name <a href="/builtin/#string">string</a>) ([]*<a href="#NS">NS</a>, <a href="/builtin/#error">error</a>)</pre>

LookupNS returns the DNS NS records for the given domain name.

<h3 id="Resolver.LookupPort">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L246">LookupPort</a>
    <a href="#Resolver.LookupPort">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupPort(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, network, service <a href="/builtin/#string">string</a>) (port <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupPort looks up the port for the given network and service.

<h3 id="Resolver.LookupSRV">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L310">LookupSRV</a>
    <a href="#Resolver.LookupSRV">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupSRV(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, service, proto, name <a href="/builtin/#string">string</a>) (cname <a href="/builtin/#string">string</a>, addrs []*<a href="#SRV">SRV</a>, err <a href="/builtin/#error">error</a>)</pre>

LookupSRV tries to resolve an SRV query of the given service, protocol, and
domain name. The proto is "tcp" or "udp". The returned records are sorted by
priority and randomized by weight within a priority.

LookupSRV constructs the DNS name to look up following RFC 2782. That is, it
looks up _service._proto.name. To accommodate services publishing SRV records
under non-standard names, if both service and proto are empty strings, LookupSRV
looks up name directly.

<h3 id="Resolver.LookupTXT">func (*Resolver) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/lookup.go#L340">LookupTXT</a>
    <a href="#Resolver.LookupTXT">¶</a></h3>
<pre>func (r *<a href="#Resolver">Resolver</a>) LookupTXT(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, name <a href="/builtin/#string">string</a>) ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

LookupTXT returns the DNS TXT records for the given domain name.

<h2 id="SRV">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/dnsclient.go#L185">SRV</a>
    <a href="#SRV">¶</a></h2>
<pre>type SRV struct {
<span id="SRV.Target"></span>    Target   <a href="/builtin/#string">string</a>
<span id="SRV.Port"></span>    Port     <a href="/builtin/#uint16">uint16</a>
<span id="SRV.Priority"></span>    Priority <a href="/builtin/#uint16">uint16</a>
<span id="SRV.Weight"></span>    Weight   <a href="/builtin/#uint16">uint16</a>
}</pre>

An SRV represents a single DNS SRV record.

<h2 id="TCPAddr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L9">TCPAddr</a>
    <a href="#TCPAddr">¶</a></h2>
<pre>type TCPAddr struct {
<span id="TCPAddr.IP"></span>    IP   <a href="#IP">IP</a>
<span id="TCPAddr.Port"></span>    Port <a href="/builtin/#int">int</a>
<span id="TCPAddr.Zone"></span>    Zone <a href="/builtin/#string">string</a> <span class="comment">// IPv6 scoped addressing zone</span>
}</pre>

TCPAddr represents the address of a TCP end point.

<h3 id="ResolveTCPAddr">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L58">ResolveTCPAddr</a>
    <a href="#ResolveTCPAddr">¶</a></h3>
<pre>func ResolveTCPAddr(network, address <a href="/builtin/#string">string</a>) (*<a href="#TCPAddr">TCPAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ResolveTCPAddr returns an address of TCP end point.

The network must be a TCP network name.

If the host in the address parameter is not a literal IP address or the port is
not a literal port number, ResolveTCPAddr resolves the address to an address of
TCP end point. Otherwise, it parses the address as a pair of literal IP address
and port number. The address parameter can use a host name, but this is not
recommended, because it will return at most one of the host name's IP addresses.

See func Dial for a description of the network and address parameters.

<h3 id="TCPAddr.Network">func (*TCPAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L16">Network</a>
    <a href="#TCPAddr.Network">¶</a></h3>
<pre>func (a *<a href="#TCPAddr">TCPAddr</a>) Network() <a href="/builtin/#string">string</a></pre>

Network returns the address's network name, "tcp".

<h3 id="TCPAddr.String">func (*TCPAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L18">String</a>
    <a href="#TCPAddr.String">¶</a></h3>
<pre>func (a *<a href="#TCPAddr">TCPAddr</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="TCPConn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L75">TCPConn</a>
    <a href="#TCPConn">¶</a></h2>
<pre>type TCPConn struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

TCPConn is an implementation of the Conn interface for TCP network connections.

<h3 id="DialTCP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L196">DialTCP</a>
    <a href="#DialTCP">¶</a></h3>
<pre>func DialTCP(network <a href="/builtin/#string">string</a>, laddr, raddr *<a href="#TCPAddr">TCPAddr</a>) (*<a href="#TCPConn">TCPConn</a>, <a href="/builtin/#error">error</a>)</pre>

DialTCP acts like Dial for TCP networks.

The network must be a TCP network name; see func Dial for details.

If laddr is nil, a local address is automatically chosen. If the IP field of
raddr is nil or an unspecified IP address, the local system is assumed.

<h3 id="TCPConn.Close">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L186">Close</a>
    <a href="#TCPConn.Close">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the connection.

<h3 id="TCPConn.CloseRead">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L102">CloseRead</a>
    <a href="#TCPConn.CloseRead">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) CloseRead() <a href="/builtin/#error">error</a></pre>

CloseRead shuts down the reading side of the TCP connection. Most callers should
just use Close.

<h3 id="TCPConn.CloseWrite">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L114">CloseWrite</a>
    <a href="#TCPConn.CloseWrite">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) CloseWrite() <a href="/builtin/#error">error</a></pre>

CloseWrite shuts down the writing side of the TCP connection. Most callers
should just use Close.

<h3 id="TCPConn.File">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L281">File</a>
    <a href="#TCPConn.File">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) File() (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

File sets the underlying os.File to blocking mode and returns a copy. It is the
caller's responsibility to close f when finished. Closing c does not affect f,
and closing f does not affect c.

The returned os.File's file descriptor is different from the connection's.
Attempting to change properties of the original using this duplicate may or may
not have the desired effect.

<h3 id="TCPConn.LocalAddr">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L200">LocalAddr</a>
    <a href="#TCPConn.LocalAddr">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) LocalAddr() <a href="#Addr">Addr</a></pre>

LocalAddr returns the local network address. The Addr returned is shared by all
invocations of LocalAddr, so do not modify it.

<h3 id="TCPConn.Read">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L162">Read</a>
    <a href="#TCPConn.Read">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) Read(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Read implements the Conn Read method.

<h3 id="TCPConn.ReadFrom">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L89">ReadFrom</a>
    <a href="#TCPConn.ReadFrom">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) ReadFrom(r <a href="/io/">io</a>.<a href="/io/#Reader">Reader</a>) (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFrom implements the io.ReaderFrom ReadFrom method.

<h3 id="TCPConn.RemoteAddr">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L210">RemoteAddr</a>
    <a href="#TCPConn.RemoteAddr">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) RemoteAddr() <a href="#Addr">Addr</a></pre>

RemoteAddr returns the remote network address. The Addr returned is shared by
all invocations of RemoteAddr, so do not modify it.

<h3 id="TCPConn.SetDeadline">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L218">SetDeadline</a>
    <a href="#TCPConn.SetDeadline">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline implements the Conn SetDeadline method.

<h3 id="TCPConn.SetKeepAlive">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L148">SetKeepAlive</a>
    <a href="#TCPConn.SetKeepAlive">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetKeepAlive(keepalive <a href="/builtin/#bool">bool</a>) <a href="/builtin/#error">error</a></pre>

SetKeepAlive sets whether the operating system should send keepalive messages on
the connection.

<h3 id="TCPConn.SetKeepAlivePeriod">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L159">SetKeepAlivePeriod</a>
    <a href="#TCPConn.SetKeepAlivePeriod">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetKeepAlivePeriod(d <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>) <a href="/builtin/#error">error</a></pre>

SetKeepAlivePeriod sets period between keep alives.

<h3 id="TCPConn.SetLinger">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L136">SetLinger</a>
    <a href="#TCPConn.SetLinger">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetLinger(sec <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetLinger sets the behavior of Close on a connection which still has data
waiting to be sent or to be acknowledged.

If sec < 0 (the default), the operating system finishes sending the data in the
background.

If sec == 0, the operating system discards any unsent or unacknowledged data.

If sec > 0, the data is sent in the background as with sec < 0. On some
operating systems after sec seconds have elapsed any remaining unsent data may
be discarded.

<h3 id="TCPConn.SetNoDelay">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L173">SetNoDelay</a>
    <a href="#TCPConn.SetNoDelay">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetNoDelay(noDelay <a href="/builtin/#bool">bool</a>) <a href="/builtin/#error">error</a></pre>

SetNoDelay controls whether the operating system should delay packet
transmission in hopes of sending fewer packets (Nagle's algorithm). The default
is true (no delay), meaning that data is sent as soon as possible after a Write.

<h3 id="TCPConn.SetReadBuffer">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L252">SetReadBuffer</a>
    <a href="#TCPConn.SetReadBuffer">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetReadBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetReadBuffer sets the size of the operating system's receive buffer associated
with the connection.

<h3 id="TCPConn.SetReadDeadline">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L229">SetReadDeadline</a>
    <a href="#TCPConn.SetReadDeadline">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetReadDeadline implements the Conn SetReadDeadline method.

<h3 id="TCPConn.SetWriteBuffer">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L264">SetWriteBuffer</a>
    <a href="#TCPConn.SetWriteBuffer">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetWriteBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetWriteBuffer sets the size of the operating system's transmit buffer
associated with the connection.

<h3 id="TCPConn.SetWriteDeadline">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L240">SetWriteDeadline</a>
    <a href="#TCPConn.SetWriteDeadline">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetWriteDeadline implements the Conn SetWriteDeadline method.

<h3 id="TCPConn.SyscallConn">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L81">SyscallConn</a>
    <a href="#TCPConn.SyscallConn">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) SyscallConn() (<a href="/syscall/">syscall</a>.<a href="/syscall/#RawConn">RawConn</a>, <a href="/builtin/#error">error</a>)</pre>

SyscallConn returns a raw network connection. This implements the syscall.Conn
interface.

<h3 id="TCPConn.Write">func (*TCPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L174">Write</a>
    <a href="#TCPConn.Write">¶</a></h3>
<pre>func (c *<a href="#TCPConn">TCPConn</a>) Write(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write implements the Conn Write method.

<h2 id="TCPListener">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L214">TCPListener</a>
    <a href="#TCPListener">¶</a></h2>
<pre>type TCPListener struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

TCPListener is a TCP network listener. Clients should typically use variables of
type Listener instead of assuming TCP.

<h3 id="ListenTCP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L300">ListenTCP</a>
    <a href="#ListenTCP">¶</a></h3>
<pre>func ListenTCP(network <a href="/builtin/#string">string</a>, laddr *<a href="#TCPAddr">TCPAddr</a>) (*<a href="#TCPListener">TCPListener</a>, <a href="/builtin/#error">error</a>)</pre>

ListenTCP acts like Listen for TCP networks.

The network must be a TCP network name; see func Dial for details.

If the IP field of laddr is nil or an unspecified IP address, ListenTCP listens
on all available unicast and anycast IP addresses of the local system. If the
Port field of laddr is 0, a port number is automatically chosen.

<h3 id="TCPListener.Accept">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L233">Accept</a>
    <a href="#TCPListener.Accept">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) Accept() (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

Accept implements the Accept method in the Listener interface; it waits for the
next call and returns a generic Conn.

<h3 id="TCPListener.AcceptTCP">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L220">AcceptTCP</a>
    <a href="#TCPListener.AcceptTCP">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) AcceptTCP() (*<a href="#TCPConn">TCPConn</a>, <a href="/builtin/#error">error</a>)</pre>

AcceptTCP accepts the next incoming call and returns the new connection.

<h3 id="TCPListener.Addr">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L259">Addr</a>
    <a href="#TCPListener.Addr">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) Addr() <a href="#Addr">Addr</a></pre>

Addr returns the listener's network address, a *TCPAddr. The Addr returned is
shared by all invocations of Addr, so do not modify it.

<h3 id="TCPListener.Close">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L246">Close</a>
    <a href="#TCPListener.Close">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) Close() <a href="/builtin/#error">error</a></pre>

Close stops listening on the TCP address. Already Accepted connections are not
closed.

<h3 id="TCPListener.File">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L280">File</a>
    <a href="#TCPListener.File">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) File() (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

File returns a copy of the underlying os.File, set to blocking mode. It is the
caller's responsibility to close f when finished. Closing l does not affect f,
and closing f does not affect l.

The returned os.File's file descriptor is different from the connection's.
Attempting to change properties of the original using this duplicate may or may
not have the desired effect.

<h3 id="TCPListener.SetDeadline">func (*TCPListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L263">SetDeadline</a>
    <a href="#TCPListener.SetDeadline">¶</a></h3>
<pre>func (l *<a href="#TCPListener">TCPListener</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline sets the deadline associated with the listener. A zero time value
disables the deadline.

<h2 id="UDPAddr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L12">UDPAddr</a>
    <a href="#UDPAddr">¶</a></h2>
<pre>type UDPAddr struct {
<span id="UDPAddr.IP"></span>    IP   <a href="#IP">IP</a>
<span id="UDPAddr.Port"></span>    Port <a href="/builtin/#int">int</a>
<span id="UDPAddr.Zone"></span>    Zone <a href="/builtin/#string">string</a> <span class="comment">// IPv6 scoped addressing zone</span>
}</pre>

UDPAddr represents the address of a UDP end point.

<h3 id="ResolveUDPAddr">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L61">ResolveUDPAddr</a>
    <a href="#ResolveUDPAddr">¶</a></h3>
<pre>func ResolveUDPAddr(network, address <a href="/builtin/#string">string</a>) (*<a href="#UDPAddr">UDPAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ResolveUDPAddr returns an address of UDP end point.

The network must be a UDP network name.

If the host in the address parameter is not a literal IP address or the port is
not a literal port number, ResolveUDPAddr resolves the address to an address of
UDP end point. Otherwise, it parses the address as a pair of literal IP address
and port number. The address parameter can use a host name, but this is not
recommended, because it will return at most one of the host name's IP addresses.

See func Dial for a description of the network and address parameters.

<h3 id="UDPAddr.Network">func (*UDPAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L19">Network</a>
    <a href="#UDPAddr.Network">¶</a></h3>
<pre>func (a *<a href="#UDPAddr">UDPAddr</a>) Network() <a href="/builtin/#string">string</a></pre>

Network returns the address's network name, "udp".

<h3 id="UDPAddr.String">func (*UDPAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L21">String</a>
    <a href="#UDPAddr.String">¶</a></h3>
<pre>func (a *<a href="#UDPAddr">UDPAddr</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="UDPConn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L78">UDPConn</a>
    <a href="#UDPConn">¶</a></h2>
<pre>type UDPConn struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

UDPConn is the implementation of the Conn and PacketConn interfaces for UDP
network connections.

<h3 id="DialUDP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L192">DialUDP</a>
    <a href="#DialUDP">¶</a></h3>
<pre>func DialUDP(network <a href="/builtin/#string">string</a>, laddr, raddr *<a href="#UDPAddr">UDPAddr</a>) (*<a href="#UDPConn">UDPConn</a>, <a href="/builtin/#error">error</a>)</pre>

DialUDP acts like Dial for UDP networks.

The network must be a UDP network name; see func Dial for details.

If laddr is nil, a local address is automatically chosen. If the IP field of
raddr is nil or an unspecified IP address, the local system is assumed.

<h3 id="ListenMulticastUDP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L250">ListenMulticastUDP</a>
    <a href="#ListenMulticastUDP">¶</a></h3>
<pre>func ListenMulticastUDP(network <a href="/builtin/#string">string</a>, ifi *<a href="#Interface">Interface</a>, gaddr *<a href="#UDPAddr">UDPAddr</a>) (*<a href="#UDPConn">UDPConn</a>, <a href="/builtin/#error">error</a>)</pre>

ListenMulticastUDP acts like ListenPacket for UDP networks but takes a group
address on a specific network interface.

The network must be a UDP network name; see func Dial for details.

ListenMulticastUDP listens on all available IP addresses of the local system
including the group, multicast IP address. If ifi is nil, ListenMulticastUDP
uses the system-assigned multicast interface, although this is not recommended
because the assignment depends on platforms and sometimes it might require
routing configuration. If the Port field of gaddr is 0, a port number is
automatically chosen.

ListenMulticastUDP is just for convenience of simple, small applications. There
are golang.org/x/net/ipv4 and golang.org/x/net/ipv6 packages for general purpose
uses.

<h3 id="ListenUDP">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L217">ListenUDP</a>
    <a href="#ListenUDP">¶</a></h3>
<pre>func ListenUDP(network <a href="/builtin/#string">string</a>, laddr *<a href="#UDPAddr">UDPAddr</a>) (*<a href="#UDPConn">UDPConn</a>, <a href="/builtin/#error">error</a>)</pre>

ListenUDP acts like ListenPacket for UDP networks.

The network must be a UDP network name; see func Dial for details.

If the IP field of laddr is nil or an unspecified IP address, ListenUDP listens
on all available IP addresses of the local system except multicast IP addresses.
If the Port field of laddr is 0, a port number is automatically chosen.

<h3 id="UDPConn.Close">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L186">Close</a>
    <a href="#UDPConn.Close">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the connection.

<h3 id="UDPConn.File">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L281">File</a>
    <a href="#UDPConn.File">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) File() (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

File sets the underlying os.File to blocking mode and returns a copy. It is the
caller's responsibility to close f when finished. Closing c does not affect f,
and closing f does not affect c.

The returned os.File's file descriptor is different from the connection's.
Attempting to change properties of the original using this duplicate may or may
not have the desired effect.

<h3 id="UDPConn.LocalAddr">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L200">LocalAddr</a>
    <a href="#UDPConn.LocalAddr">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) LocalAddr() <a href="#Addr">Addr</a></pre>

LocalAddr returns the local network address. The Addr returned is shared by all
invocations of LocalAddr, so do not modify it.

<h3 id="UDPConn.Read">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L162">Read</a>
    <a href="#UDPConn.Read">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) Read(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Read implements the Conn Read method.

<h3 id="UDPConn.ReadFrom">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L104">ReadFrom</a>
    <a href="#UDPConn.ReadFrom">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) ReadFrom(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="#Addr">Addr</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFrom implements the PacketConn ReadFrom method.

<h3 id="UDPConn.ReadFromUDP">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L92">ReadFromUDP</a>
    <a href="#UDPConn.ReadFromUDP">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) ReadFromUDP(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, *<a href="#UDPAddr">UDPAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFromUDP acts like ReadFrom but returns a UDPAddr.

<h3 id="UDPConn.ReadMsgUDP">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L125">ReadMsgUDP</a>
    <a href="#UDPConn.ReadMsgUDP">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) ReadMsgUDP(b, oob []<a href="/builtin/#byte">byte</a>) (n, oobn, flags <a href="/builtin/#int">int</a>, addr *<a href="#UDPAddr">UDPAddr</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadMsgUDP reads a message from c, copying the payload into b and the associated
out-of-band data into oob. It returns the number of bytes copied into b, the
number of bytes copied into oob, the flags that were set on the message and the
source address of the message.

The packages golang.org/x/net/ipv4 and golang.org/x/net/ipv6 can be used to
manipulate IP-level socket options in oob.

<h3 id="UDPConn.RemoteAddr">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L210">RemoteAddr</a>
    <a href="#UDPConn.RemoteAddr">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) RemoteAddr() <a href="#Addr">Addr</a></pre>

RemoteAddr returns the remote network address. The Addr returned is shared by
all invocations of RemoteAddr, so do not modify it.

<h3 id="UDPConn.SetDeadline">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L218">SetDeadline</a>
    <a href="#UDPConn.SetDeadline">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline implements the Conn SetDeadline method.

<h3 id="UDPConn.SetReadBuffer">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L252">SetReadBuffer</a>
    <a href="#UDPConn.SetReadBuffer">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) SetReadBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetReadBuffer sets the size of the operating system's receive buffer associated
with the connection.

<h3 id="UDPConn.SetReadDeadline">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L229">SetReadDeadline</a>
    <a href="#UDPConn.SetReadDeadline">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetReadDeadline implements the Conn SetReadDeadline method.

<h3 id="UDPConn.SetWriteBuffer">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L264">SetWriteBuffer</a>
    <a href="#UDPConn.SetWriteBuffer">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) SetWriteBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetWriteBuffer sets the size of the operating system's transmit buffer
associated with the connection.

<h3 id="UDPConn.SetWriteDeadline">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L240">SetWriteDeadline</a>
    <a href="#UDPConn.SetWriteDeadline">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetWriteDeadline implements the Conn SetWriteDeadline method.

<h3 id="UDPConn.SyscallConn">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L84">SyscallConn</a>
    <a href="#UDPConn.SyscallConn">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) SyscallConn() (<a href="/syscall/">syscall</a>.<a href="/syscall/#RawConn">RawConn</a>, <a href="/builtin/#error">error</a>)</pre>

SyscallConn returns a raw network connection. This implements the syscall.Conn
interface.

<h3 id="UDPConn.Write">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L174">Write</a>
    <a href="#UDPConn.Write">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) Write(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write implements the Conn Write method.

<h3 id="UDPConn.WriteMsgUDP">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L172">WriteMsgUDP</a>
    <a href="#UDPConn.WriteMsgUDP">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) WriteMsgUDP(b, oob []<a href="/builtin/#byte">byte</a>, addr *<a href="#UDPAddr">UDPAddr</a>) (n, oobn <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteMsgUDP writes a message to addr via c if c isn't connected, or to c's
remote address if c is connected (in which case addr must be nil). The payload
is copied from b and the associated out-of-band data is copied from oob. It
returns the number of payload and out-of-band bytes written.

The packages golang.org/x/net/ipv4 and golang.org/x/net/ipv6 can be used to
manipulate IP-level socket options in oob.

<h3 id="UDPConn.WriteTo">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L149">WriteTo</a>
    <a href="#UDPConn.WriteTo">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) WriteTo(b []<a href="/builtin/#byte">byte</a>, addr <a href="#Addr">Addr</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteTo implements the PacketConn WriteTo method.

<h3 id="UDPConn.WriteToUDP">func (*UDPConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L137">WriteToUDP</a>
    <a href="#UDPConn.WriteToUDP">¶</a></h3>
<pre>func (c *<a href="#UDPConn">UDPConn</a>) WriteToUDP(b []<a href="/builtin/#byte">byte</a>, addr *<a href="#UDPAddr">UDPAddr</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteToUDP acts like WriteTo but takes a UDPAddr.

<h2 id="UnixAddr">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L6">UnixAddr</a>
    <a href="#UnixAddr">¶</a></h2>
<pre>type UnixAddr struct {
<span id="UnixAddr.Name"></span>    Name <a href="/builtin/#string">string</a>
<span id="UnixAddr.Net"></span>    Net  <a href="/builtin/#string">string</a>
}</pre>

UnixAddr represents the address of a Unix domain socket end point.

<h3 id="ResolveUnixAddr">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L41">ResolveUnixAddr</a>
    <a href="#ResolveUnixAddr">¶</a></h3>
<pre>func ResolveUnixAddr(network, address <a href="/builtin/#string">string</a>) (*<a href="#UnixAddr">UnixAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ResolveUnixAddr returns an address of Unix domain socket end point.

The network must be a Unix network name.

See func Dial for a description of the network and address parameters.

<h3 id="UnixAddr.Network">func (*UnixAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L13">Network</a>
    <a href="#UnixAddr.Network">¶</a></h3>
<pre>func (a *<a href="#UnixAddr">UnixAddr</a>) Network() <a href="/builtin/#string">string</a></pre>

Network returns the address's network name, "unix", "unixgram" or "unixpacket".

<h3 id="UnixAddr.String">func (*UnixAddr) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L17">String</a>
    <a href="#UnixAddr.String">¶</a></h3>
<pre>func (a *<a href="#UnixAddr">UnixAddr</a>) String() <a href="/builtin/#string">string</a></pre>


<h2 id="UnixConn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L52">UnixConn</a>
    <a href="#UnixConn">¶</a></h2>
<pre>type UnixConn struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

UnixConn is an implementation of the Conn interface for connections to Unix
domain sockets.

<h3 id="DialUnix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L187">DialUnix</a>
    <a href="#DialUnix">¶</a></h3>
<pre>func DialUnix(network <a href="/builtin/#string">string</a>, laddr, raddr *<a href="#UnixAddr">UnixAddr</a>) (*<a href="#UnixConn">UnixConn</a>, <a href="/builtin/#error">error</a>)</pre>

DialUnix acts like Dial for Unix networks.

The network must be a Unix network name; see func Dial for details.

If laddr is non-nil, it is used as the local address for the connection.

<h3 id="ListenUnixgram">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L307">ListenUnixgram</a>
    <a href="#ListenUnixgram">¶</a></h3>
<pre>func ListenUnixgram(network <a href="/builtin/#string">string</a>, laddr *<a href="#UnixAddr">UnixAddr</a>) (*<a href="#UnixConn">UnixConn</a>, <a href="/builtin/#error">error</a>)</pre>

ListenUnixgram acts like ListenPacket for Unix networks.

The network must be "unixgram".

<h3 id="UnixConn.Close">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L186">Close</a>
    <a href="#UnixConn.Close">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the connection.

<h3 id="UnixConn.CloseRead">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L67">CloseRead</a>
    <a href="#UnixConn.CloseRead">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) CloseRead() <a href="/builtin/#error">error</a></pre>

CloseRead shuts down the reading side of the Unix domain connection. Most
callers should just use Close.

<h3 id="UnixConn.CloseWrite">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L79">CloseWrite</a>
    <a href="#UnixConn.CloseWrite">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) CloseWrite() <a href="/builtin/#error">error</a></pre>

CloseWrite shuts down the writing side of the Unix domain connection. Most
callers should just use Close.

<h3 id="UnixConn.File">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L281">File</a>
    <a href="#UnixConn.File">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) File() (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

File sets the underlying os.File to blocking mode and returns a copy. It is the
caller's responsibility to close f when finished. Closing c does not affect f,
and closing f does not affect c.

The returned os.File's file descriptor is different from the connection's.
Attempting to change properties of the original using this duplicate may or may
not have the desired effect.

<h3 id="UnixConn.LocalAddr">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L200">LocalAddr</a>
    <a href="#UnixConn.LocalAddr">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) LocalAddr() <a href="#Addr">Addr</a></pre>

LocalAddr returns the local network address. The Addr returned is shared by all
invocations of LocalAddr, so do not modify it.

<h3 id="UnixConn.Read">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L162">Read</a>
    <a href="#UnixConn.Read">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) Read(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Read implements the Conn Read method.

<h3 id="UnixConn.ReadFrom">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L102">ReadFrom</a>
    <a href="#UnixConn.ReadFrom">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) ReadFrom(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="#Addr">Addr</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFrom implements the PacketConn ReadFrom method.

<h3 id="UnixConn.ReadFromUnix">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L90">ReadFromUnix</a>
    <a href="#UnixConn.ReadFromUnix">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) ReadFromUnix(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, *<a href="#UnixAddr">UnixAddr</a>, <a href="/builtin/#error">error</a>)</pre>

ReadFromUnix acts like ReadFrom but returns a UnixAddr.

<h3 id="UnixConn.ReadMsgUnix">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L123">ReadMsgUnix</a>
    <a href="#UnixConn.ReadMsgUnix">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) ReadMsgUnix(b, oob []<a href="/builtin/#byte">byte</a>) (n, oobn, flags <a href="/builtin/#int">int</a>, addr *<a href="#UnixAddr">UnixAddr</a>, err <a href="/builtin/#error">error</a>)</pre>

ReadMsgUnix reads a message from c, copying the payload into b and the
associated out-of-band data into oob. It returns the number of bytes copied into
b, the number of bytes copied into oob, the flags that were set on the message
and the source address of the message.

Note that if len(b) == 0 and len(oob) > 0, this function will still read (and
discard) 1 byte from the connection.

<h3 id="UnixConn.RemoteAddr">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L210">RemoteAddr</a>
    <a href="#UnixConn.RemoteAddr">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) RemoteAddr() <a href="#Addr">Addr</a></pre>

RemoteAddr returns the remote network address. The Addr returned is shared by
all invocations of RemoteAddr, so do not modify it.

<h3 id="UnixConn.SetDeadline">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L218">SetDeadline</a>
    <a href="#UnixConn.SetDeadline">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline implements the Conn SetDeadline method.

<h3 id="UnixConn.SetReadBuffer">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L252">SetReadBuffer</a>
    <a href="#UnixConn.SetReadBuffer">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) SetReadBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetReadBuffer sets the size of the operating system's receive buffer associated
with the connection.

<h3 id="UnixConn.SetReadDeadline">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L229">SetReadDeadline</a>
    <a href="#UnixConn.SetReadDeadline">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) SetReadDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetReadDeadline implements the Conn SetReadDeadline method.

<h3 id="UnixConn.SetWriteBuffer">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L264">SetWriteBuffer</a>
    <a href="#UnixConn.SetWriteBuffer">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) SetWriteBuffer(bytes <a href="/builtin/#int">int</a>) <a href="/builtin/#error">error</a></pre>

SetWriteBuffer sets the size of the operating system's transmit buffer
associated with the connection.

<h3 id="UnixConn.SetWriteDeadline">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L240">SetWriteDeadline</a>
    <a href="#UnixConn.SetWriteDeadline">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) SetWriteDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetWriteDeadline implements the Conn SetWriteDeadline method.

<h3 id="UnixConn.SyscallConn">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L58">SyscallConn</a>
    <a href="#UnixConn.SyscallConn">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) SyscallConn() (<a href="/syscall/">syscall</a>.<a href="/syscall/#RawConn">RawConn</a>, <a href="/builtin/#error">error</a>)</pre>

SyscallConn returns a raw network connection. This implements the syscall.Conn
interface.

<h3 id="UnixConn.Write">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L174">Write</a>
    <a href="#UnixConn.Write">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) Write(b []<a href="/builtin/#byte">byte</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

Write implements the Conn Write method.

<h3 id="UnixConn.WriteMsgUnix">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L168">WriteMsgUnix</a>
    <a href="#UnixConn.WriteMsgUnix">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) WriteMsgUnix(b, oob []<a href="/builtin/#byte">byte</a>, addr *<a href="#UnixAddr">UnixAddr</a>) (n, oobn <a href="/builtin/#int">int</a>, err <a href="/builtin/#error">error</a>)</pre>

WriteMsgUnix writes a message to addr via c, copying the payload from b and the
associated out-of-band data from oob. It returns the number of payload and
out-of-band bytes written.

Note that if len(b) == 0 and len(oob) > 0, this function will still write 1 byte
to the connection.

<h3 id="UnixConn.WriteTo">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L147">WriteTo</a>
    <a href="#UnixConn.WriteTo">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) WriteTo(b []<a href="/builtin/#byte">byte</a>, addr <a href="#Addr">Addr</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteTo implements the PacketConn WriteTo method.

<h3 id="UnixConn.WriteToUnix">func (*UnixConn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L135">WriteToUnix</a>
    <a href="#UnixConn.WriteToUnix">¶</a></h3>
<pre>func (c *<a href="#UnixConn">UnixConn</a>) WriteToUnix(b []<a href="/builtin/#byte">byte</a>, addr *<a href="#UnixAddr">UnixAddr</a>) (<a href="/builtin/#int">int</a>, <a href="/builtin/#error">error</a>)</pre>

WriteToUnix acts like WriteTo but takes a UnixAddr.

<h2 id="UnixListener">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L203">UnixListener</a>
    <a href="#UnixListener">¶</a></h2>
<pre>type UnixListener struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

UnixListener is a Unix domain socket listener. Clients should typically use
variables of type Listener instead of assuming Unix domain sockets.

<h3 id="ListenUnix">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L288">ListenUnix</a>
    <a href="#ListenUnix">¶</a></h3>
<pre>func ListenUnix(network <a href="/builtin/#string">string</a>, laddr *<a href="#UnixAddr">UnixAddr</a>) (*<a href="#UnixListener">UnixListener</a>, <a href="/builtin/#error">error</a>)</pre>

ListenUnix acts like Listen for Unix networks.

The network must be "unix" or "unixpacket".

<h3 id="UnixListener.Accept">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L227">Accept</a>
    <a href="#UnixListener.Accept">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) Accept() (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

Accept implements the Accept method in the Listener interface. Returned
connections will be of type *UnixConn.

<h3 id="UnixListener.AcceptUnix">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L214">AcceptUnix</a>
    <a href="#UnixListener.AcceptUnix">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) AcceptUnix() (*<a href="#UnixConn">UnixConn</a>, <a href="/builtin/#error">error</a>)</pre>

AcceptUnix accepts the next incoming call and returns the new connection.

<h3 id="UnixListener.Addr">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L253">Addr</a>
    <a href="#UnixListener.Addr">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) Addr() <a href="#Addr">Addr</a></pre>

Addr returns the listener's network address. The Addr returned is shared by all
invocations of Addr, so do not modify it.

<h3 id="UnixListener.Close">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L240">Close</a>
    <a href="#UnixListener.Close">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) Close() <a href="/builtin/#error">error</a></pre>

Close stops listening on the Unix address. Already accepted connections are not
closed.

<h3 id="UnixListener.File">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L274">File</a>
    <a href="#UnixListener.File">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) File() (f *<a href="/os/">os</a>.<a href="/os/#File">File</a>, err <a href="/builtin/#error">error</a>)</pre>

File returns a copy of the underlying os.File, set to blocking mode. It is the
caller's responsibility to close f when finished. Closing l does not affect f,
and closing f does not affect l.

The returned os.File's file descriptor is different from the connection's.
Attempting to change properties of the original using this duplicate may or may
not have the desired effect.

<h3 id="UnixListener.SetDeadline">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock.go#L257">SetDeadline</a>
    <a href="#UnixListener.SetDeadline">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) SetDeadline(t <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#error">error</a></pre>

SetDeadline sets the deadline associated with the listener. A zero time value
disables the deadline.

<h3 id="UnixListener.SetUnlinkOnClose">func (*UnixListener) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/unixsock_posix.go#L195">SetUnlinkOnClose</a>
    <a href="#UnixListener.SetUnlinkOnClose">¶</a></h3>
<pre>func (l *<a href="#UnixListener">UnixListener</a>) SetUnlinkOnClose(unlink <a href="/builtin/#bool">bool</a>)</pre>

SetUnlinkOnClose sets whether the underlying socket file should be removed from
the file system when the listener is closed.

The default behavior is to unlink the socket file only when package net created
it. That is, when the listener and the underlying socket file were created by a
call to Listen or ListenUnix, then by default closing the listener will remove
the socket file. but if the listener was created by a call to FileListener to
use an already existing socket file, then by default closing the listener will
not remove the socket file.

<h2 id="UnknownNetworkError">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L519">UnknownNetworkError</a>
    <a href="#UnknownNetworkError">¶</a></h2>
<pre>type UnknownNetworkError <a href="/builtin/#string">string</a></pre>


<h3 id="UnknownNetworkError.Error">func (UnknownNetworkError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L521">Error</a>
    <a href="#UnknownNetworkError.Error">¶</a></h3>
<pre>func (e <a href="#UnknownNetworkError">UnknownNetworkError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h3 id="UnknownNetworkError.Temporary">func (UnknownNetworkError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L523">Temporary</a>
    <a href="#UnknownNetworkError.Temporary">¶</a></h3>
<pre>func (e <a href="#UnknownNetworkError">UnknownNetworkError</a>) Temporary() <a href="/builtin/#bool">bool</a></pre>


<h3 id="UnknownNetworkError.Timeout">func (UnknownNetworkError) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/net.go#L522">Timeout</a>
    <a href="#UnknownNetworkError.Timeout">¶</a></h3>
<pre>func (e <a href="#UnknownNetworkError">UnknownNetworkError</a>) Timeout() <a href="/builtin/#bool">bool</a></pre>


<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/file.go#L1)  On NaCl and Windows, the FileConn, FileListener and FilePacketConn functions
  are not implemented.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L3)  On NaCl, methods and functions related to Interface are not implemented.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/interface.go#L6)  On DragonFly BSD, NetBSD, OpenBSD, Plan 9 and Solaris, the MulticastAddrs
  method of Interface is not implemented.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L2)  On every POSIX platform, reads from the "ip4" network using the ReadFrom or
  ReadFromIP method might not return a complete IPv4 packet, including its
  header, even if there is space available. This can occur even in cases where
  Read or ReadMsgIP could return a complete packet. For this reason, it is
  recommended that you do not use these methods if it is important to receive
  a full packet.

  The Go 1 compatibility guidelines make it impossible for us to change the
  behavior of these methods; use Read or ReadMsgIP instead.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L14)  On NaCl, Plan 9 and Windows, the ReadMsgIP and WriteMsgIP methods of IPConn
  are not implemented.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/iprawsock.go#L17)  On Windows, the File method of IPConn is not implemented.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/ipsock.go#L2)  On DragonFly BSD and OpenBSD, listening on the "tcp" and "udp" networks does
  not listen for both IPv4 and IPv6 connections. This is due to the fact that
  IPv4 traffic will not be routed to an IPv6 socket - two separate sockets are
  required if both address families are to be supported. See inet6(4) for
  details.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rawconn.go#L2)  On Windows, the Read and Write methods of syscall.RawConn are not
  implemented.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/rawconn.go#L5)  On NaCl and Plan 9, the Control, Read and Write methods of syscall.RawConn
  are not implemented.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/tcpsock.go#L5)  On Windows, the File method of TCPListener is not implemented.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L2)  On NaCl, Plan 9 and Windows, the ReadMsgUDP and WriteMsgUDP methods of
  UDPConn are not implemented.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L5)  On Windows, the File method of UDPConn is not implemented.
- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/udpsock.go#L8)  On NaCl, the ListenMulticastUDP function is not implemented.

## Subdirectories
- [..](..)
- [http](http/)
- [mail](mail/)
- [rpc](rpc/)
- [smtp](smtp/)
- [textproto](textproto/)
- [url](url/)
