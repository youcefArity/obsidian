## TODO
- [ ] Regarder la video sur 1food1me
- [ ] sub to Yt channel

---
# Vitrine
## Intro
> Notre offre ne vise pas à faire maigrir
- [ ] perso les changements fondés sur un bilan (app objective)
- [ ] facile a mettre en place accessible
- [ ] mesurer l'impact de ses changement

---

# Front-end
### Programmes
| Programme de 3 mois | Programme de 3 mois + Bilan de Contrôle |
| - | :- |
| Bilan Bio               | Bilan Bio  |
| Questionnaire           | Questionnaire  |
| Journee photo           | Journee photo  |
| Parcours de changements | Parcours de changements  |
| Menu perso              | Menu perso  |
|               	      | Renouvelement du **Bilan**  |
|               	      | **Comparaison** des resultat biologique  |
| 350 eur             	 | 539 eur  |

---

### Programmes === appellation ===> back-office

|  | Programme de 3 mois  |  Programme de 3 mois + Bilan de Contrôle | Cadeau | Deja clients et veulent renouveler  |
| - | :-: | :-: | :-: | :-: |
| SKU | 1F1M0001 |  1F1M000Z  |  1F1M0002   |   |
| Ref. in issues | **A**  | **Z** |  **K**  | **C** |

---

## Tunnel d'achat
> Les étapes sont utilisées dans les issues
### 1ere etape (emai)
> Creation de compte de test sur l'app, Creer un compte avec un name field describtif (QA test initial)
- [ ] bouton suivant, envoi l'email de confirmation right away.

### 2eme etape (Adresse)


### 3eme etape (choix labo)
- [ ] Nearme limité à 20m, si pas de labo:
	- [ ] Envoyer un email au service clients
	- [ ] ou agrandir la zone de recherche
- [ ] Tags (New, needs explanation)

### 4eme etape (Recapitulatif)

### 5eme etape (Paiement - Stripe)
- [Codes de test ](https://stripe.com/docs/testing)

---

## Loggé
### index -- Activités (ou Pre-Resultat)
> Input pour l'algo

#### Questionnaire
- [ ] En attente


#### Prelevement (Status de prélèvement BO)
> une étape avant qui concerne les clients qui ont reçu le service comme **cadeau**, pour pouvoir changer le labo. il y a un status en plus dans le BO; **"En attente de confirmation de laboratoire"**

1. En Attente de telechargement, **action**: telecharger le bon pdf
2. Télécharger et en attent des prélèvements , **action**: J'ai realise mes prelevm
3. prélèvements effectués et en attente des résultats; **action**: J'ai reçu mes résultats
4. résultats papier reçus, en attente du no dossier et consentement; **action**: rentrer le code recu par le labo et valider
5. no dossier entré et no match avec données Cerba : verification attendue. **action**: aucune, *Un peu de patience, nous revenons vers vous sans faute par e-mail, depuis hello@1food1me.com !*
6. données du bilan matchées, processus terminé. **action**: Commander le bilan
7. En attente du bilan de contôle. 


<option value="controlWaiting">en attente du bilan de contrôle</option>


#### Journée photo
- [ ] En attente
- [ ] Programmée
- [ ] En attente de Feedback
- [ ] Terminé

---

# BackOffice

## Client
### Ventes
- ont passé commande
### Utilisateurs
- pas de commande
### Util. Freemium (aussi appelé util. F)
- Ce qui ont fait le test sur le site. route: /TestMe. Mais pas encore commandé
- nom FA: Freemium ensuite pris le A
- nom FZ: Freemium ensuite pris le Z

## Aliments
C'est les solutions pour les changements

/biomarkers/1/edit chaque couleur correspond à la phrase qui sera affiché au front sur le route /bilan/biomarqueurs/myr

## Articles
c'est les aliments des brands

## Cerba
### Laboratoires
- 3 types de labo: Cerba, Lbi, Labexa

## Code promo (/promocodes/new)

## Mode Debug
- Activation a partir de user Edit -> Debug Mode

## Shortcut pour ne pas remplir le questionnaire
- Copy paste les donnees questionnaires a partir du compte camille88 (ctrl/cmd + F => 88 dans page utilisateurs)
- 
## Shortcut pour ne pas remplir le code labo à dix chiffres
- route : /bloodtest_results/13/edit and copy No de dossier

> ! Never change users data in production

## journee photo peut etre fait en backoffice
---

# Input Data (activités) validée (Algo déclenché)
Une fois que les 3 activités ont été faites? on arrive à l'accueil et on peut voir nos changements suggérés par l'algo.

## Menu
> Possibilite de perso les menus
- [ ] petit dejeuner
- [ ] dej
- [ ] dinner
- [ ] encas


## Liste de courses

## Au jour 7 (weekly)
- [ ] le client reçoit un sms qui l'encourage à évaluer


## Page de Fin de Programme
-  ici possibilite de voir les aliment "heros" (recap;)
- [ ] Si client type A, Proposer de commader le bilan de control
	- [ ] en cliquant, il repart vert le tunnel d'inscription, mais seul possibilite c de commander le bilan
- [ ] Si client type Z, action possible: activer le bilan

### Questions
- [x] Freemium
- [ ] Tags : specialistes...





---

Nutrition, Routine alimentaire, Personnalisation, Aliments, 

Fatigue, carences, grignotage, problèmes de sommeil, amélioration de sa performance physique,


![[Pasted image 20210216104450.png]]


---


- youcef **diplome en commerce**
- *suite à lobtention* qlq exp, notament **Emploitic**
- *En fait*, **premier contact** -- **challenge** omnipresent / **si coordonee fabuleuses**
- *j'ai decide donc d'en faire ma* **carriere**
- J'ai commence a **apprendre** en parallel de **petits boulots** pour gagner ma vie.
- **Javascript** => ==accessible== ==populaire== ==application multiples==
- Aspirer **profil full stack**, appris **design de base de donnees** (rel / noRel) en passant par les **servers** (rest/graphql) et au **front** React
- *ca m'a permis de* **realiser** qlq site notament des site de e-commerce
- Temps libre => **SvelteJs** qui a l'avantage de **compiler en amont** le benefice c'est pas de **code rel librairie sur le navigateur**