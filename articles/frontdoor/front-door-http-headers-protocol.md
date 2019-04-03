---
title: Ondersteuning voor HTTP-headers in Azure voordeur Service-protocol | Microsoft Docs
description: Dit artikel beschrijft de HTTP-header-protocollen die ondersteuning biedt voor voordeur Service.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 92e8435e4336c68982e4becc2a95f99b2c776c0e
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58861839"
---
# <a name="protocol-support-for-http-headers-in-azure-front-door-service"></a>Protocolondersteuning voor HTTP-headers in Azure voordeur Service
In dit artikel bevat een overzicht van het protocol dat voordeur Service met onderdelen van het pad van de aanroep ondersteunt (Zie afbeelding). De volgende secties bevatten meer informatie over HTTP-headers ondersteund door de voordeur Service.

![Azure voordeur Service HTTP-headers-protocol][1]

>[!IMPORTANT]
>Voordeur Service certificeren niet elke HTTP-headers die hier niet zijn beschreven.

## <a name="client-to-front-door-service"></a>Voordeur Service-client
Voordeur Service accepteert de meeste headers van de inkomende aanvraag zonder dat ze worden gewijzigd. Enkele gereserveerde headers zijn verwijderd uit de binnenkomende aanvraag als verzonden, met inbegrip van headers met de X - FD-* voorvoegsel.

## <a name="front-door-service-to-backend"></a>Back-end-Service voordeur

Voordeur Service omvat de headers van een binnenkomende aanvraag als verwijderd vanwege beperkingen. Voordeur ook de volgende headers toegevoegd:

| Header  | Voorbeeld en beschrijving |
| ------------- | ------------- |
| Via |  Via: 1.1 Azure </br> Voordeur wordt gevolgd door de versie van de client HTTP *Azure* als de waarde voor de header Via. Dit geeft aan van de client HTTP-versie en dat voordeur is een tussenliggende ontvanger voor de aanvraag tussen de client en de back-end.  |
| X-Azure-ClientIP | X-Azure-ClientIP: 127.0.0.1 </br> Hiermee geeft u de client-IP-adres dat is gekoppeld aan de aanvraag wordt verwerkt. Een aanvraag die afkomstig zijn van een proxy kan bijvoorbeeld de header X doorgestuurd voor om aan te geven van de IP-adres van de oorspronkelijke aanvrager toevoegen. |
| X-Azure-SocketIP |  X-Azure-SocketIP: 127.0.0.1 </br> Hiermee geeft u het IP-adres van het socket die is gekoppeld aan de TCP-verbinding die de huidige aanvraag afkomstig is uit. IP-adres van een aanvraag van de client zijn mogelijk niet gelijk zijn aan het socket-IP-adres omdat deze willekeurig kan worden overschreven door een gebruiker.|
| X-Azure-Ref |  X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz </br> Een unieke referentie-tekenreeks die een aanvraag die is geleverd door de voordeur aanduidt. Het wordt gebruikt om te zoeken naar Logboeken openen en essentieel is voor het oplossen van problemen.|
| X-Azure-RequestChain |  X-Azure-RequestChain: hops voor de = 1 </br> Een koptekst dat voordeur wordt gebruikt voor het detecteren van aanvraag lussen en een afhankelijkheid niet op deze gebruikers moeten uitvoeren. |
| X doorgestuurd voor | X doorgestuurd voor: 127.0.0.1 </br> Het veld X-Forwarded-For (XFF) HTTP-header wordt vaak aangeduid voor het oorspronkelijke IP-adres van een client die verbinding maken met een webserver via een HTTP-proxy- of load balancer. Als er een bestaande XFF-header, voordeur het client-socket-IP-adres toegevoegd aan het of voegt de XFF-header met de client-IP-socket. |
| X-doorgestuurd-Host | X-doorgestuurd-Host: contoso.azurefd.net </br> Het veld met X-doorgestuurd-Host HTTP-header is een veelgebruikte methode gebruikt voor het identificeren van de oorspronkelijke host aangevraagd door de client in de header van het Host-HTTP-aanvraag. Dit is omdat de hostnaam van de voordeur kan verschillen voor de back-endserver verwerken van de aanvraag. |
| X-Forwarded-Proto | X-doorgestuurd-protocol: http </br> Het veld met X-doorgestuurd Protoco HTTP-header wordt vaak gebruikt voor het identificeren van het oorspronkelijke protocol van een HTTP-aanvraag omdat voordeur, op basis van configuratie, met de back-end communiceren kan met behulp van HTTPS. Dit geldt zelfs als de aanvraag voor de omgekeerde proxy HTTP is. |

## <a name="front-door-service-to-client"></a>Client-Service voordeur

Alle headers worden verzonden naar de voordeur vanaf de back-end zijn ook doorgegeven aan de client. Hier volgen de koppen die zijn verzonden vanaf de voordeur aan clients.

| Header  | Voorbeeld |
| ------------- | ------------- |
| X-Azure-Ref |  *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Dit is een unieke referentie-tekenreeks die een aanvraag die is geleverd door de voordeur aanduidt. Dit is essentieel voor het oplossen van problemen zoals het wordt gebruikt om te zoeken naar Logboeken openen.|

## <a name="next-steps"></a>Volgende stappen

- [Een Front Door maken](quickstart-create-front-door.md)
- [De werking van de voordeur](front-door-routing-architecture.md)

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png