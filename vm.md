---
layout: article
title: 'Download and Install the RecordService VM'
share: false
---

{% include toc.html %}

## Downloading RecordService VM

Follow these steps to download the RecordService VM.

1. Install VirtualBox. The VM works with VirtualBox version 4.3 on Ubuntu 14.04 and VirtualBox version 5 on OSX 10.9. Download VirtualBox for free at [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads).
1. Clone the recordservice-quickstart repository onto your local disk from [https://github.com/cloudera/recordservice-quickstart](https://github.com/cloudera/recordservice-quickstart).

## Installing the RecordService VM

|| <b>Note:</b> If you have previously installed the VM on your host machine, follow the instructions in [Verify VM is Listed Correctly in Hosts](#verify-vm-is-listed-correctly-in-hosts) and [Verify Known Hosts](#verify-known-hosts). ||

Follow these steps to install the RecordService VM.

1. In a terminal window, navigate to the root of the RSQuickstart Git repository.
1. Run the script `install.sh`.

This script downloads an `ova` file and loads it into VirtualBox. The script might ask you to enter a password, because it edits your `/etc/hosts` file to give the VM a stable IP address, `quickstart.cloudera`. When the script completes, the running VM functions as a RecordService server.

### Testing Your VM Configuration

Test that the VM is running and IP forwarding is configured properly.

1. Enter the command `ssh cloudera@quickstart.cloudera`.
1. Enter the password `cloudera`.

If you cannot ssh to the VM, see [Troubleshooting the VM Configuration](#troubleshooting-the-vm-configuration).

Successfully connecting through ssh verifies that you can log in to the VM.

### Configuring RecordService Environment Variables

1. On your host machine, navigate to the root of your RecordServiceClient repository, `$RECORD_SERVICE_HOME`.
1. Run `source config.sh`.
1. Navigate to the `recordservice-quickstart` directory.
1. Run `source vm_env.sh`.
1. Navigate to `$RECORD_SERVICE_HOME/java`.
1. Test your environment with the command `mvn test -DargLine="-Duser.name=recordservice"`

The VM is preconfigured with sample data to execute the tests. The _recordservice_ user has access to the data via Sentry.

The VM is not secured with LDAP or Kerberos. If you want to change the security configuration, you can add roles in Sentry through `impala-shell`. If you have `impala-shell` on your host machine, you can connect to the VM by issuing the following command.

```
impala-shell -i quickstart.cloudera:21000 -u impala
```

You can also connect from within `impala-shell`.

```
CONNECT quickstart.cloudera:21000;
```

## Troubleshooting the VM Configuration

### Unable to ssh to the VM
* Ensure that the ssh daemon is running on your machine.
* Ensure that the RecordService VM is running. In your terminal, enter the following command:

```
VBoxManage list runningvms
```

&nbsp;&nbsp;&nbsp;&nbsp;You should see “rs-demo” listed as a running VM.

### Verify VM is Listed Correctly in Hosts

Check that the VM is listed correctly in your `/etc/hosts` file. If you open the file, you should see a line that lists an IP address followed by `quickstart.cloudera`. You can check the VM’s IP with the following command:

```
VBoxManage guestproperty get rs-demo /VirtualBox/GuestInfo/Net/0/V4/IP
```

### Verify Known Hosts

If you’ve used a Cloudera QuickStart VM before, your known hosts file might already have an entry for `quickstart.cloudera` registered to a different key. Delete any reference to `quickstart.cloudera` from your known hosts file, which is usually found in `~/.ssh/known_hosts`.

### Verify Workers Are Running

If you receive an error message similar to the following, your worker nodes are likely not running:
```
Exception in thread "main" java.io.IOException: com.cloudera.recordservice.core.RecordServiceException: TRecordServiceException(code:INVALID_REQUEST, message:Worker membership is empty. Please ensure all RecordService Worker nodes are running.)
```

You can correct the problem by restarting the RecordService server using the following command:

```
sudo service recordservice-server restart
```

## Debugging the VM

### Restarting a service

To restart a service, use the standard RHEL service model.
 
```
service <service-name> start|stop|restart
```

You can view all of the installed services `/etc/init.d` directory.

### Debugging Via Logs

RecordService logs are in the `/var/log/recordservice` directory. You can find most service logs in the `/var/log` directory.

### Dubugging Via Webpage

View the RecordService debug page on your host machine at `quickstart.cloudera:11050`.

### Other Service Variables

To view the default execution environment for a service, look for its file in the `/etc/default` directory.