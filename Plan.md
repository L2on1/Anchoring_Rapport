# Plan détaillé : Anchoring

- [Plan détaillé : Anchoring](#plan-détaillé--anchoring)
  - [Résumé (Français)](#résumé-français)
  - [Résumé (Anglais)](#résumé-anglais)
  - [Introduction](#introduction)
  - [I - Présentation du Projet](#i---présentation-du-projet)
    - [1) Cadre du projet en entreprise](#1-cadre-du-projet-en-entreprise)
      - [a) Présentation de l'entreprise](#a-présentation-de-lentreprise)
      - [b) Présentation de l'environnement du projet](#b-présentation-de-lenvironnement-du-projet)
    - [2 - Objectif du projet](#2---objectif-du-projet)
      - [a) Projet de recherche](#a-projet-de-recherche)
      - [b) Domaine du projet](#b-domaine-du-projet)
    - [3) Présentation du projet : Ancrage Sémantique (Anchoring)](#3-présentation-du-projet--ancrage-sémantique-anchoring)
      - [a) Présentation rapide](#a-présentation-rapide)
      - [b) Terminogy](#b-terminogy)
      - [c) Presentation Anchoring avec remplissage d'une ontologie](#c-presentation-anchoring-avec-remplissage-dune-ontologie)
      - [d) But du projet](#d-but-du-projet)
  - [II - Recherche du moteur ontologique](#ii---recherche-du-moteur-ontologique)
    - [1) Choix du OWL](#1-choix-du-owl)
    - [2) Qu'est ce qu'on veut faire avec notre moteur ontologique](#2-quest-ce-quon-veut-faire-avec-notre-moteur-ontologique)
    - [3) Protege](#3-protege)
    - [4) OWLAPI / Jena](#4-owlapi--jena)
    - [5) Ontologenius](#5-ontologenius)
    - [6) Owlready2](#6-owlready2)
    - [7) KnowRob](#7-knowrob)
    - [8) Autre moteur d'ontologie](#8-autre-moteur-dontologie)
    - [7) Qu'est ce qu'on peut améliorer ?](#7-quest-ce-quon-peut-améliorer-)
  - [III - Algorithme](#iii---algorithme)
    - [1) Presentation Algorithme](#1-presentation-algorithme)
      - [a) Algoritme Simple Septembre](#a-algoritme-simple-septembre)
      - [b) Algoritme Linéaire](#b-algoritme-linéaire)
    - [2) Algoritme : Ontology Manager](#2-algoritme--ontology-manager)
      - [a) Besoins](#a-besoins)
      - [b) Presenatation de l'algoritme](#b-presenatation-de-lalgoritme)
      - [c) Réduction de Ontology Manager dans le cadre du projet](#c-réduction-de-ontology-manager-dans-le-cadre-du-projet)
    - [3) Algorithme réduit : Algoritme implémenté](#3-algorithme-réduit--algoritme-implémenté)
      - [a) Explications de l'algoritme](#a-explications-de-lalgoritme)
      - [b) SYSML](#b-sysml)
      - [c) Implementation ROS Squidly](#c-implementation-ros-squidly)
  - [IV - Implementation](#iv---implementation)
    - [1) Design](#1-design)
      - [a) Role du design](#a-role-du-design)
      - [b) Choix de l'Upper Ontologie](#b-choix-de-lupper-ontologie)
      - [c) Présentation de SOMA](#c-présentation-de-soma)
      - [d) Modification de SOMA](#d-modification-de-soma)
      - [e) Ajout du Mid et du Domain](#e-ajout-du-mid-et-du-domain)
        - [i) Mid ajout de la compatibilité avec les autres Node](#i-mid-ajout-de-la-compatibilité-avec-les-autres-node)
        - [ii) Domain](#ii-domain)
      - [f) Comment le Design/domain est fait ?](#f-comment-le-designdomain-est-fait-)
    - [2) Setup](#2-setup)
      - [a) Role du setup](#a-role-du-setup)
      - [b) Comment le populator est fait ?](#b-comment-le-populator-est-fait-)
    - [3) Run Time](#3-run-time)
      - [a) Role du run time](#a-role-du-run-time)
      - [b) Integrateur](#b-integrateur)
        - [i) Role de l'intégrateur](#i-role-de-lintégrateur)
        - [ii) Comment l'intégrateur est fait ?](#ii-comment-lintégrateur-est-fait-)
      - [c) Rulemaker](#c-rulemaker)
        - [i) Role du rulemaker](#i-role-du-rulemaker)
        - [ii) Comment le rulemaker est fait ?](#ii-comment-le-rulemaker-est-fait-)
      - [d) Reasonner](#d-reasonner)
        - [i) Role du reasonner](#i-role-du-reasonner)
        - [ii) Comment le reasonner est fait ?](#ii-comment-le-reasonner-est-fait-)
      - [e) Reader](#e-reader)
        - [i) Role du reader](#i-role-du-reader)
        - [ii) Comment le reader est fait ?](#ii-comment-le-reader-est-fait-)
  - [V - Apprentissages, critiques, autres chose à explorer](#v---apprentissages-critiques-autres-chose-à-explorer)
    - [1) Ontologies](#1-ontologies)
    - [2) SWRL](#2-swrl)
    - [3) Moteur Ontologique](#3-moteur-ontologique)
    - [4) Algorithme](#4-algorithme)
    - [5) Design](#5-design)
    - [6) Setup](#6-setup)
    - [7) Intergrator](#7-intergrator)
    - [8) RuleMaker](#8-rulemaker)
    - [9) Reasonner](#9-reasonner)
    - [Moi-même](#moi-même)
  - [Conclusion](#conclusion)

## Résumé (Français)

Ancrage symbolique (Anchoring) processus qui transforme des données de Jumeaux numériques (fonctionnel ou géométrique) en une sémantique intelligente à l'aide d'une ontologie. Cette ontologie peut être lue pour donner des prédicats sous la forme *Sujet* *Action* *Complément*. De plus, grâce à un reasonner ontologique on peut vérifier la cohérence de l'ontologie, faire des inférences (créer des relations) et appliquer des règles. On peut ainsi dire que l'Anchoring raisonne et donc que c'est un processus intelligent. Grâce à l'Anchoring, l'ontologie est un savoir sémantique sur lequel d'autres applications peuvent interagir. Dans mon cas d'application, la robotique cognitive, le Task Planner et le jumeau numérique (Digital Twin) interagissent avec l'ontologie. Les Digital Twins permettent de remplir l'ontologie grâce à l'Anchoring (intégrateur), et l'Anchoring peut demander aux Digital Twins des informations supplémentaires si nécessaire. Le Task Planner reçoit les prédicats que fournit l'Anchoring pour créer une liste d'action pour résoudre un objectif. L'Anchoring est temps réel et donc peut s'apercevoir de problème dans les actions. De plus, l'Anchoring est basé sur une ontologie qui est créé avec une upper ontologie pour permettre à ce que ce savoir sémantique soit partageable.

## Résumé (Anglais)

Symbolic anchoring (Anchoring) process that transforms data from Digital Twins (functional or geometric) into intelligent semantics using an ontology. This ontology can be read to give predicates in the form *Subject* *Action* *Complement*. Futhermore, an ontology reasonner can be used to check the consistency of the ontology, make inferences (create relations) and apply rules.Anchoring can therefore be considered an intelligent process. Thanks to Anchoring, the ontology is a semantic knowledge on which other applications can interact. In my case study, cognitive robotics, the Task Planner and the Digital Twin interact with the ontology. The Digital Twins fill in the ontology thanks to the Anchoring (integrator), and the Anchoring can request additional information from the Digital Twins if necessary. The Task Planner receives the predicates provided by the Anchoring to create an action list to solve a goal. Anchoring is real-time, so it can spot problems in actions. Futhemore, Anchoring is based on an ontology that is created with an upper ontology to enable this semantic knowledge to be shared.

## Introduction

Bla bla

mots-clés :

- congnitive Robotics
- Robot intélligent
- Symbolique
- Sémantique
- Ontologie
- Raisonnement
- Probleme complexe
- Ancrage
- Ancrage Symbolique

## I - Présentation du Projet

### 1) Cadre du projet en entreprise

#### a) Présentation de l'entreprise

- CEA : centre de recherche français
- EPIC : transfert technologique
- Projet européen
- Projet interne

#### b) Présentation de l'environnement du projet

- Projet européen
- Projet interne
- LSEA
- SQUIDLY
- Intelligence Robot Team
- moonshot

### 2 - Objectif du projet

#### a) Projet de recherche

- Découvrir les technologies dans le domaine des ontologies et des règles ontologiques (SWRL)
- Découvir comment faire de la robotique avec des ontologies
- Faire des conclusions sur les technologies et les pistes recherchercher les points positifs et les points négatifs
- Prémier itération
- Faire des erreurs pour apprendre
- Faire un premier Anchoring

#### b) Domaine du projet

- SQUIDLY : architechture généraliste pour la robotique
- Robotique cognitive
  - Qu'est ce que c'est ?
  - Qu'est ce que cela implique ?
- Lier des données geometrique et functionnel (jumeaux numérique) à une base de données Symbolique et donc SÉMANTIQUE pour faire des raisonnements functionnels et donc intélligence

### 3) Présentation du projet : Ancrage Sémantique (Anchoring)

#### a) Présentation rapide

- Schéma : Perception -> Planning -> Acting
  - ![Schéma : Perception -> Planning -> Acting](figures/Algo_simple_global.png)
  - Perception : Capteurs -> **Jumeaux numériques** (Géométrique/Fonctionnel)
  - Plannification : ***Anchoring*** -> **Task Planner**
  - Action : Controller -> Actuators
- Expliquer ce schéma :
  - Rôle de l'Anchoring
  - Place de l'Anchoring

- Schéma : DT -> Anchoring -> Task Planner
  - ![Schéma : DT -> Anchoring -> Task Planner](figures/Algo_simple_local.png)
  - Anchoring -> Knowledge Base : Ontology + rules
- Expliquer ce schéma

#### b) Terminogy

- Digital Twin - jumeaux numérique
  - Qu'est ce que c'est ?
  - DT géométrique + exemples
  - DT fonctionnels + exemples
- Task Planner - Planneur de taches
  - Qu'est ce que c'est ?
  - Exemples
- Ontologies (Dévellopper cette partie)
  - Qu'est ce que c'est ?
  - Taxonomies associés voir taxonomie septembre
  - Exemples
  - Reasonner
  - Inferences
  - Illustrations

#### c) Presentation Anchoring avec remplissage d'une ontologie

- Schema avec ontologie
  - Design
  - Setup
  - Run
- Schema ontologie : class - individues (10/10/23)
  - ![ontologies Tree](figures/Anchoring_Algorithm-Tree.png)
  - ![ontologie region](figures/ontology_region.png)
- Grace au reasonner et au regles -> IA

#### d) But du projet

- Schema démonstration RUN 3IA
- Début
  - ![Run 3IA Init](figures/Run3IA_Init.png)
- Objectif
  - ![Run 3IA Goal](figures/Run3IA_Mission.png)
- Explications
- Comment elle a été creer ?
- List des diffenetes objects individues -> Classes
- Listes des differentes chose a faire attention : Data Properties, Objectes Properties, rules
  - ![Run 3iA Exemple](figures/Run3IA_Example.png)
- Explications
- Schema FeedBack Loop
  - ![Run 3iA FeedBack](figures/Run3IA_FeedbackLoop.png)
- Explications
- Demonstration avec le Digital Twin XDE : Vidéos
  - Demander la vidéo à Raphaël

## II - Recherche du moteur ontologique

### 1) Choix du OWL

- Autre choix que le OWL :
  - OntoUML
  - First Order Logic
  - ....
- Facile
- Protege
- Upper Ontologie
- Communauté

### 2) Qu'est ce qu'on veut faire avec notre moteur ontologique

- Manipuler l'ontologie (S'appuyer sur les schémas du haut)
  - lire les classes / individues / Data Properties / Object Properties
  - Créer des classes / individues / Data Properties / Object Properties
- Raisonner
- Appliquer les regles SWRL
- Pouvoir etre utiliser avec ROS
- Rapidité pour robot

### 3) Protege

- ![Protege avec Soma]()
- Atouts
  - visualisation
  - creation
  - on peut tout faire
  - Plugin
  - SWRL
  - ...
- Defauts
  - Programme -> pas ROS
- Conclusion
  - Programme majeur dans le devellopement d'une ontologie

### 4) OWLAPI / Jena

- Atouts
  - Api sur lequel repose Protege
  - La plus utilise pour manipulé du OWL
- Defauts
  - JAVA
- Conclusion
  - NON

### 5) Ontologenius

- ![Logo Ontolegenius]()
- Atouts
  - Developper par un proche de la team
  - penser pour la robotique
  - Rapide (Benchmark)
  - Python / C++
  - ROS2
  - Developpement Actif
  - Devloppement sous forme de plugin => contribution "facile"
- Defauts
  - Pas de SWRL
  - Manque de reasonner avec SWRL
  - Documentation pas super clair
  - ...
- Conclusion
  - Une solution a garder l'oeil
  - Pas garder par manque de SWRL
  
### 6) Owlready2

- ![Logo Owlready2]()
- Atouts
  - Python
  - Actif dans le developpment
  - Reasonner pellet/HermiT
  - Rapide (Benchmark)
  - On peut tout faire
  - Documentation + livre
- Defauts
  - Pas assez rapide a cause du reasonner java pellet
  - Documentation + livre incomplete das mon use case
- Conclusion
  - Moteur Ontologique garder garce au python, sa simplicité, et reasonner pellet

### 7) KnowRob

- Atouts
  - Université Bremen
  - relire Article
- Defauts
  - relire Article
- Conclusion
  - NON

### 8) Autre moteur d'ontologie

- Cowl
  - ![Logo Cowl]()
  - Atouts
    - lightweight
    - C++
    - -developpemet actif
  - Defauts
    - Pas de reasonner du tout
  - Conclusion
    - NON
- ORO
  - ![Logo ORO]()
  - Atouts
    - CNRS Sarthou
    - C++
  - Defauts
    - Pas de reasonner du tout
    - Plus dev depuis 2012
  - Conclusion
    - NON
  - Armor RDS
    - Atout
      - Penser Ros Docker
    - Defauts
      - Plus Dev
      - Juste un Test
      - Pas de raisonner
    - NON

### 7) Qu'est ce qu'on peut améliorer ?

À mettre à la fin plutôt

## III - Algorithme

### 1) Presentation Algorithme

#### a) Algoritme Simple Septembre

- Algoritme Global
  - ![Algorithme Global](figures/Algo_simple_global.png)
- Algortime Local (Duplex DT)
  - ![Algoritme Local](figures/Algo_simple_local.png)
- Algoritme Micro
  - ![Algoritme micro (à modifier)](figures/Algo_simple_micro.png)
- Comment on ils été pensé ?
- Quels sont les problemes ?

#### b) Algoritme Linéaire

- Extension des premier algoritme avec la notion d'ontologie
- Comment a il été pensé ?
- Quels sont les problemes ?

### 2) Algoritme : Ontology Manager

- ![Ontology manager Schematics Full](figures/Ontology_Manager_Full.png)

#### a) Besoins

- Squidly Compatible
  - ![Squidly](figures/Squidly.png)
- MAPE-K compatible
  - ![MAPE-K](figures/MAPEK.png)
- Decentralisation
- Penser Node Ros + Behavior Tree
- Modulable
  - learner
  - viewpoint
  - substracter
- Les point d'améliorations
  
#### b) Presenatation de l'algoritme

- Schéma voir au dessus
- Superposition des bloc = dependance plugin
- Explications (Bien détailler)

#### c) Réduction de Ontology Manager dans le cadre du projet

- Pourquoi?
- Quels Blocs Garder
- Schema :
  - ![Ontology manager Schematics Reduit](figures/Ontology_Manager_Red.png)

### 3) Algorithme réduit : Algoritme implémenté

#### a) Explications de l'algoritme

- Design
- Setup
- Run

#### b) SYSML

- Annexe
- Comment a il été creer ?
- [Le mettre a jour ?]

#### c) Implementation ROS Squidly

- Diagramme de sequence
- Explication du knowlege domain
- Explication des path
- Explication du overwrite etc...

## IV - Implementation

L'implementation a été fait dans un premier temos avec des script python independent de ros/Squidly puis ensuite implemente dedans.

### 1) Design

#### a) Role du design

- Creer une ontologie de base qui a :
  - une base ontologique
  - qui convient à la situations
  - qui convient au domaine
  - qui convient aux node utiliséq
- Montrer le bloc Design
  - [Ontology manager Design](figures/Ontology_Manager_Design.png)
- Expliquer les étapes

#### b) Choix de l'Upper Ontologie

- C'est quoi une upper ontologie ?
- Pourqupi ?
- Listes des Upper Ontologies :
  - IEEE1872
    - Description : +/-
  - SUMO
    - Description : +/-
  - ORO (ground)
    - Description : +/-
  - UFO
    - Description : +/-
  - SkiRos2
    - Description : +/-
  - DUL
    - Description : +/-
  - DOLCE
    - Description : +/-
  - ROBOTML
    - Description : +/-
  - ROSETTA
    - Description : +/-
  - SOMA
    - Description : +/-
- Je choisi SOMA car ...

#### c) Présentation de SOMA

- Université de Bremen
- Laboratoir de IA
- Cognitive Robotic
- Social
- Goal
- ...

#### d) Modification de SOMA

- Refait SOMA en enlevant des relation car marche pas avec pellet penser pour HermiT
- Substract avec seulement ce dont j'ai besoin
- Toujours compatible Soma

#### e) Ajout du Mid et du Domain

##### i) Mid ajout de la compatibilité avec les autres Node

- Compatiblité avec le DT
- Compatibilité avec le Task Planner

##### ii) Domain

- Ajout pour le domain robotics

Transition : Ajout des individues -> Setup + Run

#### f) Comment le Design/domain est fait ?

- Design fait à la main avec protege
- Load par l'ontology manager lors du ros launch

### 2) Setup

#### a) Role du setup

- Populer l'ontologie avec :
  - les individue initial
  - les configuration des nodes associé :
    - Digital Twin
    - Task Planner
  - Alias

#### b) Comment le populator est fait ?

- Parsing de fichier json prélablement fait
- Ajout avec le moteur ontologique
- Ontology manager lors du ros launch ou peut etre activer a n'import quelle moment
- Explication de processus dans le code avec owlready2

### 3) Run Time

#### a) Role du run time

- Etre run time pour que l'ontology représente en live le réel

#### b) Integrateur

##### i) Role de l'intégrateur

- C'est un eu juste ça l'"anchoring"
- Remplir l'ontologie avec des data propertie que le DT lui donne
- Puis les faire valider par l'ontology manager pour qui infere pour creer des relations

##### ii) Comment l'intégrateur est fait ?

- parsing d'un message ros
- Ajout dans l'ontology avec le moteur ontologique
- Parler du processus dans le code avec les Alias/ID
- Parler du processus dans le code pour ecraser les Data properties
- Parler du processus dans le code pour que ce soit automatique peut importe la properties a remplir si il y a bien un alias
- Puis les faire valider par l'ontology manager pour qui infere pour creer des relations

#### c) Rulemaker

##### i) Role du rulemaker

- Ajout de regles python pour palier au probleme du SWRL
  
##### ii) Comment le rulemaker est fait ?

- recherche et ajout dans l'ontology avec owlready 2
- Parler des regles
- Parler du processus de supression de propriété fausse

#### d) Reasonner

##### i) Role du reasonner

- assure la coherence de l'ontology
- infere
- Applique les SWRL rules
- pellet / Hermit

##### ii) Comment le reasonner est fait ?

- Ligne python avec owlready2

#### e) Reader

##### i) Role du reader

- Transformer les fait sémantique de l'ontologie en predicat pour un Task Planner
- peut transforer pour plusieurs Task Planner PDDL / HRT / ...

##### ii) Comment le reader est fait ?

- Parsing avec owlready 2 : [[ObjectProperty, Subject, Object], ...]

## V - Apprentissages, critiques, autres chose à explorer

### 1) Ontologies

- Apprentissages :
  - Domain ou l'IRT a appris et mis en pratique
  - Application de Semantique dans la robotique
  - relation symbolique
- Critiques :
  - Il faut etre expert en ontologies
    - j'ai moi meme pas ete rigoureux pour des expert en ontologie avec soma
  - interaction avec compliqué
  - domain tres vasste et donc peu clair
  - Schema : 2 ontologie differentes meme signification
- Autres chose à explorer :
  - Algo pour chaque regle (revient a ce qu'on fait avec rule maker) + SQL/DataLake

### 2) SWRL

- Apprentissages :
  - Pas de négations
  - pas de division dans aucun des reasonner
  - dependant du reasonner
  - peu utilisé par la communaute
  - déconseille de l'utilse par la communauté
  - long pour ecrire des regle
  - pas de var tmp
  - pas possble d'ajouter et/ou d'écraser des Data properties
- Critiques :
- Autres chose à explorer :
  - Biblioteque de regle python en axioms

### 3) Moteur Ontologique

- Apprentissages :
  - Owlready super bien mais utilise pellet qui est lent
- Critiques :
- Autres chose à explorer :
  - Maintenant plus de SWRL + maj de ontologenius pour ROS 2 => essayé Ontologenius

### 4) Algorithme

- Apprentissages :
- Critiques :
- Autres chose à explorer :

### 5) Design

- Apprentissages :
- Critiques :
  - Pas automatique
  - Pas import
  - En un seul bloc
- Autres chose à explorer :
  - Le faire automatiquement en dennat une UPPER les differentes nodes ros pour definir les domain ontologies a prendre et le tout avec des import

### 6) Setup

- Apprentissages :
- Critiques :
- Autres chose à explorer :
  - Creer les ficheir de setup de node automatiquement
  - Creer le fichier des setup de la situation automatiquement

### 7) Intergrator

- Apprentissages :
- Critiques :
- Autres chose à explorer :

### 8) RuleMaker

- Apprentissages :
- Critiques :
- Autres chose à explorer :

### 9) Reasonner

- Apprentissages :
  - Pellet c'est bien
  - pellet c'est puissant
  - Pellet necessaire pour SWRL
- Critiques :
- Autres chose à explorer :
  - On pourrait essayer Hermit ou essayer un resonner tiers plus rapide => Ontologenius

### Moi-même

- Apprentissages :
  - la recherche
  - le langage formel
  - la semantique
  - entreprise
  - cognitive robotique
- Critiques :
  - mauvaise estimation du temps
- Autres chose à explorer :

## Conclusion
