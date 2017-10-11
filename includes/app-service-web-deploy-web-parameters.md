Met Azure Resource Manager kunt u parameters definiëren voor waarden die u wilt opgeven wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam van de Parameters die alle van de parameterwaarden bevat.
U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u om te implementeert definiëren. Geen parameters op voor waarden die u altijd hetzelfde gedefinieerd. De waarde van elke parameter wordt gebruikt in de sjabloon voor het definiëren van de resources die worden geïmplementeerd. 

Bij het definiëren van parameters, gebruiken de **allowedValues** aan te geven op welke waarden van een gebruiker kunt opgeven tijdens de implementatie. Gebruik de **defaultValue** veld een waarde toewijzen aan de parameter als u geen waarde is opgegeven tijdens de implementatie.

Elke parameter in de sjabloon wordt beschreven.

### <a name="sitename"></a>Sitenaam
De naam van de web-app die u wilt maken.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
De naam van de App Service-abonnement moet worden gebruikt voor het hosten van de web-app.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU
De prijscategorie voor de hosting-plan.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

De sjabloon definieert de waarden die zijn toegestaan voor deze parameter en een standaardwaarde (S1) wordt toegewezen als er geen waarde is opgegeven.

### <a name="workersize"></a>workerSize
De exemplaargrootte van de hosting-abonnement (klein, Gemiddeld of grote).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

De sjabloon definieert de waarden die zijn toegestaan voor deze parameter (0, 1 of 2), en wijst een standaardwaarde (0) als er geen waarde is opgegeven. De waarden komen overeen met kleine, middelgrote en grote.

