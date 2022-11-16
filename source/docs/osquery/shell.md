# Osquery shell

One of the ways to interact with Osquery is by using the interactive mode:

    $ osqueryi

To list all the available tables that can be queried:

    osquery> .table
    => appcompat_shims
      => arp_cache
      => atom_packages
      => authenticode
      => autoexec
      => azure_instance_metadata
      => azure_instance_tags
      => background_activities_moderator
      => bitlocker_info
      => carbon_black_info
      => carves
      => certificates
      => chassis_info
      => chocolatey_packages

To list all the tables with the term `user` in them:

    osquery> .table user
      => user_groups
      => user_ssh_keys
      => userassist
      => users

## Schemas

To list the user's table schema:

    osquery> .schema users
    CREATE TABLE users(`uid` BIGINT, `gid` BIGINT, `uid_signed` BIGINT, `gid_signed` BIGINT, `username` TEXT, `description` TEXT, `directory` TEXT, `shell` TEXT, `uuid` TEXT, `type` TEXT, `is_hidden` INTEGER HIDDEN, `pid_with_namespace` INTEGER HIDDEN, PRIMARY KEY (`uid`, `username`, `uuid`, `pid_with_namespace`)) WITHOUT ROWID;

To display the columns from the user table:

    osquery>select gid, uid, description, username, directory from users;
    +-----+------+------------------------------------------------------------+----------------------+-------------------------------------------+
    | gid | uid  | description                                                | username           | directory                                   |
    +-----+------+-------------------------------------------------------------------------------------------------------------------------------+
    | 544 | 500  | Built-in account for administering the computer/domain     | Administrator      |                                             |
    | 581 | 503  | A user account managed by the system.                      | DefaultAccount     |                                             |
    | 546 | 501  | Built-in account for guest access to the computer/domain   | Guest              |                                             |
    | 544 | 1002 |                                                            | James              | C:\Users\James                              |
    | 18  | 18   |                                                            | SYSTEM             | %systemroot%\system32\config\systemprofile  |
    | 19  | 19   |                                                            | LOCAL SERVICE      | %systemroot%\ServiceProfiles\LocalService   |
    | 20  | 20   |                                                            | NETWORK SERVICE    | %systemroot%\ServiceProfiles\NetworkService |
    +-----+------+------------------------------------------------------------+--------------------+---------------------------------------------+

## Display Mode

    .mode MODE       Set output mode where MODE is one of:
                       csv      Comma-separated values
                       column   Left-aligned columns see .width
                       line     One value per line
                       list     Values delimited by .separator string
                       pretty   Pretty printed SQL results (default)

