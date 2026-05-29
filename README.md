# pi-splash 🎮

*Animation ASCII rétro pour Pi Agent*

---

## 📌 Description
**pi-splash** est un ensemble de scripts Bash qui ajoutent une **animation ASCII rétro** inspirée des jeux vidéo des années 80/90 avant le lancement de **[Pi Agent](https://github.com/Earendil-Works/pi-coding-agent)** (un agent IA de codage en ligne de commande).
L'animation affiche un logo stylisé du symbole **π** avec une barre de chargement progressive, le tout dans un cadre rétro avec des couleurs orange sur fond sombre.

---

## 📁 Structure du projet
```bash
pi-splash/
├── pi                  # Wrapper principal (appelant pi-animated)
├── pi-animated         # Script qui lance l'animation + Pi Agent
└── pi-logo.sh          # Script contenant l'animation ASCII et les frames
```

---

## 🚀 Fonctionnement

### 1. Flux d'exécution
1. **`pi`** (wrapper)
   - Appelle `pi-animated` avec tous les arguments passés.
   - *Rôle* : Point d'entrée simplifié pour l'utilisateur.

2. **`pi-animated`** (orchestrateur)
   - Vérifie la présence :
     - Du binaire **Pi Agent** (`/opt/homebrew/bin/pi`).
     - Du script d'animation (`~/bin/pi-logo.sh`).
   - Lance **`pi-logo.sh`** pour afficher l'animation.
   - Transmet les arguments à **Pi Agent** une fois l'animation terminée.

3. **`pi-logo.sh`** (animation)
   - Affiche **7 frames** séquentiels avec des délais pour créer un effet d'animation.
   - Utilise des **codes ANSI** pour les couleurs (orange, gris clair, gris foncé) et le fond.
   - Affiche la version de Pi Agent (si disponible) en bas de l'écran.

---

### 2. Frames de l'animation
L'animation simule un chargement avec :
- Un **logo π** en ASCII art (style pixel art).
- Une **barre de progression** qui se remplit (`[          ]` → `[██████████]`).
- Des **effets visuels** :
  - Particules (`░░░`, `▒▒▒`, `▓▓▓`) qui apparaissent/disparaissent.
  - Cadre rétro avec des coins arrondis (`╔═══╗`, `╚═══╝`).
  - Texte "LOADING..." puis "READY" à la fin.

---

## 🎨 Design & Style
| Élément          | Couleur ANSI       | Code          | Rendu visuel          |
|------------------|--------------------|---------------|-----------------------|
| Logo π           | Orange vif         | `38;5;208`    | ██████                |
| Fond             | Gris très foncé    | `48;5;232`    | ░░░ (espace coloré)  |
| Texte secondaire | Gris clair         | `38;5;94`     | LOADING...            |
| Particules       | Gris moyen         | `░`, `▒`, `▓`| Effet de profondeur   |

---

## ⚙️ Configuration

### Prérequis
1. **Pi Agent installé** :
   ```bash
   npm install -g @earendil-works/pi-coding-agent
   ```
   - Le binaire doit être accessible à `/opt/homebrew/bin/pi` (ou modifier `PI_BIN` dans `pi-animated`).

2. **Emplacement des scripts** :
   - Les scripts doivent être placés dans `~/bin/` (ou adapter les chemins dans `pi-animated`).
   - Rendre les scripts exécutables :
     ```bash
     chmod +x ~/bin/pi ~/bin/pi-animated ~/bin/pi-logo.sh
     ```

3. **Node.js** :
   - Requiert `node` pour récupérer la version de Pi Agent (ligne 120 dans `pi-logo.sh`).

---

### Personnalisation
#### Modifier les chemins
Dans `pi-animated` :
```bash
PI_BIN="/chemin/vers/pi"          # Chemin vers le binaire Pi Agent
PI_LOGO_SCRIPT="/chemin/vers/pi-logo.sh"  # Chemin vers le script d'animation
```

#### Modifier l'animation
Dans `pi-logo.sh` :
- **Ajouter/supprimer des frames** : Dupliquer ou modifier les variables `FRAME1` à `FRAME7`.
- **Changer les couleurs** : Modifier les codes ANSI (ex: `COLOR_ORANGE='\033[38;5;214m'` pour un orange plus clair).
- **Ajuster la vitesse** : Modifier les `sleep` (ex: `sleep 0.2` pour ralentir).

#### Exemple de frame personnalisé
```bash
FRAME_CUSTOM="
 ╔══════════════════════════════════════╗
 ║  ✨       ██████       ✨             ║
 ║          ██  ██                       ║
 ║          ██████                       ║
 ║          ██  ██                       ║
 ║                                      ║
 ║         CUSTOM TEXT...               ║
 ║         [████    ]                   ║
 ╚══════════════════════════════════════╝"
```

---

## 📥 Installation
1. Cloner le dépôt :
   ```bash
   git clone https://github.com/YanLords/pi-splash.git
   cd pi-splash
   ```
2. Copier les scripts dans `~/bin/` :
   ```bash
   cp pi pi-animated pi-logo.sh ~/bin/
   chmod +x ~/bin/pi ~/bin/pi-animated ~/bin/pi-logo.sh
   ```
3. Vérifier que `~/bin` est dans votre `PATH` :
   ```bash
   echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc  # ou ~/.zshrc
   source ~/.bashrc
   ```
4. Tester :
   ```bash
   pi --help
   ```
   → L'animation devrait s'afficher avant le lancement de Pi Agent.

---

## 🐛 Dépannage

| Problème                          | Solution                                                                 |
|-----------------------------------|--------------------------------------------------------------------------|
| `Erreur: Le binaire Pi Agent n'est pas trouvé` | Installer Pi Agent (`npm install -g @earendil-works/pi-coding-agent`) ou mettre à jour `PI_BIN` dans `pi-animated`. |
| `Erreur: Le script d'animation n'existe pas` | Vérifier que `pi-logo.sh` est dans `~/bin/` et que le chemin dans `pi-animated` est correct. |
| L'animation est trop rapide/lente | Modifier les valeurs de `sleep` dans `pi-logo.sh`.                     |
| Couleurs incorrectes              | Vérifier que votre terminal supporte les codes ANSI 256 couleurs.       |
| `node: command not found`         | Installer Node.js ou commenter la ligne de récupération de version.   |

---

## 🎬 Démonstration
### Sortie attendue (extrait) :
```
 ╔══════════════════════════════════════╗
 ║  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ ║
 ║  ░          ██  ██              ░░░ ║
 ║  ░          ██████              ░░░ ║
 ║  ░          ██  ██              ░░░ ║
 ║  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ ║
 ║            LOADING...                ║
 ║            [██        ]              ║
 ╚══════════════════════════════════════╝

                        π  v1.2.3
```
*(L'animation défile avec 7 frames avant d'afficher Pi Agent.)*

---

## 📜 Licence
Ce projet est sous licence **MIT**. Consultez le fichier [LICENSE](LICENSE) pour plus de détails.

---

## 🤝 Contribution
1. Forker le dépôt.
2. Créer une branche (`git checkout -b feature/ma-fonctionnalité`).
3. Commiter vos changements (`git commit -m "Ajout de X"`).
4. Pousser (`git push origin feature/ma-fonctionnalité`).
5. Ouvrir une Pull Request.

---

## 📞 Contact
- **Auteur** : [YanLords](https://github.com/YanLords)
- **Dépôt** : [YanLords/pi-splash](https://github.com/YanLords/pi-splash)

---

## 🔧 Améliorations possibles
- [ ] Ajouter un mode "silencieux" (`--no-animation`).
- [ ] Supporter d'autres gestionnaires de paquets (ex: `apt` pour Pi Agent).
- [ ] Rendre les chemins configurables via des variables d'environnement.
- [ ] Ajouter des thèmes de couleurs (ex: `--theme blue`).
- [ ] Détecter automatiquement le chemin de Pi Agent.
