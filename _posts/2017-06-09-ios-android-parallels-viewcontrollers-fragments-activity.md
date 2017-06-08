---
layout: post
title:  "UIViewControler = Activity et/ou Fragment"
date:   2017-06-08
excerpt: "UIViewControler = Activity et/ou Fragment"
published: true
tag:
- iOS
- Android
- iOS <3 Android
- Android <3 iOS
- UIViewControler
- Fragment
- Activity
---

## Prérequis
* Connaitre iOS et avoir envie de comprendre Android

## Pourquoi comprendre Android
* Vous ne ferez pas que de l'iOS toute votre vie, donc restez curieux
* Pour travailler efficacement avec les autres plateformes (vous avez surement des soucis commun, pourquoi ne pas les solutionner ensemble ... mais pour ça il faut se comprendre)
* Android est une grosse plateforme (80% du marché) et elle est devenu mature

## UIViewControler, Activity, Fragments
Sur iOS, Android, le design pattern le plus commun est le **MVC** (Model View Controller).
+ **Model** : les données de base que vous gérées.
+ **View** : une représentation visuelle de l'écran.
+ **Controller** : un object qui va gérer les événements qui se produisent sur la vue et répercuter cela sur le modèle.

|      |     iOS    |  Android |
| :-------------: |: -------------: | :---------: |
| **View**     |     UIView     |      View |
| **Controller**        |        UIViewControler       |      Activity, Fragment |
| **Model**     |    x        |      x |

### iOS : UIViewControler
Depuis la sortie du l'environnement de développement, (iPhone OS 1, Mars 2008), les UIViewControllers sont disponible.
En 2010, sort l'iPad 1.
Les applications iPhone et iPad doivent (logiquement) partager une grosse partie des concepts graphiques. Pour ce faire, nous pouvons gérer de la composition, c'est à dire réutiliser plusieurs UIViewControllers d'iPhone pour proposer un écran d'iPad.

```swift
let parentViewController = UIViewControler()
let childController = UIViewControler()
parentViewController.addChildViewController(childController)
parentViewController.view.addSubview(childController.view)
childController.didMoveToParentViewController(self)
```

```objc
UIViewControler *parentViewController = new childController();
UIViewControler *childController = new childController();
[parentViewController addChildViewController:childController];
[parentViewController.view addSubview:childController.view];
[childController didMoveToParentViewController:parentViewController];
```

**Ce qu'il faut retenir, c'est que c'est au développeur de se débrouiller pour construire et réutiliser ses briques.**

#### Complexité ?
Avec la complexité grandissante des applications il est devenu important de pouvoir découper son écran en plusieurs view/controlleur plus petit.
Apple nous laisse la liberté de faire ce que l'on veut.


#### Android : Activity et fragments

<img src="https://developer.android.com/images/fundamentals/fragments.png" alt="Google-fundamentals-fragments">

Sur Android c'est différent, Google a fait le choix à notre place et impose 2 composants.

+ Les **activités** sont la depuis le début et on peut se dire qu' **un écran = une activité**.

+ Les **fragments** sont arrivés avec les premières tablettes Android, le 22 février 2011. (Android 3.0, API level 11, Honeycomb).

L'idée étant de proposer un choix pour mutualiser son code et réutiliser un maximum de chose entre les interfaces.

Alors faut-il utiliser les fragments ou les activités ?
Fausse question et faux problème, il faut utiliser les 2, en connaissant les quelques limitations :
- Un fragment doit être dans un activité
- Un fragment ne peut pas présenter une activité (pas de startActivity)
- Un fragment n'a pas besoin d'être déclaré dans le manifest (youpi)
- ViewPager : fragments !
- Pour une mutualisation smartphone tablettes : fragments !
