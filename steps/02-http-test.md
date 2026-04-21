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

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location /login {
        return 200 "HTTP (NOT SECURE) - credentials sent in clear text";
    }
}
```

## Créer la première page `index.html`

Dans `sudo nano /usr/share/nginx/html/index.html`, collez ce contenu :

```html
<!DOCTYPE html>
<html>
<head>
    <title>Login Demo</title>
</head>
<body>
    <h2>Login Form</h2>

    <form action="/login" method="POST">
        <label>Username:</label>
        <input type="text" name="username"><br><br>

        <label>Password:</label>
        <input type="password" name="password"><br><br>

        <button type="submit">Login</button>
    </form>
</body>
</html>
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
