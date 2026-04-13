# Étape 3 : Création d’un certificat auto-signé pour Nginx

Objectif : générer une clé privée et un certificat auto-signé pour activer HTTPS dans Nginx.

## Créer un dossier pour le certificat

```bash
sudo mkdir -p /etc/nginx/ssl
sudo chmod 700 /etc/nginx/ssl
```

## Générer la clé privée

```bash
sudo openssl genrsa -out /etc/nginx/ssl/selfsigned.key 2048
```

## Générer le certificat auto-signé

```bash
sudo openssl req -x509 -nodes -days 365 \
  -key /etc/nginx/ssl/selfsigned.key \
  -out /etc/nginx/ssl/selfsigned.crt
```


Lors de l’exécution, OpenSSL vous demandera :

- Country Name (2 letter code)
- State or Province Name
- Locality Name
- Organization Name
- Organizational Unit Name
- Common Name: localhost
- Email Address


## Vérifier le contenu du certificat

```bash
openssl x509 -in /etc/nginx/ssl/selfsigned.crt -noout -text
```

## Configurer Nginx pour HTTPS

Éditez le fichier de configuration :

```bash
sudo nano /etc/nginx/sites-available/default
```

Ajoutez ou remplacez avec ces blocs :

```nginx
server {
    listen 80;
    server_name localhost;

    root /var/www/html;
    index index.html;

    location / {
        add_header Cache-Control "no-store, no-cache, must-revalidate, proxy-revalidate, max-age=0";
        add_header Pragma "no-cache";
        add_header Expires 0;

        try_files $uri $uri/ =200;
    }
}

server {
    listen 443 ssl default_server;
    listen [::]:443 ssl default_server;

    server_name localhost;

    ssl_certificate /etc/nginx/ssl/selfsigned.crt;
    ssl_certificate_key /etc/nginx/ssl/selfsigned.key;
    ssl_protocols TLSv1.2;

    root /var/www/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

## Tester la configuration Nginx

```bash
sudo nginx -t
sudo systemctl reload nginx
```

## Vérification HTTPS

```bash
curl -I https://localhost --insecure
```

Le flag `--insecure` est nécessaire car le certificat est auto-signé.
