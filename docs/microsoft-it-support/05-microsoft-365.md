# Module 05 – Écosystème Microsoft 365

<small>Parcours Microsoft IT Support Specialist · Niveau intermédiaire à avancé · Théorie + laboratoire</small>

## 1. Objectifs

Comprendre les services Microsoft 365, distinguer identité et licences, connaître les rôles d'administration, diagnostiquer des incidents de collaboration et de messagerie et appliquer les principes de sécurité.

## 2. Cartographie des services

| Service | Fonction | Exemples de support |
|---|---|---|
| Microsoft Entra ID | Identités et accès | Connexion, MFA, comptes |
| Exchange Online | Messagerie et calendriers | Flux mail, boîte, délégation |
| SharePoint Online | Sites et contenus | Accès, partage, synchronisation |
| OneDrive | Stockage individuel professionnel | Synchronisation, restauration |
| Microsoft Teams | Collaboration et réunions | Audio, présence, équipes |
| Intune | Gestion des terminaux | Enrôlement, conformité, applications |
| Microsoft Defender | Protection et détection | Alertes, menaces |
| Microsoft Purview | Gouvernance et conformité | Rétention, classification, audit |

Un tenant est l'instance organisationnelle Microsoft 365 / Entra ID. Les fonctionnalités disponibles dépendent des licences, du tenant, des paramètres et des évolutions du service.

## 3. Licences, rôles et gouvernance

Les licences attribuent des droits d'usage ; elles ne remplacent pas les autorisations sur les ressources ni la configuration. Privilégier les rôles intégrés et le moindre privilège, limiter les rôles globaux, utiliser des élévations temporaires avec PIM lorsqu'il est disponible et auditer les accès.

Ne jamais publier le nom d'un tenant professionnel, les domaines internes, les identifiants ou des captures d'écran de consoles réelles.

## 4. Microsoft Entra ID : identité et accès

Concepts clés :
- **Utilisateur** : identité dans l'annuaire.
- **Groupe** : simplifie l'attribution d'accès.
- **Rôle** : permissions d'administration.
- **MFA** : preuve supplémentaire d'authentification.
- **Accès conditionnel** : décision basée sur l'utilisateur, l'application, le contexte et l'état du terminal.
- **PIM** : activation contrôlée et temporaire des rôles privilégiés.

Pour une erreur de connexion, recueillir l'heure, l'application, le type de client, le code d'erreur, l'état MFA et les informations de corrélation. Ne jamais demander le mot de passe ou les codes de validation.

## 5. Exchange Online

Objets courants : boîte utilisateur, boîte partagée, groupe de distribution, groupe Microsoft 365, règles de transport et connecteurs.

Diagnostic d'un message absent :
1. confirmer expéditeur, destinataire, date, sujet et fuseau horaire ;
2. vérifier courrier indésirable, règles et éléments supprimés ;
3. examiner le suivi des messages avec les droits autorisés ;
4. vérifier quarantaine et politiques anti-spam ;
5. contrôler les incidents de service et les enregistrements DNS pertinents ;
6. escalader avec les traces strictement nécessaires.

Full Access, Send As et Send on Behalf sont des permissions différentes. Accorder uniquement les droits justifiés.

## 6. SharePoint, OneDrive et Teams

Un accès SharePoint refusé peut venir des droits du site, de la bibliothèque, du fichier, d'un lien expiré ou d'une stratégie de partage externe. Distinguer l'accès au site des droits sur un élément.

Pour OneDrive, vérifier l'état du client, l'espace disque, la version, les noms et chemins de fichiers, les conflits de synchronisation et l'authentification.

Pour Teams, qualifier le périmètre (un utilisateur, un appareil, un réseau ou un service), vérifier le périphérique audio sélectionné, les autorisations système, le client, le réseau et l'état du service.

## 7. Intune : gestion des terminaux

- **Enrôlement** : association du terminal au service de gestion.
- **Configuration** : paramètres appliqués au terminal.
- **Conformité** : évaluation de critères définis par l'organisation.
- **Protection applicative** : politiques appliquées aux applications, y compris certains scénarios BYOD.
- **Remédiation** : correction des écarts détectés.

Avant une action distante (effacement, retrait, réinitialisation), vérifier identité, propriété du terminal, autorisation, impact et procédure.

## 8. Laboratoire : Microsoft Graph PowerShell

Utiliser un tenant de développement ou de laboratoire autorisé. Les permissions doivent être minimales et consenties explicitement lorsque nécessaire.

Exemple de lecture de l'annuaire :

    Install-Module Microsoft.Graph -Scope CurrentUser
    Connect-MgGraph -Scopes "User.Read.All"
    Get-MgContext
    Get-MgUser -Top 10 -Property DisplayName, UserPrincipalName |
      Select-Object DisplayName, UserPrincipalName
    Disconnect-MgGraph

Ce script lit des données d'annuaire. Ne l'exécutez pas sur un tenant réel sans autorisation et justification. Ne stockez jamais jetons, secrets ou exports contenant des données personnelles dans Git.

## 9. Cas pratique : Teams inaccessible

1. Définir le message d'erreur exact et l'heure.
2. Vérifier si l'accès fonctionne dans un autre client autorisé.
3. Contrôler l'état du service.
4. Vérifier connexion Entra ID, MFA et accès conditionnel.
5. Vérifier licence et état du compte avec les droits appropriés.
6. Examiner client, réseau et journaux pertinents.
7. Documenter la cause et valider avec l'utilisateur.

## 10. Quiz

1. Quelle différence entre Entra ID et Intune ?
2. Une licence donne-t-elle automatiquement tous les droits d'accès ?
3. Quelle différence entre Full Access et Send As ?
4. Que vérifier avant un effacement distant ?
5. Pourquoi limiter les permissions Graph ?

<details><summary>Corrigé</summary>

1. Entra ID gère identité et accès ; Intune gère terminaux et applications. 2. Non, autorisations et configuration restent nécessaires. 3. Full Access permet d'ouvrir et gérer une boîte ; Send As permet d'envoyer comme cette boîte. 4. Identité, propriété, autorisation, impact et procédure. 5. Pour limiter l'exposition et les conséquences d'une compromission.

</details>

## 11. Checklist support Microsoft 365

- [ ] Identifier le service et le périmètre.
- [ ] Recueillir heure, application, message et identifiant de corrélation.
- [ ] Vérifier compte, licence, rôle et autorisations selon habilitation.
- [ ] Examiner l'état du service et les journaux pertinents.
- [ ] Appliquer le moindre privilège.
- [ ] Ne pas exporter ni publier de données réelles sans autorisation.
- [ ] Valider le rétablissement avec l'utilisateur.

## 12. Références

- [Microsoft Learn – Microsoft 365](https://learn.microsoft.com/microsoft-365/)
- [Microsoft Learn – Microsoft Entra](https://learn.microsoft.com/entra/)
- [Microsoft Learn – Exchange Online](https://learn.microsoft.com/exchange/exchange-online)
- [Microsoft Learn – Microsoft Intune](https://learn.microsoft.com/mem/intune/)
- [Microsoft Graph PowerShell](https://learn.microsoft.com/powershell/microsoftgraph/)
