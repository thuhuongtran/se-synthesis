## Postgresql
### What is Postgresql
PostgreSQL is an object-relational database management system (ORDBMS)

It supports a large part of the SQL standard and offers many modern features:
* complex queries
* foreign keys
* triggers
* updatable views
* transactional integrity
* multiversion concurrency control

### Data types
Besides the `numeric, floating-point, string, boolean` and date types you'd expect (and many options within these), PostgreSQL boasts `uuid, monetary, enumerated, geometric, binary, network address, bit string, text search, xml, json, array, composite and range types`, as well as some internal types for object identification and log `location`
### Data size
PostgreSQL can handle a lot of data. The current posted size limits are listed below:

| Limit                     | Value                                |
|---------------------------|--------------------------------------|
| Maximum Database Size     | Unlimited                            |
| Maximum Table Size        | 32 TB                                |
| Maximum Row Size          | 1.6 TB                               |
| Maximum Field Size        | 1 GB                                 |
| Maximum Rows per Table    | Unlimited                            |
| Maximum Columns per Table | 250 - 1600 depending on column types |
| Maximum Indexes per Table | Unlimited                            |

### When to (not) use Postgresql
In general, PostgreSQL is best suited for systems that require execution of complex queries, or data warehousing and data analysis. 

When should we avoid using Postgresql: Speed is imperative, Simple setups: Because of its large feature set and strong adherence to standard SQL, Postgres can be overkill for simple database setups, Complex replication: Although PostgreSQL does provide strong support for replication, it’s still a relatively new feature and some configurations, Replication is a more mature feature on MySQL and many users see MySQL’s replication to be easier to implement.
### Data definition
#### System columns
Every table has several system columns that are implicitly defined by the system.

`tableoid`:  This column is particularly handy for queries that select from partitioned tables or inheritance hierarchies, since without it, it's difficult to tell which individual table a row came from.

`xmin`: The identity (transaction ID) of the inserting transaction for this row version.

`cmin`: The command identifier (starting at zero) within the inserting transaction.

`xmax`: The identity (transaction ID) of the deleting transaction, or zero for an undeleted row version.

`cmax`: The command identifier within the deleting transaction, or zero.

`ctid`: The physical location of the row version within its table. Note that although the ctid can be used to locate the row version very quickly.

### Full Text Search
`Like` uses wildcards only, and isn't all that powerful. `Full text` allows much more complex searching, including And, Or, Not, even similar sounding results (SOUNDEX) and many more items.

In PostgreSQL, you use two functions to perform Full Text Search. They are `to_tsvector()` and `to_tsquery()`. `to_tsvector()` function breaks up the input string and creates tokens out of it, which is then used to perform Full Text Search using the `to_tsquery()` function.

```roomsql
SELECT fieldNames FROM tableName
     WHERE to_tsvector(fieldName) @@ to_tsquery(conditions)
```
```roomsql
SELECT id, title FROM products WHERE to_tsvector(description)
    @@ to_tsquery('laptop & desktop');
```
### Sequence in Postgresql
The `sequence` in PostgreSQL is a special kind of object which is used to generate numeric identifiers. This is typically used to generate an artificial primary key in PostgreSQL. 

A `sequence` is a set of integers `1, 2, 3, ...` that are generated in order on demand. Sequences are frequently used in databases because many applications require each row in a table to contain a unique value and sequences provide an easy way to generate them.

### Indexes
PostgreSQL provides several index types: `B-tree, Hash, GiST, SP-GiST and GIN`. Each Index type uses a different algorithm that is best suited to different types of queries. By default, the `CREATE INDEX` command creates `B-tree` indexes, which fit the most common situations.

Create and drop an index in Postgresql is as same as other Sql database.