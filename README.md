# LAB 2 — Rooting Android
**Cours :** Sécurité des Applications Mobiles  


---



##  Rooter l'AVD

```bash
adb root
adb remount
adb shell id
adb shell getprop ro.boot.verifiedbootstate
adb shell getprop ro.boot.veritymode
adb shell getprop ro.boot.vbmeta.device_state
adb shell "su -c id"
```
<img width="960" height="508" alt="img1" src="https://github.com/user-attachments/assets/5289b2e3-f755-43a8-b525-c33209ffd2d9" />

| Commande | Résultat |
|---|---|
| `adb shell id` | `uid=0(root) gid=0(root)` |
| `ro.boot.verifiedbootstate` | `orange` |
| `ro.boot.veritymode` | `enforcing` |
| `ro.boot.vbmeta.device_state` | (vide) |
| `su -c id` | Erreur — déjà uid=0 |



---

##  Fiche Périmètre

| Champ | Valeur |
|---|---|
| Objectif | Comprendre le rooting et ses impacts sur la sécurité Android |
| Données | Fictives uniquement |
| Réseau | Réseau de test isolé |

---

##  Démarrer un AVD Propre

```bash
adb devices
# emulator-5554   device
```



---

##  Installer l'App de Test

```bash
adb install app-debug.apk
# Success

adb shell pm list packages -3
# package:ma.ens.myapplication
# package:com.topjohnwu.magisk
```
<img width="661" height="122" alt="img4" src="https://github.com/user-attachments/assets/50acbd76-36aa-411d-a2c4-8ead2b9c5095" />



---

##  3 Scénarios de Test

| # | Scénario | Action | Résultat |
|---|---|---|---|
| 1 | Ouvrir l'écran d'accueil | Lancer l'app | Formulaire Nom/Prénom affiché |
| 2 | Saisir des données fictives | Remplir les champs → Envoyer | Données soumises |
| 3 | Vérifier la notification | Observer la notification | Données affichées |

<img width="260" height="393" alt="img5" src="https://github.com/user-attachments/assets/7b34e88c-cfa8-4144-afe5-c5e011f94301" />

---

## Sécurité Android

1. **Sandboxing** — chaque app s'exécute dans un environnement isolé
2. **Permissions** — accès aux ressources sensibles requis explicitement
3. **Intégrité système** — le noyau protège les partitions contre les modifications

>  Le rooting contourne ces trois couches en accordant un accès `uid=0` illimité.

---

## Verified Boot
<img width="960" height="508" alt="img1" src="https://github.com/user-attachments/assets/b1339d4a-4a7d-489d-b3e0-50de835272a5" />

| Couleur | Signification |
|---|---|
|  Green | Système vérifié et intègre |
|  Orange | Système modifié, intégrité non garantie |
|  Red | Intégrité compromise, démarrage dangereux |

Notre AVD → `ro.boot.verifiedbootstate = orange`

---

## — AVB (Android Verified Boot 2.0)

Vérification cryptographique de chaque partition au démarrage + protection anti-rollback.

---

##  Définition du Rooting

Obtenir les privilèges `uid=0/root` sur un appareil Android. Contourne sandbox, permissions et intégrité des partitions. Utile en labo uniquement, dans un environnement isolé avec remise à zéro obligatoire.

---

## Matrice de Risques

| # | Risque | Impact |
|---|---|---|
| 1 | Intégrité non garantie | Conclusions biaisées |
| 2 | Surface d'attaque accrue | Exposition externe |
| 3 | Données sensibles exposées | Violation de confidentialité |
| 4 | Instabilité système | Tests non reproductibles |
| 5 | Mélange comptes perso/test | Fuite d'informations |
| 6 | Mauvais nettoyage | Persistance de données |
| 7 | Réseau non isolé | Effets sur systèmes externes |
| 8 | Traçabilité insuffisante | Impossible d'auditer |

---

##  Mesures Défensives

| # | Mesure | Objectif |
|---|---|---|
| 1 | Réseau isolé | Éviter communications non contrôlées |
| 2 | Données fictives | Éliminer tout risque de fuite |
| 3 | AVD dédié | Aucune contamination personnelle |
| 4 | Wipe en fin de séance | Ne laisser aucune trace |
| 5 | Journal de configuration | Reproductibilité des tests |
| 6 | Aucun compte personnel | Éviter mélange de données |
| 7 | Contrôle des APK | Limiter les risques |
| 8 | Horodatage + captures | Traçabilité complète |

---

## OWASP MASVS

**STORAGE-1** — Les données sensibles ne doivent jamais être stockées en clair (SharedPreferences, DB non chiffrée, fichiers accessibles).

**NETWORK-1** — Toutes les communications doivent utiliser TLS avec vérification des certificats serveur.

---




##  Synthèse des Commandes

```bash
adb devices
adb root
adb remount
# → Successfully disabled verity
# → Using overlayfs for /system /vendor /product /system_dlkm /system_ext
# → Verity disabled; overlayfs enabled.
adb shell id                                    # uid=0(root)
adb shell getprop ro.boot.verifiedbootstate    # orange
adb shell getprop ro.boot.veritymode           # enforcing
```





## Remise à Zéro
<img width="284" height="189" alt="img7" src="https://github.com/user-attachments/assets/7a08a991-ad6d-4de2-ab95-bbcbc087b36a" />

<img width="319" height="497" alt="img8" src="https://github.com/user-attachments/assets/e6573f7b-1c17-4962-87c3-4f3e5a9f59c8" />



Android Studio → Device Manager → rooted_avd → **Wipe Data**

