### General  HBase shell commands
#### status	
```
hbase> status
hbase> status ‘simple’
hbase> status ‘summary’
hbase> status ‘detailed’
```

#### version

```
hbase> version
```

#### whoami

```
hbase> whoami
```

### Tables Management commands
#### alter

To change or add the ‘f1’ column family in table ‘t1’ from current value to keep a maximum of 5 cell VERSIONS, do:

```
hbase> alter ‘t1’, NAME => ‘f1’, VERSIONS => 5
hbase> alter ‘t1’, ‘f1’, {NAME => ‘f2’, IN_MEMORY => true}, {NAME => ‘f3’, VERSIONS => 5}
```
To delete the ‘f1’ column family in table ‘t1’, use one of:hbase> alter ‘t1’, NAME => ‘f1’, METHOD => ‘delete’
```
hbase> alter ‘t1’, ‘delete’ => ‘f1’
```
change table-scope attributes like MAX_FILESIZE, READONLY,MEMSTORE_FLUSHSIZE, DEFERRED_LOG_FLUSH, etc. These can be put at the end;
```
hbase> alter ‘t1’, MAX_FILESIZE => ‘134217728’
```
add a table coprocessor by setting a table coprocessor attribute:
```
hbase> alter ‘t1’,
‘coprocessor’=>’hdfs:///foo.jar|com.foo.FooRegionObserver|1001|arg1=1,ar

/*Since you can have multiple coprocessors configured for a table, a
sequence number will be automatically appended to the attribute name
to uniquely identify it.

The coprocessor attribute must match the pattern below in order for the framework to understand how to load the coprocessor classes:

[coprocessor jar file location] | class name | [priority] | [arguments]*/
```
set configuration settings specific to this table or column family:
```
hbase> alter ‘t1’, CONFIGURATION => {‘hbase.hregion.scan.loadColumnFamiliesOnDemand’ => ‘true’}
hbase> alter ‘t1’, {NAME => ‘f2’, CONFIGURATION => {‘hbase.hstore.blockingStoreFiles’ => ’10’}}
```
remove a table-scope attribute:
```
hbase> alter ‘t1’, METHOD => ‘table_att_unset’, NAME => ‘MAX_FILESIZE’

hbase> alter ‘t1’, METHOD => ‘table_att_unset’, NAME => ‘coprocessor$1’
```
#### create
Create table; pass table name, a dictionary of specifications per column family, and optionally a dictionary of table configuration.
```
hbase> create ‘t1’, {NAME => ‘f1’, VERSIONS => 5}
hbase> create ‘t1’, {NAME => ‘f1’}, {NAME => ‘f2’}, {NAME => ‘f3’}
hbase> # The above in shorthand would be the following:
hbase> create ‘t1’, ‘f1’, ‘f2’, ‘f3’
hbase> create ‘t1’, {NAME => ‘f1’, VERSIONS => 1, TTL => 2592000, BLOCKCACHE => true}
hbase> create ‘t1’, {NAME => ‘f1’, CONFIGURATION => {‘hbase.hstore.blockingStoreFiles’ => ’10’}}
```

```
hbase> describe ‘t1’
hbase> disable ‘t1’
hbase> disable_all ‘t.*’
hbase> is_disabled ‘t1’
hbase> drop ‘t1
hbase> drop_all ‘t.*’
hbase> enable ‘t1’
hbase> enable_all ‘t.*’
hbase> is_enabled ‘t1’
hbase> exists ‘t1’
hbase> list
hbase> list ‘abc.*’
hbase> show_filters
hbase> alter_status ‘t1’
```

#### alter_async
To change or add the ‘f1’ column family in table ‘t1’ from defaults
to instead keep a maximum of 5 cell VERSIONS, do:hbase> alter_async ‘t1’, NAME => ‘f1’, VERSIONS => 5To delete the ‘f1’ column family in table ‘t1’, do:
```
hbase> alter_async ‘t1’, NAME => ‘f1’, METHOD => ‘delete’or a shorter version:hbase> alter_async ‘t1’, ‘delete’ => ‘f1’
```

### Data Manipulation commands  
#### Count
Count the number of rows in a table. Return value is the number of rows.
This operation may take a LONG time (Run ‘$HADOOP_HOME/bin/hadoop jar
hbase.jar rowcount’ to run a counting mapreduce job). Current count is shown
every 1000 rows by default. Count interval may be optionally specified. Scan
caching is enabled on count scans by default. Default cache size is 10 rows.
If your rows are small in size, you may want to increase this
parameter. Examples:hbase> count ‘t1’

```
hbase> count ‘t1’, INTERVAL => 100000
hbase> count ‘t1’, CACHE => 1000
hbase> count ‘t1’, INTERVAL => 10, CACHE => 1000
hbase> t.count INTERVAL => 100000
hbase> t.count CACHE => 1000
hbase> t.count INTERVAL => 10, CACHE => 1000
```

#### delete
To delete a cell from ‘t1’ at row ‘r1’ under column ‘c1’
```
hbase> delete ‘t1’, ‘r1’, ‘c1’, ts1
```

#### deleteall
Delete all cells in a given row; pass a table name, row, and optionally
a column and timestamp. Examples:hbase> deleteall ‘t1’, ‘r1’
```
hbase> deleteall ‘t1’, ‘r1’, ‘c1’
hbase> deleteall ‘t1’, ‘r1’, ‘c1’, ts1
```

#### get
Get row or cell contents; pass table name, row, and optionally
a dictionary of column(s), timestamp, timerange and versions. Examples:
```
hbase> get ‘t1’, ‘r1’
hbase> get ‘t1’, ‘r1’, {TIMERANGE => [ts1, ts2]}
hbase> get ‘t1’, ‘r1’, {COLUMN => ‘c1’}
hbase> get ‘t1’, ‘r1’, {COLUMN => [‘c1’, ‘c2’, ‘c3’]}
hbase> get ‘t1’, ‘r1’, {COLUMN => ‘c1’, TIMESTAMP => ts1}
hbase> get ‘t1’, ‘r1’, {COLUMN => ‘c1’, TIMERANGE => [ts1, ts2], VERSIONS => 4}
hbase> get ‘t1’, ‘r1’, {COLUMN => ‘c1’, TIMESTAMP => ts1, VERSIONS => 4}
hbase> get ‘t1’, ‘r1’, {FILTER => “ValueFilter(=, ‘binary:abc’)”}
hbase> get ‘t1’, ‘r1’, ‘c1’
hbase> get ‘t1’, ‘r1’, ‘c1’, ‘c2’
hbase> get ‘t1’, ‘r1’, [‘c1’, ‘c2’]
```
#### get_counter
Return a counter cell value at specified table/row/column coordinates.
A cell cell should be managed with atomic increment function oh HBase
and the data should be binary encoded. Example:
```
hbase> get_counter ‘t1’, ‘r1’, ‘c1’
```

#### incr
Increments a cell ‘value’ at specified table/row/column coordinates.
To increment a cell value in table ‘t1’ at row ‘r1’ under column
‘c1’ by 1 (can be omitted) or 10 do:
```
hbase> incr ‘t1’, ‘r1’, ‘c1’
hbase> incr ‘t1’, ‘r1’, ‘c1’, 1
hbase> incr ‘t1’, ‘r1’, ‘c1’, 10
```

#### put
```
hbase> put ‘t1’, ‘r1’, ‘c1’, ‘value’, ts1
```

#### scan
scanScan a table; pass table name and optionally a dictionary of scanner
specifications. Scanner specifications may include one or more of:
TIMERANGE, FILTER, LIMIT, STARTROW, STOPROW, TIMESTAMP, MAXLENGTH,
or COLUMNS, CACHEIf no columns are specified, all columns will be scanned.
To scan all members of a column family, leave the qualifier empty as in
‘col_family:’.The filter can be specified in two ways:
1. Using a filterString – more information on this is available in the
Filter Language document attached to the HBASE-4176 JIRA
2. Using the entire package name of the filter.Some examples:hbase> scan ‘.META.’
```
hbase> scan ‘.META.’, {COLUMNS => ‘info:regioninfo’}
hbase> scan ‘t1’, {COLUMNS => [‘c1’, ‘c2’], LIMIT => 10, STARTROW => ‘xyz’}
hbase> scan ‘t1’, {COLUMNS => ‘c1’, TIMERANGE => [1303668804, 1303668904]}
hbase> scan ‘t1’, {FILTER => “(PrefixFilter (‘row2’) AND
(QualifierFilter (>=, ‘binary:xyz’))) AND (TimestampsFilter ( 123, 456))”}
hbase> scan ‘t1’, {FILTER =>
org.apache.hadoop.hbase.filter.ColumnPaginationFilter.new(1, 0)}
```

#### truncate
```
hbase>truncate ‘t1’
```

### HBase surgery tools
#### assign
```
hbase> assign ‘REGION_NAME’
```
#### balancer
```
hbase> balancer
```
#### balance_switch
Enable/Disable balancer. Returns previous balancer state.
Examples:
```
hbase> balance_switch true
hbase> balance_switch false
```
#### close_region
```
hbase> close_region ‘REGIONNAME’, ‘SERVER_NAME’
```
#### compact
Compact all regions in a table:
hbase> compact ‘t1’
Compact an entire region:
hbase> compact ‘r1’
Compact only a column family within a region:
```
hbase> compact ‘r1’, ‘c1’
Compact a column family within a table:
hbase> compact ‘t1’, ‘c1’
```
#### flush
Flush all regions in passed table or pass a region row to
flush an individual region. For example:hbase> flush ‘TABLENAME’
```
hbase> flush ‘REGIONNAME’
```
#### major_compact
```
Compact all regions in a table:
hbase> major_compact ‘t1’
Compact an entire region:
hbase> major_compact ‘r1’
Compact a single column family within a region:
hbase> major_compact ‘r1’, ‘c1’
Compact a single column family within a table:
hbase> major_compact ‘t1’, ‘c1’
```
#### move
```
hbase> move ‘ENCODED_REGIONNAME’, ‘SERVER_NAME’
```
#### split
```
split ‘tableName’
split ‘regionName’ # format: ‘tableName,startKey,id’
split ‘tableName’, ‘splitKey’
split ‘regionName’, ‘splitKey’
```
#### unassign
```
hbase> unassign ‘REGIONNAME’, true
```
#### hlog_roll
Roll the log writer. That is, start writing log messages to a new file.
```
hbase>hlog_roll
```
#### zk_dump
```
hbase>zk_dump
```

### Cluster replication tools
#### add_peer
```
hbase> add_peer ‘2’, “zk1,zk2,zk3:2182:/hbase-prod”
```
#### remove_peer
```
hbase> remove_peer ‘1’
```
#### list_peers
```
hbase> list_peers
```
#### #### enable_peer
```
hbase> enable_peer ‘1’
```
#### disable_peer
```
hbase> disable_peer ‘1’
```
#### start_replication
```
hbase> start_replication
```
#### stop_replication
```
hbase> stop_replication
```
### Security tools
#### grant
```
hbase> grant ‘bobsmith’, ‘RW’, ‘t1’, ‘f1’, ‘col1’
```
#### revoke
```
hbase> revoke ‘bobsmith’, ‘t1’, ‘f1’, ‘col1’
```
#### user_permission
```
hbase> user_permission ‘table1’
```
