---
layout: post
title:  "Android, le passage d'arguments"
date:   2017-06-08
excerpt: "Android, le passage d'arguments"
published: true
tag:
- iOS
- Android
- iOS <3 Android
- Android <3 iOS
-
---

## Prérequis
* Connaitre iOS et vouloir comprendre Android

## Pourquoi comprendre Android
* Vous ne ferez pas que de l'iOS toute votre vie, donc restez curieux
* Pour travailler efficacement avec les autres plateformes (vous avez surement des soucis commun, pourquoi ne pas les solutionner ensemble ... mais pour ça il faut se comprendre)
* Android est une plateforme importante (80% du marché) et elle est devenue mature

## iOS, beaucoup de liberté

## Android
### Activity, intent.putExtra
Le lancement d'une nouvelle activité passe par la création d'un Intent.
On configure un `Intent`, qui va préparer l'activité.

```java
Intent intent = new Intent(context, ContactProfileActivity.class);
intent.putExtra(Constants.CONTACT_ID, contact.getId());
startActivity(intent);
```

Au moment du `onCreate` on pourra récupérer **la valeur**.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    int contactId = getIntent().getIntExtra(Constants.CONTACT_ID, -1);
}
```

|   Avantages   |    inconvénients |
| :-------------: |: -------------: |
| C'est facile | Pour des données simples |

### Activity, Bus.put
A la création de l'intent, on va garder un pointeur vers l'objet que l'on passe en paramètre. (C'est l'équivalent d'un [associated object sur iOS] (https://developer.apple.com/documentation/objectivec/1418509-objc_setassociatedobject)

```java
Intent intent = new Intent(context, ContactProfileActivity.class);
Bus.put(Constants.BUS_FULL_CONTACT,contact);
startActivity(intent);
```

Au moment du `onCreate` on pourra récupérer ce **pointeur**.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Contact contact = Bus.get(Constants.BUS_FULL_CONTACT, Contact.class);
}
```

|   Avantages   |    inconvénients |
| :-------------: |: -------------: |
| Pas besoin de serialiser l'objet | Attention à la gestion mémoire et ne pas oublié libérer les pointeurs |

### Fragment, setArguments

Le fragment expose publiquement des méthodes pour l'instancier. On lui passe des paramètres et le système fait le reste.

```java
public static DetailsFragment newInstance(int index) {
    DetailsFragment f = new DetailsFragment();
    Bundle args = new Bundle();
    args.putInt("index", index);
    f.setArguments(args);
    return f;
}
```

|   Avantages   |    inconvénients |
| :-------------: |: -------------: |
| Le système est capable de décider la reconstruction du fragment, il passera par le constructeur vide mais les arguments seront encore là, ouf ! | x |

### Fragment, directement par un setter !
On instancie un fragment et on le configure après ...

```java
public class DetailsFragment extends Fragment {
  private int index;

  public void refreshIndex(int index){
    this.index = index;
    // reload the fragment, invalidate the adapter or whatever
  }
}
```

|   Avantages   |    inconvénients |
| :-------------: |: -------------: |
| C'est une bonne idée de pouvoir rafraîchir un fragment | Perte des données si le système décide de reconstruire le fragment |

### Et sur iOS ?
On fait ce que l'on veut car on utilise les références des `viewControllers` et le système ne désalloue pas les instances si facilement.

Il y a tout de même quelques conseils :
1. Utiliser les [DESIGNATED_INITIALIZER](https://developer.apple.com/library/content/documentation/General/Conceptual/CocoaEncyclopedia/Initialization/Initialization.html) si vous ne connaissez pas, demandez vous ce que va appeler `new` dans cet example

```objc
MonObjet *obj = [MonObjet new];
```
ou en Swift ... vous le sentirez passer car le compilateur vérifie :)

2. Faites vos propres DESIGNATED_INITIALIZER pour expliquer les paramètres obligatoires ou non.
