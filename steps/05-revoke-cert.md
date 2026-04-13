# Étape 5 : Révocation du certificat

Objectif : révoquer le certificat auto-signé et nettoyer la configuration.

## Suppression du certificat et de la clé

```bash
sudo rm -f /etc/nginx/ssl/selfsigned.crt /etc/nginx/ssl/selfsigned.key
```

## Revenir à une configuration HTTP simple

Ouvrez `/etc/nginx/sites-available/default` et retirez ou commentez le bloc `server` sur le port 443.

Une configuration minimale HTTP :

```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name localhost;
    root /var/www/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

## Recharger Nginx

```bash
sudo nginx -t
sudo systemctl reload nginx
```

## Vérification

```bash
curl -I http://localhost
```

## Note sur la révocation réelle

Pour un certificat auto-signé local, la révocation consiste surtout à supprimer la clé et le certificat. Dans un environnement PKI avec une autorité de certification, on utiliserait une liste de révocation CRL ou OCSP.
