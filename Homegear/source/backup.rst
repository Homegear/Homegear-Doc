Backup and Restore
##################

Backup
******

To back up Homegear, just copy the directories ``/etc/homegear`` and ``/var/lib/homegear`` (see :ref:`files-and-directories`). You can exclude the binary module files (ending with ".so"). This is mandatory if restoring to a system with a different CPU architecture or to a different distribution or distribution version::

    tar -zcpf homegear-backup.tar.gz --exclude="*.so" /etc/homegear /var/lib/homegear /data/homegear-data

.. note:: When the backup is meant to be restored on exactly the same system (same distribution, same distribution version, same CPU architecture), omit ``--exclude="*.so"``. This has the advantage that all manually installed additional Node-BLUE nodes are backed up as well.

Restore
*******

1. Install Homegear with all modules previously installed. The Homegear version can be newer than the backed up one.
2. Restore your backup::

	tar -zxf homegear-backup.tar.gz
	cp -a etc/homegear /etc
	cp -a var/lib/homegear /var/lib
	cp -a data/homegear-data /data

3. If the Homegear version installed now is newer than the backed up one, reinstall Homegear with all modules. On Debian-like systems with::

    apt install --reinstall homegear homegear-module1 homegear-module2 ...

4. Reinstall all Node-BLUE nodes previously installed. For the core and extra nodes on Debian-like systems run::

    apt install --reinstall homegear-nodes-core homegear-nodes-extra

5. If you previously installed additional nodes, restore them as well.