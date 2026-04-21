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
# HTTP
server {
    listen 80 default_server;
    server_name localhost;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location /login {
        return 200 "HTTP (NOT SECURE) - credentials sent in clear text";
    }
}

# HTTPS
server {
    listen 443 ssl default_server;
    server_name localhost;

    ssl_certificate     /etc/nginx/ssl/selfsigned.crt;
    ssl_certificate_key /etc/nginx/ssl/selfsigned.key;
    ssl_protocols       TLSv1.2;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location /login {
        return 200 "HTTPS (SECURE) - data is encrypted with TLS";
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
