# Module 06 – Diagnostic technique et dépannage

<small>Parcours Microsoft IT Support Specialist · Niveau intermédiaire à avancé · Théorie + laboratoire</small>

## 1. Objectifs

Appliquer une méthode de diagnostic reproductible, qualifier un incident, collecter des preuves pertinentes, isoler une cause probable, rétablir le service, valider le résultat et documenter les actions.

## 2. Incident, demande et problème

- **Incident** : interruption ou dégradation non planifiée d'un service.
- **Demande de service** : demande standard d'accès, d'information ou d'installation autorisée.
- **Problème** : cause réelle ou potentielle d'un ou plusieurs incidents.
- **Changement** : ajout, modification ou retrait susceptible d'affecter un service.

La priorité combine impact et urgence selon la matrice de l'organisation. Un incident de sécurité suit aussi le processus de réponse à incident prévu.

## 3. Méthode structurée

1. **Qualifier** : utilisateur, service, terminal, heure et impact.
2. **Observer** : distinguer symptôme, fréquence et conditions.
3. **Formuler** des hypothèses vérifiables.
4. **Tester** en commençant par les vérifications les moins intrusives.
5. **Corriger** avec une action autorisée, limitée et réversible.
6. **Valider** avec des critères concrets.
7. **Clôturer** en documentant cause, action, résultat et prévention.

Éviter plusieurs changements simultanés : ils rendent difficile l'identification de la cause et peuvent aggraver l'incident.

## 4. Collecte de preuves

Collecter uniquement les éléments utiles :
- date, heure et fuseau horaire ;
- message d'erreur et code exacts ;
- périmètre : un utilisateur, un site, un service ;
- changements récents connus ;
- versions et état des composants ;
- journaux pertinents et identifiants de corrélation ;
- tests réalisés et résultats.

Ne pas collecter de mots de passe, tokens, clés de récupération, contenus privés ou données non nécessaires. Masquer les identifiants personnels dans les exemples publiés.

## 5. Outils Windows

| Outil | Usage |
|---|---|
| Gestionnaire des tâches | Processus, CPU, mémoire, démarrage |
| Moniteur de ressources | Détail des ressources |
| Observateur d'événements | Journaux système et application |
| Moniteur de fiabilité | Historique de stabilité |
| Gestionnaire de périphériques | État matériel et pilotes |
| Services | État des services Windows |
| PowerShell | Inventaire et tests reproductibles |
| ipconfig, ping, tracert | Configuration et connectivité |
| Test-NetConnection | Test TCP et diagnostic réseau |

### Commandes d'observation

    Get-Process | Sort-Object CPU -Descending |
      Select-Object -First 10 Name, Id, CPU

    Get-Service | Where-Object Status -eq 'Stopped'

    Get-WinEvent -FilterHashtable @{
      LogName = 'System'
      Level = 1,2
      StartTime = (Get-Date).AddDays(-2)
    } -MaxEvents 50

    Get-NetIPConfiguration
    Test-NetConnection -ComputerName example.com -Port 443

L'absence d'événement critique ne prouve pas l'absence de panne : les journaux peuvent être incomplets ou non activés.

## 6. Diagnostic par symptôme

| Symptôme | Contrôles initiaux |
|---|---|
| Poste ne démarre pas | Alimentation, écran, voyants, périphériques |
| Session impossible | Réseau, état du compte, MFA, heure, identité |
| Application lente | CPU, RAM, disque, processus, réseau, service |
| Site inaccessible | DNS, proxy, certificat, port, filtrage |
| Impression impossible | File, connectivité, pilote, serveur |
| Fichier inaccessible | Chemin, réseau, droits, synchronisation |
| Batterie faible | Santé batterie, chargeur, consommation |

Distinguer panne locale et panne de service partagé. Une comparaison avec un utilisateur ou terminal témoin autorisé peut aider à isoler le problème.

## 7. Escalade N1 vers N2/N3

Une escalade utile contient :
- résumé et impact ;
- nombre d'utilisateurs ou services affectés ;
- heure de début et fréquence ;
- tests réalisés et résultats ;
- hypothèses éliminées et cause suspectée ;
- pièces jointes expurgées et utiles ;
- actions réalisées et risques résiduels.

Une escalade doit permettre au niveau suivant de poursuivre sans recommencer toute la collecte.

## 8. Laboratoire : application inaccessible

Dans une VM ou un environnement de test :
1. vérifier la configuration avec Get-NetIPConfiguration ;
2. résoudre le nom avec Resolve-DnsName ;
3. examiner la route avec tracert ;
4. tester le port avec Test-NetConnection ;
5. consulter les événements applicatifs pertinents ;
6. consigner chaque résultat sans changer plusieurs paramètres à la fois.

Exemple d'interprétation :

| Test | Résultat | Conclusion provisoire |
|---|---|---|
| DNS | Adresse retournée | Résolution fonctionnelle à cet instant |
| TCP | Échec | Port, route, filtrage ou service à examiner |
| Poste témoin | Réussite | Piste locale ou liée au compte |

Ces indices ne prouvent pas seuls la cause racine.

## 9. Modèle de compte rendu

    Titre :
    Service / équipement concerné :
    Date et heure (avec fuseau) :
    Impact et urgence :
    Symptôme et message exact :
    Périmètre :
    Changements récents :
    Tests et résultats :
    Hypothèses :
    Cause identifiée (ou inconnue) :
    Actions réalisées :
    Validation du rétablissement :
    Escalade / suivi :
    Prévention :

Ne publiez jamais un ticket réel, des coordonnées, des noms de clients, domaines internes, adresses IP d'entreprise, captures de consoles, secrets ou identifiants. Utilisez des scénarios fictifs ou anonymisés.

## 10. Cas pratique : lenteur après une mise à jour

1. Vérifier si d'autres postes sont concernés.
2. Recueillir l'heure, la version et le symptôme précis.
3. Observer CPU, mémoire, disque et processus.
4. Consulter événements et historique de fiabilité.
5. Vérifier services et connectivité.
6. Comparer avec un poste témoin.
7. Escalader ou appliquer une correction approuvée.
8. Valider les performances et documenter.

Ne pas désinstaller une mise à jour de sécurité sans évaluer le risque et valider le retour arrière.

## 11. Quiz

1. Quelle différence entre incident et problème ?
2. Pourquoi tester une hypothèse à la fois ?
3. Quelles informations rendent une escalade exploitable ?
4. Un ping réussi prouve-t-il qu'une application fonctionne ?
5. Pourquoi valider la résolution avec l'utilisateur ?

<details><summary>Corrigé</summary>

1. L'incident est une interruption ou dégradation ; le problème concerne sa cause réelle ou potentielle. 2. Pour isoler l'effet de chaque action. 3. Impact, chronologie, symptômes, tests, résultats, hypothèses et actions. 4. Non, ICMP peut répondre alors que le port ou le service applicatif est indisponible. 5. Pour confirmer le besoin réel et éviter une clôture prématurée.

</details>

## 12. Checklist de clôture

- [ ] Impact et périmètre qualifiés.
- [ ] Tests et preuves consignés.
- [ ] Cause identifiée ou limites du diagnostic explicitées.
- [ ] Correction autorisée et réversible si possible.
- [ ] Rétablissement validé techniquement et par l'utilisateur.
- [ ] Documentation expurgée des données sensibles.
- [ ] Escalade ou action préventive créée si nécessaire.

## 13. Références

- [Microsoft Learn – Troubleshoot Windows client](https://learn.microsoft.com/troubleshoot/windows-client/)
- [Microsoft Learn – Windows](https://learn.microsoft.com/windows/)
- [PeopleCert – ITIL](https://www.peoplecert.org/)
- [NIST SP 800-61 – Incident Handling](https://csrc.nist.gov/pubs/sp/800/61/r2/final)
