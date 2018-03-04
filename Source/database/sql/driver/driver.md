version: 1.10
## package driver

  `import "database/sql/driver"`

## Overview

Package driver defines interfaces to be implemented by database drivers as used
by package sql.

Most code should use package sql.

## Index

- [Variables](#pkg-variables)
- [func IsScanValue(v interface{}) bool](#IsScanValue)
- [func IsValue(v interface{}) bool](#IsValue)
- [type ColumnConverter](#ColumnConverter)
- [type Conn](#Conn)
- [type ConnBeginTx](#ConnBeginTx)
- [type ConnPrepareContext](#ConnPrepareContext)
- [type Connector](#Connector)
- [type Driver](#Driver)
- [type DriverContext](#DriverContext)
- [type Execer](#Execer)
- [type ExecerContext](#ExecerContext)
- [type IsolationLevel](#IsolationLevel)
- [type NamedValue](#NamedValue)
- [type NamedValueChecker](#NamedValueChecker)
- [type NotNull](#NotNull)
  - [func (n NotNull) ConvertValue(v interface{}) (Value, error)](#NotNull.ConvertValue)
- [type Null](#Null)
  - [func (n Null) ConvertValue(v interface{}) (Value, error)](#Null.ConvertValue)
- [type Pinger](#Pinger)
- [type Queryer](#Queryer)
- [type QueryerContext](#QueryerContext)
- [type Result](#Result)
- [type Rows](#Rows)
- [type RowsAffected](#RowsAffected)
  - [func (RowsAffected) LastInsertId() (int64, error)](#RowsAffected.LastInsertId)
  - [func (v RowsAffected) RowsAffected() (int64, error)](#RowsAffected.RowsAffected)
- [type RowsColumnTypeDatabaseTypeName](#RowsColumnTypeDatabaseTypeName)
- [type RowsColumnTypeLength](#RowsColumnTypeLength)
- [type RowsColumnTypeNullable](#RowsColumnTypeNullable)
- [type RowsColumnTypePrecisionScale](#RowsColumnTypePrecisionScale)
- [type RowsColumnTypeScanType](#RowsColumnTypeScanType)
- [type RowsNextResultSet](#RowsNextResultSet)
- [type SessionResetter](#SessionResetter)
- [type Stmt](#Stmt)
- [type StmtExecContext](#StmtExecContext)
- [type StmtQueryContext](#StmtQueryContext)
- [type Tx](#Tx)
- [type TxOptions](#TxOptions)
- [type Value](#Value)
- [type ValueConverter](#ValueConverter)
- [type Valuer](#Valuer)

### Package files
 [driver.go](//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go) [types.go](//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/types.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="Bool">Bool</span> boolType</pre>

Bool is a ValueConverter that converts input values to bools.

The conversion rules are:

    - booleans are returned unchanged
    - for integer types,
         1 is true
         0 is false,
         other integers are an error
    - for strings and []byte, same rules as strconv.ParseBool
    - all other types are an error

<pre>var <span id="DefaultParameterConverter">DefaultParameterConverter</span> defaultConverter</pre>

DefaultParameterConverter is the default implementation of ValueConverter that's
used when a Stmt doesn't implement ColumnConverter.

DefaultParameterConverter returns its argument directly if IsValue(arg).
Otherwise, if the argument implements Valuer, its Value method is used to return
a Value. As a fallback, the provided argument's underlying type is used to
convert it to a Value: underlying integer types are converted to int64, floats
to float64, bool, string, and []byte to themselves. If the argument is a nil
pointer, ConvertValue returns a nil Value. If the argument is a non-nil pointer,
it is dereferenced and ConvertValue is called recursively. Other types are an
error.

<pre>var <span id="ErrBadConn">ErrBadConn</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;driver: bad connection&#34;)</pre>

ErrBadConn should be returned by a driver to signal to the sql package that a
driver.Conn is in a bad state (such as the server having earlier closed the
connection) and the sql package should retry on a new connection.

To prevent duplicate operations, ErrBadConn should NOT be returned if there's a
possibility that the database server might have performed the operation. Even if
the server sends back an error, you shouldn't return ErrBadConn.

<pre>var <span id="ErrRemoveArgument">ErrRemoveArgument</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;driver: remove argument from query&#34;)</pre>

ErrRemoveArgument may be returned from NamedValueChecker to instruct the sql
package to not pass the argument to the driver query interface. Return when
accepting query specific options or structures that aren't SQL query arguments.

<pre>var <span id="ErrSkip">ErrSkip</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;driver: skip fast-path; continue as if unimplemented&#34;)</pre>

ErrSkip may be returned by some optional interfaces' methods to indicate at
runtime that the fast path is unavailable and the sql package should continue as
if the optional interface was not implemented. ErrSkip is only supported where
explicitly documented.

<pre>var <span id="Int32">Int32</span> int32Type</pre>

Int32 is a ValueConverter that converts input values to int64, respecting the
limits of an int32 value.

<pre>var <span id="ResultNoRows">ResultNoRows</span> noRows</pre>

ResultNoRows is a pre-defined Result for drivers to return when a DDL command
(such as a CREATE TABLE) succeeds. It returns an error for both LastInsertId and
RowsAffected.

<pre>var <span id="String">String</span> stringType</pre>

String is a ValueConverter that converts its input to a string. If the value is
already a string or []byte, it's unchanged. If the value is of another type,
conversion to string is done with fmt.Sprintf("%v", v).

<h2 id="IsScanValue">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/types.go#L178">IsScanValue</a>
    <a href="#IsScanValue">¶</a></h2>
<pre>func IsScanValue(v interface{}) <a href="/builtin/#bool">bool</a></pre>

IsScanValue is equivalent to IsValue. It exists for compatibility.

<h2 id="IsValue">func <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/types.go#L165">IsValue</a>
    <a href="#IsValue">¶</a></h2>
<pre>func IsValue(v interface{}) <a href="/builtin/#bool">bool</a></pre>

IsValue reports whether v is a valid Value parameter type.

<h2 id="ColumnConverter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L348">ColumnConverter</a>
    <a href="#ColumnConverter">¶</a></h2>
<pre>type ColumnConverter interface {
    <span class="comment">// ColumnConverter returns a ValueConverter for the provided</span>
    <span class="comment">// column index. If the type of a specific column isn&#39;t known</span>
    <span class="comment">// or shouldn&#39;t be handled specially, DefaultValueConverter</span>
    <span class="comment">// can be returned.</span>
    ColumnConverter(idx <a href="/builtin/#int">int</a>) <a href="#ValueConverter">ValueConverter</a>
}</pre>

ColumnConverter may be optionally implemented by Stmt if the statement is aware
of its own columns' types and can convert from any type to a driver Value.

Deprecated: Drivers should implement NamedValueChecker.

<h2 id="Conn">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L181">Conn</a>
    <a href="#Conn">¶</a></h2>
<pre>type Conn interface {
    <span class="comment">// Prepare returns a prepared statement, bound to this connection.</span>
    Prepare(query <a href="/builtin/#string">string</a>) (<a href="#Stmt">Stmt</a>, <a href="/builtin/#error">error</a>)

    <span class="comment">// Close invalidates and potentially stops any current</span>
    <span class="comment">// prepared statements and transactions, marking this</span>
    <span class="comment">// connection as no longer in use.</span>
    <span class="comment">//</span>
    <span class="comment">// Because the sql package maintains a free pool of</span>
    <span class="comment">// connections and only calls Close when there&#39;s a surplus of</span>
    <span class="comment">// idle connections, it shouldn&#39;t be necessary for drivers to</span>
    <span class="comment">// do their own connection caching.</span>
    Close() <a href="/builtin/#error">error</a>

    <span class="comment">// Begin starts and returns a new transaction.</span>
    <span class="comment">//</span>
    <span class="comment">// Deprecated: Drivers should implement ConnBeginTx instead (or additionally).</span>
    Begin() (<a href="#Tx">Tx</a>, <a href="/builtin/#error">error</a>)
}</pre>

Conn is a connection to a database. It is not used concurrently by multiple
goroutines.

Conn is assumed to be stateful.

<h2 id="ConnBeginTx">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L224">ConnBeginTx</a>
    <a href="#ConnBeginTx">¶</a></h2>
<pre>type ConnBeginTx interface {
    <span class="comment">// BeginTx starts and returns a new transaction.</span>
    <span class="comment">// If the context is canceled by the user the sql package will</span>
    <span class="comment">// call Tx.Rollback before discarding and closing the connection.</span>
    <span class="comment">//</span>
    <span class="comment">// This must check opts.Isolation to determine if there is a set</span>
    <span class="comment">// isolation level. If the driver does not support a non-default</span>
    <span class="comment">// level and one is set or if there is a non-default isolation level</span>
    <span class="comment">// that is not supported, an error must be returned.</span>
    <span class="comment">//</span>
    <span class="comment">// This must also check opts.ReadOnly to determine if the read-only</span>
    <span class="comment">// value is true to either set the read-only transaction property if supported</span>
    <span class="comment">// or return an error if it is not supported.</span>
    BeginTx(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, opts <a href="#TxOptions">TxOptions</a>) (<a href="#Tx">Tx</a>, <a href="/builtin/#error">error</a>)
}</pre>

ConnBeginTx enhances the Conn interface with context and TxOptions.

<h2 id="ConnPrepareContext">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L202">ConnPrepareContext</a>
    <a href="#ConnPrepareContext">¶</a></h2>
<pre>type ConnPrepareContext interface {
    <span class="comment">// PrepareContext returns a prepared statement, bound to this connection.</span>
    <span class="comment">// context is for the preparation of the statement,</span>
    <span class="comment">// it must not store the context within the statement itself.</span>
    PrepareContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>) (<a href="#Stmt">Stmt</a>, <a href="/builtin/#error">error</a>)
}</pre>

ConnPrepareContext enhances the Conn interface with context.

<h2 id="Connector">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L74">Connector</a>
    <a href="#Connector">¶</a></h2>
<pre>type Connector interface {
    <span class="comment">// Connect returns a connection to the database.</span>
    <span class="comment">// Connect may return a cached connection (one previously</span>
    <span class="comment">// closed), but doing so is unnecessary; the sql package</span>
    <span class="comment">// maintains a pool of idle connections for efficient re-use.</span>
    <span class="comment">//</span>
    <span class="comment">// The provided context.Context is for dialing purposes only</span>
    <span class="comment">// (see net.DialContext) and should not be stored or used for</span>
    <span class="comment">// other purposes.</span>
    <span class="comment">//</span>
    <span class="comment">// The returned connection is only used by one goroutine at a</span>
    <span class="comment">// time.</span>
    Connect(<a href="/context/">context</a>.<a href="/context/#Context">Context</a>) (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)

    <span class="comment">// Driver returns the underlying Driver of the Connector,</span>
    <span class="comment">// mainly to maintain compatibility with the Driver method</span>
    <span class="comment">// on sql.DB.</span>
    Driver() <a href="#Driver">Driver</a>
}</pre>

A Connector represents a driver in a fixed configuration and can create any
number of equivalent Conns for use by multiple goroutines.

A Connector can be passed to sql.OpenDB, to allow drivers to implement their own
sql.DB constructors, or returned by DriverContext's OpenConnector method, to
allow drivers access to context and to avoid repeated parsing of driver
configuration.

<h2 id="Driver">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L40">Driver</a>
    <a href="#Driver">¶</a></h2>
<pre>type Driver interface {
    <span class="comment">// Open returns a new connection to the database.</span>
    <span class="comment">// The name is a string in a driver-specific format.</span>
    <span class="comment">//</span>
    <span class="comment">// Open may return a cached connection (one previously</span>
    <span class="comment">// closed), but doing so is unnecessary; the sql package</span>
    <span class="comment">// maintains a pool of idle connections for efficient re-use.</span>
    <span class="comment">//</span>
    <span class="comment">// The returned connection is only used by one goroutine at a</span>
    <span class="comment">// time.</span>
    Open(name <a href="/builtin/#string">string</a>) (<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)
}</pre>

Driver is the interface that must be implemented by a database driver.

Database drivers may implement DriverContext for access to contexts and to parse
the name only once for a pool of connections, instead of once per connection.

<h2 id="DriverContext">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L59">DriverContext</a>
    <a href="#DriverContext">¶</a></h2>
<pre>type DriverContext interface {
    <span class="comment">// OpenConnector must parse the name in the same format that Driver.Open</span>
    <span class="comment">// parses the name parameter.</span>
    OpenConnector(name <a href="/builtin/#string">string</a>) (<a href="#Connector">Connector</a>, <a href="/builtin/#error">error</a>)
}</pre>

If a Driver implements DriverContext, then sql.DB will call OpenConnector to
obtain a Connector and then invoke that Connector's Conn method to obtain each
needed connection, instead of invoking the Driver's Open method for each
connection. The two-step sequence allows drivers to parse the name just once and
also provides access to per-Conn contexts.

<h2 id="Execer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L132">Execer</a>
    <a href="#Execer">¶</a></h2>
<pre>type Execer interface {
    Exec(query <a href="/builtin/#string">string</a>, args []<a href="#Value">Value</a>) (<a href="#Result">Result</a>, <a href="/builtin/#error">error</a>)
}</pre>

Execer is an optional interface that may be implemented by a Conn.

If a Conn implements neither ExecerContext nor Execer Execer, the sql package's
DB.Exec will first prepare a query, execute the statement, and then close the
statement.

Exec may return ErrSkip.

Deprecated: Drivers should implement ExecerContext instead.

<h2 id="ExecerContext">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L146">ExecerContext</a>
    <a href="#ExecerContext">¶</a></h2>
<pre>type ExecerContext interface {
    ExecContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>, args []<a href="#NamedValue">NamedValue</a>) (<a href="#Result">Result</a>, <a href="/builtin/#error">error</a>)
}</pre>

ExecerContext is an optional interface that may be implemented by a Conn.

If a Conn does not implement ExecerContext, the sql package's DB.Exec will fall
back to Execer; if the Conn does not implement Execer either, DB.Exec will first
prepare a query, execute the statement, and then close the statement.

ExecerContext may return ErrSkip.

ExecerContext must honor the context timeout and return when the context is
canceled.

<h2 id="IsolationLevel">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L213">IsolationLevel</a>
    <a href="#IsolationLevel">¶</a></h2>
<pre>type IsolationLevel <a href="/builtin/#int">int</a></pre>

IsolationLevel is the transaction isolation level stored in TxOptions.

This type should be considered identical to sql.IsolationLevel along with any
values defined on it.

<h2 id="NamedValue">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L20">NamedValue</a>
    <a href="#NamedValue">¶</a></h2>
<pre>type NamedValue struct {
    <span class="comment">// If the Name is not empty it should be used for the parameter identifier and</span>
    <span class="comment">// not the ordinal position.</span>
    <span class="comment">//</span>
<span id="NamedValue.Name"></span>    <span class="comment">// Name will not have a symbol prefix.</span>
    Name <a href="/builtin/#string">string</a>

<span id="NamedValue.Ordinal"></span>    <span class="comment">// Ordinal position of the parameter starting from one and is always set.</span>
    Ordinal <a href="/builtin/#int">int</a>

<span id="NamedValue.Value"></span>    <span class="comment">// Value is the parameter value.</span>
    Value <a href="#Value">Value</a>
}</pre>

NamedValue holds both the value name and value.

<h2 id="NamedValueChecker">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L336">NamedValueChecker</a>
    <a href="#NamedValueChecker">¶</a></h2>
<pre>type NamedValueChecker interface {
    <span class="comment">// CheckNamedValue is called before passing arguments to the driver</span>
    <span class="comment">// and is called in place of any ColumnConverter. CheckNamedValue must do type</span>
    <span class="comment">// validation and conversion as appropriate for the driver.</span>
    CheckNamedValue(*<a href="#NamedValue">NamedValue</a>) <a href="/builtin/#error">error</a>
}</pre>

NamedValueChecker may be optionally implemented by Conn or Stmt. It provides the
driver more control to handle Go and database types beyond the default Values
types allowed.

The sql package checks for value checkers in the following order, stopping at
the first found match: Stmt.NamedValueChecker, Conn.NamedValueChecker,
Stmt.ColumnConverter, DefaultParameterConverter.

If CheckNamedValue returns ErrRemoveArgument, the NamedValue will not be
included in the final query arguments. This may be used to pass special options
to the query itself.

If ErrSkip is returned the column converter error checking path is used for the
argument. Drivers may wish to return ErrSkip after they have exhausted their own
special cases.

<h2 id="NotNull">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/types.go#L153">NotNull</a>
    <a href="#NotNull">¶</a></h2>
<pre>type NotNull struct {
<span id="NotNull.Converter"></span>    Converter <a href="#ValueConverter">ValueConverter</a>
}</pre>

NotNull is a type that implements ValueConverter by disallowing nil values but
otherwise delegating to another ValueConverter.

<h3 id="NotNull.ConvertValue">func (NotNull) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/types.go#L157">ConvertValue</a>
    <a href="#NotNull.ConvertValue">¶</a></h3>
<pre>func (n <a href="#NotNull">NotNull</a>) ConvertValue(v interface{}) (<a href="#Value">Value</a>, <a href="/builtin/#error">error</a>)</pre>


<h2 id="Null">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/types.go#L140">Null</a>
    <a href="#Null">¶</a></h2>
<pre>type Null struct {
<span id="Null.Converter"></span>    Converter <a href="#ValueConverter">ValueConverter</a>
}</pre>

Null is a type that implements ValueConverter by allowing nil values but
otherwise delegating to another ValueConverter.

<h3 id="Null.ConvertValue">func (Null) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/types.go#L144">ConvertValue</a>
    <a href="#Null.ConvertValue">¶</a></h3>
<pre>func (n <a href="#Null">Null</a>) ConvertValue(v interface{}) (<a href="#Value">Value</a>, <a href="/builtin/#error">error</a>)</pre>


<h2 id="Pinger">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L119">Pinger</a>
    <a href="#Pinger">¶</a></h2>
<pre>type Pinger interface {
    Ping(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>) <a href="/builtin/#error">error</a>
}</pre>

Pinger is an optional interface that may be implemented by a Conn.

If a Conn does not implement Pinger, the sql package's DB.Ping and
DB.PingContext will check if there is at least one Conn available.

If Conn.Ping returns ErrBadConn, DB.Ping and DB.PingContext will remove the Conn
from pool.

<h2 id="Queryer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L159">Queryer</a>
    <a href="#Queryer">¶</a></h2>
<pre>type Queryer interface {
    Query(query <a href="/builtin/#string">string</a>, args []<a href="#Value">Value</a>) (<a href="#Rows">Rows</a>, <a href="/builtin/#error">error</a>)
}</pre>

Queryer is an optional interface that may be implemented by a Conn.

If a Conn implements neither QueryerContext nor Queryer, the sql package's
DB.Query will first prepare a query, execute the statement, and then close the
statement.

Query may return ErrSkip.

Deprecated: Drivers should implement QueryerContext instead.

<h2 id="QueryerContext">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L173">QueryerContext</a>
    <a href="#QueryerContext">¶</a></h2>
<pre>type QueryerContext interface {
    QueryContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>, args []<a href="#NamedValue">NamedValue</a>) (<a href="#Rows">Rows</a>, <a href="/builtin/#error">error</a>)
}</pre>

QueryerContext is an optional interface that may be implemented by a Conn.

If a Conn does not implement QueryerContext, the sql package's DB.Query will
fall back to Queryer; if the Conn does not implement Queryer either, DB.Query
will first prepare a query, execute the statement, and then close the statement.

QueryerContext may return ErrSkip.

QueryerContext must honor the context timeout and return when the context is
canceled.

<h2 id="Result">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L253">Result</a>
    <a href="#Result">¶</a></h2>
<pre>type Result interface {
    <span class="comment">// LastInsertId returns the database&#39;s auto-generated ID</span>
    <span class="comment">// after, for example, an INSERT into a table with primary</span>
    <span class="comment">// key.</span>
    LastInsertId() (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)

    <span class="comment">// RowsAffected returns the number of rows affected by the</span>
    <span class="comment">// query.</span>
    RowsAffected() (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)
}</pre>

Result is the result of a query execution.

<h2 id="Rows">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L357">Rows</a>
    <a href="#Rows">¶</a></h2>
<pre>type Rows interface {
    <span class="comment">// Columns returns the names of the columns. The number of</span>
    <span class="comment">// columns of the result is inferred from the length of the</span>
    <span class="comment">// slice. If a particular column name isn&#39;t known, an empty</span>
    <span class="comment">// string should be returned for that entry.</span>
    Columns() []<a href="/builtin/#string">string</a>

    <span class="comment">// Close closes the rows iterator.</span>
    Close() <a href="/builtin/#error">error</a>

    <span class="comment">// Next is called to populate the next row of data into</span>
    <span class="comment">// the provided slice. The provided slice will be the same</span>
    <span class="comment">// size as the Columns() are wide.</span>
    <span class="comment">//</span>
    <span class="comment">// Next should return io.EOF when there are no more rows.</span>
    <span class="comment">//</span>
    <span class="comment">// The dest should not be written to outside of Next. Care</span>
    <span class="comment">// should be taken when closing Rows not to modify</span>
    <span class="comment">// a buffer held in dest.</span>
    Next(dest []<a href="#Value">Value</a>) <a href="/builtin/#error">error</a>
}</pre>

Rows is an iterator over an executed query's results.

<h2 id="RowsAffected">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L457">RowsAffected</a>
    <a href="#RowsAffected">¶</a></h2>
<pre>type RowsAffected <a href="/builtin/#int64">int64</a></pre>

RowsAffected implements Result for an INSERT or UPDATE operation which mutates a
number of rows.

<h3 id="RowsAffected.LastInsertId">func (RowsAffected) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L461">LastInsertId</a>
    <a href="#RowsAffected.LastInsertId">¶</a></h3>
<pre>func (<a href="#RowsAffected">RowsAffected</a>) LastInsertId() (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)</pre>


<h3 id="RowsAffected.RowsAffected">func (RowsAffected) <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L465">RowsAffected</a>
    <a href="#RowsAffected.RowsAffected">¶</a></h3>
<pre>func (v <a href="#RowsAffected">RowsAffected</a>) RowsAffected() (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)</pre>


<h2 id="RowsColumnTypeDatabaseTypeName">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L408">RowsColumnTypeDatabaseTypeName</a>
    <a href="#RowsColumnTypeDatabaseTypeName">¶</a></h2>
<pre>type RowsColumnTypeDatabaseTypeName interface {
    <a href="#Rows">Rows</a>
    ColumnTypeDatabaseTypeName(index <a href="/builtin/#int">int</a>) <a href="/builtin/#string">string</a>
}</pre>

RowsColumnTypeDatabaseTypeName may be implemented by Rows. It should return the
database system type name without the length. Type names should be uppercase.
Examples of returned types: "VARCHAR", "NVARCHAR", "VARCHAR2", "CHAR", "TEXT",
"DECIMAL", "SMALLINT", "INT", "BIGINT", "BOOL", "[]BIGINT", "JSONB", "XML",
"TIMESTAMP".

<h2 id="RowsColumnTypeLength">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L424">RowsColumnTypeLength</a>
    <a href="#RowsColumnTypeLength">¶</a></h2>
<pre>type RowsColumnTypeLength interface {
    <a href="#Rows">Rows</a>
    ColumnTypeLength(index <a href="/builtin/#int">int</a>) (length <a href="/builtin/#int64">int64</a>, ok <a href="/builtin/#bool">bool</a>)
}</pre>

RowsColumnTypeLength may be implemented by Rows. It should return the length of
the column type if the column is a variable length type. If the column is not a
variable length type ok should return false. If length is not limited other than
system limits, it should return math.MaxInt64. The following are examples of
returned values for various types:

    TEXT          (math.MaxInt64, true)
    varchar(10)   (10, true)
    nvarchar(10)  (10, true)
    decimal       (0, false)
    int           (0, false)
    bytea(30)     (30, true)

<h2 id="RowsColumnTypeNullable">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L433">RowsColumnTypeNullable</a>
    <a href="#RowsColumnTypeNullable">¶</a></h2>
<pre>type RowsColumnTypeNullable interface {
    <a href="#Rows">Rows</a>
    ColumnTypeNullable(index <a href="/builtin/#int">int</a>) (nullable, ok <a href="/builtin/#bool">bool</a>)
}</pre>

RowsColumnTypeNullable may be implemented by Rows. The nullable value should be
true if it is known the column may be null, or false if the column is known to
be not nullable. If the column nullability is unknown, ok should be false.

<h2 id="RowsColumnTypePrecisionScale">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L444">RowsColumnTypePrecisionScale</a>
    <a href="#RowsColumnTypePrecisionScale">¶</a></h2>
<pre>type RowsColumnTypePrecisionScale interface {
    <a href="#Rows">Rows</a>
    ColumnTypePrecisionScale(index <a href="/builtin/#int">int</a>) (precision, scale <a href="/builtin/#int64">int64</a>, ok <a href="/builtin/#bool">bool</a>)
}</pre>

RowsColumnTypePrecisionScale may be implemented by Rows. It should return the
precision and scale for decimal types. If not applicable, ok should be false.
The following are examples of returned values for various types:

    decimal(38, 4)    (38, 4, true)
    int               (0, 0, false)
    decimal           (math.MaxInt64, math.MaxInt64, true)

<h2 id="RowsColumnTypeScanType">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L398">RowsColumnTypeScanType</a>
    <a href="#RowsColumnTypeScanType">¶</a></h2>
<pre>type RowsColumnTypeScanType interface {
    <a href="#Rows">Rows</a>
    ColumnTypeScanType(index <a href="/builtin/#int">int</a>) <a href="/reflect/">reflect</a>.<a href="/reflect/#Type">Type</a>
}</pre>

RowsColumnTypeScanType may be implemented by Rows. It should return the value
type that can be used to scan types into. For example, the database column type
"bigint" this should return "reflect.TypeOf(int64(0))".

<h2 id="RowsNextResultSet">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L381">RowsNextResultSet</a>
    <a href="#RowsNextResultSet">¶</a></h2>
<pre>type RowsNextResultSet interface {
    <a href="#Rows">Rows</a>

    <span class="comment">// HasNextResultSet is called at the end of the current result set and</span>
    <span class="comment">// reports whether there is another result set after the current one.</span>
    HasNextResultSet() <a href="/builtin/#bool">bool</a>

    <span class="comment">// NextResultSet advances the driver to the next result set even</span>
    <span class="comment">// if there are remaining rows in the current result set.</span>
    <span class="comment">//</span>
    <span class="comment">// NextResultSet should return io.EOF when there are no more result sets.</span>
    NextResultSet() <a href="/builtin/#error">error</a>
}</pre>

RowsNextResultSet extends the Rows interface by providing a way to signal the
driver to advance to the next result set.

<h2 id="SessionResetter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L242">SessionResetter</a>
    <a href="#SessionResetter">¶</a></h2>
<pre>type SessionResetter interface {
    <span class="comment">// ResetSession is called while a connection is in the connection</span>
    <span class="comment">// pool. No queries will run on this connection until this method returns.</span>
    <span class="comment">//</span>
    <span class="comment">// If the connection is bad this should return driver.ErrBadConn to prevent</span>
    <span class="comment">// the connection from being returned to the connection pool. Any other</span>
    <span class="comment">// error will be discarded.</span>
    ResetSession(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>) <a href="/builtin/#error">error</a>
}</pre>

SessionResetter may be implemented by Conn to allow drivers to reset the session
state associated with the connection and to signal a bad connection.

<h2 id="Stmt">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L266">Stmt</a>
    <a href="#Stmt">¶</a></h2>
<pre>type Stmt interface {
    <span class="comment">// Close closes the statement.</span>
    <span class="comment">//</span>
    <span class="comment">// As of Go 1.1, a Stmt will not be closed if it&#39;s in use</span>
    <span class="comment">// by any queries.</span>
    Close() <a href="/builtin/#error">error</a>

    <span class="comment">// NumInput returns the number of placeholder parameters.</span>
    <span class="comment">//</span>
    <span class="comment">// If NumInput returns &gt;= 0, the sql package will sanity check</span>
    <span class="comment">// argument counts from callers and return errors to the caller</span>
    <span class="comment">// before the statement&#39;s Exec or Query methods are called.</span>
    <span class="comment">//</span>
    <span class="comment">// NumInput may also return -1, if the driver doesn&#39;t know</span>
    <span class="comment">// its number of placeholders. In that case, the sql package</span>
    <span class="comment">// will not sanity check Exec or Query argument counts.</span>
    NumInput() <a href="/builtin/#int">int</a>

    <span class="comment">// Exec executes a query that doesn&#39;t return rows, such</span>
    <span class="comment">// as an INSERT or UPDATE.</span>
    <span class="comment">//</span>
    <span class="comment">// Deprecated: Drivers should implement StmtExecContext instead (or additionally).</span>
    Exec(args []<a href="#Value">Value</a>) (<a href="#Result">Result</a>, <a href="/builtin/#error">error</a>)

    <span class="comment">// Query executes a query that may return rows, such as a</span>
    <span class="comment">// SELECT.</span>
    <span class="comment">//</span>
    <span class="comment">// Deprecated: Drivers should implement StmtQueryContext instead (or additionally).</span>
    Query(args []<a href="#Value">Value</a>) (<a href="#Rows">Rows</a>, <a href="/builtin/#error">error</a>)
}</pre>

Stmt is a prepared statement. It is bound to a Conn and not used by multiple
goroutines concurrently.

<h2 id="StmtExecContext">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L298">StmtExecContext</a>
    <a href="#StmtExecContext">¶</a></h2>
<pre>type StmtExecContext interface {
    <span class="comment">// ExecContext executes a query that doesn&#39;t return rows, such</span>
    <span class="comment">// as an INSERT or UPDATE.</span>
    <span class="comment">//</span>
    <span class="comment">// ExecContext must honor the context timeout and return when it is canceled.</span>
    ExecContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, args []<a href="#NamedValue">NamedValue</a>) (<a href="#Result">Result</a>, <a href="/builtin/#error">error</a>)
}</pre>

StmtExecContext enhances the Stmt interface by providing Exec with context.

<h2 id="StmtQueryContext">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L307">StmtQueryContext</a>
    <a href="#StmtQueryContext">¶</a></h2>
<pre>type StmtQueryContext interface {
    <span class="comment">// QueryContext executes a query that may return rows, such as a</span>
    <span class="comment">// SELECT.</span>
    <span class="comment">//</span>
    <span class="comment">// QueryContext must honor the context timeout and return when it is canceled.</span>
    QueryContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, args []<a href="#NamedValue">NamedValue</a>) (<a href="#Rows">Rows</a>, <a href="/builtin/#error">error</a>)
}</pre>

StmtQueryContext enhances the Stmt interface by providing Query with context.

<h2 id="Tx">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L450">Tx</a>
    <a href="#Tx">¶</a></h2>
<pre>type Tx interface {
    Commit() <a href="/builtin/#error">error</a>
    Rollback() <a href="/builtin/#error">error</a>
}</pre>

Tx is a transaction.

<h2 id="TxOptions">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L218">TxOptions</a>
    <a href="#TxOptions">¶</a></h2>
<pre>type TxOptions struct {
<span id="TxOptions.Isolation"></span>    Isolation <a href="#IsolationLevel">IsolationLevel</a>
<span id="TxOptions.ReadOnly"></span>    ReadOnly  <a href="/builtin/#bool">bool</a>
}</pre>

TxOptions holds the transaction options.

This type should be considered identical to sql.TxOptions.

<h2 id="Value">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/driver.go#L17">Value</a>
    <a href="#Value">¶</a></h2>
<pre>type Value interface{}</pre>

Value is a value that drivers must be able to handle. It is either nil, a type
handled by a database driver's NamedValueChecker interface, or an instance of
one of these types:

    int64
    float64
    bool
    []byte
    string
    time.Time

<h2 id="ValueConverter">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/types.go#L20">ValueConverter</a>
    <a href="#ValueConverter">¶</a></h2>
<pre>type ValueConverter interface {
    <span class="comment">// ConvertValue converts a value to a driver Value.</span>
    ConvertValue(v interface{}) (<a href="#Value">Value</a>, <a href="/builtin/#error">error</a>)
}</pre>

ValueConverter is the interface providing the ConvertValue method.

Various implementations of ValueConverter are provided by the driver package to
provide consistent implementations of conversions between drivers. The
ValueConverters have several uses:

    * converting from the Value types as provided by the sql package
      into a database table's specific column type and making sure it
      fits, such as making sure a particular int64 fits in a
      table's uint16 column.

    * converting a value as given from the database into one of the
      driver Value types.

    * by the sql package, for converting from a driver's Value type
      to a user's type in a scan.

<h2 id="Valuer">type <a href="//github.com/golang/go/blob/release-branch.go1.10/src/database/sql/driver/types.go#L29">Valuer</a>
    <a href="#Valuer">¶</a></h2>
<pre>type Valuer interface {
    <span class="comment">// Value returns a driver Value.</span>
    Value() (<a href="#Value">Value</a>, <a href="/builtin/#error">error</a>)
}</pre>

Valuer is the interface providing the Value method.

Types implementing Valuer interface are able to convert themselves to a driver
Value.


