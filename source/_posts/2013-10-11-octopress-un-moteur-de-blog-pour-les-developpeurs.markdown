---
layout: post
title: "Octopress : Un moteur de blog pour les développeurs"
date: 2013-10-11 22:23
comments: true
categories:
- Blog
---

Comme beaucoup de développeurs, je souhaite partager ma connaissance sur certains sujets. Pour ce
faire, j'ai voulu créer un blog. Cet article décrit la démarche que j'ai effectuée afin de créer
le blog.

<!--more-->

Afin de choisir un moteur de blog, j'ai fait le tour des sites de développeurs. Plusieurs solutions
sont couramment utilisées.

Premièrement, beaucoup de développeurs créent, eux-mêmes, leur site. Ne voulant pas réinventer la
roue, j'ai choisi d'utiliser un outil déjà existant. Créer un blog est assez répandu et il y a une kyrielle de bons outils créés pour répondre à ce besoin.

Il existe également beaucoup de personnes utilisant Wordpress. Wordpress ne correspond pas à ma
philosophie. Tout d'abord, c'est écrit en PHP et j'aime Ruby. Ensuite, je trouve que Wordpress est
une "usine à gaz". C'est un peu comme utiliser Joomla pour faire des sites internet. C'est
probablement très bien pour les webmasters néophites, mais je suis développeur et je souhaite
avoir plus de contrôle sur mon site, kit à mettre les mains dans le cambouis.

Jekyll
------

En continuant mes recherches, j'ai trouvé Jekyll que je connaissais seulement de nom. Jekyll est un
générateur de site statique. Celui-ci est extrêment puissant pour faire ce qu'il sait faire. Il
permet de générer des pages HTML à partir de fichiers textes créés en Markdown ou Textile et Liquid.
Avec Jekyll, il n'y a aucune base de données. Le résultat est uniquement du HTML et peut donc
être hébergé n'importe où. J'aime le fait d'avoir un rendu similaire à celui de n'importe quel site
sans avoir aucune logique du coté serveur.

Petit avertissement : Jekyll est destiné aux développeurs. J'imagine très mal une personne novice en
informatique utiliser cet outil. Si c'est ce que vous souhaitez dirigiez-vous sur Wordpress.

Jekyll est un outil très simple. Peut-être un peu trop simple pour faire ce que je souhaitais. Pour
mon blog, je souhaite avoir une partie "sociale" et un design agréable. J'ai tout d'abord trouvé
[JekyllBootstrap](http://jekyllbootstrap.com/). Sincèrement, je le déconseille. Je trouve ça
compliqué pour rien. La structure du code est assez compliquée et prend du temps à modifier
quand on n’est pas habitué à Jekyll. Pour ma part, c'était plus simple de tout faire moi-même avec
Jekyll.

Octopress
---------

Rapidement, je suis tombé sur [Octopress](http://octopress.org/). Octopress est à
la fois simple, puissant et facilement modifiable. Il offre des commandes pour générer des pages,
générer des posts et publier les modifications. Sans connaitre l'outil et en connaissant un peu
Jekyll, en 10 minutes j'ai pu créer un blog, le publier, ajouter un post et configurer twitter,
GitHub, google plus, etc. En plus de cela, Octopress vient avec un design qui peut-être modifiable,
mais qui me convient parfaitement. J'ai donc choisi de le laisser tel quel.

Malheureusement, Octopress ne supporte pas l'internationalisation. J'ai suivi [cet article](http://lkdjiin.github.io/blog/2013/07/13/je-veux-mon-blog-octopress-en-francais/) afin de franciser mon site.

GitHub-Pages
------------

Pour ce qui est de l'hébergement, j'ai choisi [GitHub-Pages](http://pages.github.com/). Il y a
énormément d'avantages à utiliser GitHub-Pages. Tout d'abord, c'est gratuit. Ensuite, cela permet
de créer et de modifier les articles en utilisant les outils d'éditions en ligne de GitHub. Enfin,
le plus gros avantage selon moi est que n'importe qui peut faire un fork du projet et proposer des
améliorations ou corriger des fautes d'orthographe (merci à [Sylvain Abélard](https://twitter.com/abelar_s)).

En général, je trouve qu'avoir un site du type "monsite.wordpress.com" ne fait pas très professionnel
et j'aurais tendance à acheter un nom de domaine. Dans ce cas, "gcorbel.github.io/blog" ça ne dérange
pas du tout. Ça fait développeur qui ne connais pas trop mal GitHub. J'économise sur le nom de domaine!

Conclusion
----------

Pour ma part, je suis très content et je conseille à tous les développeurs cherchant un moteur de blog de l'essayer.

Je vais essayer d'ajouter des articles plus souvent. Restés connectés et [follow me on twitter](https://twitter.com/GuirecCorbel).
