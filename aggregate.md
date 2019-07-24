Aggregate
=========

![](figures/Aggregate-64x64.png)

Short Description {#aggregate-short-description}
-----------------

Aggregate computes statistical information about input data records.

  -----------------------------------------------------------------------------------------------
  Component                Same input  Sorted   Inputs   Outputs   Java   CTL    Auto-propagated
                            metadata   inputs                                       metadata
  ------------------------ ---------- -------- -------- --------- ------ ------ -----------------
  Aggregate                    \-        no       1        1-n      no     no          no

  -----------------------------------------------------------------------------------------------

Ports {#aggregate-ports}
-----

  -------------------------------------------------------------------------------
  Port      Number   Required  Description                             Metadata
  type                                                                 
  -------- -------- ---------- --------------------------------------- ----------
  Input       0        yes     For input data records                  Any1

  Output     0-n       yes     For statistical information             Any2
  -------------------------------------------------------------------------------

This component has one input port and one or more output ports.

Metadata {#aggregate-metadata}
--------

Aggregate does not propagate metadata.

Aggregate has no metadata template.

Metadata on the output ports must be same.

Aggregate Attributes
--------------------


| Attribute    | Re | Description                            | Possibl |
|              | q  |                                        | e       |
|              |    |                                        | values  |
|--------------|----|----------------------------------------|---------|
| Basic        |    |                                        |         |
+--------------+----+----------------------------------------+---------+
| Aggregate    |    | A key according to which records are   | E.g.    |
| key          |    | grouped. For more information, see     | `first_ |
|              |    | [???](#group-key).                     | name;`  |
+--------------+----+----------------------------------------+---------+
| Aggregation  |    | A sequence of individual mappings for  |         |
| mapping      |    | output field names separated from each |         |
|              |    | other by a semicolon. Each mapping can |         |
|              |    | have the following form:               |         |
|              |    | `$outputField:=constant` or            |         |
|              |    | `$outputField:=$inputField` (this must |         |
|              |    | be a field name from the Aggregate     |         |
|              |    | key) or                                |         |
|              |    | `$outputField:=somefunction($inputFiel |         |
|              |    | d)`.                                   |         |
|              |    | The semicolon after the last mapping   |         |
|              |    | is optional and may be omitted.        |         |
+--------------+----+----------------------------------------+---------+
| Charset      |    | Encoding of incoming data records.     | UTF-8   |
|              |    |                                        | \|      |
|              |    |                                        | other   |
|              |    |                                        | encodin |
|              |    |                                        | g       |
+--------------+----+----------------------------------------+---------+
| Sorted input |    | By default, input data records are     | true    |
|              |    | supposed to be sorted according to     | (defaul |
|              |    | Aggregate key. If they are not sorted  | t)      |
|              |    | as specified, switch this value to     | \|      |
|              |    | `false`.                               | false   |
+--------------+----+----------------------------------------+---------+
| Equal NULL   |    | By default, records with null values   | false   |
|              |    | are considered to be different. If set | (defaul |
|              |    | to `true`, records with null values    | t)      |
|              |    | are considered to be equal.            | \| true |
+--------------+----+----------------------------------------+---------+
| Deprecated   |    |                                        |         |
+--------------+----+----------------------------------------+---------+
| Old          |    | A mapping that was used in older       |         |
| aggregation  |    | versions of CloverDX, its use is       |         |
| mapping      |    | **deprecated** now.                    |         |


Details {#aggregate-details}
-------

Aggregate receives data records through a single input port, computes
statistical information about input data records and sends them to all
output ports.

### Aggregation Mapping {#aggregate-mapping}

Aggregate mapping requires metadata on input and output edges of the
component. You must assign metadata to the component input and output
before you can create the transformation.

Define Aggregate key. The key field is necessary for grouping.

Click the Aggregation mapping attribute row to open the Aggregation
mapping dialog. In it, you can define both the mapping and the
aggregation.

The dialog consists of two panes. You can see the Input field pane on
the left and the Aggregation mapping pane on the right.

1.  Each Aggregate key field can be mapped to the output. Drag the input
    field and drop it to the Mapping column in the right pane at the row
    of the desired output field name. After that, the selected input
    field appears in the Mapping column.

    The following mapping can only be done for key fields:
    `$outField=$keyField`.

2.  Fields that are not part of Aggregate key can be used in aggregation
    functions and the result of the aggregation function is mapped to
    the output.

    To define a function for a field (either contained in the key or
    not), click the row in the Function column, select a function from
    the combo list and click Enter.

    Aggregation function `count()` has no parameter, therefore it
    requires no input field.

3.  For each output field, a constant may also be assigned to it.

    `$outputField:="Clover"`

### Aggregate Functions {#aggregate-list-of-functions}

+-----------+---------------------------+---------+---------+---------+
| Function  | Description               | Input   | Output  | Input   |
| name      |                           | data    | data    | can be  |
|           |                           | type    | type    | list    |
+:==========+:==========================+:========+:========+:========+
| avg       | Returns an average value  | numeric | numeric | no      |
|           | of numbers. Null values   | data    | data    |         |
|           | are ignored. If all       | type    | type    |         |
|           | aggregated values are     |         |         |         |
|           | null, returns null.       |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| count     | Count records, null       | \-      | numeric | yes     |
|           | values are counted as     |         | data    |         |
|           | well as other values.     |         | type    |         |
+-----------+---------------------------+---------+---------+---------+
| countnotn | Counts records, if the    | any     | numeric | yes     |
| ull       | field contains null, it   |         | data    |         |
|           | is not counted in.        |         | type    |         |
+-----------+---------------------------+---------+---------+---------+
| countuniq | Counts unique values.     | any     | numeric | yes     |
| ue        | `null` is unique value.   |         | data    |         |
|           | The function assumes 1,   |         | type    |         |
|           | 2, 2, 2, null, 1, null as |         |         |         |
|           | 3 unique values.          |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| crc32     | Calculates crc32          | any     | long    | no      |
|           | checksum. Crc of null is  |         |         |         |
|           | null.                     |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| first     | Returns the first value   | any     | any     | yes     |
|           | of group. If the first    |         |         |         |
|           | value is null, returns    |         |         |         |
|           | null.                     |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| firstnotn | Returns the first value,  | any     | any     | yes     |
| ull       | which is not null. If all |         |         |         |
|           | received values were      |         |         |         |
|           | null, returns null.       |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| last      | Returns the last value of | any     | any     | yes     |
|           | the group. If last value  |         |         |         |
|           | is null, returns null.    |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| lastnotnu | Returns the last not-null | any     | any     | yes     |
| ll        | value. If all values are  |         |         |         |
|           | null, returns null.       |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| max       | Returns the maximum       | numeric | numeric | yes     |
|           | value. If all values are  | data    | data    |         |
|           | null, returns `null`.     | type    | type    |         |
+-----------+---------------------------+---------+---------+---------+
| md5       | If a group contains one   | any     | string  | no      |
|           | record, returns           |         |         |         |
|           | base64-encoded md5        |         |         |         |
|           | checksum. If a group      |         |         |         |
|           | contains more records,    |         |         |         |
|           | the particular input      |         |         |         |
|           | records are concatenated  |         |         |         |
|           | together before the       |         |         |         |
|           | calculation of md5        |         |         |         |
|           | checksum.                 |         |         |         |
|           |                           |         |         |         |
|           | If an input is string, it |         |         |         |
|           | is converted to sequence  |         |         |         |
|           | of bytes using encoding   |         |         |         |
|           | set up in the component   |         |         |         |
|           | first. If an input is     |         |         |         |
|           | integer or long, it is    |         |         |         |
|           | printed to the string     |         |         |         |
|           | first. If an input is     |         |         |         |
|           | null, returns null. Use   |         |         |         |
|           | md5sum instead of md5.    |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| md5sum    | If a group contains one   | byte    | string  | no      |
|           | record, returns md5sum of |         |         |         |
|           | the field. If a group     |         |         |         |
|           | contains more records,    |         |         |         |
|           | the field values are      |         |         |         |
|           | concatenated first. If an |         |         |         |
|           | input is null, returns    |         |         |         |
|           | null.                     |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| median    | Returns median value.     | numeric | numeric | no      |
|           | Null values are not       | data    | data    |         |
|           | counted in. If all input  | type    | type    |         |
|           | values are null, returns  |         |         |         |
|           | null.                     |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| min       | Returns minimum value. If | numeric | numeric | yes     |
|           | all input values are      | data    | data    |         |
|           | null, returns null.       | type    | type    |         |
+-----------+---------------------------+---------+---------+---------+
| modus     | Returns the most          | any     | any     | yes     |
|           | frequently used value     |         |         |         |
|           | (null values are not      |         |         |         |
|           | counted in). If there are |         |         |         |
|           | more candidates, the      |         |         |         |
|           | first one is returned. If |         |         |         |
|           | all input values are      |         |         |         |
|           | null, returns null.       |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| sha1sum   | If a group contains one   | byte    | string  | no      |
|           | record, returns sha1sum   |         |         |         |
|           | of the field. If a group  |         |         |         |
|           | contains more records,    |         |         |         |
|           | the field values are      |         |         |         |
|           | concatenated first. If an |         |         |         |
|           | input field is null,      |         |         |         |
|           | returns null.             |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| sha256sum | If an input group         | byte    | string  | no      |
|           | contains one record,      |         |         |         |
|           | returns sha256sum of the  |         |         |         |
|           | field. If a group         |         |         |         |
|           | contains more records,    |         |         |         |
|           | the field values are      |         |         |         |
|           | concatenated first. If    |         |         |         |
|           | all input values are      |         |         |         |
|           | null, returns null.       |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| stddev    | Returns a standard        | numeric | numeric | no      |
|           | deviation. Null values    | data    | data    |         |
|           | are not counted in. If    | type    | type    |         |
|           | all input values are      |         |         |         |
|           | null, returns null.       |         |         |         |
+-----------+---------------------------+---------+---------+---------+
| sum       | Returns sum of input      | numeric | numeric | nol     |
|           | values. If all input      | data    | data    |         |
|           | values are null, returns  | type    | type    |         |
|           | null.                     |         |         |         |
+-----------+---------------------------+---------+---------+---------+

: List of Aggregate Functions

You can calculate md5sum, sha1sum and sha256sum checksums incrementally:
the group of records corresponds to the whole file whereas particular
records contain blocks of the file.

For example, there are 3 records grouped together by a value in the
field `f1`. The field `f2` contains particular blocks: `a`, `b` and `c`
(as bytes). Each value is in the different record. The sha1sum applied
on field `f2` returns `sha1sum("abc")`.

Examples {#aggregate-examples}
--------

### Basic Usage {#aggregate-example-01}

Input metadata contains fields `Weight` and `ProductType`.

Output fields are: `ProductType`, `Count`, `TotalWeight`,
`AverageWeight`, and `Date`. Output metadata can also have other fields.

Aggregate records of the same `ProductType` field. Set `Date` to
`2015-08-20`.

#### Solution {#aggregate-example-01-solution}

Set Aggregate key to `ProductType`.

Set Aggregate mapping:

-   Map `ProductType` to `ProductType`.

-   Use the `count()` aggregation function to count records with a same
    key.

-   Use the `sum()` and `avg()` functions to calculate total and average
    weight of grouped items. Both functions require an input field as an
    argument. Drag the input field `weight` to the Mapping column.

-   Set the Mapping field of `Date` to `2015-08-20`.

The Aggregate mapping is
`$ProductType:=$ProductType;$Count:=count();$TotalWeight:=sum($weight);$AverageWeight:=avg($weight);Date:=2015-08-20;`.

See also {#aggregate-see-also}
--------
