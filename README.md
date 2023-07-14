# Installation Guide

Install new database from scratch, francais no demo data

## Applications

Depuis le portail d'application installer : 
- Point de vente
- Achat
- Vente

## Modules

Activer les modules supplémentaires : <br>
Pour cela, activer le mode developper depuis la page de `configuration` <br> Aller dans Applications, puis enlever le filtre `Applications` puis taper les modules suivants et les installer
- pos_container
- pos_payment_change
- pos_ticket_logo
- pos_order_mgmt
- pos_order_to_sale_order

## Configuration

### Société

Configurer le nom, l'adresse, site web, email, logo

### Inventaire

Cocher la case `Unitées de mesure` dans la confiugration de l'inventaire, partie `Articles`

Aller dans les unités de mesures et `archiver` tout sauf les `Unité de messure de reférence pour cette catégorie`, c'est a dire `Unité(s)`, `kg`, `Jour(s)`, `m` & `Litre(s)`

### Vente

Aller dans la configuration des ventes, dans la section Tarif, selectionner `plusieurs prix de vente par produits`, puis `prix calculés a partir de formules`. <br>

#### Listes de Prix 

Aller dans la configuration des ventes, dans la section Tarif, selectionner `Listes de prix` <br>
Créer une nouvelle liste de prix :
- Nom `Remise5%`
- Groupes de pays `Europe`
- Dans la liste des éléments de prix, éditer `tous les produits` et mettre une remise de `5%` sur le calcul du prix


### Point de vente (Général)

Aller dans le menu Configuration, puis choisir `Point de Vente`

#### Taxes à la vente par défaut

Ne rien mettre (enlever TVA Collectée)

#### Listes de Prix 

Aller dans la configuration des ventes, dans la section Tarif, s'asssurer que `Listes de prix` est bien selectionné <br>
La liste de prix `Remise5%` doit également etre disponible


### Point de vente (Epicerie)

Aller dans le menu `Point de Vente` et choisir les `...` afin de choisir l'option `Configuration`

#### Nom du point de vente 

Epicerie

#### Méthodes de Paiement

NB : Bien cocher `Utiliser dans le point de vente`

NB2 : Valider les comptes si besoin avec Jerome

| Nom | Type | Code |
| --- | ---- | ---- | 
| Especes | Liquidités | ESP |
| CB | Banque | CB |
| Cheque | Banque | CHQ |

NB Pour Espèces bien mettre les compte de pertes et profits : Onglets `Paramètres Avancés` section `Options d'application Comptabilité` : <br>
Compte de profit : 758000 Produits divers de gestion courante <br>
Compte de perte : 658000 Charges diverses de gestion courante


Une fois tout créer, aller dans la configuration du point de vente et autoriser les 3 moyens de paiement


#### IoTBox  

Activer `IoT Box` dan la configuration du Point de vente <br>
IP 127.0.0.1<br>
select : <br>
- Lecteur de code barre
- Balance
- Tiroir Caisse
- Imprimante de recu

Selectionner également la case `Lecteur de Code-barres` 

Créer une nomenclature de code bare pour les contenants perso. Ouvrir `Default Nomenclature` puis ajouter une ligne avec les éléments suivants : 
- Nom : Container Barcodes
- Type : Unité de contenant
- Modèle de code-barres : 049
- Sequence : 37 
- Encodage : EAN-13

#### Reçu

Cocher la case `Ré-impression du reçu`

Enlever l'`impression automatique du reçu`

Configurer l'entete et pied de page pour le ticket. <br>
Entête :
```
Loub'Epice
Le Bourg
63410 Loubeyrat
```
Pied de page :
```
TVA non applicable - Article 293 B du CGI. 
Merci de votre visite et à bientôt.
```


## Config du controle de caisse 

Aller dans la configuration du point de vente Epicerie:

- Activer l'option `Controle de Caisse'
- Créer une piece de 1 centimes donc valeur = `0,01`
- Associer le mouvement de caisse au point de vente `Epicerie`
- Dans comptabilité, choisir le journal de vente `POS Sales Journal(EUR)`
- Est ce qu'il ne faut pas recréer toutes les pieces billet ????

## Sécurité 

### Désactiver la création de compte client par la page d'acceuil

Aller dans `Parametres généraux`, `Utilisateurs` et enfin `Compte client`
Mettre l'option `Permettre à vos clients de se connecter pour consulter leurs documents` à `On invitation`

### Master Database

Se déconnecter de l'interface puis cliquer  `Manage Databases` puis `Set Master Database`

### Email 

Parametre généraux, 
Serveur de messagerie externe => A cocher
Server de messagerie sortant => A Créer :
- Description : LWS-no-reply
- Server SMTP : mail47.lwspanel.com
- Port SMTP : 465
- S"curité : SSL/TLS
- Username : no-reply@loubepice.fr
- Mot de passe : mettre le mot de passe de messsagerie qui est configuré dans LWS panel 

### Utilisateurs 

Un nouvel utilisateur doit etre créé de cette facon 

- <b>Nom</b> : le nom de la personne => Yannick Levadoux
- <b>Adresse électronique</b> : une vrai adresse mail /!\ ce paramétre est important pour créer des clients dans le point de vente

Vue article a modifier :

Activer le mode developer (Configuration => Activer le mode développeur)

Afficher la vue article > passer en vue liste > ouvrir les outils de dev (la petite bestiole) > Modifier la vue Liste

Remplacer le code par celui ci dessous

```
<?xml version="1.0"?>
<tree string="Product">
                <field name="sequence" widget="handle"/>
                <field name="name"/>
                <field name="list_price" string="Sales Price"/>
                <field name="pos_categ_id"/>
                <field name="type"/>
                <field name="uom_id" options="{'no_open': True, 'no_create': True}" groups="uom.group_uom"/>
                <field name="active" invisible="1"/>
            </tree>
```

******************* Backup INITIAL Fait ici ******************************

Prio 2. 
- Virer le bouton de liste des prix (css jerome)

Prio 3. 
- Identifier les actions derriere les options `Backup` `Duplicate` `Restore Database` afin de savoir si elles sont utiles pour nous ou non


# Notes pour plus tard 

## Config pour les clients (quand on voudra le faire)

Mettre une nomenclature de code barre associées au client pour pouvoir les identifier par code barre (si on leur fait des cartes avec code barre) A mettre en EAN 13 !
Quand on créé un client, lui mettre une liste de prix à 5%

Pour que ce soit accessible dans le point de vente, il faut aller la config du point de vente, section tarif, activer liste de prix et ajouter la `remise 5%`



