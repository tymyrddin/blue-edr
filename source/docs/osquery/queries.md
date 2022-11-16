# Creating queries

The SQL language implemented in Osquery is a superset of SQLite. 

* All queries will start with a `SELECT` statement. There is no updating or deleting any information/data on the endpoint. 
* The exception to the rule: Using other SQL statements, such as UPDATE and DELETE, is possible, but only when creating 
run-time tables (views) or using an extension if the extension supports them. 
* Queries will also include a FROM clause and end with a semicolon.

## Exploring installed programs

To retrieve all the information about the installed programs on the endpoint (`LIMIT` limits the results to display):

    osquery>select * from programs limit 1;
                  name = 7-Zip 21.07 (x64)
               version = 21.07
      install_location = C:\Program Files\7-Zip\
        install_source =
              language =
             publisher = Igor Pavlov
      uninstall_string = "C:\Program Files\7-Zip\Uninstall.exe"
          install_date =
    identifying_number =

To select specific columns rather than retrieve every column in the table:

    osquery>select name, version, install_location, install_date from programs limit 1;
                name = 7-Zip 21.07 (x64)
             version = 21.07
    install_location = C:\Program Files\7-Zip\
        install_date =

## Count

To see how many programs or entries in any table are returned:

    osquery>select count(*) from programs;
    count(*) = 160

## WHERE clause

To get the user table and only display the result for the user James:

    osquery>SELECT * FROM users WHERE username='James';
            uid = 1002
            gid = 544
     uid_signed = 1002
     gid_signed = 544
       username = James
    description =
      directory = C:\Users\James
          shell = C:\Windows\system32\cmd.exe
           uuid = S-1-5-21-605937711-2036809076-574958819-1002
           type = local

The equal sign is not the only filtering option in a WHERE clause:

The equal sign is not the only filtering option in a WHERE clause. Below are filtering operators that can be used in a WHERE clause:

| Sign      | Meaning                                 |
|:----------|:----------------------------------------|
| `=`       | equal                                   |
| `<>`      | not equal                               |
| `>, >=`   | greater than, greater than, or equal to |
| `<, <=`   | less than or less than or equal to      | 
| `BETWEEN` | between a range                         |
| `LIKE`    | pattern wildcard searches               |
| `%`       | wildcard, multiple characters           |
| `_`       | wildcard, one character                 |

## Matching wildcard rules

| String | Meaning                                    |
|:-------|:-------------------------------------------|
| `%`    | Match all files and folders for one level  |
| `%%`   | Match all files and folders recursively    |
| `%abc` | Match all within-level ending in "abc"     |
| `abc%` | Match all within-level starting with "abc" |

Examples:

| String                | Meaning                                                                                               |
|:----------------------|:------------------------------------------------------------------------------------------------------|
| `/Users/%/Library`    | Monitor for changes to every user's Library folder, but not the contents within                       |
| `/Users/%/Library/`   | Monitor for changes to files within each Library folder, but not the contents of their subdirectories |
| `/Users/%/Library/%`  | Same, changes to files within each Library folder                                                     |
| `/Users/%/Library/%%` | Monitor changes recursively within each Library                                                       |
| `/bin/%sh`            | Monitor the bin directory for changes ending in sh                                                    |

Some tables require a WHERE clause, such as the file table, to return a value. If the required `WHERE` clause is not 
included in the query, `osquery` will give an error.

    osquery>select * from file;
    W1017 12:38:29.730041 45744 virtual_table.cpp:965] Table file was queried without a required column in the WHERE clause
    W1017 12:38:29.730041 45744 virtual_table.cpp:976] Please see the table documentation: https://osquery.io/schema/#file
    Error: constraint failed

## Joining Tables

OSquery can also be used to join two tables based on a column that is shared by both tables.

    osquery>.schema users
    CREATE TABLE users(`uid` BIGINT, `gid` BIGINT, `uid_signed` BIGINT, `gid_signed` BIGINT, `username` TEXT, `description` TEXT, `directory` TEXT, `shell` TEXT, `uuid` TEXT, `type` TEXT, `is_hidden` INTEGER HIDDEN, `pid_with_namespace` INTEGER HIDDEN, PRIMARY KEY (`uid`, `username`, `uuid`, `pid_with_namespace`)) WITHOUT ROWID;
    
    osquery>.schema processes
    CREATE TABLE processes(`pid` BIGINT, `name` TEXT, `path` TEXT, `cmdline` TEXT, `state` TEXT, `cwd` TEXT, `root` TEXT, `uid` BIGINT, `gid` BIGINT, `euid` BIGINT, `egid` BIGINT, `suid` BIGINT, `sgid` BIGINT, `on_disk` INTEGER, `wired_size` BIGINT, `resident_size` BIGINT, `total_size` BIGINT, `user_time` BIGINT, `system_time` BIGINT, `disk_bytes_read` BIGINT, `disk_bytes_written` BIGINT, `start_time` BIGINT, `parent` BIGINT, `pgroup` BIGINT, `threads` INTEGER, `nice` INTEGER, `elevated_token` INTEGER, `secure_process` INTEGER, `protection_type` TEXT, `virtual_process` INTEGER, `elapsed_time` BIGINT, `handle_count` BIGINT, `percent_processor_time` BIGINT, `upid` BIGINT HIDDEN, `uppid` BIGINT HIDDEN, `cpu_type` INTEGER HIDDEN, `cpu_subtype` INTEGER HIDDEN, `translated` INTEGER HIDDEN, `cgroup_path` TEXT HIDDEN, `phys_footprint` BIGINT HIDDEN, PRIMARY KEY (`pid`)) WITHOUT ROWID;

Looking at both schemas, `uid` in `users` table is meant to identify the user record, and in the `processes` table, 
the column `uid` represents the user responsible for executing the particular process. 

The tables can be joined using this uid field:

    $ osqueryi
    Using a virtual database. Need help, type '.help'
    osquery>select p.pid, p.name, p.path, u.username from processes p JOIN users u on u.uid=p.uid LIMIT 10;
    +-------+-------------------+---------------------------------------+----------+
    | pid   | name              | path                                  | username |
    +-------+-------------------+---------------------------------------+----------+
    | 7560  | sihost.exe        | C:\Windows\System32\sihost.exe        | James    |
    | 6984  | svchost.exe       | C:\Windows\System32\svchost.exe       | James    |
    | 7100  | svchost.exe       | C:\Windows\System32\svchost.exe       | James    |
    | 7144  | svchost.exe       | C:\Windows\System32\svchost.exe       | James    |
    | 8636  | ctfmon.exe        | C:\Windows\System32\ctfmon.exe        | James    |
    | 8712  | taskhostw.exe     | C:\Windows\System32\taskhostw.exe     | James    |
    | 9260  | svchost.exe       | C:\Windows\System32\svchost.exe       | James    |
    | 10168 | RuntimeBroker.exe | C:\Windows\System32\RuntimeBroker.exe | James    |
    | 10232 | RuntimeBroker.exe | C:\Windows\System32\RuntimeBroker.exe | James    |
    | 8924  | svchost.exe       | C:\Windows\System32\svchost.exe       | James    |
    +-------+-------------------+---------------------------------------+----------+

## Example use

A table autoexec contains the list of executables that are automatically executed on the target machine. There seems 
to be a batch file that runs automatically. What is the name of that batch file (with the extension .bat)?

What is the full path of the batch file found in the above question?

    osquery> .schema autoexec
    CREATE TABLE autoexec(`path` TEXT, `name` TEXT, `source` TEXT, PRIMARY KEY (`path`)) WITHOUT ROWID;
    osquery> select * from autoexec WHERE name LIKE '%.bat';
    +---------------------------------------------------------------------------------------------+----------------+---------------+
    | path                                                                                        | name           | source        |
    +---------------------------------------------------------------------------------------------+----------------+---------------+
    | C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup\batstartup.bat                 | batstartup.bat | startup_items |
    | C:\Users\James\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\batstartup.bat | batstartup.bat | startup_items |
    +---------------------------------------------------------------------------------------------+----------------+---------------+
    osquery>