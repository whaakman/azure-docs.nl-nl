---
title: Een upgrade naar de Azure Search .NET Management SDK versie 2 - Azure Search
description: Upgrade uitvoeren naar de Azure Search .NET Management SDK versie 2 van eerdere versies. Ontdek wat er nieuw is en welke wijzigingen in de code zijn vereist.
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 01/15/2018
ms.custom: seodec2018
ms.openlocfilehash: 369e5283d7350729e0d8cc44f94f1f2e71b133c3
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316863"
---
# <a name="upgrading-to-the-azure-search-net-management-sdk-version-2"></a>Een upgrade naar de Azure Search .NET Management SDK versie 2
Als u versie 1.0.2 of ouder bent van de [Azure Search .NET Management SDK](https://aka.ms/search-mgmt-sdk), in dit artikel ziet u een upgrade van uw toepassing voor het gebruik van versie 2.

Versie 2 van het beheer van Azure Search .NET SDK bevat enkele wijzigingen van eerdere versies. Dit zijn doorgaans kleine, zodat u uw code wijzigt, moet alleen minimale inspanning vereisen. Zie [stappen voor het upgraden](#UpgradeSteps) voor instructies over het wijzigen van uw code voor het gebruik van de nieuwe SDK-versie.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2"></a>Wat is er nieuw in versie 2
Versie 2 van het beheer van Azure Search .NET SDK is gericht op de dezelfde algemeen beschikbare versie van de Azure Search Management REST-API als eerdere versies van de SDK, specifiek 2015-08-19. De wijzigingen in de SDK zijn strikt client-side-wijzigingen voor het verbeteren van de bruikbaarheid van de SDK zelf. Deze wijzigingen omvatten het volgende:

* `Services.CreateOrUpdate` en de asynchrone versies nu automatisch de inrichting `SearchService` en niet retourneren totdat het inrichten van de service is voltooid. Dit bespaart u niet hoeft te schrijven van dergelijke code polling zelf.
* Als u nog steeds wilt te peilen service handmatig wordt ingericht, kunt u de nieuwe `Services.BeginCreateOrUpdate` methode of een van de asynchrone versies.
* Nieuwe methoden `Services.Update` en de asynchrone versies zijn toegevoegd aan de SDK. Deze methoden gebruiken HTTP-PATCH ter ondersteuning van incrementele updates van een service. Bijvoorbeeld, kunt u nu een service schalen door door te geven een `SearchService` exemplaar voor deze methoden die alleen de gewenste bevat `partitionCount` en `replicaCount` eigenschappen. De oude manier van aanroepen `Services.Get`, wijzigen van de geretourneerde `SearchService`, en daaraan te `Services.CreateOrUpdate` wordt nog steeds ondersteund, maar is niet meer nodig. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Stappen voor het bijwerken
Update eerst uw NuGet-verwijzing voor `Microsoft.Azure.Management.Search` met behulp van de NuGet Package Manager-Console of door met de rechtermuisknop op uw projectverwijzingen en 'Beheren NuGet-pakketten...' selecteren in Visual Studio.

Nadat NuGet heeft de nieuwe pakketten en de bijbehorende afhankelijkheden gedownload, bouwt u uw project opnieuw. Afhankelijk van hoe uw code is opgebouwd, kan deze opnieuw is. Als dit het geval is, bent u klaar om te beginnen.

Als uw build mislukt, kan het zijn dat u hebt geïmplementeerd enkele van de SDK-interfaces (bijvoorbeeld voor de doeleinden van Eenheidstesten), die zijn gewijzigd. U kunt dit oplossen, moet u de nieuwe methoden, zoals implementeren `BeginCreateOrUpdateWithHttpMessagesAsync`.

Als u eventuele fouten in de build hebt opgelost, kunt u wijzigingen aanbrengen aan uw toepassing om te profiteren van nieuwe functionaliteit als u wenst. Nieuwe functies in de SDK worden beschreven in [wat is er nieuw in versie 2](#WhatsNew).

## <a name="conclusion"></a>Conclusie
We stellen uw feedback over de SDK. Als u problemen ondervindt, gerust ons vragen voor meer informatie over op de [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Als u een bug vinden, kunt u het bestand een probleem in de [Azure .NET SDK GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-net/issues). Zorg ervoor dat u het voorvoegsel van de titel van uw probleem met '[Azure Search]'.

Hartelijk dank voor het gebruik van Azure Search.
