# Rapport – Rooting et Sécurité Android

Date / Auteur : Saad Makkouch
Support : AVD – Pixel 6 Android 16.0 (x86_64)
Données : Fictives
Réseau : Environnement de test isolé

## Verification
<img width="399" height="128" alt="image" src="https://github.com/user-attachments/assets/f2291ed8-caf4-43f2-9823-b5a50e08df57" />

<img width="436" height="331" alt="image" src="https://github.com/user-attachments/assets/3141a293-ab58-4b67-89f2-8f915757029b" />

<img width="602" height="230" alt="image" src="https://github.com/user-attachments/assets/d1f20c0a-d61f-4d49-be0e-45c56c3a7f91" />

<img width="600" height="250" alt="image" src="https://github.com/user-attachments/assets/a428403c-ce6a-47f6-bfa8-2a584f87307c" />

<img width="426" height="327" alt="image" src="https://github.com/user-attachments/assets/f0082e27-529d-4f18-846b-2aa094c8d283" />



## Objectif du rapport

Ce rapport a pour objectif de comprendre le principe du rooting sur Android et d’analyser ses impacts sur la sécurité du système dans un cadre de laboratoire contrôlé.

## 1. Sécurité Android (Étape 6)

La sécurité Android repose sur plusieurs couches, comparables aux couches d’un oignon.
Le sandboxing isole chaque application afin qu’elle ne puisse pas accéder aux données des autres.
Le modèle de permissions impose une validation explicite avant l’accès aux ressources sensibles.
L’intégrité globale du système protège contre les modifications non autorisées du système.
Ces mécanismes réduisent l’impact d’une compromission.
Le rooting affaiblit directement ces protections.

## 2. Verified Boot (Étape 7)

Objectif principal :
Garantir que le système qui démarre est bien celui prévu par le fabricant, sans modifications malveillantes.

Chain of trust :
Suite de vérifications où chaque composant valide l’authenticité du suivant avant de lui faire confiance.
Chaque maillon protège le suivant contre les altérations.

Pourquoi c’est critique :
Si le démarrage est compromis, toutes les protections ultérieures peuvent être contournées.
<img width="607" height="85" alt="image" src="https://github.com/user-attachments/assets/9cdd9acb-dd34-4a9f-9aeb-54a5e66357bd" />


📸 Screenshot à insérer ici :
Résultat de la commande :
getprop ro.boot.verifiedbootstate

## 3. AVB – Android Verified Boot (Étape 8)

AVB est la version moderne de Verified Boot, plus flexible et plus robuste.
Il ajoute une vérification d’intégrité cryptographique et une protection contre le rollback.
La protection anti-rollback empêche l’installation d’anciennes versions vulnérables du système.

Schéma simple – Chaîne de confiance
Bootloader → Kernel → System → Services → Applications
(chacun vérifie l’intégrité du suivant)
<img width="605" height="41" alt="image" src="https://github.com/user-attachments/assets/a525c35c-f02d-4ac2-a929-46a6cd8f9f72" />



📸 Screenshot facultatif :
Écran Device Manager montrant l’AVD utilisé.

## 4. Définition du rooting (Étape 9 – 4 phrases)

Le rooting consiste à obtenir les privilèges super-utilisateur appelés « root ».
Il supprime une partie des protections natives du système Android.
Il est utile en laboratoire pour observer des comportements internes.
Il est risqué et nécessite isolement, traçabilité et réinitialisation.

## 5. Intérêt en laboratoire (Étape 10)

En laboratoire, un environnement privilégié peut aider à :

observer des artefacts système normalement inaccessibles,

analyser le comportement runtime d’une application,

tester la robustesse du stockage face à un attaquant privilégié.

Ces pratiques sont strictement limitées à un labo autorisé.

## 6. Matrice de risques (Étape 11)

Intégrité non garantie → conclusions biaisées.

Surface d’attaque accrue → exposition externe.

Données sensibles exposées → violation de confidentialité.

Instabilité système → résultats incohérents.

Mélange comptes perso/test → fuite d’informations.

Mauvais nettoyage → persistance de données.

Réseau non isolé → impacts involontaires.

Traçabilité insuffisante → tests non auditables.

## 7. Mesures défensives (Étape 12)

Réseau isolé.

Données fictives uniquement.

AVD dédié.

Wipe après chaque séance.

Journal de configuration.

Aucun compte personnel.

APK contrôlées.

Horodatage et captures.

## 8. OWASP MASVS (Étape 13)

STORAGE-1 :
Les données sensibles doivent être stockées de manière chiffrée.

NETWORK-1 :
Les communications réseau doivent utiliser TLS avec validation correcte.

## 9. OWASP MASTG (Étape 14)

Idées de tests :

Examiner les fichiers dans /data/data/[package]/shared_prefs/.

Analyser les logs avec adb logcat pour détecter des fuites.

## 10. Traçabilité – Fiche environnement (Étape 16)
Champ	Valeur
Date / auteur	Saad Makkouch
Support	Pixel 6 AVD
Android	16.0
DivaApplication
Observations	Root actif
Limites	Environnement virtuel
Reset effectué	Oui

Application lancée

Résultat adb root

Résultat getprop ro.boot.verifiedbootstate

## 11. Remise à zéro AVD (Étape 17)

Commandes utilisées :

adb emu avd stop
adb emu avd wipe-data

<img width="608" height="302" alt="image" src="https://github.com/user-attachments/assets/59a09b65-7595-4bbf-bea4-01ea3f6580b3" />


Pourquoi c’est crucial :
Ne pas réinitialiser l’environnement compromet la fiabilité des tests futurs.
