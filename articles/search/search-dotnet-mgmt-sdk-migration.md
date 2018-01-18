---
title: Upgraden naar Azure Search .NET Management SDK versie 2 | Microsoft Docs
description: Upgraden naar Azure Search .NET Management SDK versie 2
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/15/2018
ms.author: brjohnst
ms.openlocfilehash: ade32dcb4e0885c251c17fad46fb753b6134d027
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="upgrading-to-the-azure-search-net-management-sdk-version-2"></a>Upgraden naar Azure Search .NET Management SDK versie 2
Als u versie 1.0.2 of ouder van de [Azure Search .NET Management SDK](https://aka.ms/search-mgmt-sdk), dit artikel helpt u bij het bijwerken van uw toepassing te gebruiken versie 2.

Versie 2 van het beheer van Azure Search .NET SDK bevat een aantal wijzigingen uit eerdere versies. Dit zijn vooral kleine, zodat u uw code wijzigt, moet alleen minimale inspanning nodig. Zie [stappen voor het upgraden](#UpgradeSteps) voor instructies over het wijzigen van uw code voor het gebruik van de nieuwe versie van de SDK.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2"></a>Wat is er nieuw in versie 2
Versie 2 van het beheer van Azure Search .NET SDK is bedoeld voor de algemeen beschikbaar dezelfde versie van de Azure Search Management REST API als bij eerdere versies van de SDK, speciaal 2015-08-19. De wijzigingen in de SDK zijn uitsluitend clientzijde wijzigingen voor het verbeteren van de bruikbaarheid van de SDK zelf. Deze wijzigingen omvatten het volgende:

* `Services.CreateOrUpdate`en via de asynchrone versies nu automatisch de inrichting `SearchService` en niet retourneren, totdat de service inrichten is voltooid. Dit bespaart u code hoeft te schrijven dergelijke polling zelf.
* Als u nog steeds pollen service handmatig inrichting wilt, kunt u de nieuwe `Services.BeginCreateOrUpdate` methode of een van de asynchrone versies.
* Nieuwe methoden `Services.Update` en de asynchrone versies zijn toegevoegd aan de SDK. Deze methoden gebruiken HTTP PATCH ter ondersteuning van incrementele updates van een service. Bijvoorbeeld, kunt u nu een service schalen door het doorgeven van een `SearchService` exemplaar van deze methoden die alleen de gewenste bevat `partitionCount` en `replicaCount` eigenschappen. De oude manier om aanroepen `Services.Get`, wijzigen van de geretourneerde `SearchService`, en door te geven aan `Services.CreateOrUpdate` wordt nog steeds ondersteund, maar is niet meer nodig. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Stappen voor het upgraden
Eerst uw NuGet-verwijzing voor bijwerken `Microsoft.Azure.Management.Search` via de NuGet Package Manager-Console of door met de rechtermuisknop op uw projectverwijzingen en 'Beheren NuGet-pakketten...' selecteren in Visual Studio.

Zodra de NuGet heeft de nieuwe pakketten en afhankelijkheden zijn gedownload, opnieuw worden opgebouwd uw project. Afhankelijk van hoe uw code is gestructureerd, deze mogelijk opnieuw worden opgebouwd is. Zo ja, bent u nu klaar voor!

Als uw build is mislukt, kan het zijn dat u hebt geïmplementeerd enkele van de SDK-interfaces (bijvoorbeeld voor de doeleinden van testen unit) die zijn gewijzigd. U lost dit op, moet u voor het implementeren van de nieuwe methoden zoals `BeginCreateOrUpdateWithHttpMessagesAsync`.

Als u eventuele fouten in de build hebt opgelost, kunt u wijzigingen aanbrengen aan uw toepassing om te profiteren van nieuwe functionaliteit als u wenst. Nieuwe functies in de SDK worden beschreven in [wat is er nieuw in versie 2](#WhatsNew).

## <a name="conclusion"></a>Conclusie
Uw feedback is Welkom op de SDK. Als u problemen ondervindt, gerust ons om hulp te vragen op de [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Als u een bug vinden, kunt u het bestand een probleem in de [Azure .NET SDK GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-net/issues). Zorg ervoor dat het voorvoegsel van de titel van uw probleem met '[Azure Search]'.

Hartelijk dank voor het gebruik van Azure Search.
