# Symfony 3.4

## Certification

Il est possible d'obtenir une certification Symfony en passant un examen proposé par la société Sensio Labs qui développe le framework.

L'inscription et la liste des sujets abordés : [Symfony Certification](https://certification.symfony.com/).

La liste donne une idée précise de tous les sujets à maîtriser pour être un expert en Symfony.

Perdu face à la montagne de choses à apprendre ?
[ThomasBerends](https://github.com/ThomasBerends) maintient une page web qui pointe vers de la documentation pour (quasiment) tous les sujets abordés par la certification Symfony : [Symfony Certification Preparation List](https://thomasberends.github.io/symfony-certification-preparation-list/).

Pour finir, voici le témoignage d'Andrew Marcinkevičius, un développeur (contributeur du framework) qui a passé la certification : [My experience with SensioLabs Symfony certification exam — ifdattic](http://www.ifdattic.com/my-experience-sensiolabs-symfony-certification-exam/).
Je note entre autre ceci :

> made a habit of 90 minutes reading Symfony documentation for breakfast, um, tasty.

## Concepts de base

Ces concepts sont les implémentations du modèle MVC avec Symfony :

- Controller
- Entity
- Template Twig

Ces autres concepts sont indispensables pour développer des applications en Symfony :

- Route
- ORM (object relational mapping)
- EntityManager
- Repository
- Form
- Validation
- Container
- Service

## Installation

Se rendre dans le dossier des projets (Windows) :

    cd C:\wamp64\www

Se rendre dans le dossier des projets (Macos) :

    cd /Applications/MAMP/htdocs

Installer Symfony avec l'installeur `symfony` :

    symfony new my_project 3.4

Aller dans le dossier du projet :

    cd my_project

Lancer le serveur web de développement :

    php bin/console server:run

Note : pour stopper le serveur, appuyer sur `CTRL + C`.

Pour tester l'installation, ouvrir l'url suivante dans un navigateur :

    http://localhost:8000/

Conseil : si vous rencontrer des problèmes à cette étape, consultez « Erreur lors de l'affichage de la home » dans [symfoy-3.4-trouble-shooting.md](symfoy-3.4-trouble-shooting.md).

## Configuration de l'accès à la base de données

Ouvrir le fichier `app/config/parameters.yml` et modifier le bloc :

    database_name: symfony
    database_user: root
    database_password: null

afin d'obtenir :

    database_name: my_project
    database_user: root
    database_password: null

Adapter le `database_user` et le `database_password` avec ceux que vous utilisez quand vous vous connectez à PhpMyAdmin.

Conseils :

- avec MAMP, le `database_user` et le `database_password` par défaut sont tout les deux `root`
- si vous avez une erreur de connexion avec MAMP, consultez « Erreur de connexion à la base de données avec MAMP » dans [symfoy-3.4-trouble-shooting.md](symfoy-3.4-trouble-shooting.md)

## Le `secret token`

Si vous voulez regénérer le `secret_token` de votre fichier `parameters.yml`, le plus simple est d'utiliser l'outil en ligne :

- [Symfony 2 Secret Generator - nux.net](http://nux.net/secret)

### Création de la base de données

#### Avec la console

Interrompre le serveur web avec `CTRL + C` si nécessaire.

Dans le terminal :

    php bin/console doctrine:database:create

Relancer le serveur web avec la commande `php bin/console server:run` si nécessaire.

#### Avec PhpMyAdmin

Avec PhpMyAdmin, créer une nouvelle base de données.

Les noms (BDD, tables, colonnes, etc) ne doivent comporter aucun espace ` `, ni tiret `-`, ni point `.`, ni accent.
Il faut donc se limiter aux caractères minuscules de l'alphabet, aux chiffres et au « underscore » (tiret du bas) `_`.

Pour le nom de la BDD, choisissez le même nom que votre projet, en remplaçant les caractères interdits par des caractères autorisés.

L'interclassement de la BDD doit être `utf8mb4_unicode_ci`.

## Langue

Pour configurer la langue, ouvrir le fichier `app/config/config.yml` et modifier le bloc :

    parameters:
        locale: en

    framework:
        #esi: ~
        #translator: { fallbacks: ['%locale%'] }

afin d'obtenir :

    parameters:
        locale: fr

    framework:
        #esi: ~
        translator: { fallbacks: ['%locale%'] }

## Bundles

Voir [symfony-bundles.md](symfony-bundles.md).

## Bundle `remg/generator-bundle`

Voir [remg-generator.md](remg-generator.md).

## Bundle `doctrine/doctrine-migrations-bundle`

Voir [doctrine-migrations.md](doctrine-migrations.md).

## Entités

### Relations cardinales entre entités

Il existe trois types de relations cardinales entre entités :

- `many to one`
- `one to many`
- `many to many`

Voir [code-style.md](code-style.md) pour les questions de nommage de variable au singulier ou au pluriel.

### Relation `many to one`

![Diagramme de classe Foo Bar](img/class-diagram-foo-m-1-bar.png)

Une entité `Foo` ne peut avoir qu'une seule entité `Bar`.
Mais une entité `Bar` peut avoir plusieurs entités `Foo`.

Exemple : une voiture de fonction ne peut être rattaché qu'à une seule entreprise.
Mais une entreprise peut avoir plusieurs voitures de fonction.

### Relation `one to many`

![Diagramme de classe Bar Foo](img/class-diagram-bar-1-m-foo.png)

C'est la même relation qu'un `one to many` mais du point de vue de l'autre entité.

Exemple : une entreprise peut avoir plusieurs voitures de fonction.
Et une voiture de fonction ne peut être rattaché qu'à une seule entreprise.

### Relation `many to many`

![Diagramme de classe Foo Baz](img/class-diagram-foo-m-m-baz.png)

Une entité `Foo` peut avoir plusieurs entités `Baz`.
Et une entité `Baz` peut avoir plusieurs entités `Foo`.

Exemple : une voiture de fonction peut être réservée (successivement) par plusieurs salariés.
Et un salarié peut réserver (successivement) plusieurs voitures de fonction.

### Configuration de valeurs par défaut

Les valeurs par défaut sont définies dans le constructeur.

Par convention, le constructeur est toujours situé entre le dernier attribut (variable) et la première méthode (fonction).

Si l'on veut que la variable `isDone` de l'entité `Foo` soit initialisée avec la valeur `true` par défaut, il faut ouvrir le fichier `src/Entity/Foo.php` et ajouter ou modifier le constructeur :

    private $bar;                   // <= dernier attribut

    /**
     * Constructor
     */
    public function __construct()   // <= constructeur
    {
        $this->isDone = true;
    }

    /**
     * Get id
     *
     * @return integer
     */
    public function getId() {       // <= première méthode

## Interface front-end

L'interface est composée de contrôleurs, de form types et de templates Twig.

### Génération

Pour générer un CRUD :

    php bin/console generate:doctrine:crud

Pour créer les interfaces de création, de mise à jour et de suppression, répondre `y` à la question `Do you want to generate the "write" actions`.

Astuce : si vous voulez regénérer un CRUD parce que votre entité a évolué, vous pouvez utiliser l'option `--overwrite` qui écrase celui-qui a déjà été créé.
Mais attention, toutes les customisation précédentes seront perdues !

    php bin/console doctrine:generate:crud --overwrite

### Correction du form type généré

Attention : le générateur n'implémente par correctement les champs de type entité dans le form type. Si votre entité contient une ou des associations à d'autres entités, vous devez corriger le form type vous-même.

### Association `many to one`

Cette correction s'applique si l'entité `Foo` possède une association de type `many to one` avec l'entité `Bar` :

![Diagramme de classe Foo Bar](img/class-diagram-foo-m-1-bar.png)

Ajouter le `use` suivant dans le fichier `src/AppBundle/Form/FooType.php` :

    use Symfony\Bridge\Doctrine\Form\Type\EntityType;

Puis modifier le bloc suivant dans le même fichier :

    $builder->add('lorem')->add('ipsum')->add('bar');

afin d'obtenir :

    $builder
        ->add('lorem')
        ->add('ipsum')
        ->add('bar', EntityType::class, [
            'class' => Bar::class,
            'choice_label' => 'name',
        ])
    ;

Note : pour en savoir plus sur les options des champs de type entité, voir [EntityType Field (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/forms/types/entity.html).

### Association `many to many`

Cette correction s'applique si l'entité `Foo` possède une association de type `many to many` avec l'entité `Baz` :

![Diagramme de classe Foo Baz](img/class-diagram-foo-m-m-baz.png)

Ajouter le `use` suivant dans le fichier `src/AppBundle/Form/FooType.php` :

    use Symfony\Bridge\Doctrine\Form\Type\EntityType;

Puis modifier le bloc suivant dans le même fichier :

    $builder->add('lorem')->add('ipsum')->add('baz');

afin d'obtenir :

    $builder
        ->add('lorem')
        ->add('ipsum')
        ->add('baz', EntityType::class, [
            'class' => 'AppBundle:Baz',
            'choice_label' => 'name',
            'multiple' => true,
            'required' => false,
        ])
    ;

Attention : Si nécessaire, cette correction doit être appliquée de la même façon dans le form type (fichier `src/AppBundle/Form/BazType.php`) de l'entité `Baz`.

Note : pour en savoir plus sur les options des champs de type entité, voir [EntityType Field (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/forms/types/entity.html).

## Templates Twig

Pour en savoir plus, voir [twig.md](twig.md).

### Modification des templates

Si l'entité `Foo` possède une association de type `one to many` avec l'entité `Bar`, on peut afficher la liste des « foos » dans la page de détail d'un « bar ».

Ouvrir le fichier `app/Resources/views/bar/show.html.twig` et ajouter le bloc :

    <table>
        <thead>
            <tr>
                <th>Foo</th>
            </tr>
        </thead>
        <tbody>
            {% for foo in bar.foos %}
            <tr>
                <td><a href="{{ path('foo_show', {id: foo.id}) }}">{{ foo.lorem }} {{ foo.ipsum }}</a></td>
            </tr>
            {% endfor %}
        </tbody>
    </table>

Si l'entité `Bar` possède une association de type `many to one` avec l'entité `Foo`, on peut afficher la liste le « bar » dans la page de détail d'un « foo ».

Ouvrir le fichier `app/Resources/views/foo/show.html.twig` et modifier le bloc :

    <table>
        <tbody>
            <tr>
                <th>Id</th>
                <td>{{ task.id }}</td>
            </tr>
            <tr>
                <th>Lorem</th>
                <td>{{ foo.lorem }}</td>
            </tr>
            <tr>
                <th>Ipsum</th>
                <td>{{ foo.ipsum }}</td>
            </tr>
            <tr>
                <th>Bar</th>
                <td>{% if foo.bar %}<a href="{{ path('bar_show', {id: foo.bar.id}) }}">{{ foo.bar.lorem }} {{ foo.bar.ipsum }}</a>{% endif %}</td>
            </tr>
        </tbody>
    </table>

### Intégration de Bootstrap et jQuery

#### Configuration des formulaires

Pour afficher les formulaire en mode Bootstrap, ouvrir le fichier `app/config/config.yml` et modifier le bloc :

    # Twig Configuration
    twig:
        debug: '%kernel.debug%'
        strict_variables: '%kernel.debug%'

afin d'obtenir :

    # Twig Configuration
    twig:
        debug: '%kernel.debug%'
        strict_variables: '%kernel.debug%'
        form_themes:
        - 'bootstrap_3_layout.html.twig'

#### Intégration des fichiers

Plutôt que de télécharger les fichiers, nous allons utiliser un CDN pour intégrer Bootstrap et jQuery.

Pour intégrer les fichiers CSS de Bootstrap dans toutes les pages, ouvrir le fichier `app/Resources/views/base.html.twig` et modifier le bloc :

    <head>
        <meta charset="UTF-8" />
        <title>{% block title %}Welcome!{% endblock %}</title>
        {% block stylesheets %}{% endblock %}
        <link rel="icon" type="image/x-icon" href="{{ asset('favicon.ico') }}" />
    </head>

afin d'obtenir :

    <head>
        <meta charset="UTF-8" />
        <title>{% block title %}Welcome!{% endblock %}</title>
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" integrity="sha384-rHyoN1iRsVXV4nD0JutlnGaslCJuC7uwjduW9SVrLvRYooPp2bWYgmgJQIXwl/Sp" crossorigin="anonymous">
        {% block stylesheets %}{% endblock %}
        <link rel="icon" type="image/x-icon" href="{{ asset('favicon.ico') }}" />
    </head>

Pour intégrer les fichiers JavaScript de Bootstrap et jQuery dans toutes les pages, ouvrir le fichier `app/Resources/views/base.html.twig` et modifier le bloc :

    <body>
        {% block body %}{% endblock %}
        {% block javascripts %}{% endblock %}
    </body>

afin d'obtenir :

    <body>
        {% block body %}{% endblock %}
        <script src="https://code.jquery.com/jquery-3.2.1.min.js" integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4=" crossorigin="anonymous"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
        {% block javascripts %}{% endblock %}
    </body>

## Validation

### Désactivation de la validation côté client

Pour tester la validation côté serveur, il est possible de désactiver la validation côté client (dans le navigateur web).

Pour désactiver la validation côté client de l'entité `Foo`, ouvrir le fichier `Form/FooType.php` et modifier la fonction `configureOptions()` du form type afin d'obtenir :

    public function configureOptions(OptionsResolver $resolver)
    {
        $resolver->setDefaults([
            'attr'=> ['novalidate' => 'novalidate'],
            'data_class' => 'AppBundle\Entity\Foo'
        ]);
    }

Attention : veiller à adapter le nom de l'entité (`AppBundle\Entity\Foo` dans l'exemple).

## Bundle `javiereguiluz/easyadmin-bundle`

Voir [easyadmin.md](easyadmin.md).

## Commandes

Lancer le serveur web de développement :

    php bin/console server:run

Note : pour stopper le serveur, appuyer sur `CTRL + C`.

Afficher la liste des routes disponibles :

    php bin/console debug:router

Afficher la liste des services disponibles dans le container :

    php bin/console debug:container

Afficher la correspondance entre les services et le nom des classes PHP qui peuvent être utilisés en « type hinting » :

    php bin/console debug:autowiring

Afficher la liste des events et leur degré de priorité :

    php bin/console debug:event-dispatcher

## Doc

- [Symfony 3.4 Documentation](https://symfony.com/doc/3.4/index.html)

### Install

- [Installing & Setting up the Symfony Framework (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/setup.html)

### Entity

- [Databases and the Doctrine ORM (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/doctrine.html)
- [5. Association Mapping — Doctrine 2 ORM 2 documentation](http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/association-mapping.html)
- [Class Doctrine\Common\Collections\ArrayCollection | Collections](http://www.doctrine-project.org/api/collections/1.3/class-Doctrine.Common.Collections.ArrayCollection.html)
- [Remg/GeneratorBundle: Code generation tools for Symfony3.](https://github.com/Remg/GeneratorBundle)
- [DoctrineMigrationsBundle (The Symfony Bundles Documentation)](http://symfony.com/doc/current/bundles/DoctrineMigrationsBundle/index.html)

### Form

- [Form Types Reference (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/forms/types.html)
- [EntityType Field (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/forms/types/entity.html)
- [Validation Constraints Reference (Symfony 3.4 Docs)](http://symfony.com/doc/3.4/reference/constraints.html)

### Route

- [Routing (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/routing.html)
- [SensioFrameworkExtraBundle (Symfony Bundles Docs)](https://symfony.com/doc/master/bundles/SensioFrameworkExtraBundle/index.html)

## Bootstrap et jQuery

- [twig - What is the correct way to add bootstrap to a symfony app? - Stack Overflow](https://stackoverflow.com/questions/36453039/what-is-the-correct-way-to-add-bootstrap-to-a-symfony-app)
- [Bootstrap · The world's most popular mobile-first and responsive front-end framework.](https://getbootstrap.com/docs/3.3/)
- [jQuery](http://jquery.com/)
- [Getting started · Bootstrap](https://getbootstrap.com/docs/3.3/getting-started/)
- [jQuery CDN](https://code.jquery.com/)

## Service

- [Service Container (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/service_container.html)
- [How to Create Service Aliases and Mark Services as Private (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/service_container/alias_private.html)
- [Introduction to Parameters (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/service_container/parameters.html)
- [Service Method Calls and Setter Injection (Symfony 3.4 Docs)](https://symfony.com/doc/3.4/service_container/calls.html)
