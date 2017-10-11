
### <a name="cacheskuname"></a>cacheSKUName
De prijscategorie van de nieuwe Azure Redis-Cache.

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Redis Cache."
      }
    },

De sjabloon definieert de waarden die zijn toegestaan voor deze parameter (basis of standaard) en een standaardwaarde (basis) wordt toegewezen als er geen waarde is opgegeven. Basic biedt één knooppunt met meerdere groottes beschikbaar up tot 53 GB.
Standaard biedt twee knooppunten primair/Replica met meerdere groottes beschikbaar om aan de SLA 53 GB en 99,9%.

### <a name="cacheskufamily"></a>cacheSKUFamily
De familie voor de sku.

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },


### <a name="cacheskucapacity"></a>cacheSKUCapacity
De grootte van het nieuwe exemplaar van Azure Redis-Cache. 

    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "The size of the new Azure Redis Cache instance. "
      }
    }


De sjabloon definieert de waarden die zijn toegestaan voor deze parameter (0, 1, 2, 3, 4, 5 of 6), en wijst een standaardwaarde (1) als er geen waarde is opgegeven. Deze getallen die overeenkomen met de volgende cachegrootte: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB

