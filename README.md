# Docs on backing up random Chef things

# Compliance

## Backup
Make a postgres export using the following commands:
```
export THEDATE=`date '+%Y-%m-%d-%H-%M-%S'`
sudo -E -u chef-pgsql bash
/opt/chef-compliance/embedded/bin/pg_dump -c chef_compliance | gzip --fast > /tmp/compliance-dump-$THEDATE.gz
```

Synchronize to make sure that all of the data is present on-disk:
```
sync
```

Backup the /etc/chef-compliance directories and include the postgres export file as root
```
tar cvfzp compliance-$THEDATE.tar.gz /etc/chef-compliance /tmp/compliance-dump-$THEDATE.gz
```

## Restore
```
# re-install Chef Compliance pkg if necessary to get binaries then
sudo -u chef-pgsql /opt/chef-compliance/embedded/bin/psql chef_compliance < compliance_dump.sql
chef-compliance-ctl reconfigure
chef-compliance-ctl restart
```

# Supermarket

## Backup
Make a postgres export using the following commands:
```
export THEDATE=`date '+%Y-%m-%d-%H-%M-%S'`
sudo -E -u supermarket bash
/opt/supermarket/embedded/bin/pg_dump -c supermarket -h localhost -p 15432 | gzip --fast > /tmp/supermarket-dump-$THEDATE.gz
```

Synchronize to make sure that all of the data is present on-disk:
```
sync
```

Backup the /etc/supermarket and /var/opt/supermarket directories and include the postgres export file as root
```
tar cvfzp supermarket-$THEDATE.tar.gz /etc/supermarket /tmp/supermarket-dump-$THEDATE.gz
```

## Restore
```
# re-install Supermarket if necessary to get binaries then
sudo -u supermarket /opt/supermarket/embedded/bin/psql supermarket < supermarket_dump.sql
supermarket-ctl reconfigure
supermarket-ctl restart
```

# Chef Automate

Chef Automate backup operations are easy.

## Backup
```
vagrant@automate-server:~$ sudo delivery-ctl backup-data
 [Delivery Backup] Create backup directory '/var/tmp/delivery-backup-2016-11-01T1140.HFiC'
 [Delivery Backup] Capturing database dump
 [Delivery Backup] Copying git repositories
 [Delivery Backup] Copying config files
 [Delivery Backup] Writing backup to '/var/tmp/delivery-backup-2016-11-01T1140.HFiC/delivery_backup.tar'
 [Delivery Backup] Completed!
vagrant@automate-server:~$
```

## Restore and other operations
```
delivery-ctl Backup Commands:
  create-backup
    Create a Chef Automate backup
  delete-backups
    Delete one or many Chef Automate backups
  list-backups
    List known Chef Automate backups
  restore-backup
    Restore a Chef Automate backup
```
