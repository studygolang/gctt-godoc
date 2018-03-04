version: 1.10
## package user

  `import "os/user"`

## Overview

Package user allows user account lookups by name or id.

## Index

- [type Group](#Group)
  - [func LookupGroup(name string) (*Group, error)](#LookupGroup)
  - [func LookupGroupId(gid string) (*Group, error)](#LookupGroupId)
- [type UnknownGroupError](#UnknownGroupError)
  - [func (e UnknownGroupError) Error() string](#UnknownGroupError.Error)
- [type UnknownGroupIdError](#UnknownGroupIdError)
  - [func (e UnknownGroupIdError) Error() string](#UnknownGroupIdError.Error)
- [type UnknownUserError](#UnknownUserError)
  - [func (e UnknownUserError) Error() string](#UnknownUserError.Error)
- [type UnknownUserIdError](#UnknownUserIdError)
  - [func (e UnknownUserIdError) Error() string](#UnknownUserIdError.Error)
- [type User](#User)
  - [func Current() (*User, error)](#Current)
  - [func Lookup(username string) (*User, error)](#Lookup)
  - [func LookupId(uid string) (*User, error)](#LookupId)
  - [func (u *User) GroupIds() ([]string, error)](#User.GroupIds)

### Package files
 [cgo_lookup_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/user/cgo_lookup_unix.go) [getgrouplist_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/user/getgrouplist_unix.go) [listgroups_unix.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/user/listgroups_unix.go) [lookup.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/user/lookup.go) [user.go](//github.com/golang/go/blob/release-branch.go1.10/src/os/user/user.go)

<h2 id="Group">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/user.go#L35">Group</a>
    <a href="#Group">¶</a></h2>
<pre>type Group struct {
<span id="Group.Gid"></span>    Gid  <a href="/builtin/#string">string</a> <span class="comment">// group ID</span>
<span id="Group.Name"></span>    Name <a href="/builtin/#string">string</a> <span class="comment">// group name</span>
}</pre>

Group represents a grouping of users.

On POSIX systems Gid contains a decimal number representing the group ID.

<h3 id="LookupGroup">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/lookup.go#L36">LookupGroup</a>
    <a href="#LookupGroup">¶</a></h3>
<pre>func LookupGroup(name <a href="/builtin/#string">string</a>) (*<a href="#Group">Group</a>, <a href="/builtin/#error">error</a>)</pre>

LookupGroup looks up a group by name. If the group cannot be found, the returned
error is of type UnknownGroupError.

<h3 id="LookupGroupId">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/lookup.go#L42">LookupGroupId</a>
    <a href="#LookupGroupId">¶</a></h3>
<pre>func LookupGroupId(gid <a href="/builtin/#string">string</a>) (*<a href="#Group">Group</a>, <a href="/builtin/#error">error</a>)</pre>

LookupGroupId looks up a group by groupid. If the group cannot be found, the
returned error is of type UnknownGroupIdError.

<h2 id="UnknownGroupError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/user.go#L65">UnknownGroupError</a>
    <a href="#UnknownGroupError">¶</a></h2>
<pre>type UnknownGroupError <a href="/builtin/#string">string</a></pre>

UnknownGroupError is returned by LookupGroup when a group cannot be found.

<h3 id="UnknownGroupError.Error">func (UnknownGroupError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/user.go#L67">Error</a>
    <a href="#UnknownGroupError.Error">¶</a></h3>
<pre>func (e <a href="#UnknownGroupError">UnknownGroupError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="UnknownGroupIdError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/user.go#L57">UnknownGroupIdError</a>
    <a href="#UnknownGroupIdError">¶</a></h2>
<pre>type UnknownGroupIdError <a href="/builtin/#string">string</a></pre>

UnknownGroupIdError is returned by LookupGroupId when a group cannot be found.

<h3 id="UnknownGroupIdError.Error">func (UnknownGroupIdError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/user.go#L59">Error</a>
    <a href="#UnknownGroupIdError.Error">¶</a></h3>
<pre>func (e <a href="#UnknownGroupIdError">UnknownGroupIdError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="UnknownUserError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/user.go#L49">UnknownUserError</a>
    <a href="#UnknownUserError">¶</a></h2>
<pre>type UnknownUserError <a href="/builtin/#string">string</a></pre>

UnknownUserError is returned by Lookup when a user cannot be found.

<h3 id="UnknownUserError.Error">func (UnknownUserError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/user.go#L51">Error</a>
    <a href="#UnknownUserError.Error">¶</a></h3>
<pre>func (e <a href="#UnknownUserError">UnknownUserError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="UnknownUserIdError">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/user.go#L41">UnknownUserIdError</a>
    <a href="#UnknownUserIdError">¶</a></h2>
<pre>type UnknownUserIdError <a href="/builtin/#int">int</a></pre>

UnknownUserIdError is returned by LookupId when a user cannot be found.

<h3 id="UnknownUserIdError.Error">func (UnknownUserIdError) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/user.go#L43">Error</a>
    <a href="#UnknownUserIdError.Error">¶</a></h3>
<pre>func (e <a href="#UnknownUserIdError">UnknownUserIdError</a>) Error() <a href="/builtin/#string">string</a></pre>


<h2 id="User">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/user.go#L8">User</a>
    <a href="#User">¶</a></h2>
<pre>type User struct {
<span id="User.Uid"></span>    <span class="comment">// Uid is the user ID.</span>
    <span class="comment">// On POSIX systems, this is a decimal number representing the uid.</span>
    <span class="comment">// On Windows, this is a security identifier (SID) in a string format.</span>
    <span class="comment">// On Plan 9, this is the contents of /dev/user.</span>
    Uid <a href="/builtin/#string">string</a>
<span id="User.Gid"></span>    <span class="comment">// Gid is the primary group ID.</span>
    <span class="comment">// On POSIX systems, this is a decimal number representing the gid.</span>
    <span class="comment">// On Windows, this is a SID in a string format.</span>
    <span class="comment">// On Plan 9, this is the contents of /dev/user.</span>
    Gid <a href="/builtin/#string">string</a>
<span id="User.Username"></span>    <span class="comment">// Username is the login name.</span>
    Username <a href="/builtin/#string">string</a>
<span id="User.Name"></span>    <span class="comment">// Name is the user&#39;s real or display name.</span>
    <span class="comment">// It might be blank.</span>
    <span class="comment">// On POSIX systems, this is the first (or only) entry in the GECOS field</span>
    <span class="comment">// list.</span>
    <span class="comment">// On Windows, this is the user&#39;s display name.</span>
    <span class="comment">// On Plan 9, this is the contents of /dev/user.</span>
    Name <a href="/builtin/#string">string</a>
<span id="User.HomeDir"></span>    <span class="comment">// HomeDir is the path to the user&#39;s home directory (if they have one).</span>
    HomeDir <a href="/builtin/#string">string</a>
}</pre>

User represents a user account.

<h3 id="Current">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/lookup.go#L1">Current</a>
    <a href="#Current">¶</a></h3>
<pre>func Current() (*<a href="#User">User</a>, <a href="/builtin/#error">error</a>)</pre>

Current returns the current user.

<h3 id="Lookup">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/lookup.go#L18">Lookup</a>
    <a href="#Lookup">¶</a></h3>
<pre>func Lookup(username <a href="/builtin/#string">string</a>) (*<a href="#User">User</a>, <a href="/builtin/#error">error</a>)</pre>

Lookup looks up a user by username. If the user cannot be found, the returned
error is of type UnknownUserError.

<h3 id="LookupId">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/lookup.go#L27">LookupId</a>
    <a href="#LookupId">¶</a></h3>
<pre>func LookupId(uid <a href="/builtin/#string">string</a>) (*<a href="#User">User</a>, <a href="/builtin/#error">error</a>)</pre>

LookupId looks up a user by userid. If the user cannot be found, the returned
error is of type UnknownUserIdError.

<h3 id="User.GroupIds">func (*User) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/os/user/lookup.go#L47">GroupIds</a>
    <a href="#User.GroupIds">¶</a></h3>
<pre>func (u *<a href="#User">User</a>) GroupIds() ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

GroupIds returns the list of group IDs that the user is a member of.


