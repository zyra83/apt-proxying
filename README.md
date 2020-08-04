Références: 

- https://wiki.debian-fr.xyz/Apt-cacher-ng
- http://www.man-linux-magique.net/man8/apt-proxy.html

```bash
docker run -p 3142:3142 -it debian bash
apt udpte
apt install apt-cacher-ng nano
nano /etc/apt-cacher-ng/acng.conf
# décommenter le port en 3142
/etc/init.d/apt-cacher-ng start
# tester l'url http://172.17.0.2:3142/acng-report.html (ou celle retournée par la command ip a dans le conteneur)
# pour fouiller un peu les dépôts
du /var/cache/apt-cacher-ng
```
Dans le preseed, placer la ligne `d-i mirror/http/proxy string http://172.17.0.2:3142`.

Sinon sur une bécane classique, ajouter un ficher dans apt.conf.d contenant :
```
Acquire::http {
        Proxy "http://172.17.0.2:3142";
};
```
Attention ça merde pour les dépôts en https, le proxy ne fait pas son boulo, du coup le dépôt docker par exemple ne passera pas le proxy cache correctement.
Il existe un réglage pour saute le proxy en https mais ça règle pas le problème de cache des dépôts HTTPS.
ça va surement finir en mirroir de dépôt avec apt-mirror...


```
# Source depuis https://debgen.simplylinux.ch/
# apt install curl wget apt-transport-https dirmngr

#------------------------------------------------------------------------------#
#                   OFFICIAL DEBIAN REPOS
#------------------------------------------------------------------------------#

###### Debian Main Repos
deb http://deb.debian.org/debian/ buster main contrib non-free

deb http://deb.debian.org/debian/ buster-updates main contrib non-free

deb http://deb.debian.org/debian-security buster/updates main

deb http://ftp.debian.org/debian buster-backports main

#------------------------------------------------------------------------------#
#                   OFFICIAL DEBIAN REPOS
#------------------------------------------------------------------------------#

###### Debian Main Repos
deb http://deb.debian.org/debian/ stretch main contrib non-free

deb http://deb.debian.org/debian/ stretch-updates main contrib non-free

deb http://deb.debian.org/debian-security stretch/updates main

deb http://ftp.debian.org/debian stretch-backports main

#------------------------------------------------------------------------------#
#                   OFFICIAL DEBIAN REPOS
#------------------------------------------------------------------------------#

###### Debian Main Repos
deb http://deb.debian.org/debian/ jessie main contrib non-free

deb http://deb.debian.org/debian/ jessie-updates main contrib non-free

deb http://deb.debian.org/debian-security jessie/updates main

deb http://ftp.debian.org/debian jessie-backports main

#------------------------------------------------------------------------------#
#                   Nettoie tout l'inutile
#------------------------------------------------------------------------------#

clean http://ftp.debian.org/debian
clean http://deb.debian.org/debian/
clean http://deb.debian.org/debian-security
```
