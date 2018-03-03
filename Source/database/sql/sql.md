version: 1.10
## package sql

  `import "database/sql"`

## Overview

Package sql provides a generic interface around SQL (or SQL-like) databases.

The sql package must be used in conjunction with a database driver. See
https://golang.org/s/sqldrivers for a list of drivers.

Drivers that do not support context cancelation will not return until after the
query is completed.

For usage examples, see the wiki page at https://golang.org/s/sqlwiki.

## Index

- [Variables](#pkg-variables)
- [func Drivers() []string](#Drivers)
- [func Register(name string, driver driver.Driver)](#Register)
- [type ColumnType](#ColumnType)
  - [func (ci *ColumnType) DatabaseTypeName() string](#ColumnType.DatabaseTypeName)
  - [func (ci *ColumnType) DecimalSize() (precision, scale int64, ok bool)](#ColumnType.DecimalSize)
  - [func (ci *ColumnType) Length() (length int64, ok bool)](#ColumnType.Length)
  - [func (ci *ColumnType) Name() string](#ColumnType.Name)
  - [func (ci *ColumnType) Nullable() (nullable, ok bool)](#ColumnType.Nullable)
  - [func (ci *ColumnType) ScanType() reflect.Type](#ColumnType.ScanType)
- [type Conn](#Conn)
  - [func (c *Conn) BeginTx(ctx context.Context, opts *TxOptions) (*Tx, error)](#Conn.BeginTx)
  - [func (c *Conn) Close() error](#Conn.Close)
  - [func (c *Conn) ExecContext(ctx context.Context, query string, args ...interface{}) (Result, error)](#Conn.ExecContext)
  - [func (c *Conn) PingContext(ctx context.Context) error](#Conn.PingContext)
  - [func (c *Conn) PrepareContext(ctx context.Context, query string) (*Stmt, error)](#Conn.PrepareContext)
  - [func (c *Conn) QueryContext(ctx context.Context, query string, args ...interface{}) (*Rows, error)](#Conn.QueryContext)
  - [func (c *Conn) QueryRowContext(ctx context.Context, query string, args ...interface{}) *Row](#Conn.QueryRowContext)
- [type DB](#DB)
  - [func Open(driverName, dataSourceName string) (*DB, error)](#Open)
  - [func OpenDB(c driver.Connector) *DB](#OpenDB)
  - [func (db *DB) Begin() (*Tx, error)](#DB.Begin)
  - [func (db *DB) BeginTx(ctx context.Context, opts *TxOptions) (*Tx, error)](#DB.BeginTx)
  - [func (db *DB) Close() error](#DB.Close)
  - [func (db *DB) Conn(ctx context.Context) (*Conn, error)](#DB.Conn)
  - [func (db *DB) Driver() driver.Driver](#DB.Driver)
  - [func (db *DB) Exec(query string, args ...interface{}) (Result, error)](#DB.Exec)
  - [func (db *DB) ExecContext(ctx context.Context, query string, args ...interface{}) (Result, error)](#DB.ExecContext)
  - [func (db *DB) Ping() error](#DB.Ping)
  - [func (db *DB) PingContext(ctx context.Context) error](#DB.PingContext)
  - [func (db *DB) Prepare(query string) (*Stmt, error)](#DB.Prepare)
  - [func (db *DB) PrepareContext(ctx context.Context, query string) (*Stmt, error)](#DB.PrepareContext)
  - [func (db *DB) Query(query string, args ...interface{}) (*Rows, error)](#DB.Query)
  - [func (db *DB) QueryContext(ctx context.Context, query string, args ...interface{}) (*Rows, error)](#DB.QueryContext)
  - [func (db *DB) QueryRow(query string, args ...interface{}) *Row](#DB.QueryRow)
  - [func (db *DB) QueryRowContext(ctx context.Context, query string, args ...interface{}) *Row](#DB.QueryRowContext)
  - [func (db *DB) SetConnMaxLifetime(d time.Duration)](#DB.SetConnMaxLifetime)
  - [func (db *DB) SetMaxIdleConns(n int)](#DB.SetMaxIdleConns)
  - [func (db *DB) SetMaxOpenConns(n int)](#DB.SetMaxOpenConns)
  - [func (db *DB) Stats() DBStats](#DB.Stats)
- [type DBStats](#DBStats)
- [type IsolationLevel](#IsolationLevel)
- [type NamedArg](#NamedArg)
  - [func Named(name string, value interface{}) NamedArg](#Named)
- [type NullBool](#NullBool)
  - [func (n *NullBool) Scan(value interface{}) error](#NullBool.Scan)
  - [func (n NullBool) Value() (driver.Value, error)](#NullBool.Value)
- [type NullFloat64](#NullFloat64)
  - [func (n *NullFloat64) Scan(value interface{}) error](#NullFloat64.Scan)
  - [func (n NullFloat64) Value() (driver.Value, error)](#NullFloat64.Value)
- [type NullInt64](#NullInt64)
  - [func (n *NullInt64) Scan(value interface{}) error](#NullInt64.Scan)
  - [func (n NullInt64) Value() (driver.Value, error)](#NullInt64.Value)
- [type NullString](#NullString)
  - [func (ns *NullString) Scan(value interface{}) error](#NullString.Scan)
  - [func (ns NullString) Value() (driver.Value, error)](#NullString.Value)
- [type Out](#Out)
- [type RawBytes](#RawBytes)
- [type Result](#Result)
- [type Row](#Row)
  - [func (r *Row) Scan(dest ...interface{}) error](#Row.Scan)
- [type Rows](#Rows)
  - [func (rs *Rows) Close() error](#Rows.Close)
  - [func (rs *Rows) ColumnTypes() ([]*ColumnType, error)](#Rows.ColumnTypes)
  - [func (rs *Rows) Columns() ([]string, error)](#Rows.Columns)
  - [func (rs *Rows) Err() error](#Rows.Err)
  - [func (rs *Rows) Next() bool](#Rows.Next)
  - [func (rs *Rows) NextResultSet() bool](#Rows.NextResultSet)
  - [func (rs *Rows) Scan(dest ...interface{}) error](#Rows.Scan)
- [type Scanner](#Scanner)
- [type Stmt](#Stmt)
  - [func (s *Stmt) Close() error](#Stmt.Close)
  - [func (s *Stmt) Exec(args ...interface{}) (Result, error)](#Stmt.Exec)
  - [func (s *Stmt) ExecContext(ctx context.Context, args ...interface{}) (Result, error)](#Stmt.ExecContext)
  - [func (s *Stmt) Query(args ...interface{}) (*Rows, error)](#Stmt.Query)
  - [func (s *Stmt) QueryContext(ctx context.Context, args ...interface{}) (*Rows, error)](#Stmt.QueryContext)
  - [func (s *Stmt) QueryRow(args ...interface{}) *Row](#Stmt.QueryRow)
  - [func (s *Stmt) QueryRowContext(ctx context.Context, args ...interface{}) *Row](#Stmt.QueryRowContext)
- [type Tx](#Tx)
  - [func (tx *Tx) Commit() error](#Tx.Commit)
  - [func (tx *Tx) Exec(query string, args ...interface{}) (Result, error)](#Tx.Exec)
  - [func (tx *Tx) ExecContext(ctx context.Context, query string, args ...interface{}) (Result, error)](#Tx.ExecContext)
  - [func (tx *Tx) Prepare(query string) (*Stmt, error)](#Tx.Prepare)
  - [func (tx *Tx) PrepareContext(ctx context.Context, query string) (*Stmt, error)](#Tx.PrepareContext)
  - [func (tx *Tx) Query(query string, args ...interface{}) (*Rows, error)](#Tx.Query)
  - [func (tx *Tx) QueryContext(ctx context.Context, query string, args ...interface{}) (*Rows, error)](#Tx.QueryContext)
  - [func (tx *Tx) QueryRow(query string, args ...interface{}) *Row](#Tx.QueryRow)
  - [func (tx *Tx) QueryRowContext(ctx context.Context, query string, args ...interface{}) *Row](#Tx.QueryRowContext)
  - [func (tx *Tx) Rollback() error](#Tx.Rollback)
  - [func (tx *Tx) Stmt(stmt *Stmt) *Stmt](#Tx.Stmt)
  - [func (tx *Tx) StmtContext(ctx context.Context, stmt *Stmt) *Stmt](#Tx.StmtContext)
- [type TxOptions](#TxOptions)

### Examples

- [DB.Query](#exampleDB_Query)
- [DB.QueryRow](#exampleDB_QueryRow)
- [DB.Query (MultipleResultSets)](#exampleDB_Query_multipleResultSets)

### Package files
 [convert.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/convert.go) [ctxutil.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/ctxutil.go) [sql.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go)

<h2 id="pkg-variables">Variables</h2>

<pre>var <span id="ErrConnDone">ErrConnDone</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;database/sql: connection is already closed&#34;)</pre>

ErrConnDone is returned by any operation that is performed on a connection that
has already been returned to the connection pool.

<pre>var <span id="ErrNoRows">ErrNoRows</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;sql: no rows in result set&#34;)</pre>

ErrNoRows is returned by Scan when QueryRow doesn't return a row. In such a
case, QueryRow returns a placeholder *Row value that defers this error until a
Scan.

<pre>var <span id="ErrTxDone">ErrTxDone</span> = <a href="/errors/">errors</a>.<a href="/errors/#New">New</a>(&#34;sql: Transaction has already been committed or rolled back&#34;)</pre>

ErrTxDone is returned by any operation that is performed on a transaction that
has already been committed or rolled back.

<h2 id="Drivers">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L53">Drivers</a>
    <a href="#Drivers">¶</a></h2>
<pre>func Drivers() []<a href="/builtin/#string">string</a></pre>

Drivers returns a sorted list of the names of the registered drivers.

<h2 id="Register">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L33">Register</a>
    <a href="#Register">¶</a></h2>
<pre>func Register(name <a href="/builtin/#string">string</a>, driver <a href="/database/sql/driver/">driver</a>.<a href="/database/sql/driver/#Driver">Driver</a>)</pre>

Register makes a database driver available by the provided name. If Register is
called twice with the same name or if driver is nil, it panics.

<h2 id="ColumnType">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2722">ColumnType</a>
    <a href="#ColumnType">¶</a></h2>
<pre>type ColumnType struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

ColumnType contains the name and type of a column.

<h3 id="ColumnType.DatabaseTypeName">func (*ColumnType) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2775">DatabaseTypeName</a>
    <a href="#ColumnType.DatabaseTypeName">¶</a></h3>
<pre>func (ci *<a href="#ColumnType">ColumnType</a>) DatabaseTypeName() <a href="/builtin/#string">string</a></pre>

DatabaseTypeName returns the database system name of the column type. If an
empty string is returned the driver type name is not supported. Consult your
driver documentation for a list of driver data types. Length specifiers are not
included. Common type include "VARCHAR", "TEXT", "NVARCHAR", "DECIMAL", "BOOL",
"INT", "BIGINT".

<h3 id="ColumnType.DecimalSize">func (*ColumnType) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2753">DecimalSize</a>
    <a href="#ColumnType.DecimalSize">¶</a></h3>
<pre>func (ci *<a href="#ColumnType">ColumnType</a>) DecimalSize() (precision, scale <a href="/builtin/#int64">int64</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

DecimalSize returns the scale and precision of a decimal type. If not applicable
or if not supported ok is false.

<h3 id="ColumnType.Length">func (*ColumnType) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2747">Length</a>
    <a href="#ColumnType.Length">¶</a></h3>
<pre>func (ci *<a href="#ColumnType">ColumnType</a>) Length() (length <a href="/builtin/#int64">int64</a>, ok <a href="/builtin/#bool">bool</a>)</pre>

Length returns the column type length for variable length column types such as
text and binary field types. If the type length is unbounded the value will be
math.MaxInt64 (any database limits will still apply). If the column type is not
variable length, such as an int, or if not supported by the driver ok is false.

<h3 id="ColumnType.Name">func (*ColumnType) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2738">Name</a>
    <a href="#ColumnType.Name">¶</a></h3>
<pre>func (ci *<a href="#ColumnType">ColumnType</a>) Name() <a href="/builtin/#string">string</a></pre>

Name returns the name or alias of the column.

<h3 id="ColumnType.Nullable">func (*ColumnType) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2766">Nullable</a>
    <a href="#ColumnType.Nullable">¶</a></h3>
<pre>func (ci *<a href="#ColumnType">ColumnType</a>) Nullable() (nullable, ok <a href="/builtin/#bool">bool</a>)</pre>

Nullable returns whether the column may be null. If a driver does not support
this property ok will be false.

<h3 id="ColumnType.ScanType">func (*ColumnType) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2760">ScanType</a>
    <a href="#ColumnType.ScanType">¶</a></h3>
<pre>func (ci *<a href="#ColumnType">ColumnType</a>) ScanType() <a href="/reflect/">reflect</a>.<a href="/reflect/#Type">Type</a></pre>

ScanType returns a Go type suitable for scanning into using Rows.Scan. If a
driver does not support this property ScanType will return the type of an empty
interface.

<h2 id="Conn">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1639">Conn</a>
    <a href="#Conn">¶</a></h2>
<pre>type Conn struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Conn represents a single database connection rather than a pool of database
connections. Prefer running queries from DB unless there is a specific need for
a continuous single database connection.

A Conn must call Close to return the connection to the database pool and may do
so concurrently with a running query.

After a call to Close, all operations on the connection fail with ErrConnDone.

<h3 id="Conn.BeginTx">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1731">BeginTx</a>
    <a href="#Conn.BeginTx">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) BeginTx(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, opts *<a href="#TxOptions">TxOptions</a>) (*<a href="#Tx">Tx</a>, <a href="/builtin/#error">error</a>)</pre>

BeginTx starts a transaction.

The provided context is used until the transaction is committed or rolled back.
If the context is canceled, the sql package will roll back the transaction.
Tx.Commit will return an error if the context provided to BeginTx is canceled.

The provided TxOptions is optional and may be nil if defaults should be used. If
a non-default isolation level is used that the driver doesn't support, an error
will be returned.

<h3 id="Conn.Close">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1773">Close</a>
    <a href="#Conn.Close">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) Close() <a href="/builtin/#error">error</a></pre>

Close returns the connection to the connection pool. All operations after a
Close will return with ErrConnDone. Close is safe to call concurrently with
other operations and will block until all other operations finish. It may be
useful to first cancel any used context and then call close directly after.

<h3 id="Conn.ExecContext">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1676">ExecContext</a>
    <a href="#Conn.ExecContext">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) ExecContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>, args ...interface{}) (<a href="#Result">Result</a>, <a href="/builtin/#error">error</a>)</pre>

ExecContext executes a query without returning any rows. The args are for any
placeholder parameters in the query.

<h3 id="Conn.PingContext">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1666">PingContext</a>
    <a href="#Conn.PingContext">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) PingContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>) <a href="/builtin/#error">error</a></pre>

PingContext verifies the connection to the database is still alive.

<h3 id="Conn.PrepareContext">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1713">PrepareContext</a>
    <a href="#Conn.PrepareContext">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) PrepareContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>) (*<a href="#Stmt">Stmt</a>, <a href="/builtin/#error">error</a>)</pre>

PrepareContext creates a prepared statement for later queries or executions.
Multiple queries or executions may be run concurrently from the returned
statement. The caller must call the statement's Close method when the statement
is no longer needed.

The provided context is used for the preparation of the statement, not for the
execution of the statement.

<h3 id="Conn.QueryContext">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1686">QueryContext</a>
    <a href="#Conn.QueryContext">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) QueryContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>, args ...interface{}) (*<a href="#Rows">Rows</a>, <a href="/builtin/#error">error</a>)</pre>

QueryContext executes a query that returns rows, typically a SELECT. The args
are for any placeholder parameters in the query.

<h3 id="Conn.QueryRowContext">func (*Conn) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1700">QueryRowContext</a>
    <a href="#Conn.QueryRowContext">¶</a></h3>
<pre>func (c *<a href="#Conn">Conn</a>) QueryRowContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>, args ...interface{}) *<a href="#Row">Row</a></pre>

QueryRowContext executes a query that is expected to return at most one row.
QueryRowContext always returns a non-nil value. Errors are deferred until Row's
Scan method is called. If the query selects no rows, the *Row's Scan will return
ErrNoRows. Otherwise, the *Row's Scan scans the first selected row and discards
the rest.

<h2 id="DB">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L309">DB</a>
    <a href="#DB">¶</a></h2>
<pre>type DB struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

DB is a database handle representing a pool of zero or more underlying
connections. It's safe for concurrent use by multiple goroutines.

The sql package creates and frees connections automatically; it also maintains a
free pool of idle connections. If the database has a concept of per-connection
state, such state can only be reliably observed within a transaction. Once
DB.Begin is called, the returned Tx is bound to a single connection. Once Commit
or Rollback is called on the transaction, that transaction's connection is
returned to DB's idle connection pool. The pool size can be controlled with
SetMaxIdleConns.

<h3 id="Open">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L647">Open</a>
    <a href="#Open">¶</a></h3>
<pre>func Open(driverName, dataSourceName <a href="/builtin/#string">string</a>) (*<a href="#DB">DB</a>, <a href="/builtin/#error">error</a>)</pre>

Open opens a database specified by its database driver name and a
driver-specific data source name, usually consisting of at least a database name
and connection information.

Most users will open a database via a driver-specific connection helper function
that returns a *DB. No database drivers are included in the Go standard library.
See https://golang.org/s/sqldrivers for a list of third-party drivers.

Open may just validate its arguments without creating a connection to the
database. To verify that the data source name is valid, call Ping.

The returned DB is safe for concurrent use by multiple goroutines and maintains
its own pool of idle connections. Thus, the Open function should be called just
once. It is rarely necessary to close a DB.

<h3 id="OpenDB">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L613">OpenDB</a>
    <a href="#OpenDB">¶</a></h3>
<pre>func OpenDB(c <a href="/database/sql/driver/">driver</a>.<a href="/database/sql/driver/#Connector">Connector</a>) *<a href="#DB">DB</a></pre>

OpenDB opens a database using a Connector, allowing drivers to bypass a string
based data source name.

Most users will open a database via a driver-specific connection helper function
that returns a *DB. No database drivers are included in the Go standard library.
See https://golang.org/s/sqldrivers for a list of third-party drivers.

OpenDB may just validate its arguments without creating a connection to the
database. To verify that the data source name is valid, call Ping.

The returned DB is safe for concurrent use by multiple goroutines and maintains
its own pool of idle connections. Thus, the OpenDB function should be called
just once. It is rarely necessary to close a DB.

<h3 id="DB.Begin">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1551">Begin</a>
    <a href="#DB.Begin">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) Begin() (*<a href="#Tx">Tx</a>, <a href="/builtin/#error">error</a>)</pre>

Begin starts a transaction. The default isolation level is dependent on the
driver.

<h3 id="DB.BeginTx">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1534">BeginTx</a>
    <a href="#DB.BeginTx">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) BeginTx(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, opts *<a href="#TxOptions">TxOptions</a>) (*<a href="#Tx">Tx</a>, <a href="/builtin/#error">error</a>)</pre>

BeginTx starts a transaction.

The provided context is used until the transaction is committed or rolled back.
If the context is canceled, the sql package will roll back the transaction.
Tx.Commit will return an error if the context provided to BeginTx is canceled.

The provided TxOptions is optional and may be nil if defaults should be used. If
a non-default isolation level is used that the driver doesn't support, an error
will be returned.

<h3 id="DB.Close">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L709">Close</a>
    <a href="#DB.Close">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the database, releasing any open resources.

It is rare to Close a DB, as the DB handle is meant to be long-lived and shared
between many goroutines.

<h3 id="DB.Conn">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1605">Conn</a>
    <a href="#DB.Conn">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) Conn(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>) (*<a href="#Conn">Conn</a>, <a href="/builtin/#error">error</a>)</pre>

Conn returns a single connection by either opening a new connection or returning
an existing connection from the connection pool. Conn will block until either a
connection is returned or ctx is canceled. Queries run on the same Conn will be
run in the same database session.

Every Conn must be returned to the database pool after use by calling
Conn.Close.

<h3 id="DB.Driver">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1590">Driver</a>
    <a href="#DB.Driver">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) Driver() <a href="/database/sql/driver/">driver</a>.<a href="/database/sql/driver/#Driver">Driver</a></pre>

Driver returns the database's underlying driver.

<h3 id="DB.Exec">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1352">Exec</a>
    <a href="#DB.Exec">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) Exec(query <a href="/builtin/#string">string</a>, args ...interface{}) (<a href="#Result">Result</a>, <a href="/builtin/#error">error</a>)</pre>

Exec executes a query without returning any rows. The args are for any
placeholder parameters in the query.

<h3 id="DB.ExecContext">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1335">ExecContext</a>
    <a href="#DB.ExecContext">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) ExecContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>, args ...interface{}) (<a href="#Result">Result</a>, <a href="/builtin/#error">error</a>)</pre>

ExecContext executes a query without returning any rows. The args are for any
placeholder parameters in the query.

<h3 id="DB.Ping">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L701">Ping</a>
    <a href="#DB.Ping">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) Ping() <a href="/builtin/#error">error</a></pre>

Ping verifies a connection to the database is still alive, establishing a
connection if necessary.

<h3 id="DB.PingContext">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L679">PingContext</a>
    <a href="#DB.PingContext">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) PingContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>) <a href="/builtin/#error">error</a></pre>

PingContext verifies a connection to the database is still alive, establishing a
connection if necessary.

<h3 id="DB.Prepare">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1282">Prepare</a>
    <a href="#DB.Prepare">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) Prepare(query <a href="/builtin/#string">string</a>) (*<a href="#Stmt">Stmt</a>, <a href="/builtin/#error">error</a>)</pre>

Prepare creates a prepared statement for later queries or executions. Multiple
queries or executions may be run concurrently from the returned statement. The
caller must call the statement's Close method when the statement is no longer
needed.

<h3 id="DB.PrepareContext">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1262">PrepareContext</a>
    <a href="#DB.PrepareContext">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) PrepareContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>) (*<a href="#Stmt">Stmt</a>, <a href="/builtin/#error">error</a>)</pre>

PrepareContext creates a prepared statement for later queries or executions.
Multiple queries or executions may be run concurrently from the returned
statement. The caller must call the statement's Close method when the statement
is no longer needed.

The provided context is used for the preparation of the statement, not for the
execution of the statement.

<h3 id="DB.Query">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1422">Query</a>
    <a href="#DB.Query">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) Query(query <a href="/builtin/#string">string</a>, args ...interface{}) (*<a href="#Rows">Rows</a>, <a href="/builtin/#error">error</a>)</pre>

Query executes a query that returns rows, typically a SELECT. The args are for
any placeholder parameters in the query.

<a id="exampleDB_Query"></a>
Example:

    age := 27
    rows, err := db.Query("SELECT name FROM users WHERE age=?", age)
    if err != nil {
        log.Fatal(err)
    }
    defer rows.Close()
    for rows.Next() {
        var name string
        if err := rows.Scan(&name); err != nil {
            log.Fatal(err)
        }
        fmt.Printf("%s is %d\n", name, age)
    }
    if err := rows.Err(); err != nil {
        log.Fatal(err)
    }


<a id="exampleDB_Query_multipleResultSets"></a>
Example:

    age := 27
    q := `
    create temp table uid (id bigint); -- Create temp table for queries.
    insert into uid
    select id from users where age < ?; -- Populate temp table.
    
    -- First result set.
    select
    	users.id, name
    from
    	users
    	join uid on users.id = uid.id
    ;
    
    -- Second result set.
    select 
    	ur.user, ur.role
    from
    	user_roles as ur
    	join uid on uid.id = ur.user
    ;
    	`
    rows, err := db.Query(q, age)
    if err != nil {
        log.Fatal(err)
    }
    defer rows.Close()

    for rows.Next() {
        var (
            id   int64
            name string
        )
        if err := rows.Scan(&id, &name); err != nil {
            log.Fatal(err)
        }
        fmt.Printf("id %d name is %s\n", id, name)
    }
    if !rows.NextResultSet() {
        log.Fatal("expected more result sets", rows.Err())
    }
    var roleMap = map[int64]string{
        1:  "user",
        2:  "admin",
        3:  "gopher",
    }
    for rows.Next() {
        var (
            id   int64
            role int64
        )
        if err := rows.Scan(&id, &role); err != nil {
            log.Fatal(err)
        }
        fmt.Printf("id %d has role %s\n", id, roleMap[role])
    }
    if err := rows.Err(); err != nil {
        log.Fatal(err)
    }

<h3 id="DB.QueryContext">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1405">QueryContext</a>
    <a href="#DB.QueryContext">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) QueryContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>, args ...interface{}) (*<a href="#Rows">Rows</a>, <a href="/builtin/#error">error</a>)</pre>

QueryContext executes a query that returns rows, typically a SELECT. The args
are for any placeholder parameters in the query.

<h3 id="DB.QueryRow">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1520">QueryRow</a>
    <a href="#DB.QueryRow">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) QueryRow(query <a href="/builtin/#string">string</a>, args ...interface{}) *<a href="#Row">Row</a></pre>

QueryRow executes a query that is expected to return at most one row. QueryRow
always returns a non-nil value. Errors are deferred until Row's Scan method is
called. If the query selects no rows, the *Row's Scan will return ErrNoRows.
Otherwise, the *Row's Scan scans the first selected row and discards the rest.

<a id="exampleDB_QueryRow"></a>
Example:

    id := 123
    var username string
    err := db.QueryRow("SELECT username FROM users WHERE id=?", id).Scan(&username)
    switch {
    case err == sql.ErrNoRows:
        log.Printf("No user with that ID.")
    case err != nil:
        log.Fatal(err)
    default:
        fmt.Printf("Username is %s\n", username)
    }

<h3 id="DB.QueryRowContext">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1509">QueryRowContext</a>
    <a href="#DB.QueryRowContext">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) QueryRowContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>, args ...interface{}) *<a href="#Row">Row</a></pre>

QueryRowContext executes a query that is expected to return at most one row.
QueryRowContext always returns a non-nil value. Errors are deferred until Row's
Scan method is called. If the query selects no rows, the *Row's Scan will return
ErrNoRows. Otherwise, the *Row's Scan scans the first selected row and discards
the rest.

<h3 id="DB.SetConnMaxLifetime">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L812">SetConnMaxLifetime</a>
    <a href="#DB.SetConnMaxLifetime">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) SetConnMaxLifetime(d <a href="/time/">time</a>.<a href="/time/#Duration">Duration</a>)</pre>

SetConnMaxLifetime sets the maximum amount of time a connection may be reused.

Expired connections may be closed lazily before reuse.

If d <= 0, connections are reused forever.

<h3 id="DB.SetMaxIdleConns">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L761">SetMaxIdleConns</a>
    <a href="#DB.SetMaxIdleConns">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) SetMaxIdleConns(n <a href="/builtin/#int">int</a>)</pre>

SetMaxIdleConns sets the maximum number of connections in the idle connection
pool.

If MaxOpenConns is greater than 0 but less than the new MaxIdleConns then the
new MaxIdleConns will be reduced to match the MaxOpenConns limit

If n <= 0, no idle connections are retained.

<h3 id="DB.SetMaxOpenConns">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L794">SetMaxOpenConns</a>
    <a href="#DB.SetMaxOpenConns">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) SetMaxOpenConns(n <a href="/builtin/#int">int</a>)</pre>

SetMaxOpenConns sets the maximum number of open connections to the database.

If MaxIdleConns is greater than 0 and the new MaxOpenConns is less than
MaxIdleConns, then MaxIdleConns will be reduced to match the new MaxOpenConns
limit

If n <= 0, then there is no limit on the number of open connections. The default
is 0 (unlimited).

<h3 id="DB.Stats">func (*DB) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L892">Stats</a>
    <a href="#DB.Stats">¶</a></h3>
<pre>func (db *<a href="#DB">DB</a>) Stats() <a href="#DBStats">DBStats</a></pre>

Stats returns database statistics.

<h2 id="DBStats">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L886">DBStats</a>
    <a href="#DBStats">¶</a></h2>
<pre>type DBStats struct {
<span id="DBStats.OpenConnections"></span>    <span class="comment">// OpenConnections is the number of open connections to the database.</span>
    OpenConnections <a href="/builtin/#int">int</a>
}</pre>

DBStats contains database statistics.

<h2 id="IsolationLevel">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L108">IsolationLevel</a>
    <a href="#IsolationLevel">¶</a></h2>
<pre>type IsolationLevel <a href="/builtin/#int">int</a></pre>

IsolationLevel is the transaction isolation level used in TxOptions.

<pre>const (
    <span id="LevelDefault">LevelDefault</span> <a href="#IsolationLevel">IsolationLevel</a> = <a href="/builtin/#iota">iota</a>
    <span id="LevelReadUncommitted">LevelReadUncommitted</span>
    <span id="LevelReadCommitted">LevelReadCommitted</span>
    <span id="LevelWriteCommitted">LevelWriteCommitted</span>
    <span id="LevelRepeatableRead">LevelRepeatableRead</span>
    <span id="LevelSnapshot">LevelSnapshot</span>
    <span id="LevelSerializable">LevelSerializable</span>
    <span id="LevelLinearizable">LevelLinearizable</span>
)</pre>

Various isolation levels that drivers may support in BeginTx. If a driver does
not support a given isolation level an error may be returned.

See https://en.wikipedia.org/wiki/Isolation_(database_systems)#Isolation_levels.

<h2 id="NamedArg">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L70">NamedArg</a>
    <a href="#NamedArg">¶</a></h2>
<pre>type NamedArg struct {

<span id="NamedArg.Name"></span>    <span class="comment">// Name is the name of the parameter placeholder.</span>
    <span class="comment">//</span>
    <span class="comment">// If empty, the ordinal position in the argument list will be</span>
    <span class="comment">// used.</span>
    <span class="comment">//</span>
    <span class="comment">// Name must omit any symbol prefix.</span>
    Name <a href="/builtin/#string">string</a>

<span id="NamedArg.Value"></span>    <span class="comment">// Value is the value of the parameter.</span>
    <span class="comment">// It may be assigned the same value types as the query</span>
    <span class="comment">// arguments.</span>
    Value interface{}
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

A NamedArg is a named argument. NamedArg values may be used as arguments to
Query or Exec and bind to the corresponding named parameter in the SQL
statement.

For a more concise way to create NamedArg values, see the Named function.

<h3 id="Named">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L99">Named</a>
    <a href="#Named">¶</a></h3>
<pre>func Named(name <a href="/builtin/#string">string</a>, value interface{}) <a href="#NamedArg">NamedArg</a></pre>

Named provides a more concise way to create NamedArg values.

Example usage:

    db.ExecContext(ctx, `
        delete from Invoice
        where
            TimeCreated < @end
            and TimeCreated >= @start;`,
        sql.Named("start", startTime),
        sql.Named("end", endTime),
    )

<h2 id="NullBool">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L229">NullBool</a>
    <a href="#NullBool">¶</a></h2>
<pre>type NullBool struct {
<span id="NullBool.Bool"></span>    Bool  <a href="/builtin/#bool">bool</a>
<span id="NullBool.Valid"></span>    Valid <a href="/builtin/#bool">bool</a> <span class="comment">// Valid is true if Bool is not NULL</span>
}</pre>

NullBool represents a bool that may be null. NullBool implements the Scanner
interface so it can be used as a scan destination, similar to NullString.

<h3 id="NullBool.Scan">func (*NullBool) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L235">Scan</a>
    <a href="#NullBool.Scan">¶</a></h3>
<pre>func (n *<a href="#NullBool">NullBool</a>) Scan(value interface{}) <a href="/builtin/#error">error</a></pre>

Scan implements the Scanner interface.

<h3 id="NullBool.Value">func (NullBool) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L245">Value</a>
    <a href="#NullBool.Value">¶</a></h3>
<pre>func (n <a href="#NullBool">NullBool</a>) Value() (<a href="/database/sql/driver/">driver</a>.<a href="/database/sql/driver/#Value">Value</a>, <a href="/builtin/#error">error</a>)</pre>

Value implements the driver Valuer interface.

<h2 id="NullFloat64">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L203">NullFloat64</a>
    <a href="#NullFloat64">¶</a></h2>
<pre>type NullFloat64 struct {
<span id="NullFloat64.Float64"></span>    Float64 <a href="/builtin/#float64">float64</a>
<span id="NullFloat64.Valid"></span>    Valid   <a href="/builtin/#bool">bool</a> <span class="comment">// Valid is true if Float64 is not NULL</span>
}</pre>

NullFloat64 represents a float64 that may be null. NullFloat64 implements the
Scanner interface so it can be used as a scan destination, similar to
NullString.

<h3 id="NullFloat64.Scan">func (*NullFloat64) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L209">Scan</a>
    <a href="#NullFloat64.Scan">¶</a></h3>
<pre>func (n *<a href="#NullFloat64">NullFloat64</a>) Scan(value interface{}) <a href="/builtin/#error">error</a></pre>

Scan implements the Scanner interface.

<h3 id="NullFloat64.Value">func (NullFloat64) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L219">Value</a>
    <a href="#NullFloat64.Value">¶</a></h3>
<pre>func (n <a href="#NullFloat64">NullFloat64</a>) Value() (<a href="/database/sql/driver/">driver</a>.<a href="/database/sql/driver/#Value">Value</a>, <a href="/builtin/#error">error</a>)</pre>

Value implements the driver Valuer interface.

<h2 id="NullInt64">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L177">NullInt64</a>
    <a href="#NullInt64">¶</a></h2>
<pre>type NullInt64 struct {
<span id="NullInt64.Int64"></span>    Int64 <a href="/builtin/#int64">int64</a>
<span id="NullInt64.Valid"></span>    Valid <a href="/builtin/#bool">bool</a> <span class="comment">// Valid is true if Int64 is not NULL</span>
}</pre>

NullInt64 represents an int64 that may be null. NullInt64 implements the Scanner
interface so it can be used as a scan destination, similar to NullString.

<h3 id="NullInt64.Scan">func (*NullInt64) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L183">Scan</a>
    <a href="#NullInt64.Scan">¶</a></h3>
<pre>func (n *<a href="#NullInt64">NullInt64</a>) Scan(value interface{}) <a href="/builtin/#error">error</a></pre>

Scan implements the Scanner interface.

<h3 id="NullInt64.Value">func (NullInt64) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L193">Value</a>
    <a href="#NullInt64.Value">¶</a></h3>
<pre>func (n <a href="#NullInt64">NullInt64</a>) Value() (<a href="/database/sql/driver/">driver</a>.<a href="/database/sql/driver/#Value">Value</a>, <a href="/builtin/#error">error</a>)</pre>

Value implements the driver Valuer interface.

<h2 id="NullString">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L151">NullString</a>
    <a href="#NullString">¶</a></h2>
<pre>type NullString struct {
<span id="NullString.String"></span>    String <a href="/builtin/#string">string</a>
<span id="NullString.Valid"></span>    Valid  <a href="/builtin/#bool">bool</a> <span class="comment">// Valid is true if String is not NULL</span>
}</pre>

NullString represents a string that may be null. NullString implements the
Scanner interface so it can be used as a scan destination:

    var s NullString
    err := db.QueryRow("SELECT name FROM foo WHERE id=?", id).Scan(&s)
    ...
    if s.Valid {
       // use s.String
    } else {
       // NULL value
    }

<h3 id="NullString.Scan">func (*NullString) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L157">Scan</a>
    <a href="#NullString.Scan">¶</a></h3>
<pre>func (ns *<a href="#NullString">NullString</a>) Scan(value interface{}) <a href="/builtin/#error">error</a></pre>

Scan implements the Scanner interface.

<h3 id="NullString.Value">func (NullString) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L167">Value</a>
    <a href="#NullString.Value">¶</a></h3>
<pre>func (ns <a href="#NullString">NullString</a>) Value() (<a href="/database/sql/driver/">driver</a>.<a href="/database/sql/driver/#Value">Value</a>, <a href="/builtin/#error">error</a>)</pre>

Value implements the driver Valuer interface.

<h2 id="Out">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L279">Out</a>
    <a href="#Out">¶</a></h2>
<pre>type Out struct {

<span id="Out.Dest"></span>    <span class="comment">// Dest is a pointer to the value that will be set to the result of the</span>
    <span class="comment">// stored procedure&#39;s OUTPUT parameter.</span>
    Dest interface{}

<span id="Out.In"></span>    <span class="comment">// In is whether the parameter is an INOUT parameter. If so, the input value to the stored</span>
    <span class="comment">// procedure is the dereferenced value of Dest&#39;s pointer, which is then replaced with</span>
    <span class="comment">// the output value.</span>
    In <a href="/builtin/#bool">bool</a>
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Out may be used to retrieve OUTPUT value parameters from stored procedures.

Not all drivers and databases support OUTPUT value parameters.

Example usage:

    var outArg string
    _, err := db.ExecContext(ctx, "ProcName", sql.Named("Arg1", sql.Out{Dest: &outArg}))

<h2 id="RawBytes">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L136">RawBytes</a>
    <a href="#RawBytes">¶</a></h2>
<pre>type RawBytes []<a href="/builtin/#byte">byte</a></pre>

RawBytes is a byte slice that holds a reference to memory owned by the database
itself. After a Scan into a RawBytes, the slice is only valid until the next
call to Next, Scan, or Close.

<h2 id="Result">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2982">Result</a>
    <a href="#Result">¶</a></h2>
<pre>type Result interface {
    <span class="comment">// LastInsertId returns the integer generated by the database</span>
    <span class="comment">// in response to a command. Typically this will be from an</span>
    <span class="comment">// &#34;auto increment&#34; column when inserting a new row. Not all</span>
    <span class="comment">// databases support this feature, and the syntax of such</span>
    <span class="comment">// statements varies.</span>
    LastInsertId() (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)

    <span class="comment">// RowsAffected returns the number of rows affected by an</span>
    <span class="comment">// update, insert, or delete. Not every database or database</span>
    <span class="comment">// driver may support this.</span>
    RowsAffected() (<a href="/builtin/#int64">int64</a>, <a href="/builtin/#error">error</a>)
}</pre>

A Result summarizes an executed SQL command.

<h2 id="Row">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2927">Row</a>
    <a href="#Row">¶</a></h2>
<pre>type Row struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Row is the result of calling QueryRow to select a single row.

<h3 id="Row.Scan">func (*Row) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2938">Scan</a>
    <a href="#Row.Scan">¶</a></h3>
<pre>func (r *<a href="#Row">Row</a>) Scan(dest ...interface{}) <a href="/builtin/#error">error</a></pre>

Scan copies the columns from the matched row into the values pointed at by dest.
See the documentation on Rows.Scan for details. If more than one row matches the
query, Scan uses the first row and discards the rest. If no row matches the
query, Scan returns ErrNoRows.

<h2 id="Rows">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2539">Rows</a>
    <a href="#Rows">¶</a></h2>
<pre>type Rows struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Rows is the result of a query. Its cursor starts before the first row of the
result set. Use Next to advance through the rows:

    rows, err := db.Query("SELECT ...")
    ...
    defer rows.Close()
    for rows.Next() {
        var id int
        var name string
        err = rows.Scan(&id, &name)
        ...
    }
    err = rows.Err() // get any error encountered during iteration
    ...

<h3 id="Rows.Close">func (*Rows) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2892">Close</a>
    <a href="#Rows.Close">¶</a></h3>
<pre>func (rs *<a href="#Rows">Rows</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the Rows, preventing further enumeration. If Next is called and
returns false and there are no further result sets, the Rows are closed
automatically and it will suffice to check the result of Err. Close is
idempotent and does not affect the result of Err.

<h3 id="Rows.ColumnTypes">func (*Rows) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2706">ColumnTypes</a>
    <a href="#Rows.ColumnTypes">¶</a></h3>
<pre>func (rs *<a href="#Rows">Rows</a>) ColumnTypes() ([]*<a href="#ColumnType">ColumnType</a>, <a href="/builtin/#error">error</a>)</pre>

ColumnTypes returns column information such as column type, length, and
nullable. Some information may not be available from some drivers.

<h3 id="Rows.Columns">func (*Rows) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2689">Columns</a>
    <a href="#Rows.Columns">¶</a></h3>
<pre>func (rs *<a href="#Rows">Rows</a>) Columns() ([]<a href="/builtin/#string">string</a>, <a href="/builtin/#error">error</a>)</pre>

Columns returns the column names. Columns returns an error if the rows are
closed, or if the rows are from QueryRow and there was a deferred error.

<h3 id="Rows.Err">func (*Rows) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2677">Err</a>
    <a href="#Rows.Err">¶</a></h3>
<pre>func (rs *<a href="#Rows">Rows</a>) Err() <a href="/builtin/#error">error</a></pre>

Err returns the error, if any, that was encountered during iteration. Err may be
called after an explicit or implicit Close.

<h3 id="Rows.Next">func (*Rows) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2587">Next</a>
    <a href="#Rows.Next">¶</a></h3>
<pre>func (rs *<a href="#Rows">Rows</a>) Next() <a href="/builtin/#bool">bool</a></pre>

Next prepares the next result row for reading with the Scan method. It returns
true on success, or false if there is no next result row or an error happened
while preparing it. Err should be consulted to distinguish between the two
cases.

Every call to Scan, even the first one, must be preceded by a call to Next.

<h3 id="Rows.NextResultSet">func (*Rows) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2641">NextResultSet</a>
    <a href="#Rows.NextResultSet">¶</a></h3>
<pre>func (rs *<a href="#Rows">Rows</a>) NextResultSet() <a href="/builtin/#bool">bool</a></pre>

NextResultSet prepares the next result set for reading. It returns true if there
is further result sets, or false if there is no further result set or if there
is an error advancing to it. The Err method should be consulted to distinguish
between the two cases.

After calling NextResultSet, the Next method should always be called before
scanning. If there are further result sets they may not have rows in the result
set.

<h3 id="Rows.Scan">func (*Rows) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2861">Scan</a>
    <a href="#Rows.Scan">¶</a></h3>
<pre>func (rs *<a href="#Rows">Rows</a>) Scan(dest ...interface{}) <a href="/builtin/#error">error</a></pre>

Scan copies the columns in the current row into the values pointed at by dest.
The number of values in dest must be the same as the number of columns in Rows.

Scan converts columns read from the database into the following common Go types
and special types provided by the sql package:

    *string
    *[]byte
    *int, *int8, *int16, *int32, *int64
    *uint, *uint8, *uint16, *uint32, *uint64
    *bool
    *float32, *float64
    *interface{}
    *RawBytes
    any type implementing Scanner (see Scanner docs)

In the most simple case, if the type of the value from the source column is an
integer, bool or string type T and dest is of type *T, Scan simply assigns the
value through the pointer.

Scan also converts between string and numeric types, as long as no information
would be lost. While Scan stringifies all numbers scanned from numeric database
columns into *string, scans into numeric types are checked for overflow. For
example, a float64 with value 300 or a string with value "300" can scan into a
uint16, but not into a uint8, though float64(255) or "255" can scan into a
uint8. One exception is that scans of some float64 numbers to strings may lose
information when stringifying. In general, scan floating point columns into
*float64.

If a dest argument has type *[]byte, Scan saves in that argument a copy of the
corresponding data. The copy is owned by the caller and can be modified and held
indefinitely. The copy can be avoided by using an argument of type *RawBytes
instead; see the documentation for RawBytes for restrictions on its use.

If an argument has type *interface{}, Scan copies the value provided by the
underlying driver without conversion. When scanning from a source value of type
[]byte to *interface{}, a copy of the slice is made and the caller owns the
result.

Source values of type time.Time may be scanned into values of type *time.Time,
*interface{}, *string, or *[]byte. When converting to the latter two,
time.Format3339Nano is used.

Source values of type bool may be scanned into types *bool, *interface{},
*string, *[]byte, or *RawBytes.

For scanning into *bool, the source may be true, false, 1, 0, or string inputs
parseable by strconv.ParseBool.

<h2 id="Scanner">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L253">Scanner</a>
    <a href="#Scanner">¶</a></h2>
<pre>type Scanner interface {
    <span class="comment">// Scan assigns a value from a database driver.</span>
    <span class="comment">//</span>
    <span class="comment">// The src value will be of one of the following types:</span>
    <span class="comment">//</span>
    <span class="comment">//    int64</span>
    <span class="comment">//    float64</span>
    <span class="comment">//    bool</span>
    <span class="comment">//    []byte</span>
    <span class="comment">//    string</span>
    <span class="comment">//    time.Time</span>
    <span class="comment">//    nil - for NULL values</span>
    <span class="comment">//</span>
    <span class="comment">// An error should be returned if the value cannot be stored</span>
    <span class="comment">// without loss of information.</span>
    Scan(src interface{}) <a href="/builtin/#error">error</a>
}</pre>

Scanner is an interface used by Scan.

<h2 id="Stmt">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2173">Stmt</a>
    <a href="#Stmt">¶</a></h2>
<pre>type Stmt struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Stmt is a prepared statement. A Stmt is safe for concurrent use by multiple
goroutines.

<h3 id="Stmt.Close">func (*Stmt) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2482">Close</a>
    <a href="#Stmt.Close">¶</a></h3>
<pre>func (s *<a href="#Stmt">Stmt</a>) Close() <a href="/builtin/#error">error</a></pre>

Close closes the statement.

<h3 id="Stmt.Exec">func (*Stmt) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2242">Exec</a>
    <a href="#Stmt.Exec">¶</a></h3>
<pre>func (s *<a href="#Stmt">Stmt</a>) Exec(args ...interface{}) (<a href="#Result">Result</a>, <a href="/builtin/#error">error</a>)</pre>

Exec executes a prepared statement with the given arguments and returns a Result
summarizing the effect of the statement.

<h3 id="Stmt.ExecContext">func (*Stmt) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2213">ExecContext</a>
    <a href="#Stmt.ExecContext">¶</a></h3>
<pre>func (s *<a href="#Stmt">Stmt</a>) ExecContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, args ...interface{}) (<a href="#Result">Result</a>, <a href="/builtin/#error">error</a>)</pre>

ExecContext executes a prepared statement with the given arguments and returns a
Result summarizing the effect of the statement.

<h3 id="Stmt.Query">func (*Stmt) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2420">Query</a>
    <a href="#Stmt.Query">¶</a></h3>
<pre>func (s *<a href="#Stmt">Stmt</a>) Query(args ...interface{}) (*<a href="#Rows">Rows</a>, <a href="/builtin/#error">error</a>)</pre>

Query executes a prepared query statement with the given arguments and returns
the query results as a *Rows.

<h3 id="Stmt.QueryContext">func (*Stmt) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2365">QueryContext</a>
    <a href="#Stmt.QueryContext">¶</a></h3>
<pre>func (s *<a href="#Stmt">Stmt</a>) QueryContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, args ...interface{}) (*<a href="#Rows">Rows</a>, <a href="/builtin/#error">error</a>)</pre>

QueryContext executes a prepared query statement with the given arguments and
returns the query results as a *Rows.

<h3 id="Stmt.QueryRow">func (*Stmt) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2477">QueryRow</a>
    <a href="#Stmt.QueryRow">¶</a></h3>
<pre>func (s *<a href="#Stmt">Stmt</a>) QueryRow(args ...interface{}) *<a href="#Row">Row</a></pre>

QueryRow executes a prepared query statement with the given arguments. If an
error occurs during the execution of the statement, that error will be returned
by a call to Scan on the returned *Row, which is always non-nil. If the query
selects no rows, the *Row's Scan will return ErrNoRows. Otherwise, the *Row's
Scan scans the first selected row and discards the rest.

Example usage:

    var name string
    err := nameByUseridStmt.QueryRow(id).Scan(&name)

<h3 id="Stmt.QueryRowContext">func (*Stmt) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2458">QueryRowContext</a>
    <a href="#Stmt.QueryRowContext">¶</a></h3>
<pre>func (s *<a href="#Stmt">Stmt</a>) QueryRowContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, args ...interface{}) *<a href="#Row">Row</a></pre>

QueryRowContext executes a prepared query statement with the given arguments. If
an error occurs during the execution of the statement, that error will be
returned by a call to Scan on the returned *Row, which is always non-nil. If the
query selects no rows, the *Row's Scan will return ErrNoRows. Otherwise, the
*Row's Scan scans the first selected row and discards the rest.

Example usage:

    var name string
    err := nameByUseridStmt.QueryRowContext(ctx, id).Scan(&name)

<h2 id="Tx">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1787">Tx</a>
    <a href="#Tx">¶</a></h2>
<pre>type Tx struct {
    <span class="comment">// contains filtered or unexported fields</span>
}</pre>

Tx is an in-progress database transaction.

A transaction must end with a call to Commit or Rollback.

After a call to Commit or Rollback, all operations on the transaction fail with
ErrTxDone.

The statements prepared for a transaction by calling the transaction's Prepare
or Stmt methods are closed by the call to Commit or Rollback.

<h3 id="Tx.Commit">func (*Tx) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1905">Commit</a>
    <a href="#Tx.Commit">¶</a></h3>
<pre>func (tx *<a href="#Tx">Tx</a>) Commit() <a href="/builtin/#error">error</a></pre>

Commit commits the transaction.

<h3 id="Tx.Exec">func (*Tx) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2107">Exec</a>
    <a href="#Tx.Exec">¶</a></h3>
<pre>func (tx *<a href="#Tx">Tx</a>) Exec(query <a href="/builtin/#string">string</a>, args ...interface{}) (<a href="#Result">Result</a>, <a href="/builtin/#error">error</a>)</pre>

Exec executes a query that doesn't return rows. For example: an INSERT and
UPDATE.

<h3 id="Tx.ExecContext">func (*Tx) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2097">ExecContext</a>
    <a href="#Tx.ExecContext">¶</a></h3>
<pre>func (tx *<a href="#Tx">Tx</a>) ExecContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>, args ...interface{}) (<a href="#Result">Result</a>, <a href="/builtin/#error">error</a>)</pre>

ExecContext executes a query that doesn't return rows. For example: an INSERT
and UPDATE.

<h3 id="Tx.Prepare">func (*Tx) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1988">Prepare</a>
    <a href="#Tx.Prepare">¶</a></h3>
<pre>func (tx *<a href="#Tx">Tx</a>) Prepare(query <a href="/builtin/#string">string</a>) (*<a href="#Stmt">Stmt</a>, <a href="/builtin/#error">error</a>)</pre>

Prepare creates a prepared statement for use within a transaction.

The returned statement operates within the transaction and can no longer be used
once the transaction has been committed or rolled back.

To use an existing prepared statement on this transaction, see Tx.Stmt.

<h3 id="Tx.PrepareContext">func (*Tx) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1966">PrepareContext</a>
    <a href="#Tx.PrepareContext">¶</a></h3>
<pre>func (tx *<a href="#Tx">Tx</a>) PrepareContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>) (*<a href="#Stmt">Stmt</a>, <a href="/builtin/#error">error</a>)</pre>

PrepareContext creates a prepared statement for use within a transaction.

The returned statement operates within the transaction and will be closed when
the transaction has been committed or rolled back.

To use an existing prepared statement on this transaction, see Tx.Stmt.

The provided context will be used for the preparation of the context, not for
the execution of the returned statement. The returned statement will run in the
transaction context.

<h3 id="Tx.Query">func (*Tx) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2122">Query</a>
    <a href="#Tx.Query">¶</a></h3>
<pre>func (tx *<a href="#Tx">Tx</a>) Query(query <a href="/builtin/#string">string</a>, args ...interface{}) (*<a href="#Rows">Rows</a>, <a href="/builtin/#error">error</a>)</pre>

Query executes a query that returns rows, typically a SELECT.

<h3 id="Tx.QueryContext">func (*Tx) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2112">QueryContext</a>
    <a href="#Tx.QueryContext">¶</a></h3>
<pre>func (tx *<a href="#Tx">Tx</a>) QueryContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>, args ...interface{}) (*<a href="#Rows">Rows</a>, <a href="/builtin/#error">error</a>)</pre>

QueryContext executes a query that returns rows, typically a SELECT.

<h3 id="Tx.QueryRow">func (*Tx) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2143">QueryRow</a>
    <a href="#Tx.QueryRow">¶</a></h3>
<pre>func (tx *<a href="#Tx">Tx</a>) QueryRow(query <a href="/builtin/#string">string</a>, args ...interface{}) *<a href="#Row">Row</a></pre>

QueryRow executes a query that is expected to return at most one row. QueryRow
always returns a non-nil value. Errors are deferred until Row's Scan method is
called. If the query selects no rows, the *Row's Scan will return ErrNoRows.
Otherwise, the *Row's Scan scans the first selected row and discards the rest.

<h3 id="Tx.QueryRowContext">func (*Tx) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2132">QueryRowContext</a>
    <a href="#Tx.QueryRowContext">¶</a></h3>
<pre>func (tx *<a href="#Tx">Tx</a>) QueryRowContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, query <a href="/builtin/#string">string</a>, args ...interface{}) *<a href="#Row">Row</a></pre>

QueryRowContext executes a query that is expected to return at most one row.
QueryRowContext always returns a non-nil value. Errors are deferred until Row's
Scan method is called. If the query selects no rows, the *Row's Scan will return
ErrNoRows. Otherwise, the *Row's Scan scans the first selected row and discards
the rest.

<h3 id="Tx.Rollback">func (*Tx) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L1952">Rollback</a>
    <a href="#Tx.Rollback">¶</a></h3>
<pre>func (tx *<a href="#Tx">Tx</a>) Rollback() <a href="/builtin/#error">error</a></pre>

Rollback aborts the transaction.

<h3 id="Tx.Stmt">func (*Tx) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2091">Stmt</a>
    <a href="#Tx.Stmt">¶</a></h3>
<pre>func (tx *<a href="#Tx">Tx</a>) Stmt(stmt *<a href="#Stmt">Stmt</a>) *<a href="#Stmt">Stmt</a></pre>

Stmt returns a transaction-specific prepared statement from an existing
statement.

Example:

    updateMoney, err := db.Prepare("UPDATE balance SET money=money+? WHERE id=?")
    ...
    tx, err := db.Begin()
    ...
    res, err := tx.Stmt(updateMoney).Exec(123.45, 98293203)

The returned statement operates within the transaction and will be closed when
the transaction has been committed or rolled back.

<h3 id="Tx.StmtContext">func (*Tx) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L2007">StmtContext</a>
    <a href="#Tx.StmtContext">¶</a></h3>
<pre>func (tx *<a href="#Tx">Tx</a>) StmtContext(ctx <a href="/context/">context</a>.<a href="/context/#Context">Context</a>, stmt *<a href="#Stmt">Stmt</a>) *<a href="#Stmt">Stmt</a></pre>

StmtContext returns a transaction-specific prepared statement from an existing
statement.

Example:

    updateMoney, err := db.Prepare("UPDATE balance SET money=money+? WHERE id=?")
    ...
    tx, err := db.Begin()
    ...
    res, err := tx.StmtContext(ctx, updateMoney).Exec(123.45, 98293203)

The provided context is used for the preparation of the statement, not for the
execution of the statement.

The returned statement operates within the transaction and will be closed when
the transaction has been committed or rolled back.

<h2 id="TxOptions">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/database/sql/sql.go#L126">TxOptions</a>
    <a href="#TxOptions">¶</a></h2>
<pre>type TxOptions struct {
<span id="TxOptions.Isolation"></span>    <span class="comment">// Isolation is the transaction isolation level.</span>
    <span class="comment">// If zero, the driver or database&#39;s default level is used.</span>
    Isolation <a href="#IsolationLevel">IsolationLevel</a>
<span id="TxOptions.ReadOnly"></span>    ReadOnly  <a href="/builtin/#bool">bool</a>
}</pre>

TxOptions holds the transaction options to be used in DB.BeginTx.

## Subdirectories
- [..](..)
- [driver](driver/)
