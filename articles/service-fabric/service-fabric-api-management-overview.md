---
title: Azure Service Fabric met overzicht van API Management | Microsoft Docs
description: In dit artikel bevat een inleiding voor het gebruik van Azure API Management als een gateway aan uw Service Fabric-toepassingen.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: vturecek
ms.openlocfilehash: 0dac2730bcc13b979de6a8faaaa53c0aaf15e902
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669347"
---
# <a name="service-fabric-with-azure-api-management-overview"></a>Service Fabric met Azure API Management-overzicht

Cloudtoepassingen hebben meestal een gateway in de front-end nodig om een centraal ingangspunt te bieden voor gebruikers, apparaten of andere toepassingen. In Service Fabric, een gateway kan worden elke stateless service, zoals een [ASP.NET Core-toepassing](service-fabric-reliable-services-communication-aspnetcore.md), of een andere service die is ontworpen voor inkomend verkeer, zoals [Event Hubs](https://docs.microsoft.com/azure/event-hubs/), [IoT-Hub](https://docs.microsoft.com/azure/iot-hub/), of [Azure API Management](https://docs.microsoft.com/azure/api-management/).

In dit artikel bevat een inleiding voor het gebruik van Azure API Management als een gateway aan uw Service Fabric-toepassingen. API Management wordt rechtstreeks geïntegreerd met Service Fabric, zodat u kunt voor het publiceren van API's met een uitgebreide set regels voor doorsturen naar uw back-end-Service Fabric-services. 

## <a name="availability"></a>Beschikbaarheid

> [!IMPORTANT]
> Deze functie is beschikbaar in de **Premium** en **Developer** lagen van API Management vanwege de vereiste virtuele netwerk ondersteuning.

## <a name="architecture"></a>Architectuur

Een algemene Service Fabric-architectuur maakt gebruik van een webtoepassing van één pagina die HTTP-aanroepen naar back-end-services die beschikbaar maken van HTTP APIs maakt. De [aan de slag-voorbeeld van Service Fabric-toepassing](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) toont een voorbeeld van deze architectuur.

In dit scenario wordt fungeert een stateless webservice als de gateway in de Service Fabric-toepassing. Deze methode moet u een webservice die proxy HTTP-aanvragen met back-end-services, kunt schrijven, zoals wordt weergegeven in het volgende diagram:

![Service Fabric met Azure API Management-topologie-overzicht][sf-web-app-stateless-gateway]

Als toepassingen in complexiteit toeneemt, dus de gateways die aanwezig van een API voor allerlei back-end-services zijn moeten te doen. Met Azure API Management is ontworpen voor het verwerken van complexe API's met regels voor doorsturen, toegangsbeheer, Frequentiebeperkingen, bewaking, logboekregistratie en antwoorden in cache opslaan met minimale inspanningen aan uw kant. Met Azure API Management biedt ondersteuning voor detectie van Service Fabric-service, partitie-omzetting en replica selecteren voor het op intelligente wijze routeren van aanvragen rechtstreeks naar de back-end-services in Service Fabric, zodat u hoeft te schrijven van uw eigen stateless API-gateway. 

In dit scenario wordt de web UI is nog steeds voldaan via een webservice, terwijl HTTP API-aanroepen worden beheerd en gerouteerd via Azure API Management, zoals wordt weergegeven in het volgende diagram:

![Service Fabric met Azure API Management-topologie-overzicht][sf-apim-web-app]

## <a name="application-scenarios"></a>Toepassingsscenario's

Services in Service Fabric stateless of stateful kunnen zijn en ze kunnen worden gepartitioneerd met behulp van een van drie schema's: singleton, int-64-bereik, en met de naam. Service-eindpunt resolutie is vereist voor het identificeren van een specifieke partitie van een specifieke service-exemplaar. Bij het omzetten van een eindpunt van een service, zowel de service-exemplaarnaam (bijvoorbeeld `fabric:/myapp/myservice`), evenals de specifieke partitie van de service moet worden opgegeven, behalve in het geval van singleton-partitie.

Met Azure API Management kunnen worden gebruikt met een combinatie van staatloze, stateful services en een partitieschema.

## <a name="send-traffic-to-a-stateless-service"></a>Verkeer verzenden naar een stateless service

In het meest eenvoudige geval is, wordt verkeer doorgestuurd naar een stateless service-exemplaar. Om dit te bereiken, bevat een API Management-bewerking een beleid voor binnenkomende verwerking met een Service Fabric-back-end die wordt toegewezen aan een specifieke stateless service-exemplaar in de Service Fabric-back-end. Aanvragen die worden verzonden naar deze service worden verzonden naar een willekeurige replica van de stateless service-exemplaar.

#### <a name="example"></a>Voorbeeld
In het volgende scenario, een Service Fabric-toepassing bevat een stateless service met de naam `fabric:/app/fooservice`, die een interne HTTP-API beschikbaar stelt. De naam van de service-exemplaar is erg bekend is en kan worden vastgelegd in het binnenkomende verwerking van API Management-beleid. 

![Service Fabric met Azure API Management-topologie-overzicht][sf-apim-static-stateless]

## <a name="send-traffic-to-a-stateful-service"></a>Verkeer verzenden naar een stateful service

Net als bij de stateless service-scenario, kan verkeer worden doorgestuurd naar een stateful service-exemplaar. In dit geval een API Management-bewerking bevat een beleid voor binnenkomende verwerking met een Service Fabric-back-end die een aanvraag wordt toegewezen aan een specifieke partitie van een specifieke *stateful* service-exemplaar. De partitie voor het toewijzen van elke aanvraag aan via een lambda-methode met behulp van een invoer van de binnenkomende HTTP-aanvraag, zoals een waarde in het URL-pad wordt berekend. Het beleid kan worden geconfigureerd voor het verzenden van aanvragen naar de primaire replica alleen, of naar een willekeurige replica voor leesbewerkingen.

#### <a name="example"></a>Voorbeeld

In het volgende scenario, een Service Fabric-toepassing bevat een gepartitioneerde stateful service met de naam `fabric:/app/userservice` die een interne HTTP-API beschikbaar stelt. De naam van de service-exemplaar is erg bekend is en kan worden vastgelegd in het binnenkomende verwerking van API Management-beleid.  

De service is gepartitioneerd met behulp van het partitieschema Int64 met twee partities en een sleutelbereik die zich uitstrekt `Int64.MinValue` naar `Int64.MaxValue`. Het beleid voor back-end een partitiesleutel binnen dat bereik worden berekend door het converteren van de `id` waarde die is opgegeven in het URL-aanvraag-pad naar een 64-bits geheel getal zijn, hoewel elk algoritme kan hier worden gebruikt voor het berekenen van de partitiesleutel. 

![Service Fabric met Azure API Management-topologie-overzicht][sf-apim-static-stateful]

## <a name="send-traffic-to-multiple-stateless-services"></a>Verkeer verzenden naar meerdere stateless services

U kunt een bewerking voor het beheer van de API waarmee aanvragen worden toegewezen aan meer dan één service-exemplaar definiëren in meer geavanceerde scenario's. In dit geval bevat elke bewerking een beleid waarmee aanvragen worden toegewezen aan een specifieke service-exemplaar op basis van waarden uit de binnenkomende HTTP-aanvraag, zoals de URL-pad of de query-tekenreeks en in het geval van stateful services, een partitie in het service-exemplaar. 

Om dit te bereiken, bevat een API Management-bewerking een beleid voor binnenkomende verwerking met een Service Fabric-back-end die wordt toegewezen aan een stateless service-exemplaar in de Service Fabric back-end op basis van waarden die zijn opgehaald uit de binnenkomende HTTP-aanvraag. Aanvragen voor een service-exemplaar worden verzonden naar een willekeurige replica van de service-exemplaar.

#### <a name="example"></a>Voorbeeld

In dit voorbeeld wordt een nieuw stateless service-exemplaar voor elke gebruiker van een toepassing gemaakt met de naam van een dynamisch gegenereerd met behulp van de volgende formule:
 
- `fabric:/app/users/<username>`

  Elke service heeft een unieke naam, maar de namen worden niet vooraf bekend omdat de services zijn gemaakt in reactie op de gebruiker of beheerder invoeren en dus kan niet worden vastgelegd in de APIM-beleid of regels voor doorsturen. In plaats daarvan de naam van de service voor het verzenden van een aanvraag wordt gegenereerd in de beleidsdefinitie van het voor back-end van de `name` waarde die is opgegeven in het URL-Aanvraagpad. Bijvoorbeeld:

  - Een aanvraag voor `/api/users/foo` wordt doorgestuurd naar de service-exemplaar `fabric:/app/users/foo`
  - Een aanvraag voor `/api/users/bar` wordt doorgestuurd naar de service-exemplaar `fabric:/app/users/bar`

![Service Fabric met Azure API Management-topologie-overzicht][sf-apim-dynamic-stateless]

## <a name="send-traffic-to-multiple-stateful-services"></a>Verkeer verzenden naar meerdere stateful services

Net als bij het voorbeeld stateless service, een bewerking voor het beheer van de API kunt toewijzen aanvragen met meer dan één **stateful** service-exemplaar, in welk geval u ook mogelijk om uit te voeren van partitie-omzetting voor elke stateful service-exemplaar.

Om dit te bereiken, bevat een API Management-bewerking een beleid voor binnenkomende verwerking met een Service Fabric-back-end die wordt toegewezen aan een stateful service-exemplaar in de Service Fabric back-end op basis van waarden die zijn opgehaald uit de binnenkomende HTTP-aanvraag. Naast het toewijzen van een aanvraag naar specifieke service-exemplaar, kan de aanvraag ook worden toegewezen aan een specifieke partitie binnen het service-exemplaar en eventueel op de primaire replica of een willekeurige secundaire replica in de partitie.

#### <a name="example"></a>Voorbeeld

In dit voorbeeld wordt een nieuw stateful service-exemplaar voor elke gebruiker van de toepassing gemaakt met de naam van een dynamisch gegenereerd met behulp van de volgende formule:
 
- `fabric:/app/users/<username>`

  Elke service heeft een unieke naam, maar de namen worden niet vooraf bekend omdat de services zijn gemaakt in reactie op de gebruiker of beheerder invoeren en dus kan niet worden vastgelegd in de APIM-beleid of regels voor doorsturen. In plaats daarvan de naam van de service voor het verzenden van een aanvraag wordt gegenereerd in de beleidsdefinitie van het voor back-end van de `name` waarde opgegeven in het pad van URL-aanvraag. Bijvoorbeeld:

  - Een aanvraag voor `/api/users/foo` wordt doorgestuurd naar de service-exemplaar `fabric:/app/users/foo`
  - Een aanvraag voor `/api/users/bar` wordt doorgestuurd naar de service-exemplaar `fabric:/app/users/bar`

Elk service-exemplaar is ook gepartitioneerd met behulp van het partitieschema Int64 met twee partities en een sleutelbereik die zich uitstrekt `Int64.MinValue` naar `Int64.MaxValue`. Het beleid voor back-end een partitiesleutel binnen dat bereik worden berekend door het converteren van de `id` waarde die is opgegeven in het URL-aanvraag-pad naar een 64-bits geheel getal zijn, hoewel elk algoritme kan hier worden gebruikt voor het berekenen van de partitiesleutel. 

![Service Fabric met Azure API Management-topologie-overzicht][sf-apim-dynamic-stateful]

## <a name="next-steps"></a>Volgende stappen

Ga als volgt de [zelfstudie](service-fabric-tutorial-deploy-api-management.md) voor het instellen van uw eerste Service Fabric-cluster met API Management en flow aanvragen via API Management voor uw services.

<!-- links -->

<!-- pics -->
[sf-apim-web-app]: ./media/service-fabric-api-management-overview/sf-apim-web-app.png
[sf-web-app-stateless-gateway]: ./media/service-fabric-api-management-overview/sf-web-app-stateless-gateway.png
[sf-apim-static-stateless]: ./media/service-fabric-api-management-overview/sf-apim-static-stateless.png
[sf-apim-static-stateful]: ./media/service-fabric-api-management-overview/sf-apim-static-stateful.png
[sf-apim-dynamic-stateless]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateless.png
[sf-apim-dynamic-stateful]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateful.png