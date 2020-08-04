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
