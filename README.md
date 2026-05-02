# transformer.js · GPU

Application web **tout-en-un** en un seul fichier HTML pour entraîner un mini modèle de type GPT dans le navigateur, avec **TensorFlow.js**, **WebGPU/WebGL**, **Web Worker** et **autosave IndexedDB**. Le projet charge les bibliothèques via CDN et force l’entraînement sur GPU, parce que visiblement laisser le CPU souffrir n’était pas assez élégant.

## Fonctionnalités

- Entraînement d’un petit **decoder transformer** dans le navigateur
- Backend GPU prioritaire :
  - **WebGPU** d’abord
  - **WebGL** en fallback
  - **CPU refusé**
- Entraînement dans un **Web Worker**
- Tokenizer au choix :
  - **char**
  - **word**
- Hyperparamètres configurables :
  - `ctx len`, `n_embd`, `n_heads`, `n_layers`
  - `steps`, `batch`, `lr`, `lr end`, `warmup`
  - `weight decay`, `grad clip`, `dropout`
  - `top-k`, `top-p`, `patience`, `min delta`
- Génération de texte après entraînement
- Export / import de checkpoints JSON
- **Autosave** dans **IndexedDB**
- Courbe de loss, métriques d’entraînement, journal d’événements
- Interface dark style “Apple HIG” avec responsive design

## Aperçu technique

- **Modèle** : mini transformer causal de type GPT
- **Optimiseur** : AdamW maison avec clipping global du gradient
- **Scheduling** : warmup linéaire puis cosine decay
- **Checkpoint** : format JSON `schema 3`
- **Stockage local** :
  - paramètres utilisateur dans `localStorage`
  - autosave du modèle dans `IndexedDB`

## Utilisation

1. Ouvre `index.html` dans un navigateur compatible.
2. Attends la détection du backend GPU.
3. Ajoute ou remplace le texte d’entraînement.
4. Choisis les hyperparamètres.
5. Clique sur **Train on GPU**.
6. Une fois le modèle entraîné, utilise la section **Generation** pour produire du texte.
7. Exporte le checkpoint si tu veux le sauvegarder.

## Exigences

- Un navigateur moderne
- **WebGPU** ou **WebGL**
- Connexion réseau au premier chargement, car les scripts TensorFlow.js sont chargés via CDN

## Limitations

- Le projet est conçu pour de petits jeux de données
- Les anciens checkpoints **v2** ne sont pas compatibles avec le format actuel **schema 3**
- Si aucun backend GPU n’est disponible, l’entraînement ne démarre pas
- Tout est dans un seul fichier HTML, donc l’entretien du code demande un peu d’amour, ou au moins de la patience

## Structure

```text
index.html
```

## Notes

- Le worker charge :
  - `@tensorflow/tfjs`
  - `@tensorflow/tfjs-backend-webgpu`
- La génération utilise :
  - température
  - top-k
  - top-p
- L’interface affiche aussi :
  - backend détecté
  - appareil
  - mémoire GPU
  - vocabulaire
  - nombre de paramètres
  - loss actuelle et meilleure loss

## Licence

Ajoute ici la licence de ton choix si le projet doit être publié sur GitHub.
