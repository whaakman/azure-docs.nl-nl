---
title: Wat is App-configuratie voor Azure? | Microsoft Docs
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
ms.openlocfilehash: 8c2c31b4d87ac2db123c0cae63679e3773734b4f
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226280"
---
# <a name="what-is-azure-app-configuration"></a>Wat is App-configuratie voor Azure?

Azure App-configuratie biedt een service voor toepassingsinstellingen centraal beheren. Moderne programma's, met name programma's die worden uitgevoerd in een cloud hebben doorgaans veel onderdelen die in de natuur worden gedistribueerd. Het verspreiden van configuratie-instellingen over deze onderdelen kan leiden tot moeilijk oplosbare fouten tijdens de implementatie van een toepassing. App-configuratie gebruiken voor het opslaan van de instellingen voor uw toepassing en hun toegang tot op één plek te beveiligen.

App-configuratie is gratis tijdens de preview-periode moet worden gebruikt. Als u wilt uitproberen, [registreren](https://aka.ms/azconfig/register) voor de Preview-versie.

## <a name="why-use-app-configuration"></a>Waarom App-configuratie gebruiken?

Cloudgebaseerde toepassingen worden vaak uitgevoerd op meerdere virtuele machines of containers in meerdere regio's en gebruiken meerdere externe services. Het maken van een gedistribueerde toepassing die is robuust en schaalbaar is een uitdaging. 

Verschillende programming methodologieën zodat ontwikkelaars die omgaan met de toenemende complexiteit van het bouwen van toepassingen. De app 12-factor wordt bijvoorbeeld beschreven veel goed geteste architectonische patronen en best practices voor gebruik met cloud-toepassingen. Een belangrijke aanbeveling in deze handleiding is om de configuratie van de code te scheiden. In dit geval moeten de configuratie-instellingen van een toepassing worden gehouden van het uitvoerbare bestand en opgehaald uit de runtime-omgeving of een externe bron.

Elke toepassing gebruik kan maken van App-configuratie, zijn de volgende voorbeelden de typen toepassingen die baat bij het gebruik van deze hebben:

* Microservices op basis van Azure Kubernetes Service, Azure Service Fabric of andere beperkte apps die zijn geïmplementeerd in een of meer regio 's
* Serverloze apps, waaronder Azure Functions of andere staatloze compute gebeurtenisgestuurde apps
* Pijplijn voor continue implementatie

App Configuration biedt de volgende voordelen:

* Een volledig beheerde service die kan worden ingesteld in minuten
* Flexibele belangrijke opmerkingen en toewijzingen
* Labelen met labels
* Point-in-time-herhaling van instellingen
* Vergelijking van twee sets met configuraties voor aangepaste gedefinieerde dimensies
* Verbeterde beveiliging via Azure beheerde identiteiten
* Volledige gegevens coderingen, in rust en doorvoer
* Systeemeigen integratie met populaire frameworks

App-configuratie is een aanvulling op [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), die wordt gebruikt voor het opslaan van toepassingsgeheimen. App-configuratie wordt het gemakkelijker te implementeren de volgende scenario's:

* Gecentraliseerd beheer en distributie van hiërarchische configuratiegegevens voor verschillende omgevingen en regio 's
* Wijzigingen in de dynamische configuratie zonder de noodzaak om te implementeren of een toepassing opnieuw hebt gestart
* Functie-management

## <a name="use-app-configuration"></a>App-configuratie gebruiken

De eenvoudigste manier om een appconfiguratiearchief toe te voegen aan uw toepassing is via een door Microsoft aangeboden clientbibliotheek. Op basis van de programmeertaal en framework, zijn de volgende aanbevolen methoden beschikbaar voor u.

| Computertaal en framework | Verbinding maken |
|---|---|
| .NET core en ASP.NET Core | App-configuratie-provider voor .NET Core |
| .NET- en ASP.NET | Opbouwfunctie voor App-configuratie voor .NET |
| Java Spring | App-configuratie-client voor Spring Cloud |
| Andere | App-configuratie REST-API |

## <a name="next-steps"></a>Volgende stappen

* [Snelstart: een ASP.NET-web-app maken](quickstart-aspnet-core-app.md) 
