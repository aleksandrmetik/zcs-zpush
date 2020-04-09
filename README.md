# Still in progress. Fork for Centos.

# zcs-zpush
This repository is source for integrating Zimbra Single Server and Z-Push + Zimbra Backend to achieve ActiveSync on Zimbra OSE.

# Integrating Zimbra and Z-push 2.5.1 with zimbrabackend69 on Ubuntu 18.04 and Zimbra 8.8.15+

Install dependecies

```bash
yum update
yum upgrade
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install yum-utils
yum -y install centos-release-scl.noarch
yum -y install rh-php72 rh-php72-php rh-php72-php-fpm httpd
yum install git php72-php php72-php-cli php72-php-soap php72-php-mbstring php72-php-curl php72-php-xml php72-php-pecl-memcached php72-php-pecl-memcache -y
```

Clone repo

```bash
git clone https://github.com/LinuxCuba/zcs-zpush.git
```

Create folder for log

```bash
mkdir /var/lib/z-push /var/log/z-push
chmod 755 /var/lib/z-push /var/log/z-push
chown zimbra:zimbra /var/lib/z-push /var/log/z-push
```

Save z-push folder on /opt/

```bash
cp -rvf zcs-push /opt/z-push
```

Create symlink

```bash
ln -sf /opt/z-push/z-push /opt/zimbra/jetty/webapps/
```

Save php script on /usr/bin

```bash
cp php-cgi-fix.sh /usr/bin/php-cgi-fix.sh
chmod +x /usr/bin/php-cgi-fix.sh
```

Change publicHostname domain on your Zimbra into localhost

```bash
su - zimbra -c 'zmprov md yourzimbradomain.tld zimbraPublicServiceHostname localhost'
su - zimbra -c 'zmprov md yourzimbradomain.tld zimbraPublicServiceProtocol https'
```

Backup and replace jetty.xml.in

```bash
cp /opt/zimbra/jetty/etc/jetty.xml.in /opt/zimbra/jetty/etc/jetty.xml.in.backup
cp /opt/z-push/jetty.xml.in-for-zcs-8815 /opt/zimbra/jetty/etc/jetty.xml.in
chown zimbra.zimbra /opt/zimbra/jetty/etc/jetty.xml.in
```

Find. backup and replace php.ini

```bash
php72 -v --ini
cp /etc/php.ini  /etc/php.ini.backup
cp /opt/z-push/php.ini /etc/php.ini
```

Restart Zimbra Mailbox

```bash
su - zimbra -c 'zmmailboxdctl restart'
```

For testing, please access https://ip-of-zimbra/Microsoft-Server-ActiveSync from your browser. Or you can configure your mobile devices and ensure exchange as protocol
