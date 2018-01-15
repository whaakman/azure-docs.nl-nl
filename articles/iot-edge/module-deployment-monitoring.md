---
title: Modules voor Azure IoT rand implementeren | Microsoft Docs
description: "Meer informatie over hoe modules ophalen geïmplementeerd op de edge-apparaten"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 0fb8c55937c1f4c29c542204673a2f41e3ae29db
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="understand-iot-edge-deployments-for-single-devices-or-at-scale---preview"></a>Rand van de IoT-implementaties voor één apparaten of op grote schaal begrijpen - voorbeeld

De randapparaten van Azure IoT Volg een [levenscyclus van apparaten] [ lnk-lifecycle] die gelijk is aan andere typen van IoT-apparaten:

1. IoT Edge-apparaten zijn ingericht, die installatie imaging van een apparaat met een besturingssysteem en het installeren van de [IoT rand runtime][lnk-runtime].
1. De apparaten zijn geconfigureerd om uit te voeren [IoT rand modules][lnk-modules], en vervolgens health gecontroleerd. 
1. Ten slotte apparaten mogelijk buiten gebruik worden gesteld wanneer ze worden vervangen of verouderd raken.  

Azure IoT Edge biedt twee manieren voor het configureren van de modules mogen worden uitgevoerd op de IoT Edge-apparaten: één voor ontwikkeling en snelle iteraties op één apparaat (die u hebt gebruikt in de zelfstudies voor Azure IoT Edge) en één voor het beheer van grote wagenparken van IoT Edge-apparaten. Deze beide benaderingen zijn beschikbaar in de Azure-Portal en programmatisch.

Dit artikel is gericht op de configuratie en bewaking van de fasen voor wagenparken van apparaten, gezamenlijk worden aangeduid als de rand van de IoT-implementaties. De algemene implementatiestappen zijn als volgt:   

1. Een operator definieert een implementatie die een set van modules, evenals de doelapparaten beschrijft. Elke implementatie heeft een implementatiemanifest die overeenkomt met deze informatie. 
1. De IoT Hub-service communiceert met alle apparaten configureren met de gewenste modules. 
1. De IoT Hub-service wordt de status opgehaald van de rand van de IoT-apparaten en geeft weer die voor de operator om te controleren.  Bijvoorbeeld, ziet een operator wanneer een Edge-apparaat is niet correct geconfigureerd of als een module is mislukt tijdens runtime. 
1. Nieuwe IoT Edge-apparaten die voldoen aan de doelitems voorwaarden zijn op elk gewenst moment geconfigureerd voor de implementatie. Bijvoorbeeld configureert een implementatie die gericht is op alle rand van de IoT-apparaten in de staat Washington automatisch een nieuwe IoT-randapparaat nadat deze is ingericht en toegevoegd aan de groep van de staat Washington apparaat. 
 
Dit artikel begeleidt bij elk onderdeel is betrokken bij het configureren en controleren van een implementatie. Zie voor een overzicht van het maken en bijwerken van een implementatie [implementeren en controleren van de rand van de IoT-modules op grote schaal][lnk-howto].

## <a name="deployment"></a>Implementatie

Een implementatie toegewezen IoT rand module installatiekopieën uit te voeren als de exemplaren van een bepaalde reeks IoT Edge-apparaten. Hierbij worden voor het configureren van een manifest voor de implementatie van IoT Edge zodanig dat die een lijst met modules met de overeenkomstige initialisatieparameters. Een implementatie kan worden toegewezen aan één apparaat (meestal op basis van apparaat-Id) of op een groep apparaten (op basis van labels). Zodra een IoT-randapparaat een manifest voor implementatie ontvangt, downloadt en installeert de installatiekopieën van de module-container van de betreffende container-opslagplaatsen, en configureert deze ze dienovereenkomstig. Zodra een implementatie is gemaakt, kunt de status van de implementatie om te zien of de betreffende apparaten correct zijn geconfigureerd door een operator bewaken.   

Apparaten moeten worden ingericht als Edge van de IoT-apparaten worden geconfigureerd met een implementatie. De volgende vereisten en worden niet opgenomen in de implementatie:
* Het basisbesturingssysteem
* Docker 
* Het inrichten van de rand van de IoT-runtime 

### <a name="deployment-manifest"></a>Manifest voor implementatie

Een implementatiemanifest is een JSON-document met een beschrijving van de modules worden geconfigureerd op de betreffende rand van de IoT-apparaten. De metagegevens van de configuratie voor alle modules, met inbegrip van de vereiste systeemmodules (specifiek de rand van de IoT-agent en de rand van de IoT hub) bevat.  

De metagegevens van de configuratie voor elke module omvat: 
* Versie 
* Type 
* Status (bijvoorbeeld wordt uitgevoerd of gestopt) 
* Beleid voor opnieuw starten 
* Afbeelding en de container-opslagplaats 
* Routes voor gegevens invoer en uitvoer 

### <a name="target-condition"></a>Doelvoorwaarden

De doelvoorwaarden wordt continu op te nemen van nieuwe apparaten die voldoen aan de vereisten of verwijderen van apparaten die niet langer via de levensduur van de implementatie geëvalueerd. De implementatie wordt opnieuw worden geactiveerd als de service wordt gedetecteerd door elke wijziging van de doel-voorwaarde. Bijvoorbeeld, u een implementatie met een doel voorwaarde tags.environment A hebt = de prod'. Wanneer u ere van de implementatie, zijn er 10 prod-apparaten. De modules zijn geïnstalleerd in deze 10-apparaten. De Agent-Status van de IoT-rand wordt weergegeven als totaal aantal apparaten 10, 10 is antwoorden, 0 mislukte reacties en 0 in behandeling antwoorden. Nu u 5 meer apparaten met tags.environment toevoegen = de prod'. Detecteert de wijziging van de service en de Agent-Status van de IoT-rand wordt 15 totaal aantal apparaten, 10 is antwoorden, 0 mislukte reacties en 5 in behandeling antwoorden bij een poging te implementeren op de vijf nieuwe apparaten.

Gebruik een Boole-voorwaarde op apparaat horende tags of de apparaat-id selecteren de doelapparaten. Als u voorwaarde gebruiken met labels wilt, moet u toevoegen 'labels' :{} sectie in de apparaat-twin onder hetzelfde niveau als eigenschappen. [Meer informatie over labels in de apparaat-twin](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins)

Voorbeelden van de doel-voorwaarden:
* apparaat-id = 'linuxprod1'
* tags.Environment = de prod'
* tags.Environment = prod en tags.location = 'westus'
* tags.Environment = prod OR tags.location = 'westus'
* tags.operator = 'Jan' en tags.environment = prod geen apparaat-id = 'linuxprod1'

Hier volgen enkele beperkingen wanneer u een doel-voorwaarde maken:

* In de apparaat-twin, kunt u alleen een doelvoorwaarden met tags of de apparaat-id maken.
* Dubbele aanhalingstekens zijn niet toegestaan in een gedeelte van de doel-voorwaarde. Gebruik tussen enkele aanhalingstekens.
* Enkele aanhalingstekens vertegenwoordigen de waarden van de doel-voorwaarde. Daarom moet u de enkel aanhalingsteken met een andere enkel aanhalingsteken escape als deze deel uitmaakt van de naam van het apparaat. Bijvoorbeeld, de voorwaarde voor doel: operator'sDevice zou moeten worden geschreven als de apparaat-id ='operator '' sDevice'.
* Cijfers, letters en de volgende tekens zijn toegestaan in de doel-voorwaarde values:-:.+%_#*? (),=@;$

### <a name="priority"></a>Prioriteit

Een prioriteit bepaalt of een implementatie moet worden toegepast op een doelapparaat ten opzichte van andere implementaties. De prioriteit van een implementatie is een positief geheel getal, waarbij een hogere waarde die aangeeft hogere prioriteit. Als een Edge van de IoT-apparaat door meer dan een implementatie is gericht, wordt de implementatie met de hoogste prioriteit is van toepassing.  Implementaties met een lagere prioriteit worden niet toegepast, noch zijn ze samengevoegd.  Als een apparaat is gericht met twee of meer implementaties met dezelfde prioriteit, geldt de meest recent gemaakte implementatie (bepaald door de tijdstempel van maken).

### <a name="labels"></a>Labels 

Labels zijn tekenreeks sleutel/waarde-paren die u om te filteren en groeperen van implementaties gebruiken kunt. Een implementatie kan meerdere labels hebben. Labels zijn optioneel en niet van invloed op de werkelijke configuratie met de randapparaten van IoT uitvoeren. 

### <a name="deployment-status"></a>Implementatiestatus

Een implementatie kan worden gecontroleerd om te bepalen of deze is toegepast voor elk doelapparaat IoT rand.  Een gerichte randapparaat wordt weergegeven in een of meer van de volgende statuscategorieën: 
* **Doel** toont de rand van de IoT-apparaten die overeenkomen met de implementatie die gericht is op voorwaarde.
* **Werkelijke** ziet u de betreffende IoT-rand apparaten die niet door een andere implementatie van een hogere prioriteit zijn bedoeld.
* **In orde** de IoT-rand apparaten weergegeven die hebben gemeld terug naar de service dat de modules die is geïmplementeerd. 
* **Slechte** ziet u de IoT-rand apparaten terug naar de service een hebben gemeld of modules niet met succes zijn geïmplementeerd. Extern verbinding maken met die apparaten voor verder onderzoek de fout, en bekijk de logboekbestanden.
* **Onbekende** toont de IoT-rand apparaten die geen status die deel uitmaakt van deze implementatie geen rapporteren. Verder te onderzoeken, gegevens en logboekbestanden voor service-bestanden te bekijken.

## <a name="phased-rollout"></a>Gefaseerde implementatie 

Een gefaseerde implementatie is een algemene proces waarbij een operator wijzigingen op een leven reeks IoT Edge-apparaten implementeert. Het doel is om te wijzigen geleidelijk vermindert het risico van het maken van grote schaal wijzigingen op te splitsen.  

Een gefaseerde implementatie wordt uitgevoerd in de volgende fasen en stappen: 
1. Tot stand brengen van een testomgeving met de randapparaten van IoT door ze inrichting en het instellen van een apparaat twin label zoals `tag.environment='test'`. De productie-omgeving die uiteindelijk heeft betrekking op de implementatie moet worden gespiegeld in de testomgeving. 
1. Maakt een implementatie met inbegrip van de gewenste modules en configuraties. Het doelitem voorwaarde moet de test IoT rand apparaat omgeving het doel.   
1. Valideer de moduleconfiguratie van de nieuwe in de testomgeving.
1. De implementatie zodanig dat een subset van de productie-Edge van de IoT-apparaten die door een nieuw label toe te voegen aan de voorwaarde doelitems bijwerken. Zorg er ook voor dat de prioriteit voor de implementatie hoger dan de andere implementaties momenteel gericht op die apparaten is 
1. Controleer of dat de implementatie is voltooid op de betreffende IoT-apparaten door de distributiestatus weer te geven.
1. Bijwerken van de implementatie om alle resterende productie rand van de IoT-apparaten.

## <a name="rollback"></a>Terugdraaien

Implementaties kunnen worden teruggedraaid in geval van fouten of configuratiefouten.  Omdat een implementatie de absolute moduleconfiguratie voor een IoT-randapparaat definieert, moet de implementatie van een extra ook zijn gericht op hetzelfde apparaat met een lagere prioriteit zelfs als het doel is om alle modules verwijderen.  

Voer terugdraaiacties door in de volgende volgorde: 
1. Bevestig dat een tweede implementatie ook is gericht op dezelfde set apparaten. Als het doel van het terugdraaien is het verwijderen van alle modules, moet alle modules die niet in de tweede implementatie bevatten. 
1. Wijzigen of verwijderen van de doelexpressie voor de voorwaarde van de implementatie die u wilt samenvouwen terug zodat de apparaten niet meer voldoet aan de doel-voorwaarde.
1. Controleer of dat het terugdraaien is gelukt door de distributiestatus weer te geven.
   * De teruggedraaid-back-implementatie mag niet meer weergegeven status voor de apparaten die zijn teruggedraaid.
   * De tweede implementatie omvatten moet nu de implementatiestatus voor de apparaten die zijn teruggedraaid.


## <a name="next-steps"></a>Volgende stappen

* Doorloop de stappen voor het maken, bijwerken of verwijderen van een implementatie in [implementeren en controleren van de rand van de IoT-modules op grote schaal][lnk-howto].
* Meer informatie over andere concepten IoT rand zoals de [IoT rand runtime] [ lnk-runtime] en [IoT rand modules][lnk-modules].

<!-- Links -->
[lnk-lifecycle]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-runtime]: iot-edge-runtime.md
[lnk-modules]: iot-edge-modules.md
[lnk-howto]: how-to-deploy-monitor.md