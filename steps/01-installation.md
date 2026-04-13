# Étape 1 : Installation des outils sur Ubuntu

Objectif : installer Nginx, OpenSSL et Wireshark pour préparer la configuration du certificat auto-signé et capturer le trafic.

## Commandes d’installation

```bash
sudo apt update
sudo apt install -y nginx openssl wireshark curl
```

> Vous pouvez aussi utiliser `sudo apt-get install -y nginx openssl wireshark curl` si vous préférez la variante `apt-get`.

## Rôles des outils installés

- `nginx` : serveur web qui permet de servir du contenu HTTP et HTTPS.
- `openssl` : génération de la clé privée et du certificat auto-signé.
- `wireshark` : capture et analyse de paquets réseau pour observer HTTP et TLS.
- `curl` : test des connexions HTTP/HTTPS depuis la ligne de commande.

> Pendant l’installation de Wireshark, acceptez l’option pour les captures en mode non-root si vous souhaitez lancer Wireshark sans `sudo`.

## Vérification

```bash
nginx -v
openssl version
wireshark --version
```

## Démarrage de Nginx

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
sudo systemctl status nginx
```

## Remarque

Nous commençons d’abord par un point HTTP pour observer l’échange sans chiffrement.
