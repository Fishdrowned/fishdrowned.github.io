Custom CA Support for Insomnia
==
Run following scripts as `root`
## 1. Install custom CA (Ubuntu)
```bash
cp your-ca.crt /usr/local/share/ca-certificates/
dpkg-reconfigure ca-certificates
```
## 2. Make Insomnia to use system CA
Change the version number (i.e. <kbd>2021.3.0</kbd>) accordingly:
```bash
rm /tmp/insomnia_2021.3.0/ca-certs.pem
echo "L+ /tmp/insomnia_2021.3.0/ca-certs.pem - - - - /etc/ssl/certs/ca-certificates.crt" \
    > /usr/lib/tmpfiles.d/insomnia.conf
systemd-tmpfiles --create /usr/lib/tmpfiles.d/insomnia.conf
```
### On Ubuntu:
* Install Insomnia by `apt`
```bash
echo "deb [trusted=yes arch=amd64] https://download.konghq.com/insomnia-ubuntu/ default all" \
    | sudo tee /etc/apt/sources.list.d/insomnia.list
apt update
apt install insomnia
```
* Update CA for Insomnia
```bash
echo "L+ /tmp/insomnia_$(dpkg -s insomnia | grep Version | awk '{print $2}')\
/ca-certs.pem - - - - /etc/ssl/certs/ca-certificates.crt" \
    > /usr/lib/tmpfiles.d/insomnia.conf &&
    systemd-tmpfiles --create /usr/lib/tmpfiles.d/insomnia.conf
```

Credits: [Force Insomnia to use the system trust store](https://kdecherf.com/blog/2018/07/13/force-insomnia-to-use-the-system-trust-store/)
