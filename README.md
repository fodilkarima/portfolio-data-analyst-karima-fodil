# Détection de faux billets — Machine Learning avec Python

## Contexte / besoin métier

L'**Organisation nationale de lutte contre le faux-monnayage (ONCFM)** souhaite automatiser la détection des billets contrefaits.

L'objectif est de construire un modèle capable de déterminer si un billet est authentique ou contrefait à partir de ses caractéristiques dimensionnelles.

## Données

- **1 500 billets**
- **1 000 authentiques**
- **500 contrefaits**
- **6 caractéristiques dimensionnelles**
- variable cible : `1 = authentique`, `0 = contrefait`
- **37 valeurs manquantes** dans `margin_low`, remplacées par la médiane

La classe d'intérêt est le **billet contrefait**.

## Modèles étudiés

- K-means
- Régression logistique
- K-nearest neighbors (KNN)
- Random Forest

## Démarche

1. analyse exploratoire ;
2. traitement des valeurs manquantes ;
3. séparation des variables explicatives et de la cible ;
4. standardisation ;
5. séparation entraînement / test ;
6. stratification des classes ;
7. entraînement des modèles ;
8. comparaison via matrice de confusion et métriques ;
9. sélection et sauvegarde du modèle final ;
10. création d'une fonction de prédiction réutilisable.

## Prétraitement

La séparation utilisée est :

- **80 % entraînement** : 1 200 billets ;
- **20 % test** : 300 billets.

La stratification conserve les proportions de classes :

- entraînement : 800 authentiques / 400 contrefaits ;
- test : 200 authentiques / 100 contrefaits.

La standardisation est particulièrement importante pour KNN, K-means et la régression logistique.

## Résultats

| Modèle | Accuracy | Précision faux | Rappel faux | F1 faux | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| K-means | 98,67 % | 98 % | 98 % | 98 % | — |
| Régression logistique | 99 % | 98,99 % | 98 % | 98,49 % | **0,9994** |
| KNN | 98,33 % | 97,98 % | 97 % | 97,49 % | 0,997 |
| Random Forest | 99 % | 98,99 % | 98 % | 98,49 % | 0,9992 |

Pour K-means, le score de silhouette obtenu est **0,33**, ce qui indique une séparation géométrique modérée des clusters malgré une classification proche des classes réelles.

## Choix final

La **régression logistique** est retenue.

Elle offre :

- **99 % d'exactitude** ;
- **98 % de rappel** sur les faux billets ;
- une **ROC-AUC de 0,9994** ;
- une bonne interprétabilité ;
- une mise en production simple.

## Réutilisation du modèle

Le modèle final est intégré dans un pipeline comprenant :

1. imputation ;
2. standardisation ;
3. régression logistique.

Une fonction `prediction_billet_RL()` permet ensuite de charger le pipeline, vérifier les variables et produire une prédiction :

- `0 = contrefait`
- `1 = authentique`

## Valeur métier

Le projet aboutit à un outil directement réutilisable pour automatiser la détection de nouveaux billets et réduire le temps nécessaire au contrôle manuel.

## Limites et prochaines pistes

- valider le modèle sur de nouveaux jeux de données ;
- surveiller les performances dans le temps ;
- tester la robustesse aux erreurs de mesure ;
- étudier le réglage du seuil si la priorité métier devient la réduction maximale des faux négatifs.

## Compétences démontrées

`Python` `Machine Learning` `Classification` `Régression logistique` `Random Forest` `KNN` `K-means` `ROC-AUC` `Pipeline`
