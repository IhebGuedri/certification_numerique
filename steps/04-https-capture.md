# Étape 4 : Capturer l’échange HTTPS et le certificat avec Wireshark

Objectif : observer l’établissement de la session TLS et l’échange du certificat.

## Préparer Wireshark

1. Ouvrez Wireshark.
2. Sélectionnez l’interface locale.
3. Ajoutez un filtre d’affichage : `tls`ou `tcp.port == 443`.
4. Lancez la capture.

## Tester HTTPS

```bash
curl -I https://localhost --insecure
```

## Éléments à observer

1. ClientHello : début de la négociation TLS.
2. ServerHello : réponse du serveur.
3. Certificate : envoi du certificat du serveur.
4. ServerHelloDone.
5. ClientKeyExchange et autres paquets de chiffrement.

## Capturer le certificat

Dans Wireshark, utilisez le filtre `tls.handshake.type == 11` ou regardez le paquet `Certificate`.

## Notes

- Le contenu HTTP est chiffré, donc vous ne verrez pas le corps de la requête ou de la réponse en clair.
- Vous verrez en clair l’échange de certificats et les négociations TLS.
