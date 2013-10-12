---
layout: post
title: "Quand j'utilise des Helpers, des Partials, des Presenters et des Decorators"
date: 2013-10-12 07:38
comments: true
categories:
- Ruby
- Ruby On Rails
---
Il existe plusieurs différentes façons d'avoir une meilleure organisation des vues dans une application Ruby on Rails. Les Partials et les Heplers sont des méthodes standard. Il y a également les Presenters et les Decorators. Cela peut-être quelque peu difficile de savoir comment et quand les utiliser.

<!--more-->

Mon organisation
===========

Toutes les techniques ont leur propre utilité.

Les Helpers
-------

Les Helpers sont des méthodes génériques qui peuvent être utilisées pour différents types d'objet. Je crée des helpers du style `link_to_update`, `big_image`, `styled_form`, etc. Ces méthodes créent du code html avec, par exemple, un style css ou un texte standard.

Les Partials
-------

Les Partials sont utilisés pour diviser une grosse vue dans de plus petites parties logiques. Je peux avoir un partial `side_menu`, `comment_list`, `header`, etc.

Les Presenters
----------

Les Presenters sont créés pour des requêtes plus compliquées avec un modèle ou plus. J'ai des presenters comme `@page_presenter.page_in_category(ruby_category)` ou `@user_presenter.user_following(an_article)`.

Les Decorators
----------

Les Decorators doivent interagir avec un modèle seulement et ne doivent pas avoir de paramètre (si possible). Je peux faire cette sorte de code : `user.full_name`, `page.big_title` ou `category.permalink`. J'utilise la Gem [Draper](https://github.com/drapergem/draper).

Si je cherche plusieurs modèles, Je n'accède pas à la classe du modèle directement dans la vue. J'utilise la fonction de Draper [decorates_finders](https://github.com/drapergem/draper#decorated-finders).

Conclusion
----------

Il peut y avoir de meilleures solutions mais ça marche pour moi. Si vous pensez avoir de meilleures solutions, dites le moi!

Il y a juste une chose que je n'aime pas avec les Presenters. Je n'aime pas instancier un objet dans le contrôleur et le passer à la vue. Cela ne respecte pas la [règle de Sandi Metz](http://robots.thoughtbot.com/post/50655960596/sandi-metz-rules-for-developers). Toutes les règles peuvent être brisées avec des bonnes raisons...
