---
title: Mettre à jour un Timeboard
type: apicontent
order: 20.2
external_redirect: /api/#update-a-timeboard
---

## Mettre à jour un Timeboard

##### Arguments

* **`title`** [*obligatoire*]:  
    Le nom du dashboard.
* **`description`** [*optionnel*]:  
    Une description du contenu du dashboard.
* **`graphs`** [*optionnel*]:  
  Une liste de définitions de graphe. Les définitions de graphe suivent cette forme:
    * **`title`** [*obligatoire*]:  
        Le nom du graphe.
    * **`definition`** [*obligatoire*]:  
    La définition du graphe Lisez le [Guide du graphe](/graphing/) pour en savoir plus sur les graphe. Exemple:
    `{"requests": [{"q": "system.cpu.idle{*} by {host}"}`

* **`template_variables`** [*optionnel*, *defaut*=**None**]:  
    Liste des template variables utilisable dans le templating Dashboard. Les Templates variables suivent cette forme:
    * **`name`** [*obligatoire*]:
     Le nom de la variable.

    * **`prefix`** [*optionnel*, *defaut*=**None**]:  
    Le préfixe de tag associé à la variable. Seuls les tags avec ce préfixe apparaissent dans la liste déroulante des variables.

    * **`default`** [*optionnel*, *defaut*=**None**]:  
    La valeur par défaut de la Template variable lors du chargement du dasboard.