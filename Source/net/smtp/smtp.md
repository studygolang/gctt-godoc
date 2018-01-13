version: 1.9.2
## package smtp

  `import "net/smtp"`

## Overview

Package smtp implements the Simple Mail Transfer Protocol as defined in RFC
5321. It also implements the following extensions:

    8BITMIME  RFC 1652
    AUTH      RFC 2554
    STARTTLS  RFC 3207

Additional extensions may be handled by clients.

The smtp package is frozen and is not accepting new features. Some external
packages provide more functionality. See:

    https://godoc.org/?q=smtp

<a id="example"></a>
Example:

    // Connect to the remote SMTP server.
    c, err := smtp.Dial("mail.example.com:25")
    if err != nil {
        log.Fatal(err)
    }

    // Set the sender and recipient first
    if err := c.Mail("sender@example.org"); err != nil {
        log.Fatal(err)
    }
    if err := c.Rcpt("recipient@example.net"); err != nil {
        log.Fatal(err)
    }

    // Send the email body.
    wc, err := c.Data()
    if err != nil {
        log.Fatal(err)
    }
    _, err = fmt.Fprintf(wc, "This is the email body")
    if err != nil {
        log.Fatal(err)
    }
    err = wc.Close()
    if err != nil {
        log.Fatal(err)
    }

    // Send the QUIT command and close the connection.
    err = c.Quit()
    if err != nil {
        log.Fatal(err)
    }

## Index

- [func SendMail(addr string, a Auth, from string, to []string, msg []byte) error](#SendMail)
- [type Auth](#Auth)
  - [func CRAMMD5Auth(username, secret string) Auth](#CRAMMD5Auth)
  - [func PlainAuth(identity, username, password, host string) Auth](#PlainAuth)
- [type Client](#Client)
  - [func Dial(addr string) (*Client, error)](#Dial)
  - [func NewClient(conn net.Conn, host string) (*Client, error)](#NewClient)
  - [func (c *Client) Auth(a Auth) error](#Client.Auth)
  - [func (c *Client) Close() error](#Client.Close)
  - [func (c *Client) Data() (io.WriteCloser, error)](#Client.Data)
  - [func (c *Client) Extension(ext string) (bool, string)](#Client.Extension)
  - [func (c *Client) Hello(localName string) error](#Client.Hello)
  - [func (c *Client) Mail(from string) error](#Client.Mail)
  - [func (c *Client) Quit() error](#Client.Quit)
  - [func (c *Client) Rcpt(to string) error](#Client.Rcpt)
  - [func (c *Client) Reset() error](#Client.Reset)
  - [func (c *Client) StartTLS(config *tls.Config) error](#Client.StartTLS)
  - [func (c *Client) TLSConnectionState() (state tls.ConnectionState, ok bool)](#Client.TLSConnectionState)
  - [func (c *Client) Verify(addr string) error](#Client.Verify)
- [type ServerInfo](#ServerInfo)

### Examples

- [Package](#example)
- [PlainAuth](#examplePlainAuth)
- [SendMail](#exampleSendMail)

### Package files
 [auth.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/auth.go) [smtp.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go)

<h2 id="SendMail">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L297">SendMail</a>
    <a href="#SendMail">¶</a></h2>
<pre>func SendMail(addr <a href="/builtin/#string">string</a>, a <a href="#Auth">Auth</a>, from <a href="/builtin/#string">string</a>, to []<a href="/builtin/#string">string</a>, msg []<a href="/builtin/#byte">byte</a>) <a href="/builtin/#error">error</a></pre>

SendMail connects to the server at addr, switches to TLS if possible,
authenticates with the optional mechanism a if possible, and then sends an email
from address from, to addresses to, with message msg. The addr must include a
port, as in "mail.example.com:smtp".

The addresses in the to parameter are the SMTP RCPT addresses.

The msg parameter should be an RFC 822-style email with headers first, a blank
line, and then the message body. The lines of msg should be CRLF terminated. The
msg headers should usually include fields such as "From", "To", "Subject", and
"Cc". Sending "Bcc" messages is accomplished by including an email address in
the to parameter but not including it in the msg headers.

The SendMail function and the net/smtp package are low-level mechanisms and
provide no support for DKIM signing, MIME attachments (see the mime/multipart
package), or other mail functionality. Higher-level packages exist outside of
the standard library.

<a id="exampleSendMail"></a>
Example:

    // Set up authentication information.
    auth := smtp.PlainAuth("", "user@example.com", "password", "mail.example.com")

    // Connect to the server, authenticate, set the sender and recipient,
    // and send the email all in one step.
    to := []string{"recipient@example.net"}
    msg := []byte("To: recipient@example.net\r\n" +
        "Subject: discount Gophers!\r\n" +
        "\r\n" +
        "This is the email body.\r\n")
    err := smtp.SendMail("mail.example.com:25", auth, "sender@example.org", to, msg)
    if err != nil {
        log.Fatal(err)
    }

<h2 id="Auth">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/auth.go#L5">Auth</a>
    <a href="#Auth">¶</a></h2>
<pre>type Auth interface {
    <span class="comment">// Start begins an authentication with a server.</span>
    <span class="comment">// It returns the name of the authentication protocol</span>
    <span class="comment">// and optionally data to include in the initial AUTH message</span>
    <span class="comment">// sent to the server. It can return proto == &#34;&#34; to indicate</span>
    <span class="comment">// that the authentication should be skipped.</span>
    <span class="comment">// If it returns a non-nil error, the SMTP client aborts</span>
    <span class="comment">// the authentication attempt and closes the connection.</span>
    Start(server *<a href="#ServerInfo">ServerInfo</a>) (proto <a href="/builtin/#string">string</a>, toServer []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)

    <span class="comment">// Next continues the authentication. The server has just sent</span>
    <span class="comment">// the fromServer data. If more is true, the server expects a</span>
    <span class="comment">// response, which Next should return as toServer; otherwise</span>
    <span class="comment">// Next should return toServer == nil.</span>
    <span class="comment">// If Next returns a non-nil error, the SMTP client aborts</span>
    <span class="comment">// the authentication attempt and closes the connection.</span>
    Next(fromServer []<a href="/builtin/#byte">byte</a>, more <a href="/builtin/#bool">bool</a>) (toServer []<a href="/builtin/#byte">byte</a>, err <a href="/builtin/#error">error</a>)
}</pre>

Auth is implemented by an SMTP authentication mechanism.

<h3 id="CRAMMD5Auth">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/auth.go#L84">CRAMMD5Auth</a>
    <a href="#CRAMMD5Auth">¶</a></h3>
<pre>func CRAMMD5Auth(username, secret <a href="/builtin/#string">string</a>) <a href="#Auth">Auth</a></pre>

CRAMMD5Auth returns an Auth that implements the CRAM-MD5 authentication
mechanism as defined in RFC 2195. The returned Auth uses the given username and
secret to authenticate to the server using the challenge-response mechanism.

<h3 id="PlainAuth">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/auth.go#L44">PlainAuth</a>
    <a href="#PlainAuth">¶</a></h3>
<pre>func PlainAuth(identity, username, password, host <a href="/builtin/#string">string</a>) <a href="#Auth">Auth</a></pre>

PlainAuth returns an Auth that implements the PLAIN authentication mechanism as
defined in RFC 4616. The returned Auth uses the given username and password to
authenticate to host and act as identity. Usually identity should be the empty
string, to act as username.

PlainAuth will only send the credentials if the connection is using TLS or is
connected to localhost. Otherwise authentication will fail with an error,
without sending the credentials.

<a id="examplePlainAuth"></a>
Example:

    // hostname is used by PlainAuth to validate the TLS certificate.
    hostname := "mail.example.com"
    auth := smtp.PlainAuth("", "user@example.com", "password", hostname)

    err := smtp.SendMail(hostname+":25", auth, from, recipients, msg)
    if err != nil {
        log.Fatal(err)
    }

<h2 id="Client">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L20">Client</a>
    <a href="#Client">¶</a></h2>
<pre>type Client struct {
<span id="Client.Text"></span>    <span class="comment">// Text is the textproto.Conn used by the Client. It is exported to allow for</span>
    <span class="comment">// clients to add extensions.</span>
    Text *<a href="/net/textproto/">textproto</a>.<a href="/net/textproto/#Conn">Conn</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A Client represents a client connection to an SMTP server.

<h3 id="Dial">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L41">Dial</a>
    <a href="#Dial">¶</a></h3>
<pre>func Dial(addr <a href="/builtin/#string">string</a>) (*<a href="#Client">Client</a>, <a href="/builtin/#error">error</a>)</pre>

Dial returns a new Client connected to an SMTP server at addr. The addr must
include a port, as in "mail.example.com:smtp".

<h3 id="NewClient">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L52">NewClient</a>
    <a href="#NewClient">¶</a></h3>
<pre>func NewClient(conn <a href="/net/">net</a>.<a href="/net/#Conn">Conn</a>, host <a href="/builtin/#string">string</a>) (*<a href="#Client">Client</a>, <a href="/builtin/#error">error</a>)</pre>

NewClient returns a new Client using an existing connection and host as a server
name to be used when authenticating.

<h3 id="Client.Auth">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L183">Auth</a>
    <a href="#Client.Auth">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Auth(a <a href="#Auth">Auth</a>) <a href="/builtin/#error">error</a></pre>

Auth authenticates a client using the provided authentication mechanism. A
failed authentication closes the connection. Only servers that advertise the
AUTH extension support this function.

<h3 id="Client.Close">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L65">Close</a>
    <a href="#Client.Close">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the connection.

<h3 id="Client.Data">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L267">Data</a>
    <a href="#Client.Data">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Data() (<a href="/io/">io</a>.<a href="/io/#WriteCloser">WriteCloser</a>, <a href="/builtin/#error">error</a>)</pre>

Data issues a DATA command to the server and returns a writer that can be used
to write the mail headers and body. The caller should close the writer before
calling any more methods on c. A call to Data must be preceded by one or more
calls to Rcpt.

<h3 id="Client.Extension">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L349">Extension</a>
    <a href="#Client.Extension">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Extension(ext <a href="/builtin/#string">string</a>) (<a href="/builtin/#bool">bool</a>, <a href="/builtin/#string">string</a>)</pre>

Extension reports whether an extension is support by the server. The extension
name is case-insensitive. If the extension is supported, Extension also returns
a string that contains any parameters the server specifies for the extension.

<h3 id="Client.Hello">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L86">Hello</a>
    <a href="#Client.Hello">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Hello(localName <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Hello sends a HELO or EHLO to the server as the given host name. Calling this
method is only necessary if the client needs control over the host name used.
The client will introduce itself as "localhost" automatically otherwise. If
Hello is called, it must be called before any of the other methods.

<h3 id="Client.Mail">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L230">Mail</a>
    <a href="#Client.Mail">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Mail(from <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Mail issues a MAIL command to the server using the provided email address. If
the server supports the 8BITMIME extension, Mail adds the BODY=8BITMIME
parameter. This initiates a mail transaction and is followed by one or more Rcpt
calls.

<h3 id="Client.Quit">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L372">Quit</a>
    <a href="#Client.Quit">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Quit() <a href="/builtin/#error">error</a></pre>

Quit sends the QUIT command and closes the connection to the server.

<h3 id="Client.Rcpt">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L247">Rcpt</a>
    <a href="#Client.Rcpt">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Rcpt(to <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Rcpt issues a RCPT command to the server using the provided email address. A
call to Rcpt must be preceded by a call to Mail and may be followed by a Data
call or another Rcpt call.

<h3 id="Client.Reset">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L363">Reset</a>
    <a href="#Client.Reset">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Reset() <a href="/builtin/#error">error</a></pre>

Reset sends the RSET command to the server, aborting the current mail
transaction.

<h3 id="Client.StartTLS">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L143">StartTLS</a>
    <a href="#Client.StartTLS">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) StartTLS(config *<a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#Config">Config</a>) <a href="/builtin/#error">error</a></pre>

StartTLS sends the STARTTLS command and encrypts all further communication. Only
servers that advertise the STARTTLS extension support this function.

<h3 id="Client.TLSConnectionState">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L160">TLSConnectionState</a>
    <a href="#Client.TLSConnectionState">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) TLSConnectionState() (state <a href="/crypto/tls/">tls</a>.<a href="/crypto/tls/#ConnectionState">ConnectionState</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

TLSConnectionState returns the client's TLS connection state. The return values
are their zero values if StartTLS did not succeed.

<h3 id="Client.Verify">func (*Client) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/smtp.go#L172">Verify</a>
    <a href="#Client.Verify">¶</a></h3>
<pre>func (c *<a href="#Client">Client</a>) Verify(addr <a href="/builtin/#string">string</a>) <a href="/builtin/#error">error</a></pre>

Verify checks the validity of an email address on the server. If Verify returns
nil, the address is valid. A non-nil return does not necessarily indicate an
invalid address. Many servers will not verify addresses for security reasons.

<h2 id="ServerInfo">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/net/smtp/auth.go#L25">ServerInfo</a>
    <a href="#ServerInfo">¶</a></h2>
<pre>type ServerInfo struct {
<span id="ServerInfo.Name"></span>    Name <a href="/builtin/#string">string</a>   <span class="comment">// SMTP server name</span>
<span id="ServerInfo.TLS"></span>    TLS  <a href="/builtin/#bool">bool</a>     <span class="comment">// using TLS, with valid certificate for Name</span>
<span id="ServerInfo.Auth"></span>    Auth []<a href="/builtin/#string">string</a> <span class="comment">// advertised authentication mechanisms</span>
}</pre>

ServerInfo records information about an SMTP server.


