---
layout: post
title: "Leap Motion : le point de vue d'un développeur"
date: 2013-10-12 07:41
comments: true
categories:
- Leap Motion
---

La semaine dernière, la société [Optik 360](http://www.optik360.com/), spécialisée en photo 360 degrés, m'a demandé de tester Leap Motion en faisant une application permettant de visionner des images panoramiques. L'image devait se déplacer en fonction des mouvements fait par l'utilisateur.

Pour ceux qui ne connaissent pas encore Leap Motion, il s'agit d'un petit capteur de mouvements d'environ 8 centimètres. Pour plus d'informations, rendez-vous sur [leur site officiel](https://www.leapmotion.com/).

Phase 1 : Installation
----------------------

Sous Windows, l'installation s'est faite en moins de 5 minutes. Je me suis rendu sur la page indiquée pour télécharger l'application, je l'ai installée et mes mouvements ont été détectés par le contrôleur Leap Motion.

Je m'attendais à ce que mon CPU soit surchargé. Ce n'est vraiment pas le cas. Quand je bouge devant le contrôleur, Leap Motion prend 7% du CPU. Windows Media Player prend 8%. J'ai pourtant choisi le maximum de précision, au détriment de la rapidité, dans le panneau de configuration. Donc, il n'y a aucun problème au niveau de la rapidité.

Après avoir bougé les mains dans tous les sens en testant les applications fournies (mon chien me prend pour un fou), j'ai voulu l'installer sur Ubuntu 12.04. Il est possible de télécharger une version du SDK compatible Linux sur leur site. L'installation se fait également très rapidement. Le contrôleur et le panneau de configuration se sont lancés correctement. Le seul problème que j'ai rencontré : ça ne fonctionne pas. Le contrôleur ne reconnait pas mes mouvements alors qu'il n'y a aucun problème sur Windows. J'ai envoyé [un message sur leur forum](https://developer.leapmotion.com/forums/forums/10/topics/1811). Pour le moment, je n'ai eu aucune réponse. On va voir pour la suite.

Phase 2 : Premier développement
-------------------------------

Par la suite, je suis retourné sur Windows pour commencer à développer. Leap Motion possède des SDK officiels dans plusieurs langages. Malheureusement, mon langage préféré, Ruby, n'est pas supporté officiellement. Je trouve ça génial d'avoir choisi d'être compatible avec plusieurs langages. Ça laisse énormément de possibilités aux développeurs.

Pour mon projet, j'ai choisi de développer en JavaScript. LeapMotion permet de faire une application coté client. L'application écoute un certain port pour recevoir des données depuis le contrôleur. Le choix du JavaScript s'est fait uniquement sur le fait qu'il n'est pas nécessaire de faire une installation sur un PC. Un simple navigateur (moderne), permet d'avoir une application qui fonctionne avec Leap. Génial!

La bibliothèque offerte par Leap est impressionnante. C'est super simple de faire une application. Il suffit de quelques lignes de code. Voici un exemple :

``` javascript
Leap.loop(function(frame) {
  console.log(frame.fingers.length);
});
```

Ce petit bout de code permet de voir le nombre de doigts détectés par le contrôleur. N'importe quel développeur est capable de faire ça.

Phase 3 : Recherche d'une bibliothèque JavaScript
-------------------------------------------------

Après quelques tests, je me suis mis à la recherche d'un code existant permettant d'afficher une image panoramique. Je suis tombé sur [ce projet](http://mrdoob.github.io/three.js/examples/webgl_panorama_equirectangular.html). L'avantage de ce code est qu'il utilise [Three.js](http://threejs.org/). Three.js est une bibliothèque permettant de faire des animations 3D en JavaScript. Il existe beaucoup d'[exemples fournis pas les développeurs de Three.js](http://mrdoob.github.io/three.js/). Avec du travail, on peut développer une version compatible avec Leap de tous ces exemples.

J'ai donc téléchargé le code en local et j'ai commencé à le modifier pour le mettre un peu plus "à ma sauce". Je l'ai changé pour qu'il soit un peu plus orienté objet et j'ai utilisé coffeescript.

Phase 4 : Intégration de Leap à la bibliothèque
-----------------------------------------------

J'ai étudié le comportement de la souris et j'ai voulu le reproduire. Tout d'abord, j'ai extrait le code relatif à la souris dans un objet à part. Pour tester le déplacement avec Leap Motion, mon premier réflexe a été de faire ce genre de code :

``` coffeescript
class LeapPano.LeapMotion
  constructor: (view) ->
    @view = view

  init: =>
    Leap.loop((frame) =>
      @view.setLon(@view.getLon() + .1)
    )
```

Ce code permet, à chaque itération, de déplacer horizontalement la caméra. Le problème que je trouve avec ce que est que je ne gère pas la rapidité des itérations. Je n'ai pas trouvé de façon de faire dans la documentation de Leap. J'ai donc choisi de modifier le code comme ceci :

``` coffeescript
class LeapPano.LeapMotion
  constructor: (view) ->
    @view = view

  setFrame: (frame) ->
    @frame = frame

  init: ->
    setTimeout(@checkMotion, 10)

  checkMotion: =>
    if @frame?
      @view.setLon(@view.getLon() + .1)

    setTimeout(@checkMotion, 10)
```

L'appel de la fonction `Leap.loop` est extraite de cet objet et le résultat est passé via un setter à chaque itération. À l'initialisation, la fonction setTimeout, permettant d’exécuter la fonction de reconnaissance des mouvements toutes les dix millisecondes, est lancée. Ce code permet également de ne pas avoir plusieurs appels à la fonction `Leap.loop`. La boucle est lancée dans la page principale et les données détectées sont transmises aux différents objets en ayant besoin. Je crois que c'est une meilleure architecture.

Maintenant que ça marche comme je veux, je peux m'occuper de la reconnaissance des mouvements. J'ai modifié la fonction `checkMotion` comme ceci :

``` coffeescript
checkMotion: =>
  if @frame?
    finger = @frame.fingers[0]
    if finger?
      x = finger.tipPosition[0]
      y = finger.tipPosition[1]
      @view.setLon(@view.getLon() + (x/320))
      @view.setLat(@view.getLat() + ((y-160)/320))

  setTimeout(@checkMotion, 10)
```

`@frame.fingers[0]` permet de récupérer le premier doigt détecté. `finger.tipPosition` retourne un tableau contenant la position en X, en Y et en Z. Enfin, je change la position de la caméra dans la scène 3D. Le calcul d'un ratio permet de faire en sorte d’accélérer ou de ralentir le mouvement selon la position du doigt. Plus le doigt est éloigné du centre, plus le déplacement est rapide.

La dernière fonction que j'ai voulu ajouté a été de changer l'image lorsque l'on décrit un cercle. J'ai donc ajouté ceci :

``` coffeescript
if(@frame.gestures.length > 0)
  gesture = @frame.gestures[0]
  if gesture.type == "circle"
    @view.switchFile()
```

Encore une fois, c'est très simple. `@frame.gestures` retourne une liste des mouvements détectés. Par la suite, il faut vérifier si le type de mouvement est un cercle. Si c'est le cas, alors on change d'image.

Pour permettre la reconnaissance des mouvements, il est nécessaire d'appeller la fonction `Leap.loop` :

``` coffeescript
Leap.loop({enableGestures: true}, function(frame) {
  pano.setFrame(frame)
})
```

Et voila! Tout fonctionne. En tant que bon contributeur Open-Source j'ai affiché le code ici : [https://github.com/GCorbel/LeapPano](https://github.com/GCorbel/LeapPano). J'ai également créé un page grâce a [GitHub-Pages](http://pages.github.com/) visible ici : [http://gcorbel.github.io/LeapPano/](http://gcorbel.github.io/LeapPano/).

Ce que j'aime dans Leap Motion
------------------------------

Premièrement, j'ai beaucoup aimé la facilité d'installation. On branche, on installe et ça marche. Le développement est extrêmement facile. L'API fournie est la plus simple possible. Le fait qu'elle soit disponible en plusieurs langages est génial. Je suis vraiment impressionné des possibilités fournies et de la simplicité.

Là où j'ai des doutes
---------------------

Petit bémol pour la documentation. Je trouve que la documentation officielle est assez peu détaillée. Il y a peu de code affiché. Pour voir du code réel, il est nécessaire de rechercher les exemples disponibles sur GitHub.

Au niveau de la reconnaissance des mouvements, il existe trois mouvements "de base" : Circle, Swipe et Tap. Ajouter d'autres mouvements est beaucoup plus complexe. On ne peut pas imaginer faire un jeu comme [Black & White](http://fr.wikipedia.org/wiki/Black_and_White_%28jeu_vid%C3%A9o%29) pour le moment.

Le gros point faible est la précision. Le nombre de doigts détectés n'est pas toujours réel et la position est approximative. Quand on y pense, c'est normal. Si l'on pointe une image vers l'écran, notre doigt tremble. Viser une icône peut être très long. Leap Motion ne n'est pas prêt à remplacer la souris.

De plus, je trouve que la détection de pointeurs est très hasardeuse. Un crayon passe de détecté à non détecté sans raison. La détection des doigts est beaucoup plus stable.

Ceci dit, Leap Motion est à sa version 0.8. Il s'agit d'un système tout nouveau. On peut imaginer que le système va s'améliorer au fil du temps.

Conclusion
----------

J'adore Leap Motion. Il suffit de compter le nombre de fois ou j'ai marqué "Génial" dans cet article pour le comprendre.

Le futur comme imaginé dans les années 80/90 est à notre portée. On n'est pas encore capable de faire des tableaux comme dans Minority Report mais on s'y approche. Des technologies comme Leap, Google Voice, Google glaces, la domotique, etc. font rêver. Le futur est accessible et n'importe quel développeur est capable de faire des choses impressionnantes.
