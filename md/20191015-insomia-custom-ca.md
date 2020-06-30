Custom CA Support for Insomnia
==
Run following scripts as `root`
## 1. Install custom CA (Ubuntu)
```bash
cp your-ca.crt /usr/local/share/ca-certificates/
dpkg-reconfigure ca-certificates
```
## 2. Make Insomnia to use system CA
Change the version number (i.e. <kbd>7.0.1</kbd>) accordingly:
```bash
rm /tmp/insomnia_7.0.1/2017-09-20.pem
echo "L+ /tmp/insomnia_7.0.1/2017-09-20.pem - - - - /etc/ssl/certs/ca-certificates.crt" \
    > /usr/lib/tmpfiles.d/insomnia.conf
systemd-tmpfiles --create /usr/lib/tmpfiles.d/insomnia.conf
```
On Ubuntu:
```bash
echo "L+ /tmp/insomnia_$(dpkg -s insomnia | grep Version | awk '{print $2}')\
/2017-09-20.pem - - - - /etc/ssl/certs/ca-certificates.crt" \
    > /usr/lib/tmpfiles.d/insomnia.conf &&
    systemd-tmpfiles --create /usr/lib/tmpfiles.d/insomnia.conf
```

Credits: [Force Insomnia to use the system trust store](https://kdecherf.com/blog/2018/07/13/force-insomnia-to-use-the-system-trust-store/)
