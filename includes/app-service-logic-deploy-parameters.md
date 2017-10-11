Met Azure Resource Manager kunt u parameters definiëren voor waarden die u wilt opgeven wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam van de Parameters die alle van de parameterwaarden bevat.
U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u om te implementeert definiëren. Geen parameters op voor waarden die u altijd hetzelfde gedefinieerd. De waarde van elke parameter wordt gebruikt in de sjabloon voor het definiëren van de resources die zijn implementeren. 

Bij het definiëren van parameters, gebruiken de **allowedValues** aan te geven op welke waarden van een gebruiker kunt opgeven tijdens de implementatie. Gebruik de **defaultValue** veld een waarde toewijzen aan de parameter als u geen waarde is opgegeven tijdens de implementatie.

Elke parameter in de sjabloon wordt beschreven.

### <a name="logicappname"></a>logicAppName
De naam van de logische app maken.

    "logicAppName": {
        "type": "string"
    }