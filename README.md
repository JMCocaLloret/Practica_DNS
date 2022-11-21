# Practica_DNS
Primer de tot instal·larem el serveis de DNS en el Ubuntu server amb la comanda:

```
sudo apt install bind9
```

Un paquet molt útil per provar i resoldre problemes de DNS és el dnsutils. Molt sovint aquestes eines ja estaran instal·lades, però per comprovar i/o instal·lar dnsutils introduïu el següent:

```
sudo apt install dnsutils
```

### Configuració de zones

En el arxiu `named.conf.local` és on es declaren les zones del nostres domini stucomx.net

```
zone "stucomx.net" {
    type master;
    file "/etc/bind/db.stucomx.net";
};
```

### Configuració zona primaria

Copiarem el ariux db.local per a tenir-lo com a plantilla per crear la nostre zona primaria amb la comanda. Li possarem stucomx.net de nom a l'arxiu.

```
sudo cp /etc/bind/db.local /etc/bind/db.stucomx.net
```

Obrim el fitxer per editar i afegim els registres, s'ha de tenir en compte que depen del que es vulgui fer posarem un tipus de registre o un altre d'aquesta manera.
En el arxiu db.stucomx.net penjat en el repositori, es poden obserbat els registres comentats.

Vaig buscar els registres MX de Google amb les seves respectives prioritats i també els arxius TXT. Adjunto els links on vaig fer la cerca: MX --> https://support.google.com/a/answer/174125?hl=es i TXT: https://support.google.com/a/answer/2716802?hl=es#.

També vaig buscar el registre CAA de LetsEncrpy per poder afegir-lo al domini. (CAA 0 issue "letsencrypt.org")

### Configuració zona inversa

Guardarem i copiarem el fitxer **db.127** canvian el nom a **db.192** amb la comanda `sudo cp /etc/bind/db.127 /etc/bind/db.192`

A continuació crearem la zona inversa, editem el fitxer `named.conf.local` i afegim

```
zone "1.168.192.in-addr-arpa" {
    type master;
    file "/etc/bind/db.192";
};

Copiarem el ariux db.172 per a tenir-lo com a plantilla per crear la nostre zona inversa amb la comanda. Li posarem el primer octet de la nostre IP de xarxa com a nom del arxiu, tot i que no influeix en res.

```
sudo cp /etc/bind/db.172 /etc/bind/db.192
```
Per cadascun dels registre A fet eque he configurat en /etc/bind/db.example.com, és a dir, he creat un registre PTR en /etc/bind/*db.192.

Al finalitzar cada zona he reiniciat el servei amb la comanda.
