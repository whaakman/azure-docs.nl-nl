---
title: Automatische implementatie voor apparaatgroepen - Azure IoT Edge | Microsoft Docs
description: Gebruik van automatische implementaties in Azure IoT Edge voor groepen van apparaten op basis van gedeelde tags beheren
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 72238db8322132f42e14656c106b82d8604af21f
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57435096"
---
# <a name="understand-iot-edge-automatic-deployments-for-single-devices-or-at-scale"></a>Informatie over IoT Edge-automatische implementaties voor individuele apparaten of op schaal

Azure IoT Edge-apparaten gaat u als volgt een [levenscyclus van apparaat](../iot-hub/iot-hub-device-management-overview.md) die is vergelijkbaar met andere typen IoT-apparaten:

1. Nieuwe IoT Edge-apparaten inrichten met een apparaat met een besturingssysteem imaging en het installeren van de [IoT Edge-runtime](iot-edge-runtime.md).
2. Configureren van de apparaten om uit te voeren [IoT Edge-modules](iot-edge-modules.md), en vervolgens hun status controleren. 
3. Ten slotte apparaten buiten gebruik stellen wanneer ze worden vervangen of verouderd raken.  

Azure IoT Edge zorgt voor het configureren van de modules mogen worden uitgevoerd op IoT Edge-apparaten op twee manieren: één voor ontwikkeling en snelle iteraties op één apparaat (u hebt deze methode gebruikt in de Azure IoT Edge [zelfstudies](tutorial-deploy-function.md)), en één voor het beheren van grote vissersvloten van IoT Edge-apparaten. Beide van deze methoden zijn beschikbaar in Azure portal en programmatisch. Die zijn gericht op groepen of een groot aantal apparaten, kunt u opgeven welke apparaten die u wilt implementeren de modules voor het gebruik van [tags](../iot-edge/how-to-deploy-monitor.md#identify-devices-using-tags) op het dubbele apparaat. De volgende stappen uit praten over een implementatie op een apparaatgroep de staat Washington is geïdentificeerd door de eigenschap tags. 

In dit artikel is gericht op de configuratie en bewaking van de fasen voor vloten van apparaten, gezamenlijk aangeduid als automatische IoT Edge-implementaties. De algemene implementatiestappen zijn als volgt: 

1. Een operator definieert een implementatie die een reeks modules, evenals de doelapparaten beschrijft. Elke implementatie heeft een implementatie-manifest die overeenkomt met deze informatie. 
2. De IoT Hub-service communiceert met alle apparaten uit de doelgroep om deze te configureren met de gewenste modules. 
3. De IoT Hub-service wordt de status opgehaald van de IoT Edge-apparaten en maakt ze beschikbaar voor de operator.  Bijvoorbeeld, ziet een operator wanneer een Edge-apparaat niet is geconfigureerd of als een module is mislukt tijdens runtime. 
4. Op elk gewenst moment zijn nieuwe IoT Edge-apparaten die voldoen aan de doelitems voorwaarden geconfigureerd voor de implementatie. Bijvoorbeeld, configureert een implementatie die gericht is op alle IoT Edge-apparaten in de staat Washington automatisch een nieuwe IoT Edge-apparaat nadat deze is ingericht en aan de groep met apparaten van de staat Washington toevoegen. 
 
Dit artikel wordt beschreven voor elk onderdeel is betrokken bij het configureren en controleren van een implementatie. Zie voor een overzicht van het maken en bijwerken van een implementatie [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md).

## <a name="deployment"></a>Implementatie

De automatische implementatie van een IoT Edge toegewezen IoT Edge module installatiekopieën uit te voeren als de exemplaren in een bepaalde set van IoT Edge-apparaten. Dit gebeurt door het configureren van een manifest van de IoT Edge-implementatie om op te nemen van een lijst met modules met de bijbehorende initialisatieparameters. Een implementatie kan worden toegewezen aan één apparaat (op basis van apparaat-ID) of aan een groep van apparaten (op basis van tags). Zodra een IoT Edge-apparaat ontvangt een implementatie van het manifest, het downloadt en installeert de containerinstallatiekopieën vanuit opslagplaatsen voor de desbetreffende container en configureert u deze dienovereenkomstig. Nadat een implementatie wordt gemaakt, kunt de status van de implementatie om te zien of de beoogde apparaten correct zijn geconfigureerd door een operator controleren.

IoT Edge-apparaten kunnen worden geconfigureerd met een implementatie. De volgende vereisten moeten zijn op het apparaat voordat de implementatie kan ontvangen:

* Het basisbesturingssysteem
* Een container management-systeem, zoals Moby of Docker
* Het inrichten van IoT Edge-runtime 

### <a name="deployment-manifest"></a>Distributiemanifest

Het manifest voor een implementatie is een JSON-document met een beschrijving van de modules worden geconfigureerd op de betreffende IoT Edge-apparaten. Het bevat de metagegevens van de configuratie voor alle modules, met inbegrip van de modules voor het systeem verplicht (specifiek de IoT Edge-agent en IoT Edge hub).  

De metagegevens van de configuratie voor elke module bevat: 

* Versie 
* Type 
* Status (bijvoorbeeld wordt uitgevoerd of gestopt) 
* Beleid voor opnieuw opstarten 
* Afbeeldings- en container registry
* Routes voor invoer en uitvoer 

Als de module-installatiekopie is opgeslagen in een privécontainerregister, bevat de IoT Edge-agent de registerreferenties. 

### <a name="target-condition"></a>Doelvoorwaarde

De doelvoorwaarde wordt voortdurend geëvalueerd, gedurende de levensduur van de implementatie. Nieuwe apparaten die voldoen aan de vereisten zijn opgenomen en alle bestaande apparaten die niet langer worden verwijderd. De implementatie opnieuw wordt geactiveerd als de service wordt gedetecteerd door elke wijziging van de voorwaarde doel. 

Bijvoorbeeld, hebt u een A-implementatie met een doel voorwaarde tags.environment = 'prod'. Wanneer u een vliegende start de implementatie, zijn er 10 productieapparaten. De modules zijn geïnstalleerd in deze 10-apparaten. De Status van de IoT Edge-Agent wordt weergegeven als het totaal aantal apparaten 10, 10 gelukt-antwoorden, 0 mislukte reacties en 0 in behandeling-antwoorden. Nu u bij het toevoegen van vijf meer apparaten met tags.environment = 'prod'. De service detecteert de wijziging en de Status van de IoT Edge-Agent wordt 15 totaal aantal apparaten, 10 gelukt-antwoorden, 0 mislukte reacties en 5 in behandeling-antwoorden als er wordt geprobeerd om in de vijf nieuwe apparaten te implementeren.

Gebruik een Boole-voorwaarde op device twins tags of deviceId de doelapparaten selecteren. Als u wilt de voorwaarde met tags gebruiken, moet u 'tags' toevoegen:{} sectie in de apparaatdubbel onder hetzelfde niveau als eigenschappen. [Meer informatie over tags in de apparaatdubbel](../iot-hub/iot-hub-devguide-device-twins.md)

Voorbeelden van de doel-voorwaarden:

* apparaat-id = 'linuxprod1'
* tags.Environment = 'prod'
* tags.Environment = 'prod' AND tags.location = 'westus'
* tags.Environment = 'prod' of tags.location = 'westus'
* tags.operator = 'John' en tags.environment = 'prod' geen apparaat-id = 'linuxprod1'

Hier volgen enkele beperkingen wanneer u een doelvoorwaarde maken:

* In de apparaatdubbel, kunt u alleen een doelvoorwaarde met behulp van labels of deviceId bouwen.
* Dubbele aanhalingstekens zijn niet toegestaan in een gedeelte van de doelvoorwaarde. Gebruikt u enkele aanhalingstekens.
* Enkele aanhalingstekens vertegenwoordigen de waarden van de doelvoorwaarde. Daarom moet u de enkel aanhalingsteken met een andere enkel aanhalingsteken escape-als het deel van de naam van het apparaat uitmaakt. Bijvoorbeeld: op een apparaat met de naam `operator'sDevice`, schrijven `deviceId='operator''sDevice'`.
* Cijfers, letters en de volgende tekens zijn toegestaan in de voorwaarde doelwaarden: `-:.+%_#*?!(),=@;$`.

### <a name="priority"></a>Prioriteit

Een prioriteit bepaalt of een implementatie moet worden toegepast op een doelapparaat ten opzichte van andere implementaties. De prioriteit van een implementatie is een positief geheel getal, met grotere aantallen die aangeeft hogere prioriteit. Als een IoT Edge-apparaat door meer dan één implementatie is gericht, wordt de implementatie met de hoogste prioriteit is van toepassing.  Implementaties met een lagere prioriteit worden niet toegepast, noch zijn ze samengevoegd.  Als een apparaat is gericht met twee of meer implementaties met dezelfde prioriteit, geldt de meest recent gemaakte implementatie (zoals bepaald door het tijdstempel van maken).

### <a name="labels"></a>Labels 

Labels zijn een reeks sleutel/waarde-paren die u om te filteren en groeperen van implementaties gebruiken kunt. Een implementatie mogelijk meerdere labels. Labels zijn optioneel en niet van invloed op de werkelijke configuratie van IoT Edge-apparaten uitvoeren. 

### <a name="deployment-status"></a>Implementatiestatus

Een implementatie kan worden gecontroleerd om te bepalen of deze is toegepast voor alle betreffende IoT Edge-apparaat.  Een gerichte Edge-apparaat wordt weergegeven in een of meer van de volgende status categorieën: 

* **Doel** IoT Edge-apparaten die overeenkomen met de implementatie die gericht is op voorwaarde weergegeven.
* **Werkelijke** ziet u de betreffende IoT Edge-apparaten die niet door een andere implementatie van een hogere prioriteit zijn bedoeld.
* **In orde** ziet u de IoT Edge-apparaten die zijn gerapporteerd terug aan de service dat de modules met succes is geïmplementeerd. 
* **Niet in orde** ziet u de IoT Edge apparaten terug naar de service die een hebben gerapporteerd of modules niet met succes zijn geïmplementeerd. Voor verder onderzoek van de fout, extern verbinding maken met deze apparaten en de logboekbestanden.
* **Onbekende** IoT Edge-apparaten die elke status die deel uitmaakt van deze implementatie geen rapporteren weergegeven. Verder te onderzoeken, service-gegevens en logboekbestanden bestanden weergeven

## <a name="phased-rollout"></a>Gefaseerde implementatie 

Een gefaseerde implementatie is een algemene proces waarbij wijzigingen in een operator worden geïmplementeerd op een leven set IoT Edge-apparaten. Het doel is om wijzigingen aanbrengen geleidelijk in vermindert het risico van het maken van grote schaal belangrijke wijzigingen.  

Een gefaseerde implementatie wordt uitgevoerd in de volgende fasen en stappen: 

1. Een testomgeving van IoT Edge-apparaten tot stand brengen door ze inrichting en het instellen van een label van het dubbele apparaat, zoals `tag.environment='test'`. De testomgeving moet een mirror van de productie-omgeving die uiteindelijk heeft betrekking op de implementatie. 
2. Maak een implementatie met inbegrip van de gewenste modules en configuraties. De doelitems voorwaarde moet het doel de test-omgeving van IoT Edge-apparaten.   
3. Valideer de moduleconfiguratie van de nieuwe in de testomgeving.
4. De implementatie zodat een subset van de productie IoT Edge-apparaten die door een nieuwe tag toe te voegen aan de voorwaarde doelitems bijwerken. Zorg er ook voor dat de prioriteit voor de implementatie hoger dan andere implementaties op dit moment is gericht op deze apparaten is 
5. Controleer of dat de implementatie is voltooid op de betreffende IoT-apparaten door de implementatiestatus te bekijken.
6. Bijwerken van de implementatie wilt richten op alle resterende productie IoT Edge-apparaten.

## <a name="rollback"></a>Ongedaan maken

Implementaties kunnen worden teruggedraaid terug als er fouten of onjuiste configuraties.  Omdat een implementatie de absolute moduleconfiguratie voor een IoT Edge-apparaat definieert, moet de implementatie van een extra ook zijn gericht op hetzelfde apparaat met een lagere prioriteit, zelfs als het doel is om het verwijderen van alle modules.  

Terugdraaiacties in de volgende handelingen uitvoeren: 

1. Bevestig dat een tweede implementatie ook is gericht op dezelfde apparaatset. Als het doel van het terugdraaien is het verwijderen van alle modules, moet alle modules die niet in de tweede implementatie bevatten. 
2. Wijzigen of verwijderen van de doel-voorwaarde-expressie van de implementatie die u wilt opnieuw zodat de apparaten niet langer voldoet aan de doelitems voorwaarde samenvouwen.
3. Controleren of het terugdraaien is geslaagd door de implementatiestatus te bekijken.
   * De service-Update-back-implementatie moet status voor de apparaten die worden teruggedraaid niet meer weergeven.
   * De tweede implementatie bevatten moet nu de implementatiestatus voor de apparaten die worden teruggedraaid.


## <a name="next-steps"></a>Volgende stappen

* Doorloop de stappen voor het maken, bijwerken of verwijderen van een implementatie in [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md).
* Meer informatie over andere IoT Edge-concepten, zoals de [IoT Edge-runtime](iot-edge-runtime.md) en [IoT Edge-modules](iot-edge-modules.md).

