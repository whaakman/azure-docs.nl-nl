---
title: Implementeren, bewaken van modules voor Azure IoT rand | Microsoft Docs
description: De modules die worden uitgevoerd op de edge-apparaten beheren
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: be52a57f10f286bded9a31d84b36a49717b94006
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029754"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-portal"></a>Implementeren en controleren van de rand van de IoT-modules op grote schaal met behulp van de Azure-portal

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-edge-how-to-deploy-monitor-selector.md)]

Azure IoT-rand kunt u analytics verplaatsen naar de rand en biedt een interface voor de cloud zodat u kunt beheren en controleren van uw IoT-randapparaten zonder fysieke toegang tot elkaar. De mogelijkheid tot extern kunt beheren apparaten wordt steeds belangrijker naarmate Internet der dingen oplossingen grotere en complexere groeien. Azure IoT-rand is ontworpen ter ondersteuning van uw zakelijke doelstellingen, ongeacht hoeveel apparaten die u toevoegt.

U kunt afzonderlijke apparaten beheren en implementeren van modules aan hen één tegelijk. Echter, als u wijzigingen aanbrengen voor apparaten op grote schaal wilt, kunt u een **IoT rand automatische implementatie**, die deel uitmaakt van automatische Apparaatbeheer in IoT-Hub. Implementaties zijn dynamische processen waarmee u meerdere modules in één keer in te implementeren op meerdere apparaten, de status en de status van de modules bijhouden en wijzig indien nodig. 

## <a name="identify-devices-using-tags"></a>Identificatie van apparaten met tags

Voordat u een implementatie maken kunt, moet u kunnen opgeven welke apparaten die u wilt beïnvloeden. Azure IoT-rand identificeert apparaten met behulp van **labels** in de apparaat-twin. Elk apparaat kan meerdere labels hebben, en kunt u ze een manier die zinvol is voor uw oplossing. Als u een bereik van de smartcard gebouwen beheert, kunt u de volgende codes toevoegen op een apparaat:

```json
"tags":{
    "location":{
        "building": "20",
        "floor": "2"
    },
    "roomtype": "conference",
    "environment": "prod"
}
```

Zie voor meer informatie over apparaat horende en labels [Understand and gebruik apparaat horende in IoT-Hub][lnk-device-twin].

## <a name="create-a-deployment"></a>Een implementatie maken

1. In de [Azure-portal][lnk-portal], gaat u naar uw IoT-hub. 
1. Selecteer **IoT rand**.
1. Selecteer **IoT rand implementatie toevoegen**.

Er zijn vijf stappen voor het maken van een implementatie. De volgende secties helpt bij elkaar. 

### <a name="step-1-name-and-label"></a>Stap 1: Naam en een Label

1. Geef uw implementatie een unieke naam die maximaal 128 kleine letters. Vermijd spaties en de volgende ongeldige tekens: `& ^ [ ] { } \ | " < > /`.
1. Labels voor het bijhouden van uw implementaties toevoegen. Labels zijn **naam**, **waarde** paren met een beschrijving van uw implementatie. Bijvoorbeeld: `HostPlatform, Linux` of `Version, 3.0.1`.
1. Selecteer **volgende** verplaatsen naar stap 2. 

### <a name="step-2-add-modules-optional"></a>Stap 2: (Optioneel) Modules toevoegen

Er zijn twee typen modules die u aan een implementatie toevoegen kunt. De eerste is een module die is gebaseerd op een Azure-service, zoals Storage-Account of Stream Analytics. De tweede is een module die is gebaseerd op uw eigen code. U kunt meerdere modules van beide typen toevoegen aan een implementatie. 

Als u een implementatie met geen modules maakt, verwijdert deze geen huidige modules van de apparaten. 

>[!NOTE]
>De implementatie van geautomatiseerde Azure-service nog ondersteund niet door Azure Machine Learning en Azure Functions. De aangepaste module-implementatie gebruiken die services handmatig toevoegen aan uw implementatie. 

Als u wilt een module van Azure Stream Analytics toevoegt, de volgende stappen uit:
1. In de **implementatie Modules** sectie van de pagina, klikt u op **toevoegen**.
1. Selecteer **Azure Stream Analytics-module**.
1. Kies uw **abonnement** uit de vervolgkeuzelijst.
1. Kies uw **rand taak** uit de vervolgkeuzelijst.
1. Selecteer **opslaan** uw module toevoegen aan de implementatie. 

Aangepaste code toevoegen als een module of een Azure-service-module handmatig toevoegen Volg deze stappen:
1. In de **registerinstellingen** sectie van de pagina bieden u de namen en referenties voor elke registers privé-container met de module afbeeldingen voor deze implementatie. De Edge-Agent rapporteert fout 500 als er geen de Register-referentie contrainer voor een docker-installatiekopie gevonden.
1. In de **implementatie Modules** sectie van de pagina, klikt u op **toevoegen**.
1. Selecteer **IoT rand Module**.
1. Geef uw module een **naam**.
1. Voor de **installatiekopie URI** veld, voert u de installatiekopie van de container voor uw module. 
1. Geef een **Container maken opties** die moet worden doorgegeven aan de container. Zie voor meer informatie [docker maken][lnk-docker-create].
1. Gebruik de vervolgkeuzelijst om te selecteren een **beleid opnieuw opstarten**. Kies uit de volgende opties: 
   * **Altijd** -de module wordt altijd opnieuw opgestart als deze wordt afgesloten om een bepaalde reden.
   * **Nooit** -de module wordt nooit opnieuw opgestart als deze wordt afgesloten om een bepaalde reden.
   * **Bij niet** -de module wordt opnieuw opgestart als deze is vastgelopen, maar niet als het afsluiten foutloos. 
   * **Op de onjuiste** -de module wordt opnieuw opgestart als deze is vastgelopen of een slechte status geretourneerd. Het is aan elke module voor het implementeren van de functie voor health-status. 
1. Gebruik de vervolgkeuzelijst om te selecteren de **gewenst Status** voor de module. Kies uit de volgende opties:
   * **Met** -dit is de standaardoptie. De module wordt gestart onmiddellijk na de implementatie wordt uitgevoerd.
   * **Gestopt** -na de implementatie, de module wordt inactief totdat opgeroepen door u of een andere module op te starten.
1. Selecteer **inschakelen** als u wilt deze tags of het gewenste eigenschappen toevoegen aan de module-twin. 
1. Voer **omgevingsvariabelen** voor deze module. Omgevingsvariabelen bevatten supplement informatie aan een module om het configuratieproces te vergemakkelijken.
1. Selecteer **opslaan** uw module toevoegen aan de implementatie. 

Zodra u de modules voor een implementatie die is geconfigureerd hebt, selecteer **volgende** verplaatsen naar stap 3.

### <a name="step-3-specify-routes-optional"></a>Stap 3: Geef de Routes (optioneel)

Routes definiëren hoe modules met elkaar communiceren binnen een implementatie. Standaard de wizard u kunt een route aangeroepen **route** en zijn gedefinieerd als **FROM /* in $stroomopwaarts **, wat betekent dat alle berichten die door alle modules uitvoer naar uw IoT-hub worden verzonden.  

Toevoegen of bijwerken van de routes met informatie uit [declareren routes](module-composition.md#declare-routes), selecteer daarna **volgende** om door te gaan naar de sectie controleren.


### <a name="step-4-target-devices"></a>Stap 4: Doelapparaten

Gebruik de eigenschap labels van uw apparaten toe te passen van de specifieke apparaten die deze implementatie ontvangt. 

Omdat meerdere implementaties zijn op hetzelfde apparaat gericht kunnen, moet u elke implementatie een getal prioriteit geven. Als er een conflict optreedt is, wordt de implementatie met de hoogste prioriteit (hogere waarden geven hogere prioriteit) wins. Als twee implementaties de dezelfde prioriteitsnummer hebt, wordt het account waarmee de meeste is gemaakt onlangs wins. 

1. Voer een positief geheel getal voor de implementatie **prioriteit**. In het geval dat twee of meer implementaties zijn gericht op hetzelfde apparaat, gelden de implementatie met de hoogste numerieke waarde voor de prioriteit.
1. Voer een **doel voorwaarde** om te bepalen welke apparaten moeten worden toegepast met deze implementatie. De voorwaarde is gebaseerd op het apparaat twin tags of apparaat twin gewenst eigenschappen en moet overeenkomen met de indeling van de expressie. Bijvoorbeeld: `tags.environment='test'` of `properties.desired.devicemodel='4000x'`. 
1. Selecteer **volgende** om door te gaan naar de laatste stap.

### <a name="step-5-review-template"></a>Stap 5: Controleer sjabloon

Lees de informatie van uw implementatie en selecteer vervolgens **indienen**.

## <a name="monitor-a-deployment"></a>Controleer de implementatie van een.

De details van een implementatie bekijken en controleren van de apparaten waarop deze wordt uitgevoerd, gebruik de volgende stappen:

1. Aanmelden bij de [Azure-portal] [ lnk-portal] en navigeer naar uw IoT-hub. 
1. Selecteer **IoT rand**.
1. Selecteer **IoT rand implementaties**. 

   ![Implementaties van IoT rand weergeven][1]

1. Controleer de implementatie-lijst. U kunt de volgende gegevens bekijken voor elke implementatie:
   * **ID** -de naam van de implementatie.
   * **Doel voorwaarde** -de code die wordt gebruikt voor het definiëren van de betreffende apparaten.
   * **Prioriteit** -het getal prioriteit is toegewezen aan de implementatie.
   * **Systeem metrische gegevens** - **doel** geeft het aantal apparaten horende in IoT-Hub die overeenkomen met de doelitems voorwaarde en **toegepast** geeft het aantal apparaten waarvoor heeft de implementatie-inhoud toegepast op hun horende module in IoT-Hub. 
   * **Apparaat metrische gegevens** -het aantal apparaten in de implementatie is geslaagd of fouten in de runtime van de client IoT rand reporting rand.
   * **Aanmaaktijd** -de tijdstempel van wanneer de implementatie is gemaakt. Dit tijdstempel wordt gebruikt voor het opsplitsen ties wanneer twee implementaties dezelfde prioriteit hebben. 
2. Selecteer de implementatie die u wilt bewaken.  
3. Controleer de details van de implementatie. Tabbladen kunt u de details van de implementatie controleren.

## <a name="modify-a-deployment"></a>Een implementatie wijzigen

Wanneer u een implementatie wijzigt, worden de wijzigingen direct repliceren naar alle apparaten. 

Als u de doelvoorwaarden bijwerkt, gebeuren de volgende updates:
* Als een apparaat niet aan de oude doel voorwaarde voldoet, maar voldoet aan de nieuwe doel-voorwaarde en deze implementatie de hoogste prioriteit voor dat apparaat is, wordt deze implementatie toegepast op het apparaat. 
* Als een apparaat momenteel met deze implementatie niet meer voldoet aan de doelvoorwaarden, verwijdert deze implementatie en op de volgende hoogste prioriteit implementatie duurt. 
* Als een apparaat momenteel met deze implementatie niet meer voldoet aan de voorwaarde doel en voldoet niet aan de doelvoorwaarden van alle andere implementaties, vervolgens wordt er niets gewijzigd op het apparaat. Het apparaat wordt verder uitgevoerd, de huidige modules in de huidige status, maar niet als onderdeel van deze implementatie niet meer wordt beheerd. Zodra het voldoet aan de voorwaarde van het doel van andere implementaties, verwijdert deze implementatie en wordt op de nieuwe. 

Gebruik de volgende stappen voor het wijzigen van een implementatie: 

1. Aanmelden bij de [Azure-portal] [ lnk-portal] en navigeer naar uw IoT-hub. 
1. Selecteer **IoT rand**.
1. Selecteer **IoT rand implementaties**. 

   ![Implementaties van IoT rand weergeven][1]

1. Selecteer de implementatie die u wilt wijzigen. 
1. Updates aanbrengen in de volgende velden: 
   * Doelvoorwaarden 
   * Labels 
   * Prioriteit 
1. Selecteer **Opslaan**.
1. Volg de stappen in [Controleer de implementatie van een] [ anchor-monitor] bekijkt u de wijzigingen implementeert. 

## <a name="delete-a-deployment"></a>Verwijderen van een implementatie

Wanneer u een implementatie hebt verwijderd, krijgen alle apparaten hun volgende hoogste prioriteit-implementatie. Als uw apparaten niet voldoen aan de voorwaarde van het doel van andere implementaties, worden klikt u vervolgens de modules niet verwijderd wanneer de implementatie is verwijderd. 

1. Aanmelden bij de [Azure-portal] [ lnk-portal] en navigeer naar uw IoT-hub. 
1. Selecteer **IoT rand**.
1. Selecteer **IoT rand implementaties**. 

   ![Implementaties van IoT rand weergeven][1]

1. Gebruik het selectievakje in om de implementatie die u wilt verwijderen. 
1. Selecteer **Verwijderen**.
1. Een prompt wordt laten u weten dat deze actie verwijdert u deze implementatie en naar de vorige status voor alle apparaten terugkeren.  Dit betekent dat van toepassing is een implementatie met een lagere prioriteit.  Als geen andere implementatie is gericht, wordt er geen modules verwijderd. Als u wilt verwijderen van alle modules van uw apparaat, een implementatie met nul modules en deze implementeren in de dezelfde apparaten. Selecteer **Ja** om door te gaan. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [modules implementeren op apparaten van de rand][lnk-deployments].

<!-- Images -->
[1]: ./media/how-to-deploy-monitor/iot-edge-deployments.png

<!-- Links -->
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-portal]: https://portal.azure.com
[lnk-docker-create]: https://docs.docker.com/engine/reference/commandline/create/
[lnk-deployments]: module-deployment-monitoring.md

<!-- Anchor links -->
[anchor-monitor]: #monitor-a-deployment
