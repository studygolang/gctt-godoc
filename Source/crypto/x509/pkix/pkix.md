version: 1.10
## package pkix

  `import "crypto/x509/pkix"`

## Overview

Package pkix contains shared, low level structures used for ASN.1 parsing and
serialization of X.509 certificates, CRL and OCSP.

## Index

- [type AlgorithmIdentifier](#AlgorithmIdentifier)
- [type AttributeTypeAndValue](#AttributeTypeAndValue)
- [type AttributeTypeAndValueSET](#AttributeTypeAndValueSET)
- [type CertificateList](#CertificateList)
  - [func (certList *CertificateList) HasExpired(now time.Time) bool](#CertificateList.HasExpired)
- [type Extension](#Extension)
- [type Name](#Name)
  - [func (n *Name) FillFromRDNSequence(rdns *RDNSequence)](#Name.FillFromRDNSequence)
  - [func (n Name) String() string](#Name.String)
  - [func (n Name) ToRDNSequence() (ret RDNSequence)](#Name.ToRDNSequence)
- [type RDNSequence](#RDNSequence)
  - [func (r RDNSequence) String() string](#RDNSequence.String)
- [type RelativeDistinguishedNameSET](#RelativeDistinguishedNameSET)
- [type RevokedCertificate](#RevokedCertificate)
- [type TBSCertificateList](#TBSCertificateList)

### Package files
 [pkix.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go)

<h2 id="AlgorithmIdentifier">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L9">AlgorithmIdentifier</a>
    <a href="#AlgorithmIdentifier">¶</a></h2>
<pre>type AlgorithmIdentifier struct {
<span id="AlgorithmIdentifier.Algorithm"></span>    Algorithm  <a href="/encoding/asn1/">asn1</a>.<a href="/encoding/asn1/#ObjectIdentifier">ObjectIdentifier</a>
<span id="AlgorithmIdentifier.Parameters"></span>    Parameters <a href="/encoding/asn1/">asn1</a>.<a href="/encoding/asn1/#RawValue">RawValue</a> `asn1:&#34;optional&#34;`
}</pre>

AlgorithmIdentifier represents the ASN.1 structure of the same name. See RFC
5280, section 4.1.1.2.

<h2 id="AttributeTypeAndValue">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L89">AttributeTypeAndValue</a>
    <a href="#AttributeTypeAndValue">¶</a></h2>
<pre>type AttributeTypeAndValue struct {
<span id="AttributeTypeAndValue.Type"></span>    Type  <a href="/encoding/asn1/">asn1</a>.<a href="/encoding/asn1/#ObjectIdentifier">ObjectIdentifier</a>
<span id="AttributeTypeAndValue.Value"></span>    Value interface{}
}</pre>

AttributeTypeAndValue mirrors the ASN.1 structure of the same name in
http://tools.ietf.org/html/rfc5280#section-4.1.2.4

<h2 id="AttributeTypeAndValueSET">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L96">AttributeTypeAndValueSET</a>
    <a href="#AttributeTypeAndValueSET">¶</a></h2>
<pre>type AttributeTypeAndValueSET struct {
<span id="AttributeTypeAndValueSET.Type"></span>    Type  <a href="/encoding/asn1/">asn1</a>.<a href="/encoding/asn1/#ObjectIdentifier">ObjectIdentifier</a>
<span id="AttributeTypeAndValueSET.Value"></span>    Value [][]<a href="#AttributeTypeAndValue">AttributeTypeAndValue</a> `asn1:&#34;set&#34;`
}</pre>

AttributeTypeAndValueSET represents a set of ASN.1 sequences of
AttributeTypeAndValue sequences from RFC 2986 (PKCS #10).

<h2 id="CertificateList">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L234">CertificateList</a>
    <a href="#CertificateList">¶</a></h2>
<pre>type CertificateList struct {
<span id="CertificateList.TBSCertList"></span>    TBSCertList        <a href="#TBSCertificateList">TBSCertificateList</a>
<span id="CertificateList.SignatureAlgorithm"></span>    SignatureAlgorithm <a href="#AlgorithmIdentifier">AlgorithmIdentifier</a>
<span id="CertificateList.SignatureValue"></span>    SignatureValue     <a href="/encoding/asn1/">asn1</a>.<a href="/encoding/asn1/#BitString">BitString</a>
}</pre>

CertificateList represents the ASN.1 structure of the same name. See RFC 5280,
section 5.1. Use Certificate.CheckCRLSignature to verify the signature.

<h3 id="CertificateList.HasExpired">func (*CertificateList) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L241">HasExpired</a>
    <a href="#CertificateList.HasExpired">¶</a></h3>
<pre>func (certList *<a href="#CertificateList">CertificateList</a>) HasExpired(now <a href="/time/">time</a>.<a href="/time/#Time">Time</a>) <a href="/builtin/#bool">bool</a></pre>

HasExpired reports whether certList should have been updated by now.

<h2 id="Extension">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L103">Extension</a>
    <a href="#Extension">¶</a></h2>
<pre>type Extension struct {
<span id="Extension.Id"></span>    Id       <a href="/encoding/asn1/">asn1</a>.<a href="/encoding/asn1/#ObjectIdentifier">ObjectIdentifier</a>
<span id="Extension.Critical"></span>    Critical <a href="/builtin/#bool">bool</a> `asn1:&#34;optional&#34;`
<span id="Extension.Value"></span>    Value    []<a href="/builtin/#byte">byte</a>
}</pre>

Extension represents the ASN.1 structure of the same name. See RFC 5280, section
4.2.

<h2 id="Name">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L113">Name</a>
    <a href="#Name">¶</a></h2>
<pre>type Name struct {
<span id="Name.Country"></span>    Country, Organization, OrganizationalUnit []<a href="/builtin/#string">string</a>
<span id="Name.Locality"></span>    Locality, Province                        []<a href="/builtin/#string">string</a>
<span id="Name.StreetAddress"></span>    StreetAddress, PostalCode                 []<a href="/builtin/#string">string</a>
<span id="Name.SerialNumber"></span>    SerialNumber, CommonName                  <a href="/builtin/#string">string</a>

<span id="Name.Names"></span>    Names      []<a href="#AttributeTypeAndValue">AttributeTypeAndValue</a>
<span id="Name.ExtraNames"></span>    ExtraNames []<a href="#AttributeTypeAndValue">AttributeTypeAndValue</a>
}</pre>

Name represents an X.509 distinguished name. This only includes the common
elements of a DN. When parsing, all elements are stored in Names and
non-standard elements can be extracted from there. When marshaling, elements in
ExtraNames are appended and override other values with the same OID.

<h3 id="Name.FillFromRDNSequence">func (*Name) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L123">FillFromRDNSequence</a>
    <a href="#Name.FillFromRDNSequence">¶</a></h3>
<pre>func (n *<a href="#Name">Name</a>) FillFromRDNSequence(rdns *<a href="#RDNSequence">RDNSequence</a>)</pre>


<h3 id="Name.String">func (Name) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L216">String</a>
    <a href="#Name.String">¶</a></h3>
<pre>func (n <a href="#Name">Name</a>) String() <a href="/builtin/#string">string</a></pre>

String returns the string form of n, roughly following the RFC 2253
Distinguished Names syntax.

<h3 id="Name.ToRDNSequence">func (Name) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L193">ToRDNSequence</a>
    <a href="#Name.ToRDNSequence">¶</a></h3>
<pre>func (n <a href="#Name">Name</a>) ToRDNSequence() (ret <a href="#RDNSequence">RDNSequence</a>)</pre>


<h2 id="RDNSequence">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L14">RDNSequence</a>
    <a href="#RDNSequence">¶</a></h2>
<pre>type RDNSequence []<a href="#RelativeDistinguishedNameSET">RelativeDistinguishedNameSET</a></pre>


<h3 id="RDNSequence.String">func (RDNSequence) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L30">String</a>
    <a href="#RDNSequence.String">¶</a></h3>
<pre>func (r <a href="#RDNSequence">RDNSequence</a>) String() <a href="/builtin/#string">string</a></pre>

String returns a string representation of the sequence r, roughly following the
RFC 2253 Distinguished Names syntax.

<h2 id="RelativeDistinguishedNameSET">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L85">RelativeDistinguishedNameSET</a>
    <a href="#RelativeDistinguishedNameSET">¶</a></h2>
<pre>type RelativeDistinguishedNameSET []<a href="#AttributeTypeAndValue">AttributeTypeAndValue</a></pre>


<h2 id="RevokedCertificate">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L260">RevokedCertificate</a>
    <a href="#RevokedCertificate">¶</a></h2>
<pre>type RevokedCertificate struct {
<span id="RevokedCertificate.SerialNumber"></span>    SerialNumber   *<a href="/math/big/">big</a>.<a href="/math/big/#Int">Int</a>
<span id="RevokedCertificate.RevocationTime"></span>    RevocationTime <a href="/time/">time</a>.<a href="/time/#Time">Time</a>
<span id="RevokedCertificate.Extensions"></span>    Extensions     []<a href="#Extension">Extension</a> `asn1:&#34;optional&#34;`
}</pre>

RevokedCertificate represents the ASN.1 structure of the same name. See RFC
5280, section 5.1.

<h2 id="TBSCertificateList">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/crypto/x509/pkix/pkix.go#L247">TBSCertificateList</a>
    <a href="#TBSCertificateList">¶</a></h2>
<pre>type TBSCertificateList struct {
<span id="TBSCertificateList.Raw"></span>    Raw                 <a href="/encoding/asn1/">asn1</a>.<a href="/encoding/asn1/#RawContent">RawContent</a>
<span id="TBSCertificateList.Version"></span>    Version             <a href="/builtin/#int">int</a> `asn1:&#34;optional,default:0&#34;`
<span id="TBSCertificateList.Signature"></span>    Signature           <a href="#AlgorithmIdentifier">AlgorithmIdentifier</a>
<span id="TBSCertificateList.Issuer"></span>    Issuer              <a href="#RDNSequence">RDNSequence</a>
<span id="TBSCertificateList.ThisUpdate"></span>    ThisUpdate          <a href="/time/">time</a>.<a href="/time/#Time">Time</a>
<span id="TBSCertificateList.NextUpdate"></span>    NextUpdate          <a href="/time/">time</a>.<a href="/time/#Time">Time</a>            `asn1:&#34;optional&#34;`
<span id="TBSCertificateList.RevokedCertificates"></span>    RevokedCertificates []<a href="#RevokedCertificate">RevokedCertificate</a> `asn1:&#34;optional&#34;`
<span id="TBSCertificateList.Extensions"></span>    Extensions          []<a href="#Extension">Extension</a>          `asn1:&#34;tag:0,optional,explicit&#34;`
}</pre>

TBSCertificateList represents the ASN.1 structure of the same name. See RFC
5280, section 5.1.


