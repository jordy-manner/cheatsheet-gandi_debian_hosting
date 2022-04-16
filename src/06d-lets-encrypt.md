# 06d - Installation et configuration de Let's encrypt

## Installation

```bash
sudo apt update && sudo apt install certbot python3-certbot-apache
```

## Génération d'un certificat auto-renouvelé

### Obtention du certificat

```bash
sudo certbot certonly --webroot -w ${SERVER_DATAS_PATH}/var/www/html/000-default/htdocs \
--agree-tos --no-eff-email --rsa-key-size 4096 \
-d {{ DOMAIN.LTD }} -d {{ WWW.DOMAIN.LTD }}
``` 

### Configuration de l'hôte virtuel

```bash
sudo vi /etc/apache2/sites-enabled/000-default.conf
```

```diff
<VirtualHost *:443>            
+    SSLEngine               on
+    SSLCertificateFile      /etc/letsencrypt/live/{{ DOMAIN.LTD }}/cert.pem
+    SSLCertificateKeyFile   /etc/letsencrypt/live/{{ DOMAIN.LTD }}/privkey.pem
+    SSLCertificateChainFile /etc/letsencrypt/live/{{ DOMAIN.LTD }}/chain.pem

+    # optionnels
+    #SSLProtocol             all -SSLv2 -SSLv3
+    #SSLCipherSuite          ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA
+    #SSLHonorCipherOrder     on
+    #SSLCompression          off
+    #SSLOptions              +StrictRequire
</VirtualHost>
```