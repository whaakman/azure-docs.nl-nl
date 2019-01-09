---
title: API's van Azure van digitale dubbels navigeren | Microsoft Docs
description: Leer hoe u algemene patronen van query's op de beheer-API's van de digitale dubbels van Azure.
author: dsk-2015
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 1/7/2019
ms.author: dkshir
ms.openlocfilehash: 2221e8b22450a353da42564e5b93342a11e59f71
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54108319"
---
# <a name="how-to-use-azure-digital-twins-management-apis"></a>Het gebruik van Azure digitale dubbels beheer-API 's

De beheer-API's van de digitale dubbels van Azure bieden een krachtige functies voor uw IoT-apps. Dit artikel leest u hoe om te navigeren door de API-structuur.  

## <a name="api-summary"></a>API-overzicht

De volgende lijst bevat de onderdelen van de digitale Twins-API's.

* [/ opslagruimten](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Spaces): Deze API's communiceren met de fysieke locaties in uw instellingen. Deze om te maken, verwijderen en beheren van de digitale toewijzingen van uw fysieke locaties in de vorm van een [ruimtelijke graph](concepts-objectmodel-spatialgraph.md#spatial-intelligence-graph).

* [/resources](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Resources): Deze API's waarmee u kunt resources, zoals een IoT-hub voor uw exemplaar van digitale dubbels instellen.

* [/Devices](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Devices): Deze API's communiceren met de apparaten in uw instellingen. Deze apparaten kunnen een of meer sensoren beheren. Bijvoorbeeld, kan een apparaat worden uw telefoon of een Raspberry Pi sensor schil of een Lora-gateway, enzovoort.

* [/sensors](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Sensors): Deze API's kunnen u communiceren met de sensoren die zijn gekoppeld aan uw apparaten en uw fysieke locaties. De sensoren registreren en de waarden die vervolgens kunnen worden gebruikt voor het bewerken van uw omgeving ruimtelijke verzenden.  

* [/ typen](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Types): Deze API's kunnen u uitgebreide typen koppelen aan uw digitale Twins-objecten, specifieke kenmerken toevoegen aan deze objecten. Deze typen kunnen eenvoudig filteren en groeperen van objecten in de gebruikersinterface en de aangepaste functies die uw telemetriegegevens verwerken. Voorbeelden van uitgebreide zijn *DeviceType*, *SensorType*, *SensorDataType*, *SpaceType*, *SpaceSubType* , *SpaceBlobType*, *SpaceResourceType*, enzovoort.

* [/ontologies](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/Ontologies): Deze API's kunnen u voor het beheren van anthologieën, oftewel verzamelingen van uitgebreide typen. Anthologieën bieden namen voor objecttypen die aan de hand van de fysieke ruimte die wordt weergegeven. Bijvoorbeeld, de *BACnet* ontologie bevat specifieke namen voor *sensor typen*, *gegevenstypen*, *datasubtypes*, en *dataunittypes*. Anthologieën worden beheerd en die zijn gemaakt door de service. Gebruikers kunnen laden en anthologieën verwijderen. Wanneer een ontologie wordt geladen, worden alle van de bijbehorende typenamen zijn ingeschakeld en gereed om te worden ingericht in de grafiek ruimtelijke. 

* [/propertyKeys](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/PropertyKeys): U kunt deze API's gebruiken om te maken van aangepaste eigenschappen voor uw *spaties*, *apparaten*, *gebruikers*, en *sensoren*. Deze eigenschappen worden gemaakt als sleutel/waarde-paren. U kunt het gegevenstype voor deze eigenschappen definiëren door in te stellen hun *PrimitiveDataType*. Bijvoorbeeld, kunt u een eigenschap met de naam *BasicTemperatureDeltaProcessingRefreshTime* van het type *uint* voor uw sensoren, en vervolgens wijst u een waarde voor deze eigenschap voor elk van de sensoren. U kunt ook beperkingen voor deze waarden toevoegen tijdens het maken van de eigenschap, zoals *Min* en *Max* bereiken, en de toegestane waarden als *ValidationData*.

* [/matchers](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/Matchers): Deze API's kunnen u de voorwaarden die u wilt evalueren uit uw inkomende apparaatgegevens opgeven. Zie [in dit artikel](concepts-user-defined-functions.md#matchers) voor meer informatie. 

* [/userDefinedFunctions](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/UserDefinedFunctions): Deze API's kunnen u maken, delete of update een aangepaste functie die wordt uitgevoerd wanneer voorwaarden die zijn gedefinieerd door de *vergelijkingsprogramma voor* optreden, om gegevens te verwerken die afkomstig zijn van uw installatie. Zie [in dit artikel](concepts-user-defined-functions.md#user-defined-functions) voor meer informatie over deze aangepaste functies, ook wel de *door gebruiker gedefinieerde functies*. 

* [/endpoints](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/Endpoints): Deze API's kunnen u eindpunten maken, zodat uw oplossing digitale dubbels met andere Azure-services voor gegevensopslag en analyses communiceren kan. Lezen [in dit artikel](concepts-events-routing.md) voor meer informatie. 

* [/keyStores](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/KeyStores): Deze API's kunnen u belangrijke archieven van de beveiliging voor uw opslagruimten beheren. Deze archieven kunnen houdt een verzameling van beveiligingssleutels en kunt u eenvoudig de meest recente geldige toegangssleutels ophalen.

* [/ gebruikers](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Users): Deze API's kunnen u gebruikers koppelen aan uw opslagruimten, om te zoeken van deze personen indien nodig. 

* [/ System](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/System): Deze API's kunnen u voor het beheren van systeeminstellingen, zoals de standaardtypen van opslagruimten en sensoren. 

* [/roleAssignments](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/RoleAssignments): Deze API kunt u rollen om entiteiten, zoals gebruikers-ID, de gebruiker gedefinieerde functie-ID, enzovoort te koppelen. De toewijzing van elke rol bevat de ID van de entiteit wilt koppelen, het entiteitstype, de ID van de rol wilt koppelen, de tenant-ID en een pad dat de bovengrens van de resource die de entiteit met deze koppeling openen kunt definieert. Lezen [in dit artikel](security-role-based-access-control.md) voor meer informatie.


## <a name="api-navigation"></a>API-navigatie

De digitale Twins-API's bieden ondersteuning voor filteren en navigatie in de hele uw ruimtelijke graaf met behulp van de volgende parameters:

- **spaceId**: De API wordt filter de resultaten van de beschikbare ruimte-ID. Bovendien de Booleaanse vlag **useParentSpace** is van toepassing op de [/opslagruimten](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Spaces) API's, waarmee wordt aangegeven dat de beschikbare ruimte-ID naar de bovenliggende ruimte in plaats van de huidige ruimte verwijst. 

- **minLevel** en **maxLevel**: Hoofdmap spaties worden beschouwd als op het niveau van 1. Opslagruimten met bovenliggende ruimte op het niveau van *n* zijn op het niveau van *n + 1*. Deze waarden is ingesteld, kunt u de resultaten op specifieke niveaus filteren. Dit zijn inclusief waarden als de waarde. Apparaten, sensoren en andere objecten worden beschouwd als op hetzelfde niveau als hun dichtstbijzijnde ruimte. Als u alle objecten op een bepaald niveau, stel zowel **minLevel** en **maxLevel** op dezelfde waarde.

- **minRelative** en **maxRelative**: Wanneer u deze filters zijn opgegeven, wordt het niveau van de bijbehorende is ten opzichte van het niveau van de ID van de beschikbare ruimte:
   - Relatieve niveau *0* als hetzelfde niveau als de id van de beschikbare ruimte.
   - Relatieve niveau *1* vertegenwoordigt spaties op hetzelfde niveau als de onderliggende objecten van de beschikbare ruimte-ID. Relatieve niveau *n* lager is dan de opgegeven ruimte door opslagruimten vertegenwoordigt *n* niveaus.
   - Relatieve niveau *-1* spaties op hetzelfde niveau als het bovenliggende ruimte van de opgegeven ruimte vertegenwoordigt.

- **passeren**: Hiermee kunt u bladeren in beide richtingen van een bepaalde ruimte-ID, zoals opgegeven door de volgende waarden.
   - **Geen**: Deze standaardwaarde filters in de id van de beschikbare ruimte.
   - **Omlaag**: Hiermee worden gefilterd door de beschikbare ruimte-ID en bijbehorende descendants. 
   - **Van**: Hiermee worden gefilterd door de ID van de beschikbare ruimte en de bovenliggende elementen. 
   - **Reeks**: Hiermee worden gefilterd met een horizontale deel van de grafiek ruimtelijke op hetzelfde niveau als de id van de beschikbare ruimte. Dit moet ofwel de **minRelative** of de **maxRelative** true zijn ingesteld. 


### <a name="examples"></a>Voorbeelden

De volgende lijst bevat enkele voorbeelden van navigatie door de [/devices](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Devices) API's. Houd er rekening mee dat de tijdelijke aanduiding `YOUR_MANAGEMENT_API_URL` verwijst naar de URI van de digitale Twins-API's in de indeling `https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/api/v1.0/`, waarbij `YOUR_INSTANCE_NAME` is de naam van uw exemplaar Azure digitale dubbels en `YOUR_LOCATION` is de regio waar uw exemplaar wordt gehost.

- `YOUR_MANAGEMENT_API_URL/devices?maxLevel=1` retourneert alle apparaten die zijn gekoppeld aan basis-ruimten.
- `YOUR_MANAGEMENT_API_URL/devices?minLevel=2&maxLevel=4` retourneert alle apparaten die zijn gekoppeld aan de adresruimte van niveau 2, 3 of 4.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId` alle apparaten die rechtstreeks zijn gekoppeld aan mySpaceId retourneert.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down` retourneert alle apparaten die zijn gekoppeld aan mySpaceId of een van de afgeleiden ervan.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down&minLevel=1&minRelative=true` alle apparaten die zijn gekoppeld aan de descendanten van mySpaceId, met uitzondering van mySpaceId retourneert.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down&minLevel=1&minRelative=true&maxLevel=1&maxRelative=true` alle apparaten die zijn gekoppeld aan de direct onderliggende objecten van mySpaceId retourneert.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Up&maxLevel=-1&maxRelative=true` alle apparaten die zijn gekoppeld aan een van de bovenliggende leden van mySpaceId retourneert.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down&maxLevel=5` retourneert alle apparaten die zijn gekoppeld aan de descendanten van mySpaceId die op het niveau van kleiner zijn dan of gelijk is aan 5.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Span&minLevel=0&minRelative=true&maxLevel=0&maxRelative=true` retourneert alle apparaten die zijn gekoppeld aan die op hetzelfde niveau als mySpaceId zijn.


## <a name="odata-support"></a>OData-ondersteuning
De meeste van de API's die resulteren in verzamelingen, zoals een GET-aanroep op /spaces, ondersteunen de volgende subset van de algemene [OData](https://www.odata.org/getting-started/basic-tutorial/#queryData) queryopties system:  

* **$filter**
* **$orderby** 
* **$top**
* **$skip** -als u van plan bent om de volledige verzameling weer te geven, moet u deze als een volledige set in één aanroep aanvragen, en voer paginering in uw toepassing. 

Houd er rekening mee dat andere queryopties, zoals $count, $ wilt uitbreiden, $search, worden niet ondersteund.

### <a name="examples"></a>Voorbeelden

De volgende lijst bevat enkele voorbeelden van query's met behulp van de OData-queryopties voor systeem:

- `YOUR_MANAGEMENT_API_URL/devices?$top=3&$orderby=Name desc`
- `YOUR_MANAGEMENT_API_URL/keystores?$filter=endswith(Description,’space’)`
- `YOUR_MANAGEMENT_API_URL/propertykeys?$filter=Scope ne ‘Spaces’`
- `YOUR_MANAGEMENT_API_URL/resources?$filter=Size gt ‘M’`
- `YOUR_MANAGEMENT_API_URL/users?$top=4&$filter=endswith(LastName,’k’)&$orderby=LastName`
- `YOUR_MANAGEMENT_API_URL/spaces?$orderby=Name desc&$top=3&$filter=substringof('Floor’,Name)`
 

## <a name="next-steps"></a>Volgende stappen

Sommige algemene patronen van de API-query's vindt u meer [query's uitvoeren Azure digitale Twins-API's voor algemene taken](how-to-query-common-apis.md).


