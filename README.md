# Specification of GitData project and data configuration
A configuration file of your data **COULD** use next formats: JSON, XML or PHP (array confiration file like used by Laravel), 
but **SHOULD** contains an information of correct structure:
- [Information project block](#information-block)
- [Database specification block](#database-specification-block)
  - [Table specification](#table-specification)
    - [Columns specification](#columns-specification)
    - [Indexes specification](#indexes-specification)
    - [Constraints specification](#constraints-specification)
- [Data preload block](#data-preload-block)
  - [Key-valued data dumb](#key-valued-data-dumb)
  - [Records data dumb](#records-data-dumb)

- [Column types](#column-types)
- [Derivative types](#derivative-types)

Please, see [total example](#total-example) for clarity.
You can find full documentation and real projects on www.gitdata.net. Please use our contacts from offical site and [contacts](#contacts) block.

-------------

## Information block
> Are in root space

Value | Description | Requeried
------|-------------|----------
code | An unique code of GitData project | Yes
name | A your of then project | Yes
dataset | A name of dataset | Yes
description | Full information of your project | No
keywords | Keywords characterize your data | No
license | Licence for publicated data and functionals | Yes
author | Your full name | No
homepage | Your information or progect site | No
contacts | Your contacts | No
git | A link of your project's repository | No
version | An actual version of specification | No
modified | Date when the specification was modified | No

> For example:
```
 'code' => 'gitdata',
 'name' => 'The GitData project',
 'description' => 'Some project of GitData including many datasets',
 'dataset' => 'Some dataset',
 'license' => 'MIT',
 'author' => 'GitData team',
 'homepage' => 'http://database.gitdata.net',
 'contacts' => 'support@gitdata.net',
 'git' => 'https://github.com/GitDataOrg/.meta',
 'version' => '1.0',
 'modified' => '2016-12-06T18:08:00+03:00',
```

-------------

## Database specification block
> Is in a **schema** node. It contains specifications of tables. Each table has unique key same as the table name in your data.

### Table specification
Value | Description | Requeried
------|-------------|----------
columns | Specification of columns | Yes
indexes | Specification of database indeces | No
constraints | Specification of constraints for saving values | No
comment | A comment of table to save in database | No

#### Columns specification
> Each records has unique key same as the column name in the table.

Value | Description | Requeried | Default
------|-------------|-----------|---------
type | Specification of columns | Yes | string
length | Maximal length of characters | No | 255
precision | Size of numerics | No | 10
scale | a scale of a numeric | No | 2
unsigned | Set unsigned numeric value | No | false
increments | Set autoincrement value | No | false
nullable | Show null values are avaliable | No | true
default | A default value in cafe not set | No | 
comment | A comment for the column in database | No | 
primary | Set a primary index for table | No | false
index | Used if columns should be personal indexed | No | false
unique | Used for unique indexing, `index` no needed | No | false

#### Indexes specification
> Each records has unique key same as the index's name in the database, or can be absent.

Value | Description | Requeried
------|-------------|----------
type | A type of index (`index` or `primary`) | Yes
column | Name of a column or set of indexing columns | Yes
direction | A direction od indexing (`asc` or `desc`) | No

#### Constraints specification
> Each records has unique key same as the constraint's name in the database, or can be absent.

Value | Description | Requeried
------|-------------|----------
type | A type of constraint (`unique`, `foreign` or `check`) | Yes
column | A dependent refer column or set of a group of columns | Yes
table | Parent table wich contains records | No
references | A column or a group of columns of parent table wich contains records | No

-------------

## Column types
> Most popular types are used in the specification. See ISO `SQL-2015`, `SQL-2008` and older documents. Also `SQL-92` as a basic specification.

Code | Description
-----|------------
char | Character string, fixed length.
string | Variable length character string, maximum length fixed.
text | Big character text
uuid | Unique computer generated character string
integer | Represents an integer. The minimum and maximum values depend on the server
smallint | Same as `integer` type except that it might hold a smaller range of values, depending on the server
bigint | Same as `integer` type except that it might hold a larger range of values, depending on the server
float | Approximate numerical, mantissa precision p. Precision is greater than or equal to 1 and the maximum precision depends on the server
numeric | Exact numerical, precision p, scale s. The maximum precision depends on the server
decimal | Exact numerical, precision p, scale s. A decimal number, that is a number that can have a decimal point in it. The size argument has two parts: precision and scale. The scale can not exceed the precision. Precision comes first, and a comma must separate from the scale argument
money | Like `decimal`, used for finance values
boolean | Stores truth values - either `true` or `false`.
enum | String values from definited set
date | Represents a date. Format: `yyyy-mm-dd` (ISO)
time | Represents a date. Format: `hh:mm:ss` (ISO)
datetime | Represents a date. Format: `yyyy-mm-dd hh:mm:ss` (ISO)
timestamp | Represents a combination of `date` and `time` values separated by a space. Format: `yyyy-mm-dd hh:mm:ss`
blob | Big binary container
json | Text in `json` format
xml | Text in `xml` format

## Derivative types
> Type wich are transforming to basic schema.

Code | Transformation
-----|------------
foreign | transforms to `integer` with simple `index` and `constraint`. Must contain dataset of `constraint`
increments | transforms to `integer` with `autoincrements` and `primary` index.


-------------

## Data preload block
> Datasets wich should be preloaded

### Key-valued data dumb
> Containts records has unique key same as the tables's name and structure of columns same as specification or key-valued data.

### Records data dumb
> Data with the table structure.

-------------

## Total example
> Used PHP format of specification

```php
<?php
return [
 'code' => 'meta',
 'name' => 'Databases',
 'description' => 'Databases operations',
 'license' => 'MIT',
 'author' => 'GitData team',
 'homepage' => 'http://meta.gitdata.net',
 'contacts' => 'support@gitdata.net',
 'git' => 'https://github.com/GitDataOrg/.meta',
 'version' => '1.0',
 'modified' => '2016-12-06T18:08:00+03:00',
 
  'tables' => [
    'server_type' => [
      'columns' => [
        'id' => [
          'type' => 'integer',
          'unsigned' => true,
          'increments' => true,
          'nullable' => false,
          'comment' => 'ID',
        ],
        'code' => [
          'type' => 'varchar',
          'length' => 50,
          'nullable' => false,
          'primary' => true,
          'index' => true,
          'unique' => true,
          'comment' => 'Code',
        ],
        'name' => [
          'type' => 'string',
          'length' => 255,
          'nullable' => false,
          'comment' => 'Name',
        ],
      ],
      'indexes' => [
        'data_types_ix1' => [
          'columns' => ['name'],
          'direction' => 'asc',
        ],
      ],
      'constraints' => [
        'data_types_pk' => [
          'type' => 'primary',
          'columns' => ['id'],
        ],
        'data_types_code' => [
          'type' => 'unique',
          'columns' => ['code'],
        ],
      ],
      'comment' => 'Types of database servers',
    ],
    'data_types' => [
      'columns' => [
        'id' => [
          'type' => 'integer',
          'unsigned' => true,
          'increments' => true,
          'nullable' => false,
          'comment' => 'ID',
        ],
        'code' => [
          'type' => 'varchar',
          'length' => 50,
          'nullable' => false,
          'comment' => 'Code',
        ],
        'name' => [
          'type' => 'string',
          'length' => 255,
          'nullable' => false,
          'comment' => 'Name',
        ],
        'server_type_id' => [
          'type' => 'integer',
          'unsigned' => true,
          'nullable' => false,
          'comment' => 'Server type',
        ],
      ],
      'indexes' => [
        'data_types_ix1' => [
          'columns' => ['name'],
          'direction' => 'asc',
        ],
        'data_types_ix2' => [
          'columns' => ['server_type_id'],
        ],
      ],
      'constraints' => [
        'data_types_pk' => [
          'type' => 'primary',
          'columns' => ['id'],
        ],
        'data_types_code' => [
          'type' => 'unique',
          'columns' => ['code'],
        ],
        'data_types_fk2' => [
          'type' => 'foreign',
          'columns' => 'server_type_id',
          'ref_table' => 'server_type',
          'ref_column' => 'id',
        ],
      ],
      'comment' => 'Types of columns',
    ],
  ],
  'data' => [
    'server_type' => [
      [ 1, 'ansi',   'ANSI SQL', ],
      [ 2, 'pg',     'PostgreSQL', ],
      [ 3, 'mysql',  'MySQL', ],
      [ 4, 'mssql',  'MS SQL Server', ],
      [ 5, 'ora',    'Oracle', ],
      [ 6, 'access', 'MS Access', ],
      [ 7, 'sqlite', 'SQLite', ],
    ],
    'data_types' => [
      [ 1, 'char', 'Character value with fixed length.', 1, ],
      [ 2, 'varchar', 'Character value with variant length', 1, ],
      [ 3, 'text', 'Big character value', 1, ],
    ],
  ],
];
 
```

-------------

## Contacts
- Offical sites: http://www.gitdata.net [EN] and www.gitdata.ru [RU]
- Email: support@gitdata.net
- Vkontakte: https://vk.com/gitdata
- Facebook: https://facebook.com/groups/gitdata
- Twitter: https://twitter.com/gitdatanet
- LinkedIn: https://linkedin.com/groups/8537332
- Google+: https://plus.google.com/communities/107182608615502988258


