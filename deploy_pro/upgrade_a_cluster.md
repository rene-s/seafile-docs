# Upgrade a Seafile cluster

## Major and minor version upgrade

Seafile adds new features in major and minor versions. It is likely that some database tables need to be modified or the search index need to be updated. In general, upgrading a cluster contains the following steps:

1. Upgrade the database
2. Update symbolic link at frontend and backend nodes to point to the newest version
3. Update configuration files at each node
4. Update search index in the backend node

In general, to upgrade a cluster, you need:

1. Run the upgrade script (for example, ./upgrade/upgrade_4_0_4_1.sh) in one frontend node
2. Run the minor upgrade script (./upgrade/minor_upgrade.sh) in all other nodes to update symbolic link
3. Update configuration files at each node according to the documentation for each version
4. Delete old search index in the backend node if needed

## Maintanence upgrade

Doing maintanence upgrading is simple, you only need to run the script `./upgrade/minor_upgrade.sh` at each node to update the symbolic link.

## Specific instructions for each version

### From v4.3 to v4.4

There are no database and search index upgrade from v4.3 to v4.4. Perform the following steps to upgrade:

1. Run the minor upgrade script at frontend and backend nodes

### From v4.2 to v4.3

v4.3 contains no database table change from v4.2. But the old search index will be deleted and regenerated.

A new option COMPRESS_CACHE_BACKEND = 'locmem://' should be added to seahub_settings.py

The secret key in seahub_settings.py need to be regenerated, the old secret key lack enough randomness.

Perform the following steps to upgrade:

1. Run the upgrade script at one fronend node to modify the seahub_settings.py
2. Modify seahub_settings.py at each node, replacing the old secret key with the new one and add option COMPRESS_CACHE_BACKEND
3. Run the minor upgrade script at frontend and backend nodes
4. Delete the old search index (the folder pro-data/search) at the backend node
5. Delete the old office preview output folder (/tmp/seafile-office-output) at the backend node


