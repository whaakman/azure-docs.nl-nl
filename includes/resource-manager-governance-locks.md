---
title: bestand opnemen
description: bestand opnemen
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 0cb3de7d893ccfe638468110b1b6f5fb61b2bc7c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
Resource vergrendelingen te voorkomen dat gebruikers in uw organisatie per ongeluk worden kritieke bronnen wijzigen of verwijderen. In tegenstelling tot toegangsbeheer op basis van functie toepassen resource vergrendelingen een beperking voor alle gebruikers en rollen. 

U kunt de vergrendeling op instellen **CanNotDelete** of **ReadOnly**. In de portal voor het niveau van de vergrendelingen worden weergegeven als **verwijderen** en **alleen-lezen** respectievelijk.

* **CanNotDelete** betekent geautoriseerde gebruikers kunnen nog steeds lezen en wijzigen van een resource, maar de resource kan niet worden verwijderd. 
* **Alleen-lezen** betekent geautoriseerde gebruikers kunnen een resource lezen, maar ze niet verwijderen of bijwerken van de resource. Toepassen van deze vergrendeling is vergelijkbaar met alle gemachtigde gebruikers beperken tot de machtigingen verleend door de **lezer** rol. 

> [!TIP]
> Wees voorzichtig met het toepassen van een **ReadOnly** vergrendelen. Bepaalde bewerkingen die lijkt lezen bewerkingen daadwerkelijk extra acties vereist. Bijvoorbeeld, een **ReadOnly** vergrendeling van een opslagaccount wordt voorkomen dat alle gebruikers weergeven van de sleutels. De lijst met sleutels opnieuw wordt verwerkt door een POST-aanvraag omdat de geretourneerde sleutels zijn beschikbaar voor schrijfbewerkingen. Een **ReadOnly** vergrendelen op een App Service-bron wordt voorkomen dat Visual Studio Server Explorer-bestanden voor de resource worden weergegeven omdat die interactie voor toegang voor schrijven vereist.

Wanneer u een vergrendeling op een bovenliggend bereik toepast, nemen alle resources binnen dat bereik de dezelfde vergrendeling. Zelfs resources die u later toevoegen overnemen de vergrendeling van het bovenliggende item. De meest beperkende vergrendeling in de overname voorrang.

Vergrendelingen van Resource Manager alleen van toepassing op bewerkingen die in de vlak management, die uit de bewerkingen die worden verzonden optreden bestaat naar `https://management.azure.com`. De vergrendelingen beperken niet hoe resources hun eigen functies verwerken. Wijzigingen in de resourcedefinitie zijn beperkt, maar de bewerkingen van resources zijn niet beperkt. Bijvoorbeeld, een vergrendeling van het kenmerk alleen-lezen voor een SQL-Database wordt voorkomen dat u de database wijzigen of verwijderen. Het niet voorkomen dat u maken, bijwerken of verwijderen van gegevens in de database. Gegevenstransacties zijn toegestaan, omdat deze bewerkingen niet worden verzonden naar `https://management.azure.com`.

### <a name="who-can-create-or-delete-locks-in-your-organization"></a>Wie kunt maken of verwijderen van vergrendelingen in uw organisatie
Als u wilt maken of verwijderen van management vergrendelingen, u moet toegang hebben tot `Microsoft.Authorization/locks/*` acties. Van de ingebouwde rollen alleen **eigenaar** en **beheerder voor gebruikerstoegang** deze acties worden verleend.
