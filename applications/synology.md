# Synology

## 更新 cloudflare dns 位置 [SynologyCloudFlareDDNS](https://github.com/namukcom/SynologyCloudFlareDDNS)

## Wildcard Certificate (使用[cerbot](https://certbot.eff.org))

### 下載certbot 腳本

```sh
wget https://dl.eff.org/certbot-auto
chmod a+x ./certbot-auto
sudo ./certbot-auto
```

### 取得憑證，並放置 /usr/syno/etc/certificate/_archive/xxx

```sh
sudo ./certbot-auto certonly \
--server https://acme-v02.api.letsencrypt.org/directory \
--manual --preferred-challenges dns \
-d '*.weiting.me' -d weiting.me
```

## 重啟服務

Command:
> synoservice
> synoservicectl
> synoservicecfg

service list

```bash
> synoservice --list

DSM
apparmor
atalk
avahi
bluetoothd
bonjour
btacd
crond
cups-lpd
cupsd
dbus
dc-output
ddns
findhost
ftpd
ftpd-ssl
gcpd
heartbeat
hotplugd
iscsitrg
ldap-server
manutild
miniupnpd-handler
natpmpd
nfsd
nginx
nmbd
nslcd
ntpd-client
ntpd-server
pgsql
pkgctl-ActiveBackup
pkgctl-AntiVirus
pkgctl-Apache2.2
pkgctl-Apache2.4
pkgctl-CMS
pkgctl-Calendar
pkgctl-Chat
pkgctl-Contacts
pkgctl-DNSServer
pkgctl-DiagnosisTool
pkgctl-DirectoryServer
pkgctl-DirectoryServerForWindowsDomain
pkgctl-Docker
pkgctl-DocumentViewer
pkgctl-DownloadStation
pkgctl-FileStation
pkgctl-GLPI
pkgctl-HyperBackup
pkgctl-Java7
pkgctl-LogCenter
pkgctl-MailClient
pkgctl-MailPlus-Server
pkgctl-MariaDB10
pkgctl-MigrationAssistant
pkgctl-Node.js_v12
pkgctl-Node.js_v4
pkgctl-Node.js_v8
pkgctl-NoteStation
pkgctl-OAuthService
pkgctl-PHP5.6
pkgctl-PHP7.0
pkgctl-PHP7.2
pkgctl-PHP7.3
pkgctl-Perl
pkgctl-PhotoStation
pkgctl-ProactiveCare
pkgctl-PythonModule
pkgctl-RadiusServer
pkgctl-ReplicationService
pkgctl-SSOServer
pkgctl-SnapshotReplication
pkgctl-Spreadsheet
pkgctl-StorageAnalyzer
pkgctl-SurveillanceStation
pkgctl-SynoFinder
pkgctl-SynologyApplicationService
pkgctl-SynologyDrive
pkgctl-SynologyMoments
pkgctl-TeamViewer
pkgctl-TextEditor
pkgctl-VPNCenter
pkgctl-VideoStation
pkgctl-Virtualization
pkgctl-WebDAVServer
pkgctl-WebStation
pkgctl-phpMyAdmin
pkgctl-py3k
pkgctl-python3
pppoerelay
rsyncd
s2s_daemon
samba
scemd
scsi_plugin_server
sftp
snmp
ssdp
ssh-shell
sssd
support-remote-access
synoagentregisterd
synobackupd
synocacheclient
synocachepinfiletool
synocgid
synoconfd
synocontentextractd
synocrond
synogpoclient
synoindexd
synologanalyzer
synologrotate
synomkflvd
synomkthumbd
synomount
synonetd
synoovs-db
synoovs-vswitch
synoperfeventd
synopyntlmd
synorelayd
synosnmpcd
synostoraged
synotifyd
synotunnel
synovpnclient
synowifid
synowstransfer
syslog-acc
syslog-ng
syslog-notify
system
telnetd
tftp
upnpd
ups-net
ups-usb
usbipd
winbindd
```

help

```bash
synoservice --help
Copyright (c) 2003-2020 Synology Inc. All rights reserved.

SynoService Tool Help (Version 25426)
Usage: synoservice
        --help                                                  Show this help
        --help-dev                                              More specialty functions for deveplopment
        --is-enabled            [ServiceName]                   Check if the service is enabled
        --status                [ServiceName]                   Get the status of specified services
        --enable                [ServiceName]                   Set runkey to yes and start the service (alias to --start)
        --disable               [ServiceName]                   Set runkey to no and stop the service (alias to --stop)
        --hard-enable           [ServiceName]                   Set runkey to yes and start the service and its dependency (alias to --hard-start)
        --hard-disable          [ServiceName]                   Set runkey to no and stop the service and its dependency (alias to --hard-stop)
        --restart               [ServiceName]                   Restart the given service
        --reload                [ServiceName]                   Reload the given service
        --pause                 [ServiceName]                   Pause the given service
        --resume                [ServiceName]                   Resume the given service
        --pause-by-reason       [ServiceName]   [Reason]        Pause the service by given reason
        --resume-by-reason      [ServiceName]   [Reason]        Resume the service by given reason
        --pause-all             (-p)    [Reason]        (Event) Pause all service by given reason with optional event(use -p to include packages)
        --pause-all-no-action   (-p)    [Reason]        (Event) Set all service runkey to no but leave the current service status(use -p to include packages)
        --resume-all            (-p)    [Reason]                Resume all service by given reason(use -p to include packages)
        --reload-by-type        [type]          (buffer)        Reload services with specified type
        --restart-by-type       [type]          (buffer)        Restart services with specified type
                                                                Type may be {file_protocol|application}
                                                                Sleep $buffer seconds before exec the command (default is 0)
```