Met Azure Resource Manager kunt u parameters voor de waarden voor gebruik bij het implementeren van de sjabloon definiëren. De sjabloon bevat een `parameters` sectie waarin de parameterwaarden. De waarde van elke parameter wordt gebruikt door de sjabloon voor het definiëren van de resources die u wilt implementeren.

> [!NOTE]
> Definieer geen parameters voor waarden die altijd hetzelfde blijven. Definieer de parameters alleen voor waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving waar u implementeert.

Wanneer u parameters definiëren:

* De toegestane waarden die een gebruiker tijdens de implementatie opgeven kunt wilt opgeven, gebruikt u de **allowedValues** veld.

* Als u wilt toewijzen aan parameter standaardwaarden wanneer geen waarden zijn opgegeven tijdens de implementatie, gebruiken de **defaultValue** veld. 
