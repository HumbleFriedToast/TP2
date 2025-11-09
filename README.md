# TP2# TP N°2 : La recherche d'éléments - Guide d'explication

##  Vue d'ensemble

Ce TP compare différents algorithmes de recherche et analyse leur complexité théorique et pratique.

---

## PARTIE A : Recherche d'un élément

### 1️⃣ Recherche dans un tableau NON TRIÉ

**Fonction :** `rechElets_TabNonTries()`

**Principe :**
- Parcourir le tableau du début à la fin
- Comparer chaque élément avec la valeur recherchée `x`
- S'arrêter dès qu'on trouve l'élément

**Complexité :**
- **Meilleur cas :** O(1) - l'élément est en première position
- **Pire cas :** O(n) - l'élément est en dernière position ou n'existe pas

---

### 2️⃣ Recherche dans un tableau TRIÉ (séquentielle)

**Fonction :** `rechElets_TabTries()`

**Principe :**
- Parcourir le tableau comme avant
- **MAIS** : on peut s'arrêter dès qu'on dépasse la valeur recherchée
- Optimisation possible grâce au tri

**Complexité :**
- **Meilleur cas :** O(1) - l'élément est en première position
- **Pire cas :** O(n) - parcours complet si élément absent ou en fin

**Amélioration :** Arrêt anticipé si on dépasse la valeur

---

### 3️⃣ Recherche DICHOTOMIQUE

**Fonction :** `rechElets_Dicho()`

**Principe :**
- Diviser le tableau en deux à chaque étape
- Comparer l'élément du milieu avec `x`
- Si `x` est plus petit → chercher dans la moitié gauche
- Si `x` est plus grand → chercher dans la moitié droite
- Répéter jusqu'à trouver ou épuiser les possibilités

**Exemple avec [1, 3, 5, 7, 9, 11, 13], chercher 7 :**
```
Étape 1: milieu = 7 ✓ trouvé !
```

**Exemple avec [1, 3, 5, 7, 9, 11, 13], chercher 11 :**
```
Étape 1: milieu = 7, 11 > 7 → droite [9, 11, 13]
Étape 2: milieu = 11 ✓ trouvé !
```

**Complexité :**
- **Meilleur cas :** O(1) - l'élément est au milieu du tableau initial
- **Pire cas :** O(log n) - divisions successives jusqu'à 1 élément

**Pourquoi log n ?**
- À chaque étape, on divise par 2
- Nombre d'étapes = log₂(n)
- Exemple : n=1000 → ~10 étapes maximum

---

### 📊 Comparaison des trois méthodes

| Méthode | Tableau | Meilleur cas | Pire cas | Remarque |
|---------|---------|--------------|----------|----------|
| Non trié | Non trié | O(1) | O(n) | Simple mais lent |
| Séquentiel | Trié | O(1) | O(n) | Arrêt anticipé possible |
| Dichotomique | Trié | O(1) | O(log n) | **Le plus efficace !** |

**Constatation :** La recherche dichotomique est BEAUCOUP plus rapide pour les grands tableaux !

---

## PARTIE B : Recherche du maximum et du minimum

### 1️⃣ Approche NAÏVE (MaxEtMinA)

**Principe :**
- Initialiser max et min au premier élément
- Pour chaque élément suivant :
  - Comparer avec max → mettre à jour si nécessaire
  - Comparer avec min → mettre à jour si nécessaire

**Exemple avec [5, 2, 7, 3, 1, 8, 4] :**
```
Début: max=5, min=5
i=1: 2 → max=5, min=2
i=2: 7 → max=7, min=2
i=3: 3 → max=7, min=2
i=4: 1 → max=7, min=1
i=5: 8 → max=8, min=1
i=6: 4 → max=8, min=1
```

**Nombre de comparaisons :** 2(n-1) = 2n - 2

**Complexité :** O(2n)

---

### 2️⃣ Approche OPTIMISÉE (MaxEtMinB)

**Principe en 3 phases :**

**Phase 1 : Comparaison par paires**
- Comparer les éléments 2 par 2
- Mettre le plus grand en position paire (0, 2, 4...)
- Mettre le plus petit en position impaire (1, 3, 5...)

**Phase 2 : Chercher le minimum**
- Parcourir uniquement les positions impaires
- Trouver le minimum

**Phase 3 : Chercher le maximum**
- Parcourir uniquement les positions paires
- Trouver le maximum

**Exemple avec [5, 2, 7, 3, 1, 8, 4] :**

```
Phase 1 (comparaison par paires):
[5, 2] → 5>2 ✓ → [5, 2]
[7, 3] → 7>3 ✓ → [7, 3]
[1, 8] → 1<8 ✗ → [8, 1]
[4] → pas de paire

Résultat: [5, 2, 7, 3, 8, 1, 4]
          ↑  ↑  ↑  ↑  ↑  ↑  ↑
        pair impair pair impair pair impair impair

Phase 2 (min parmi impairs [2, 3, 1]):
min = 1

Phase 3 (max parmi pairs [5, 7, 8] + 4):
max = 8
```

**Nombre de comparaisons :**
- Phase 1 : n/2 comparaisons
- Phase 2 : n/2 - 1 comparaisons
- Phase 3 : n/2 - 1 comparaisons
- **Total :** n/2 + (n/2 - 1) + (n/2 - 1) = 3n/2 - 2

**Complexité :** O(1.5n)

---

### 📊 Comparaison MaxEtMinA vs MaxEtMinB

| Algorithme | Comparaisons | Complexité | Exemple (n=1000) |
|------------|--------------|------------|------------------|
| MaxEtMinA | 2(n-1) | O(2n) | 1998 comparaisons |
| MaxEtMinB | 3n/2 - 2 | O(1.5n) | 1498 comparaisons |

**Gain théorique :** ~25% de comparaisons en moins avec MaxEtMinB

**Pourquoi c'est plus efficace ?**
- On ne compare pas chaque élément deux fois
- On sépare intelligemment les candidats pour max et min
- Moins de comparaisons totales

---

## 🎯 Résultats attendus

### Graphe de la Partie A (pire cas, tableau trié)

```
Temps
  ↑
  |                                                    Non trié (O(n))
  |                                              ....
  |                                        ....
  |                                  ....
  |                            ....
  |                      ....        Trié séquentiel (O(n))
  |                ....
  |          ....
  |    ....
  |....________________________
  |        Dichotomique (O(log n))
  |
  +----------------------------------------→ Taille (n)
```

### Tableaux de mesures

Le programme génère automatiquement :
- **Tableau A.3 :** Temps d'exécution pour différentes tailles
- **Comparaisons B.3 :** Nombre de comparaisons MaxEtMinA vs MaxEtMinB

---

## 💡 Conclusions

1. **La recherche dichotomique est largement supérieure** pour les tableaux triés
2. **Le tri préalable est rentable** si on fait plusieurs recherches
3. **L'optimisation algorithmique** (MaxEtMinB) réduit significativement les comparaisons
4. **La complexité théorique se vérifie en pratique** avec les mesures temporelles

---

## 🚀 Comment utiliser le programme

1. **Compilation :**
   ```bash
   gcc -o tp2 tp2_recherche.c
   ```

2. **Exécution :**
   ```bash
   ./tp2
   ```

3. **Résultats :**
   - Le programme affiche tous les tableaux demandés
   - Les temps sont en microsecondes (µs)
   - Les comparaisons sont comptées automatiquement

4. **Pour les graphes :**
   - Utiliser les données affichées
   - Créer les graphes sur papier ou avec Excel/LibreOffice
   - Mettre n en abscisse, temps en ordonnée

---

## 📝 Points importants pour le rapport

- Expliquer la différence entre O(n) et O(log n)
- Justifier pourquoi la dichotomie est plus rapide
- Analyser le gain de MaxEtMinB par rapport à MaxEtMinA
- Présenter les graphes clairement
- Conclure sur l'importance du choix d'algorithme
