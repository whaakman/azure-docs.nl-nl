---
title: Azure Service Fabric met overzicht van API Management | Microsoft Docs
description: Dit artikel bevat een inleiding tot het gebruik van Azure API Management als een gateway voor uw Service Fabric-toepassingen.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: vturecek
ms.openlocfilehash: a3eedacac5efb53f82e46a56285713dece56ffe8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="service-fabric-with-azure-api-management-overview"></a>Service Fabric met Azure API Management-overzicht

Cloud-toepassingen moeten doorgaans een front-gateway voor een potentieel inkomend voor gebruikers, apparaten of andere toepassingen. In de Service Fabric, een gateway worden alle stateless Services, zoals een [ASP.NET Core toepassing](service-fabric-reliable-services-communication-aspnetcore.md), of een andere service die zijn ontworpen voor inkomend verkeer, zoals [Event Hubs](https://docs.microsoft.com/azure/event-hubs/), [IoT Hub](https://docs.microsoft.com/azure/iot-hub/), of [Azure API Management](https://docs.microsoft.com/azure/api-management/).

Dit artikel bevat een inleiding tot het gebruik van Azure API Management als een gateway voor uw Service Fabric-toepassingen. API Management wordt rechtstreeks geïntegreerd met Service Fabric, zodat u voor het publiceren van API's met een groot aantal regels voor het doorsturen naar uw back-end Service Fabric-services. 

## <a name="architecture"></a>Architectuur
Een algemene Service Fabric-architectuur gebruikt een webtoepassing van één pagina waarmee HTTP-aanroepen naar de back-end-services die HTTP APIs blootstellen. De [aan de slag Service Fabric-voorbeeldtoepassing](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) toont een voorbeeld van deze architectuur.

In dit scenario wordt fungeert een stateless webservice als de gateway in de Service Fabric-toepassing. Deze aanpak moet u een webservice die proxy HTTP-aanvragen voor back-end-services kunt schrijven, zoals wordt weergegeven in het volgende diagram:

![Service Fabric met Azure API Management-topologie-overzicht][sf-web-app-stateless-gateway]

Naarmate toepassingen complexiteit groeien, dus de gateways die een API voor allerlei back-end-services moeten aanbieden te doen. Azure API Management is ontworpen voor het verwerken van complexe API's met regels voor het doorsturen, toegangsbeheer, snelheidsbeperking, bewaking, logboekregistratie en antwoord in cache opslaan met minimale inspanning. Azure API Management biedt ondersteuning voor detectie van Service Fabric-service, partitie-resolutie en replica selecteren om te routeren op intelligente wijze aanvragen rechtstreeks naar de back-endservices in Service Fabric zodat u niet hoeft te schrijven van uw eigen staatloze API-gateway. 

In dit scenario wordt de web-UI worden nog steeds aangeleverd via een webservice terwijl HTTP API-aanroepen worden beheerd en worden gerouteerd via Azure API Management, zoals wordt weergegeven in het volgende diagram:

![Service Fabric met Azure API Management-topologie-overzicht][sf-apim-web-app]

## <a name="application-scenarios"></a>Toepassingsscenario's

Services in Service Fabric mogelijk stateless of stateful en ze kunnen worden gepartitioneerd met een van drie schema's: singleton, int-64-bereik, en met de naam. Omzetting van de service-eindpunt is vereist om een specifieke partitie van een specifieke service-exemplaar te identificeren. Bij het omzetten van een eindpunt van een service, zowel de service-exemplaar-naam (bijvoorbeeld `fabric:/myapp/myservice`) en de specifieke partitie van de service moet worden opgegeven, behalve in het geval van singleton-partitie.

Azure API Management kan worden gebruikt met elke combinatie van stateless services, stateful services en een partitieschema.

## <a name="send-traffic-to-a-stateless-service"></a>Verkeer verzenden naar een stateless service

In het meest eenvoudige geval wordt verkeer doorgestuurd naar een stateless service-exemplaar. Om dit te bereiken, bevat een API Management-bewerking een beleid voor binnenkomend verkeer met een Service Fabric back-end die is toegewezen aan een specifieke staatloze service-exemplaar in de Service Fabric-back-end. Aanvragen die worden verzonden naar die service worden verzonden naar een willekeurige replica van de staatloze service-exemplaar.

#### <a name="example"></a>Voorbeeld
In het volgende scenario, een Service Fabric-toepassing bevat een stateless service met de naam `fabric:/app/fooservice`, die wordt er een interne HTTP-API. De naam van de service-exemplaar is bekend en vastgelegde rechtstreeks in het inkomende verwerking van API Management-beleid kan worden. 

![Service Fabric met Azure API Management-topologie-overzicht][sf-apim-static-stateless]

## <a name="send-traffic-to-a-stateful-service"></a>Verkeer verzenden naar een stateful service

Net als bij het scenario staatloze service, kan verkeer worden doorgestuurd naar een stateful service-exemplaar. In dit geval een API Management-bewerking bevat een beleid voor binnenkomend verkeer met een Service Fabric back-end die een aanvraag wordt toegewezen aan een specifieke partitie van een specifieke *stateful* service-exemplaar. Toewijzen van elke aanvraag aan via een lambda-methode met een invoer van de binnenkomende HTTP-aanvraag, zoals een waarde in het URL-pad wordt berekend op basis van de partitie. Het beleid kan worden geconfigureerd voor het verzenden van aanvragen met alleen de primaire replica of naar een willekeurige replica voor leesbewerkingen.

#### <a name="example"></a>Voorbeeld

In het volgende scenario, een Service Fabric-toepassing bevat een gepartitioneerde stateful service met de naam `fabric:/app/userservice` die wordt er een interne HTTP-API. De naam van de service-exemplaar is bekend en vastgelegde rechtstreeks in het inkomende verwerking van API Management-beleid kan worden.  

De service is gepartitioneerd met behulp van het partitieschema Int64 met twee partities en een sleutel bereik die omvat `Int64.MinValue` naar `Int64.MaxValue`. Het back-end-beleid wordt een partitiesleutel binnen dat bereik berekend door het converteren van de `id` waarde die is opgegeven in het URL-aanvraag-pad naar een 64-bits geheel getal, hoewel elk algoritme kan hier worden gebruikt voor het berekenen van de partitiesleutel. 

![Service Fabric met Azure API Management-topologie-overzicht][sf-apim-static-stateful]

## <a name="send-traffic-to-multiple-stateless-services"></a>Verkeer verzenden naar meerdere stateless services

U kunt een API Management-bewerking die aanvragen wordt toegewezen aan meer dan één service-exemplaar definiëren in meer geavanceerde scenario's. In dit geval wordt bevat elke bewerking een beleid dat is toegewezen aanvragen aan een specifieke service-exemplaar op basis van waarden van de binnenkomende HTTP-aanvraag, zoals de URL-pad of de query-tekenreeks en in het geval van een stateful services een partitie in het service-exemplaar. 

Om dit te bereiken, bevat een API Management-bewerking een beleid voor binnenkomend verkeer met een Service Fabric back-end die is toegewezen aan een stateless service-exemplaar in de Service Fabric back-end op basis van waarden ophalen uit de binnenkomende HTTP-aanvraag. Aanvragen voor een service-exemplaar worden verzonden naar een willekeurige replica van de service-exemplaar.

#### <a name="example"></a>Voorbeeld

In dit voorbeeld wordt een nieuwe staatloze service-exemplaar voor elke gebruiker van een toepassing gemaakt met de naam van een dynamisch gegenereerd met de volgende formule:
 
 - `fabric:/app/users/<username>`

 Elke service een unieke naam heeft, maar de namen niet bekend zijn vooraf omdat de services worden gemaakt naar aanleiding van de gebruiker of beheerder invoer en dus kan niet worden hardgecodeerd in APIM beleid of routeringsregels. In plaats daarvan de naam van de service voor het verzenden van een aanvraag wordt gegenereerd in de beleidsdefinitie van de back-end-van de `name` waarde die is opgegeven in het URL-aanvraag-pad. Bijvoorbeeld:

  - Een aanvraag voor `/api/users/foo` wordt doorgestuurd naar de service-exemplaar`fabric:/app/users/foo`
  - Een aanvraag voor `/api/users/bar` wordt doorgestuurd naar de service-exemplaar`fabric:/app/users/bar`

![Service Fabric met Azure API Management-topologie-overzicht][sf-apim-dynamic-stateless]

## <a name="send-traffic-to-multiple-stateful-services"></a>Verkeer verzenden naar meerdere stateful services

Vergelijkbaar met het voorbeeld staatloze service, een API Management-bewerking kunt toewijzen aanvragen met meer dan één **stateful** service-exemplaar, in welk geval u ook moet mogelijk voor het uitvoeren van de omzetting van de partitie voor elk stateful service-exemplaar.

Om dit te bereiken, bevat een API Management-bewerking een beleid voor binnenkomend verkeer met een Service Fabric back-end die is toegewezen aan een stateful service-exemplaar in de Service Fabric back-end op basis van waarden ophalen uit de binnenkomende HTTP-aanvraag. Naast het toewijzen van een aanvraag aan specifieke service-exemplaar, kan de aanvraag ook worden toegewezen aan een specifieke partitie binnen het service-exemplaar, en desgewenst op de primaire replica of een willekeurige secundaire replica in de partitie.

#### <a name="example"></a>Voorbeeld

In dit voorbeeld wordt een nieuwe stateful service-exemplaar voor elke gebruiker van de toepassing gemaakt met de naam van een dynamisch gegenereerd met de volgende formule:
 
 - `fabric:/app/users/<username>`

 Elke service een unieke naam heeft, maar de namen niet bekend zijn vooraf omdat de services worden gemaakt naar aanleiding van de gebruiker of beheerder invoer en dus kan niet worden hardgecodeerd in APIM beleid of routeringsregels. In plaats daarvan de naam van de service voor het verzenden van een aanvraag wordt gegenereerd in de beleidsdefinitie van de back-end-van de `name` waarde opgegeven in het URL-aanvraag-pad. Bijvoorbeeld:

  - Een aanvraag voor `/api/users/foo` wordt doorgestuurd naar de service-exemplaar`fabric:/app/users/foo`
  - Een aanvraag voor `/api/users/bar` wordt doorgestuurd naar de service-exemplaar`fabric:/app/users/bar`

Elk exemplaar van de service ook is gepartitioneerd met behulp van het partitieschema Int64 met twee partities en een sleutel bereik die omvat `Int64.MinValue` naar `Int64.MaxValue`. Het back-end-beleid wordt een partitiesleutel binnen dat bereik berekend door het converteren van de `id` waarde die is opgegeven in het URL-aanvraag-pad naar een 64-bits geheel getal, hoewel elk algoritme kan hier worden gebruikt voor het berekenen van de partitiesleutel. 

![Service Fabric met Azure API Management-topologie-overzicht][sf-apim-dynamic-stateful]

## <a name="next-steps"></a>Volgende stappen

Ga als volgt de [snelstartgids](service-fabric-api-management-quick-start.md) voor het instellen van uw eerste Service Fabric-cluster met API Management en stroom aanvragen via de API Management voor uw service.

<!-- links -->

<!-- pics -->
[sf-apim-web-app]: ./media/service-fabric-api-management-overview/sf-apim-web-app.png
[sf-web-app-stateless-gateway]: ./media/service-fabric-api-management-overview/sf-web-app-stateless-gateway.png
[sf-apim-static-stateless]: ./media/service-fabric-api-management-overview/sf-apim-static-stateless.png
[sf-apim-static-stateful]: ./media/service-fabric-api-management-overview/sf-apim-static-stateful.png
[sf-apim-dynamic-stateless]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateless.png
[sf-apim-dynamic-stateful]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateful.png