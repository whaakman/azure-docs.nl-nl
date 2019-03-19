---
title: Wat is Azure App Configuration | Microsoft Docs
description: Een overzicht van de service Azure App Configuration.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 3e96a1af4746c6344f9565d0f0de8ab2b2b8d8bc
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58009397"
---
# <a name="what-is-azure-app-configuration"></a>Wat is Azure App Configuration

Azure App Configuration is een service voor het centraal beheren van toepassingsinstellingen. Moderne programma's, met name die in een cloud worden uitgevoerd, hebben doorgaans veel onderdelen die zijn gedistribueerd. Het verspreiden van configuratie-instellingen over deze onderdelen kan leiden tot moeilijk oplosbare fouten tijdens de implementatie van een toepassing. Met App Configuration kunt u alle instellingen voor uw toepassing opslaan en de toegang ertoe beveiligen op één plek.

App Configuration-service is momenteel in **preview**. Het is gratis te gebruiken in deze previewperiode. Hoewel er geen speciale goedkeuringsproces voor toegang tot de service, [registreren](https://aka.ms/azconfig/register) met ons zodat wij u van elke belangrijke wijziging vooraf kan informeren.

## <a name="why-use-app-configuration"></a>De voordelen van App Configuration

Cloudgebaseerde toepassingen worden vaak uitgevoerd op meerdere virtuele machines of containers in meerdere regio's en gebruiken meerdere externe services. Het maken van een gedistribueerde toepassing die robuust en schaalbaar is, is een echte uitdaging. Er zijn verschillende programmeermethoden ontstaan om ontwikkelaars te helpen omgaan met de toenemende complexiteit van het bouwen van deze toepassingen. De 12-factor app biedt bijvoorbeeld veel goed geteste architectonische patronen en best practices voor cloudtoepassingen. Een belangrijke aanbeveling in deze handleiding is om de configuratie van de code te scheiden. Dit betekent dat de configuratie van een toepassing, zoals instellingen, extern van het bijbehorende uitvoerbare bestand moeten worden gehouden en moeten worden opgehaald uit de bijbehorende runtime-omgeving of een externe bron.

Hoewel elke toepassing hier gebruik van kan maken, zouden de typen toepassingen in de volgende voorbeelden zeker App Configuration moeten gebruiken:

* Microservices op basis van AKS, Service Fabric of andere apps in containers die worden geïmplementeerd in een of meer regio's.
* Serverloze apps, met inbegrip van Azure Functions of andere gebeurtenisgestuurde, staatloze compute-apps.
* Pijplijn voor doorlopende implementatie.

App Configuration biedt de volgende voordelen:

* Een volledig beheerde service die in enkele minuten kan worden ingesteld.
* Flexibele belangrijke weergaven en toewijzingen.
* Taggen met labels.
* Point-in-time-herhaling van instellingen.
* Vergelijking van twee sets met configuraties voor speciaal gedefinieerde dimensies.
* Verbeterde beveiliging met door Azure beheerde identiteiten.
* Complete gegevensversleuteling, bij inactiviteit en tijdens overdracht.
* Systeemeigen integratie met populaire frameworks.

App-configuratie is een aanvulling op [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) gebruikt voor het opslaan van toepassingsgeheimen. App-configuratie wordt het eenvoudiger voor het implementeren van de volgende scenario's:

* Gecentraliseerd beheer en distributie van hiërarchische configuratiegegevens voor verschillende omgevingen en regio's.
* Wijzigingen in de dynamische configuratie zonder opnieuw te implementeren of een toepassing opnieuw te starten.
* Beheer van de functie.

## <a name="how-to-use-app-configuration"></a>App Configuration gebruiken

De eenvoudigste manier om een appconfiguratiearchief toe te voegen aan uw toepassing is via een door Microsoft aangeboden clientbibliotheek. Hierna worden de beste beschikbare methoden voor u weergegeven, afhankelijk van de computertaal en het framework:

| Computertaal en framework | Verbinding maken |
|---|---|
| **.NET Core** en **ASP.NET Core** | App Configuration-configuratieprovider voor .NET Core. |
| **.NET** en **ASP.NET** | App Configuration-configuratiemaker voor .NET. |
| **Java Spring** | App Configuration-configuratieclient voor Spring Cloud. |
| Andere | App Configuration REST API. |

## <a name="next-steps"></a>Volgende stappen

* [Snelstart: een ASP.NET-web-app maken](quickstart-aspnet-core-app.md)  
