# Étape 2 : Tester l’échange HTTP sans certificat

Objectif : vérifier que Nginx répond bien en HTTP avant de configurer SSL.

## Vérifier la page par défaut

```bash
curl -I http://localhost
```

Vous devez voir une réponse HTTP 200 OK.

## Configuration Nginx pour le test HTTP

Dans `sudo nano /etc/nginx/sites-available/default`, utilisez ce bloc :

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
```


## Démarrage de Nginx

```bash
sudo nginx -t
sudo systemctl enable nginx
sudo systemctl start nginx
sudo systemctl status nginx
```

## Capturer l’échange HTTP avec Wireshark

1. Lancez Wireshark.
2. Sélectionnez l’interface réseau locale (par exemple `lo` pour localhost ou l’interface du PC).
3. Ajoutez un filtre de capture ou d’affichage : `http` ou `tcp.port == 80`.
4. Lancez la capture.
5. Dans un terminal, exécutez :

```bash
curl http://localhost
```

6. Arrêtez la capture et observez les paquets HTTP, les requêtes `GET /` et les en-têtes de réponse.

## Objectif

Confirmer que l’échange HTTP est visible en clair et que Nginx fonctionne correctement avant de passer à HTTPS.
