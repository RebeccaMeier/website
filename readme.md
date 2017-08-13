# Rebecca Meier | Graphic Design

Ce dépot contient le code source du site web [rebeccameier.ch](http://rebeccameier.ch).

## Déployement

Un script à été créé pour simplifier la compilation et le déployement de chaque nouvelle version.

Ce script se déclenche lors de la mise à jour de certaines branches sur ce dépot.

Avant de déployer une modification, il faut en premier lieu l'enregistrer. Pour ce faire il faut utiliser les commandes suivantes depuis le répertoire du dépot.

```
git add -A    # Indique qu'il faut enregistrer les modifications de tous les fichiers.
git commit -m 'message indiquant les modifications effectuées'
git push      # Envoi les modification au dépot distant.
```

**Afin d'éviter de mauvaise surprise, il est conseillé de déployer d'abord sur le [site web de test](http://next.rebeccameier.ch).**

### Test

Pour déclancher le déployement vers le [site web de test](http://next.rebeccameier.ch), il suffit de lancer les commandes suivantes après avoir enregistrer les modifications avec git.

```
git push origin master:test
```

### Release

**Le déployement automatique pour la version `release` n'est pas encore activée**

Pour déclancher le déployement vers le site web, il suffit de lancer les commandes suivantes après avoir enregistrer les modifications avec git.

```
git push origin master:release
```
