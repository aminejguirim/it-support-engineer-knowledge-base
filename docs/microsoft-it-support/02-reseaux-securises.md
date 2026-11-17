# Module 02 – Introduction aux réseaux sécurisés

<small>Parcours Microsoft IT Support Specialist · Niveau intermédiaire à avancé · Théorie + laboratoire</small>

## 1. Objectifs

À la fin du module, vous saurez lire une configuration IP, expliquer DNS et DHCP, distinguer commutation et routage, comprendre les VLAN et effectuer un premier diagnostic de connectivité sous Windows.

## 2. Modèles réseau

Le modèle OSI décompose les communications en sept couches. Il sert surtout de grille de diagnostic.

| Couche | Fonction | Exemples |
|---|---|---|
| 7 Application | Services utilisés par les applications | HTTP, DNS, SMTP |
| 6 Présentation | Encodage et chiffrement | TLS, formats |
| 5 Session | Gestion des échanges | Sessions applicatives |
| 4 Transport | Communication de bout en bout | TCP, UDP |
| 3 Réseau | Adressage et routage | IPv4, IPv6, ICMP |
| 2 Liaison | Trames et adresses MAC | Ethernet, Wi-Fi |
| 1 Physique | Transmission du signal | Cuivre, fibre, radio |

Une panne peut se situer à plusieurs couches : câble débranché, mauvaise adresse IP, résolution DNS impossible, port filtré ou application indisponible.

## 3. Équipements et topologies

- **Switch** : commute les trames dans un réseau local à partir des adresses MAC.
- **Routeur** : achemine les paquets entre réseaux IP.
- **Point d'accès** : connecte les clients Wi-Fi au réseau.
- **Pare-feu** : applique des règles de filtrage aux flux.
- **Modem / ONT** : termine la liaison d'accès opérateur.

Le Wi-Fi invité, les terminaux IoT et les postes d'entreprise doivent être segmentés selon leur niveau de confiance. Privilégier WPA2-AES ou WPA3 ; les protocoles obsolètes sont à proscrire.

## 4. Adressage IPv4 et sous-réseaux

Une adresse IPv4 contient 32 bits. Le préfixe CIDR définit la partie réseau. Exemple : 192.168.10.25/24 appartient au réseau 192.168.10.0/24, avec le masque 255.255.255.0.

| Paramètre | Exemple | Fonction |
|---|---|---|
| Adresse IP | 192.168.10.25 | Identifie l'interface |
| Préfixe | /24 | Délimite le réseau |
| Passerelle | 192.168.10.1 | Accès aux autres réseaux |
| DNS | 192.168.10.10 | Résolution des noms |
| DHCP | Serveur ou relais | Attribution de configuration |

Plages privées IPv4 : 10.0.0.0/8, 172.16.0.0/12 et 192.168.0.0/16. Elles ne sont pas routées directement sur Internet.

## 5. DHCP et DNS

**DHCP** attribue automatiquement les paramètres réseau. Le processus DORA signifie Discover, Offer, Request, Acknowledge. Une réservation associe une adresse à un client ; une exclusion empêche sa distribution dynamique.

**DNS** traduit les noms en informations, notamment des adresses IP. Enregistrements courants :
- A / AAAA : nom vers adresse IPv4 / IPv6.
- CNAME : alias vers un autre nom.
- MX : serveurs de messagerie.
- TXT : métadonnées, SPF ou validation de domaine.
- PTR : résolution inverse.

Une panne DNS peut rendre un service inaccessible par son nom alors que la connectivité IP fonctionne.

## 6. TCP, UDP, ports et routage

TCP fournit un transport fiable et ordonné ; UDP est plus léger, sans garantie de livraison. Les ports identifient les services au niveau transport.

| Port | Service | Point de vigilance |
|---:|---|---|
| 22/TCP | SSH | Administration à restreindre |
| 53/TCP/UDP | DNS | Requêtes et transferts selon le cas |
| 67/68 UDP | DHCPv4 | Serveur / client |
| 80/TCP | HTTP | Préférer HTTPS |
| 123/UDP | NTP | Synchronisation horaire |
| 443/TCP | HTTPS | Web chiffré |
| 3389/TCP | RDP | Ne pas exposer directement sur Internet |

Le routage choisit le prochain saut selon la table de routage. La route par défaut est utilisée lorsqu'aucune route plus spécifique ne correspond.

## 7. VLAN et segmentation

Un VLAN sépare logiquement des domaines de diffusion. Des segments distincts peuvent être prévus pour les utilisateurs, serveurs, équipements réseau, IoT et invités. Les flux inter-VLAN doivent être autorisés explicitement selon le besoin.

Bonnes pratiques : documenter les VLAN et sous-réseaux, limiter les flux, sécuriser l'administration des équipements, désactiver les ports inutilisés selon la politique et surveiller les changements. Un VLAN seul n'est pas une frontière de sécurité suffisante.

## 8. Laboratoire Windows

À exécuter sur une machine de test ou une VM autorisée :

    ipconfig /all
    route print
    ipconfig /displaydns
    Resolve-DnsName example.com
    tracert example.com
    Test-NetConnection example.com -Port 443

Ordre de diagnostic conseillé : vérifier l'interface et l'adresse IP, tester la passerelle, tester une destination IP, vérifier DNS, puis tester le port applicatif. Collecter les résultats avant de vider les caches ou modifier la configuration.

Avec Wireshark, une capture DNS peut être filtrée avec dns et le trafic TCP 443 avec tcp.port == 443. Ne jamais publier une capture issue d'un réseau réel sans autorisation et anonymisation.

## 9. Cas pratique : « Internet ne fonctionne plus »

1. Déterminer si le problème touche un poste, un segment ou un site.
2. Vérifier le lien Ethernet/Wi-Fi et l'état de l'interface.
3. Examiner IP, masque, passerelle et DNS.
4. Repérer une adresse 169.254.x.x, souvent liée à l'absence de bail DHCPv4.
5. Tester la passerelle, puis une destination IP.
6. Tester la résolution DNS et le port requis.
7. Corréler l'heure avec les changements et alertes réseau.
8. Documenter l'impact, les tests et les actions.

## 10. Quiz

1. Quel service distribue une configuration IP ?
2. Quelle différence entre DNS et DHCP ?
3. À quoi sert la passerelle par défaut ?
4. Quel est l'intérêt d'un VLAN ?
5. Que vérifie Test-NetConnection avec le port 443 ?
6. Pourquoi 169.254.x.x est-il un indice à examiner ?

<details><summary>Corrigé</summary>

1. DHCP. 2. DNS résout les noms ; DHCP distribue la configuration IP. 3. À joindre les réseaux distants. 4. À segmenter logiquement le réseau et contrôler les flux. 5. La connectivité TCP vers ce port. 6. C'est une adresse link-local fréquemment auto-attribuée lorsque le client ne reçoit pas de bail DHCPv4.

</details>

## 11. Checklist

- [ ] Qualifier le périmètre et l'impact.
- [ ] Relever IP, masque, passerelle et DNS.
- [ ] Tester séparément connectivité IP, DNS et ports.
- [ ] Vérifier DHCP, VLAN, routage et filtrage avec les droits requis.
- [ ] Conserver les preuves utiles sans collecter de données excessives.
- [ ] Documenter les résultats et la résolution.

## 12. Références

- [RFC 1918 – Private IPv4](https://www.rfc-editor.org/rfc/rfc1918)
- [RFC 2131 – DHCP](https://www.rfc-editor.org/rfc/rfc2131)
- [RFC 1034 – DNS concepts](https://www.rfc-editor.org/rfc/rfc1034)
- [Microsoft Learn – Networking](https://learn.microsoft.com/windows-server/networking/)
- [Wireshark User Guide](https://www.wireshark.org/docs/wsug_html_chunked/)
