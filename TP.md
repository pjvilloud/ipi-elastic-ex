# Elasticsearch

## Index et indexations

1. Indexer le document suivant (identifiant 1) dans un index que l'on nommera `depute`

```json
{
	"id": 1,
	"nom": "Cédric Roussel",
	"nom_de_famille": "Roussel",
	"prenom": "Cédric",
	"sexe": "H",
	"date_naissance": "1872-10-10",
	"lieu_naissance": "Brest (Finistère)",
	"num_deptmt": "06",
	"nom_circo": "Alpes-Maritimes",
	"num_circo": 3,
	"mandat_debut": "2017-06-21",
	"groupe_sigle": "LREM",
	"parti_ratt_financier": "La République en Marche",
	"sites_web": [
	    {
		"site": "https://fr-fr.facebook.com/CRoussel06/"
	    },
	    {
		"site": "https://twitter.com/CedricRoussel06"
	    },
	    {
		"site": "http://www.cedricroussel.fr"
	    },
	    {
		"site": "http://cedricroussel.en-marche.fr"
	    }
	],
	"emails": [
	    {
		"email": "cedric.roussel@assemblee-nationale.fr"
	    }
	],
	"adresses": [
	    {
		"adresse": "Assemblée nationale, 126 Rue de l'Université, 75355 Paris 07 SP"
	    }
	],
	"collaborateurs": [
	    {
		"collaborateur": "Mme Caroline Puisségur-Ripet"
	    },
	    {
		"collaborateur": "Mme Julie Phan-Pérain"
	    },
	    {
		"collaborateur": "M. Luc Derai"
	    },
	    {
		"collaborateur": "M. Pierre-Laurent Renard"
	    }
	],
	"autres_mandats": [],
	"anciens_autres_mandats": [],
	"anciens_mandats": [
	    {
		"mandat": "21/06/2017 /  / "
	    }
	],
	"profession": "Conseiller en gestion de patrimoine indépendant",
	"place_en_hemicycle": "309",
	"url_an": "http://www2.assemblee-nationale.fr/deputes/fiche/OMC_PA718902",
	"id_an": "718902",
	"slug": "cedric-roussel",
	"url_nosdeputes": "https://www.nosdeputes.fr/cedric-roussel",
	"url_nosdeputes_api": "https://www.nosdeputes.fr/cedric-roussel/json",
	"nb_mandats": 1,
	"twitter": "CedricRoussel06"
}
```

1. Effectuer la requête permettant de récupérer le document précédemment indexé et analyser la réponse du serveur.

1. Effectuer la requête pour récupérer le mapping de l'index `depute` généré automatiquement et remarquer le type choisi automatiquement par Elasticsearch pour les champs 
	- `groupe_sigle`
	- `num_dptmt`
	- `sexe`
	- `nom_circo`
	- `parti_ratt_financier`
	- `date_naissance`

    	Sont-ils corrects ?

1. Effectuer la requête pour supprimer le document précédemment indexé. Vérifier qu'en essayant de le récupérer, on obtient bien une erreur.

1. Effectuer une requête pour modifier le mapping de l'index `depute` en spécifiant le type `keyword` pour les champs `groupe_sigle`, `num_dptmt`, `sexe`. On laisse `nom_circo`, `parti_ratt_financier` en automatique.

1. Réindexer le document précédent et vérifier que tout est OK

1. Réindexer la totalité du document en modifiant la date de naissance de `1872-10-10` à `1972-10-10`. Vérifier la prise en compte de la modification et analyser le champ `version`.

1. En utilisant la documentation https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-update.html modifier le document indexé pour ajouter un champ `nb_mandats_total` contenant le nombre de mandats (nombre d'éléments dans le tableau `anciens_mandats`)

1. Récupérer le mapping de l'index `depute` et constater l'ajout du champ avec le type automatiquement défini.

1. Indexer en masse tous les députés en utilisant le fichier `nosdeputes.fr_deputes_2020-06-16.bulk.json`. Analyser la réponse de votre requête pour vérifier s'il y a des erreurs. Essayer de récupérer en une requête les documents d'id `578`, `420`, `327`, `246`, `119` et `12`

## Recherche stricte

1. Effectuer la requête renvoyant tous les résultats des index `depute` et `people`. Vérifier que le nombre de résultats renvoyés semble correct.

1. Effectuer la requête renvoyant parmi les index `depute` et `people` les résultats contenant *Pierre* ou *Paul*. Analyser la réponse de votre requête. Comment sont classés les résultats ? Combien de résultats ont été trouvés ? Combien de résultats sont effectivement renvoyés ? 

1. Refaire la requête en renvoyant les 20 résultats suivants.

1. Effectuer une recherche renvoyant les députés dont le nom contient *Paul* ou *Pierre*.

1. Effectuer une recherche renvoyant les députés de la circonscription de la Loire.

1. Effectuer une recherche renvoyant les députés de la circonscription de la Loire ou de l'Isère. ~~Analyser la réponse et essayer de trouver la différence par rapport à la question 4.~~

1. Reprendre l'énoncé de la question 2 mais chercher uniquement dans le nom ou le prénom du député.

1. Rechercher les députés qui ont au moins une femme parmi leur collaborateurs

1. Rechercher les députés en vert sur l'image suivante : 
	<img src="img/numhemicycle.jpg" alt="drawing" width="900"/>

1. Rechercher les députés sortants

## Recherche floue/intervalle

1. Rechercher le nom *Katabi* en activant la recherche floue. Constatez les résultats obtenus.

1. Rechercher les députés qui ont 3 mandats ou plus.

## Bonus

1. Supprimer l'index `depute` et créer le mapping suivant :

	```json
	{
	    "mappings": {
		"properties": {
		    "sexe": {
			"type": "keyword"
		    },
		    "date_naissance": {
			"type": "date",
			"format": "yyyy-MM-dd"
		    },
		    "mandat_debut": {
			"type": "date",
			"format": "yyyy-MM-dd"
		    },
		    "mandat_fin": {
			"type": "date",
			"format": "yyyy-MM-dd"
		    },
		    "num_deptmt": {
			"type": "keyword"
		    },
		    "nom_circo": {
			"type": "keyword"
		    },
		    "groupe_sigle": {
			"type": "keyword"
		    },
		    "parti_ratt_financier": {
			"type": "keyword"
		    },
		    "prenom": {
			"type": "text",
			"analyzer": "phoneticAnalyzer"
		    }
		}
	    },
	    "settings": {
		"index": {
		    "analysis": {
			"analyzer": {
			    "phoneticAnalyzer": {
				"tokenizer": "standard",
				"filter": [
				    "lowercase",
				    "my_metaphone"
				]
			    }
			},
			"filter": {
			    "my_metaphone": {
				"type": "phonetic",
				"encoder": "metaphone",
				"replace": false
			    }
			}
		    }
		}
	    }
	}
	```

1. Réindexer les données de tous les députés et faire une recherche sur le prénom "Sandra". Que constatez-vous ?

## Aggrégations

1. Récuperer le nombre de députés de sexe féminin et de sexe masculin en une requête.

1. Regrouper les députés par tranche d'âge (tous les 5 ans).

1. Regrouper les députés par parti politique, puis par groupe.

1. Trouver l'âge moyen (date de naissance moyenne...) des députés, puis des députés de sexe féminin, puis des députés de sexe masculin.

# Beats

## Pré-requis

Récupérer (Fork ou Download) et ouvrir le projet Java situé à l'url https://github.com/pjvilloud/deputes-elastic-batch avec votre IDE. Ce projet contient 2 batchs Spring batch : 

1. Intègre les fichiers JSON de nosdeputes.fr de type *synthèse* qui regroupe l'activité des 12 derniers mois des députés actifs

1. Intègre les fichiers JSON de nosdeputes.fr de type *activite* qui regroupe l'activité d'un mois donné pour tous les députés.

	Ce projet possède également une application web permettant de lancer les batchs et d'accéder en lecture à l'index Elasticsearch `depute`. Voir `BatchController`.

	Modifier la classe BatchApplication en indiquant votre serveur Elasticsearch et votre user/mot de passe

	```java
	@Bean
	public RestHighLevelClient client() {
		ClientConfiguration clientConfiguration
				= ClientConfiguration.builder()
				.connectedTo("elasticsearch.eu-west-2.aws.cloud.es.io:9243")
				.usingSsl()
				.withBasicAuth("elastic", "password")
				.build();

	return RestClients.create(clientConfiguration).rest();
	}
	```

## FileBeat

L'application web Java stocke les logs Tomcat dans le répertoire `tomcat/logs/`. Installer, configurer et lancer le service FileBeats grâce à https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-getting-started.html (au moins jusqu'à l'étape 5) pour qu'il envoie les logs Tomcat à ElasticSearch. 

Lancer l'application `BatchsApplication` et exécuter dans un navigateur ou dans PostMan une requête `GET` sur l'url `localhost:8080/general?filename=nosdeputes.fr_synthese_2020-06-23.json`. Vérifier la bonne exécution du batch 

Vérifier dans Kibana (Dashboards spécialisés Apache Filebeat) que les données sont bien visibles.

## MetricsBeat

Suivre la procédure d'installation de MetricBeats sur https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-getting-started.html (au moins jusqu'à l'étape 5) et démarrer le service MetricBeats sur votre machine.

## HeartBeat

Suivre la procédure d'installation de HeartBeat sur https://www.elastic.co/guide/en/beats/heartbeat/current/heartbeat-getting-started.html (au moins jusqu'à l'étape 5). Configurer HeartBeat pour qu'il surveille l'URL `http://localhost:8080/up` et lancer le service sur votre machine.

## Logstash 

Paramétrer Filebeat (dans un nouveau pipeline) pour qu'il récupère les logs situés dans le fichier `info.log` pour les envoyer à Logstash. 
Paramétrer Logstash pour qu'il récupère les données issues de FileBeat et envoie dans Elasticsearch les logs parsés (utiliser un filtre `dissect` https://www.elastic.co/guide/en/logstash/current/plugins-filters-dissect.html).

Une ligne de log ressemble à 
`2020-06-17 08:19:36 - INFO  - importDeputesActiviteFileToES:1:WRITE_DEPUTES_ACTIVITE:50`

Le document créé sera sous la forme : 

```json
{
    "ts": "2020-06-17 08:19:36",
    "level": "INFO",
    "jobName": "importDeputesActiviteFileToES",
    "jobId": "1",
    "logKey": "WRITE_DEPUTES_ACTIVITE",
    "value": "50"
}
```

Lancer votre application

Créer l'index `activite` avec le mapping suivant : 

```json
{
    "mappings": {
        "properties": {
            "groupe": {
                "type": "keyword"
            },
            "date": {
                "type": "date",
                "format": "yyyyMM"
            }
        }
    }
}
```

Exécuter un à un les appels suivants : en attendant bien la fin de chaque requête avant de lancer la suivante

- http://localhost:8080/activite?filename=nosdeputes.fr_201706_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201707_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201708_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201709_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201710_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201711_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201712_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201801_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201802_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201803_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201804_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201805_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201806_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201807_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201808_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201809_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201810_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201811_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201812_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201901_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201902_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201903_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201904_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201905_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201906_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201907_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201908_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201909_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201910_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201911_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_201912_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_202001_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_202002_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_202003_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_202004_stats_deputes.json
- http://localhost:8080/activite?filename=nosdeputes.fr_202005_stats_deputes.json

# Kibana

## Discover

1. Effectuer diverses recherches en mode Full text

1. Filtrer les résultats par groupe *MODEM*

1. N'afficher que le nom et le prénom

## Canvas

Créer un nouveau workpard et faire figurer les éléments suivants (ne prendre en compte que les députés actifs)

1. Nombre de rapports écrits au total
1. Nombre de questions orales posées au total
1. Nombre de députés 
1. Nombre d'interventions (courtes + longues) 
1. Nombre de propositions écrites (courtes + longues) 
1. Age moyen
1. Répartition Homme/Femme
1. Répartition par parti (groupe_sigle)
1. Courbe des années de naissance
1. Filtre Homme/Femme
1. Filtre par parti (groupe_sigle)

Bonus

1. "Les timides" : 0 questions, 0 interventions et 0 interventions courtes. Afficher leur image (utiliser l'élément Markdown)
. Image en markdown `![texte](url "text hover")`. Url de l'image sous la forme : `https://nosdeputes.fr/depute/photo/{{slug}}/150` avec `slug` qui est contenu dans les documents
1. "Le bavard" : le plus d'interventions en hemicycle
1. L'"hyperactif" : le plus de propositions écrites, amendements_proposes et rapports
1. Celui "qui a piscine" : moins de semaine de présence

Faire un export en PDF ou une capture d'écran

## Dashboard

Créer un dashboard **Activité** et ajouter les visualisations suivantes : 

1. Graphique (sous forme de courbe) présentant le nombre de semaines de présence moyen des députés. Expliquer les creux...

1. Graphique (sous forme d'histogramme) présentant le nombre d'amendements proposés/signés/adoptés. Trouver la réforme des retraites...

1. Graphique (sous forme de camembert) présentant le répartition des interventions longues par parti

1. Graphique (sous forme de courbes) présentant le nombre des questions écrites au fur et a mesure du temps avec une courbe cumulative

1. Graphique (sous forme de jauge) indiquant la somme des rapports soumis

1. Graphique (sous forme d'histogramme) indiquant le nombre moyen de semaines de présence par parti.

## Logs

Aller sur l'interface Logs et afficher les logs issus de Lostash en ajoutant les colonnes définies dans le filtre Dissect.

## Metrics

Vérifier la présence de données sur cet écran (issue de MetricsBeat)

## Uptime

Vérifier la présence de données sur cet écran (issue de HeartBeat)

## APM

Suivre le tutoriel APM depuis Kibana et configurer votre goal Spring Boot pour ajouter dans `VM options` les options affichées. Cela doit ressembler à `-javaagent:elastic-apm-agent-1.17.0.jar -Delastic.apm.service_name=batch -Delastic.apm.server_urls=https://VOTREURLELASTIC.aws.cloud.es.io:443 -Delastic.apm.secret_token=VOTRETOKENSECRET -Delastic.apm.application_packages=com.ipiecoles.java.elastic`)

Relancer votre application

Exécuter des requêtes de type `http://localhost:8080/deputes?page=1&size=10`. Essayer des valeurs incorrectes, des URLs qui n'existent pas et constater les résultats sur APM.

