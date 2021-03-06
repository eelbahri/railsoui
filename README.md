# Cours Rails

###### Remember

 - rails s

### 21 mars 2017

 - Ajout de bootstrap-sass
 - Ajout de meta dans les vues
 - CRUD complet
 - link_to


### 22 mars 2017

 - Migration sur un modèle existant
 ```sh
 $ bundle exec rails generate migration AddAttributesToPokemons
 $ bundle exec rails db:migrate
 ```
  - Modification des formulaires en conséquences
  - Association Pokemon - Type
  ```sh
  $ rails generate scaffold Type name:string color:string --skip --skip-test-framework --skip-assets
  $ bundle exec rails db:migrate
  ```

### 11 avril 2017

 - Mise en place du style pour toutes les vues "types"
 - Lien entre les pokemons et les types avec association belongs_to
 ```sh
 $ bundle exec rails g migration AddTypeToPokemon type:belongs_to
 $ bundle exec rails db:migrate
```
 - Modification en conséquences des models

 - Utilisation de la console en mode bac à sable -> Permet d'exécuter qui seront supprimés à la fin de la session
 ```sh
 $ bundle exec rails console --sandbox
 ```

 - Création d'un pokemon et d'un type en une seule ligne de commande :
 ```sh
 > Pokemon.create(name: 'Toto', type: Type.create(name: 'Tutu'))
 ```
 - Pour vérifier :
 ```sh
 > Pokemon.last.type.name
 ```

 - Ajout de ```sle_form' ``` au Gemfile (redémarrer le serveur)
 ```sh
 $ bundle install
 $ bundle exec rails generate simple_form:install --bootstrap
 ```

 - Ajout des types dans le formulaire de pokemons (récuèpere directement les types existant et en fait une liste déroulante)
 - Création d'un helper pour les badges des types de pokemons
 - Performance : Amélioration de la requête sur l'index pokemon
 - Pagination : Ajout de 2 gem (will_paginate & will_paginate-bootstrap), modification de l'index dans le controller et la vue
 - Traduction de la librairie de pagination (config/initializers/locale.rb  + config/initializers/locales/fr)

 - Génération des fichiers pour les combats de pokémons
 ```sh
 $ bundle exec rails generate scaffold Move name:string description:string type:references --skip --skip-test-framework --skip-assets
 $ bundle exec rails db:migrate
 ```

### 12 avril 2017

 - Mise en style des vues "moves"
 - Création d'un modèle pour relier les attaques aux pokemons :
 ```sh
 $ bundle exec rails g model PokemonMove pokemon:references move:references --skip-test-framework
 $ bundle exec rails db:migrate
 ```
 - Edition des modèles Move et Pokemon pour les lier au nouveau modèle PokemonMove
 - Test pour vérifier le bon fonctionnement : (```$ bundle exec rails console --sandbox```)
 ```sh
 t = Type.create name: 'Electrique', color: '#FFF168'
 m = Move.create name: 'Eclair', type: t
 p = Pokemon.create name: 'Pikachu', type: t, moves: [m]
 Pokemon.last.moves.last.type.color
 ```
 Le résultat doit renvoyer le couleur de l'attaque, ici : #FFF168
 - Ajout des attaques dans le formulaire Pokemon et son affichage dans les vues
 - Factorisation : Création d'un partial pour l'affichage des attaques :_ _moves_list.html.erb_
 - Modification de l'helper types_helper : Amélioration et ajout du lien vers le badge du type correspondant

### 6 juin 2017

 - Les flash notices (voir pokemon/update + layout html pour l'affichage)
 - DEVISE : gem solution d'authentification
 gemfile
 ```sh
 gem 'devise'
 gem 'devise-i18n'
 ```

 command : (ne pas oublier de redemarrer le serveur)
 ```sh
 $ bundle install
 $ bundle exec rails generate devise:install
 $ bundle exec rails generate devise User
 $ bundle exec rails db:migrate
 $ bundle exec rails generate devise:i18n:views
 ```

### 7 juin 2017

 - Paperclip : upload d'images
 ```sh
 gem 'Paperclip'
 ```
 command :
 ```sh
 $ bundle install
 $ bundle exec rails g paperclip pokemon avatar
 $ bundle exec rails db:migrate
 ```
 - Validations
 ```sh
 $ bundle exec rails g migration add_username_to_users username:string:uniq
 $ bundle exec rails db:migrate
 ```
 - Rôles et permissions : Rolify & cancancan
 gemfile :
 ```sh
 gem 'cancancan'
 gem 'rolify'
 ```
 command :
 ```sh
 $ bundle install
 $ bundle exec rails g cancan:ability
 $ bundle exec rails g rolify Role User
 $ bundle exec rails db:migrate
 ```
