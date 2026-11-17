# Module 04 – Cybersécurité et vie privée

<small>Parcours Microsoft IT Support Specialist · Niveau intermédiaire à avancé · Théorie + laboratoire</small>

## 1. Objectifs

Comprendre les objectifs de sécurité, reconnaître les menaces, appliquer le moindre privilège et la défense en profondeur, sécuriser identités et terminaux et réagir à un incident suspect.

## 2. Triade CIA et gestion du risque

- **Confidentialité** : seules les personnes autorisées accèdent à l'information.
- **Intégrité** : l'information reste exacte et n'est modifiée que de manière autorisée.
- **Disponibilité** : systèmes et données sont accessibles lorsque nécessaire.

Le risque dépend notamment d'une menace, d'une vulnérabilité, de la probabilité et de l'impact. L'évaluation doit tenir compte du contexte métier, des dépendances et des mesures existantes. Documenter les hypothèses et faire valider les niveaux de risque.

## 3. Menaces courantes

| Menace | Exemple | Réduction du risque |
|---|---|---|
| Phishing | Faux portail de connexion | MFA robuste, filtrage, signalement |
| Ransomware | Chiffrement et extorsion | EDR, segmentation, sauvegardes isolées |
| Vol d'identifiants | Réutilisation de mot de passe | MFA, gestionnaire, détection |
| Exploitation de faille | Service non corrigé | Inventaire et patch management |
| Menace interne | Usage abusif de privilèges | Moindre privilège et audit |
| Perte d'équipement | Ordinateur oublié | Chiffrement et gestion à distance |

L'ingénierie sociale joue sur l'urgence et l'autorité. Vérifier une demande inhabituelle via un canal indépendant, jamais avec les coordonnées fournies dans le message suspect.

## 4. Défense en profondeur et Zero Trust

La défense en profondeur associe des contrôles sur l'identité, le terminal, le réseau, l'application, les données et la supervision. Zero Trust repose sur la vérification explicite, le moindre privilège et le principe de considérer qu'une compromission est possible.

Contrôles complémentaires : MFA, accès conditionnel, conformité des terminaux, EDR, segmentation, filtrage, chiffrement, journalisation et détection. Aucun contrôle isolé ne garantit la sécurité.

## 5. Identité et accès

- **Authentification** : prouver son identité.
- **Autorisation** : déterminer les actions permises.
- **Audit** : tracer les actions pertinentes.
- **MFA** : combiner plusieurs facteurs distincts.
- **RBAC** : attribuer les droits selon les rôles.
- **PAM / PIM** : encadrer et élever les privilèges.

Privilégier les méthodes MFA résistantes au phishing lorsque disponibles, comme FIDO2/passkeys. Une notification MFA non sollicitée doit être refusée et signalée. Séparer les comptes d'administration des comptes bureautiques.

## 6. Sécurité des postes et serveurs

Mesures de base :
- système et applications supportés et corrigés ;
- antivirus/EDR actif et télémétrie remontée ;
- chiffrement du disque ;
- pare-feu local activé selon la politique ;
- Secure Boot et protections matérielles disponibles ;
- services et logiciels inutiles supprimés ;
- comptes administrateurs locaux maîtrisés ;
- verrouillage automatique ;
- journaux centralisés et horodatage fiable.

La conformité doit être mesurée, les exceptions justifiées et les écarts remédiés.

## 7. Vie privée et données personnelles

Appliquer la minimisation : collecter et partager uniquement les données nécessaires à une finalité légitime. Classifier les informations, limiter les durées de conservation et utiliser des canaux approuvés.

Le RGPD prévoit notamment les principes de licéité, transparence, minimisation, exactitude, limitation de conservation, intégrité et confidentialité. En cas de doute sur une donnée personnelle ou une violation potentielle, suivre le processus interne et alerter rapidement le DPO ou le responsable désigné. Ne pas conduire seul une notification réglementaire.

## 8. Laboratoire : examiner les protections Windows

Sur une machine personnelle ou une VM de test, avec les droits nécessaires :

    Get-MpComputerStatus |
      Select-Object AntivirusEnabled, RealTimeProtectionEnabled, AntivirusSignatureLastUpdated

    Get-NetFirewallProfile |
      Select-Object Name, Enabled, DefaultInboundAction, DefaultOutboundAction

    manage-bde -status

    Get-LocalUser | Select-Object Name, Enabled, LastLogon

Ces commandes servent à observer l'état. Les paramètres effectifs peuvent être imposés par une stratégie centrale. Ne désactivez pas antivirus, pare-feu ou chiffrement sur un poste de production.

## 9. Réagir à un incident suspect

Si un utilisateur signale un lien malveillant, une demande MFA inattendue ou un fichier suspect :
1. Recueillir les faits sans blâmer l'utilisateur.
2. Noter l'heure, le terminal, le compte et les actions déjà réalisées.
3. Appliquer le processus de signalement et d'escalade.
4. Isoler le terminal uniquement selon la procédure et les habilitations.
5. Préserver les preuves ; ne pas supprimer ou réinitialiser au hasard.
6. Faire révoquer les sessions ou identifiants par l'équipe autorisée si nécessaire.
7. Documenter les décisions et communiquer selon le plan d'incident.

## 10. Cas pratique : demande MFA inattendue

L'utilisateur reçoit plusieurs demandes MFA qu'il n'a pas initiées. Il doit les refuser, ne communiquer aucun code, signaler immédiatement l'événement et contacter le support via un canal connu. L'équipe identité examine les journaux, évalue les sessions et applique les mesures prévues. Ne jamais demander le mot de passe ou le code MFA.

## 11. Quiz

1. Que signifie CIA ?
2. Quelle différence entre authentification et autorisation ?
3. Pourquoi le moindre privilège réduit-il le risque ?
4. Que faire face à une demande MFA non sollicitée ?
5. Pourquoi isoler les sauvegardes des comptes de production ?

<details><summary>Corrigé</summary>

1. Confidentialité, intégrité, disponibilité. 2. L'une vérifie l'identité, l'autre les droits. 3. Elle limite les actions après erreur ou compromission. 4. Refuser, ne pas valider et signaler par un canal connu. 5. Pour éviter qu'une compromission de production détruise aussi les sauvegardes.

</details>

## 12. Checklist

- [ ] MFA activée et méthodes robustes privilégiées.
- [ ] Comptes privilégiés séparés et contrôlés.
- [ ] Terminaux chiffrés, protégés et à jour.
- [ ] Pare-feu et EDR actifs.
- [ ] Données partagées au minimum nécessaire.
- [ ] Incidents signalés rapidement.
- [ ] Preuves et informations sensibles préservées.

## 13. Références

- [ANSSI – Guide d'hygiène informatique](https://cyber.gouv.fr/publications/guide-dhygiene-informatique)
- [CNIL – Principes clés du RGPD](https://www.cnil.fr/fr/les-principes-cles-du-rgpd)
- [NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework)
- [Microsoft Learn – Zero Trust](https://learn.microsoft.com/security/zero-trust/)
