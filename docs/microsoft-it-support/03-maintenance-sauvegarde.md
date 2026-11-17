# Module 03 – Maintenance, logiciels, matériel et sauvegarde des données

<small>Parcours Microsoft IT Support Specialist · Niveau intermédiaire à avancé · Théorie + laboratoire</small>

## 1. Objectifs

Planifier la maintenance d'un poste, gérer les mises à jour et les pilotes, comprendre les types de stockage, définir une stratégie de sauvegarde, tester une restauration et documenter les opérations à risque.

## 2. Maintenance informatique

- **Préventive** : patchs, contrôles d'état, revue des sauvegardes et nettoyage maîtrisé.
- **Corrective** : réparation après une panne ou remplacement d'un composant.
- **Prédictive** : analyse de tendances et de télémétrie pour anticiper une défaillance.

Toute opération en entreprise doit préciser son périmètre, son responsable, sa fenêtre, son plan de retour arrière et ses critères de succès. Respecter le processus de changement et les autorisations.

## 3. Cycle de vie du matériel

1. Réception et contrôle de conformité.
2. Enregistrement de l'actif : modèle, numéro de série, affectation et état.
3. Préparation : firmware, système, chiffrement, agent de gestion et mises à jour.
4. Affectation et support.
5. Maintenance, réparation ou remplacement.
6. Retrait : révocation des accès, effacement selon politique, inventaire et recyclage.

Contrôles utiles : ventilation, température, état physique, alimentation, batterie, stockage, journaux système et compatibilité. Respecter les consignes électriques et antistatiques. Ne jamais ouvrir un équipement sous tension.

## 4. Maintenance logicielle et patch management

Les mises à jour corrigent des vulnérabilités et des bugs, mais peuvent aussi introduire des incompatibilités. Une stratégie fiable utilise des anneaux de déploiement, un groupe pilote, une fenêtre approuvée et une validation après installation.

| Étape | Contrôle |
|---|---|
| Préparer | Inventaire, dépendances, sauvegarde, communication |
| Piloter | Groupe restreint et tests métier |
| Déployer | Anneaux progressifs et fenêtre approuvée |
| Vérifier | Conformité, erreurs, redémarrages |
| Remédier | Correction ou retour arrière documenté |

Distinguer mises à jour de qualité, de fonctionnalités, pilotes et firmware. Éviter les pilotes tiers non vérifiés.

### Commandes PowerShell d'observation

    Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsBuildNumber
    Get-Service | Where-Object Status -eq 'Running'
    Get-Volume | Select-Object DriveLetter, FileSystemLabel, Size, SizeRemaining
    Get-WinEvent -LogName System -MaxEvents 30 |
      Select-Object TimeCreated, Id, LevelDisplayName, ProviderName

Ces commandes sont en lecture seule. Les réparations doivent suivre les procédures de l'organisation.

## 5. Technologies de stockage

| Type | Description | Usage courant |
|---|---|---|
| DAS | Stockage directement attaché | Disque local |
| NAS | Stockage de fichiers sur réseau | Partage de fichiers |
| SAN | Réseau de stockage en blocs | Datastores et serveurs |
| Objet | Données adressées par objet et métadonnées | Archivage, stockage cloud |

RAID peut améliorer disponibilité ou performances selon le niveau, mais ne remplace jamais une sauvegarde : suppression, corruption ou chiffrement malveillant peuvent affecter tout le volume.

## 6. Sauvegarde et restauration

La règle 3-2-1 est un point de départ : trois copies, deux types de supports, une copie hors site. Renforcer avec une copie immuable ou hors ligne, des comptes dédiés, MFA et des tests réguliers.

- **Complète** : copie de l'ensemble du périmètre.
- **Incrémentielle** : changements depuis la sauvegarde précédente.
- **Différentielle** : changements depuis la dernière sauvegarde complète.

Les modalités exactes dépendent du produit et de la chaîne de sauvegarde.

**RPO** (Recovery Point Objective) : perte de données maximale acceptable, exprimée en durée. **RTO** (Recovery Time Objective) : délai cible de rétablissement du service. Ces objectifs doivent être validés avec le métier.

## 7. Chiffrement et accès

Le chiffrement protège les données au repos en cas de perte ou de vol du support. BitLocker protège les volumes Windows. Les clés de récupération doivent être stockées dans un emplacement approuvé, avec accès fortement contrôlé. Ne jamais les intégrer à un dépôt, un ticket public ou une capture d'écran.

Limiter l'accès aux sauvegardes, séparer les comptes d'administration et de sauvegarde, activer MFA et journalisation, et chiffrer les données en transit et au repos selon la politique.

## 8. Laboratoire : contrôler et vérifier une restauration

Dans une VM ou un environnement personnel de test :
1. Relever les volumes et l'espace disponible avec Get-Volume.
2. Créer quelques fichiers fictifs dans un dossier de test.
3. Copier ce dossier vers un emplacement distinct.
4. Comparer l'empreinte du fichier original et du fichier restauré.

    Get-FileHash .\document-test.txt -Algorithm SHA256

Une empreinte identique confirme que le contenu comparé est identique ; elle ne prouve pas à elle seule que toute la stratégie de sauvegarde est fiable. Ne testez jamais une suppression sur des données de production.

## 9. Cas pratique : demande de restauration

1. Confirmer l'identité du demandeur, le périmètre et l'autorisation.
2. Identifier les fichiers et le point de restauration souhaité.
3. Évaluer le risque d'écrasement.
4. Restaurer vers un emplacement alternatif lorsque possible.
5. Contrôler intégrité, droits et accessibilité.
6. Faire valider par le demandeur et documenter le résultat.

## 10. Quiz

1. Pourquoi RAID ne remplace-t-il pas une sauvegarde ?
2. Quelle différence entre RPO et RTO ?
3. Que protège BitLocker ?
4. Pourquoi tester une restauration ?
5. Quel est l'intérêt d'une copie immuable ou hors ligne ?

<details><summary>Corrigé</summary>

1. RAID ne protège pas de toutes les suppressions, corruptions ou attaques logiques. 2. RPO = perte tolérée ; RTO = délai de reprise. 3. Les données du volume au repos. 4. Une sauvegarde non restaurable n'assure pas la reprise. 5. Réduire le risque qu'une compromission altère toutes les copies.

</details>

## 11. Checklist

- [ ] Inventaire matériel et logiciel à jour.
- [ ] Mises à jour testées et déployées par étapes.
- [ ] État du stockage surveillé.
- [ ] RPO et RTO définis avec le métier.
- [ ] Copies protégées, isolées et contrôlées.
- [ ] Tests de restauration documentés.
- [ ] Clés de récupération stockées de manière sécurisée.

## 12. Références

- [CISA – Stop Ransomware](https://www.cisa.gov/stopransomware)
- [Microsoft Learn – BitLocker](https://learn.microsoft.com/windows/security/operating-system-security/data-protection/bitlocker/)
- [Microsoft Learn – Windows Update](https://learn.microsoft.com/windows/deployment/update/)
- [NIST SP 800-34 – Contingency Planning](https://csrc.nist.gov/pubs/sp/800/34/r1/upd1/final)
