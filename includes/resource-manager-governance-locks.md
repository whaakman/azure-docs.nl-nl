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
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56825085"
---
Resourcevergrendelingen te voorkomen dat gebruikers in uw organisatie per ongeluk verwijderen of aanpassen van kritieke bronnen. In tegenstelling tot toegangsbeheer op basis van rollen wordt met resourcevergrendelingen een beperking toegepast op alle gebruikers en rollen. 

U kunt de vergrendeling instellen op **CanNotDelete** of **ReadOnly**. In de portal voor het niveau van de vergrendelingen worden weergegeven als **verwijderen** en **alleen-lezen** respectievelijk.

* **CanNotDelete** betekent geautoriseerde gebruikers kunnen nog steeds lezen en wijzigen van een resource, maar ze kunnen de resource niet verwijderen. 
* **Alleen-lezen** betekent geautoriseerde gebruikers kunnen een resource lezen, maar ze niet verwijderen of bijwerken van de resource. Toepassen van deze vergrendeling is vergelijkbaar met alle gemachtigde gebruikers beperken tot de machtigingen die zijn verleend door de **lezer** rol. 

> [!TIP]
> Wees voorzichtig bij het toepassen van een **ReadOnly** vergrendelen. Bepaalde bewerkingen die kunnen worden gelezen bewerkingen daadwerkelijk extra acties vereist. Bijvoorbeeld, een **ReadOnly** vergrendeling op een storage-account wordt voorkomen dat alle gebruikers van de aanbieding van de sleutels. De lijst met sleutels bewerking via een POST-aanvraag wordt verwerkt, omdat de geretourneerde sleutels zijn beschikbaar voor schrijfbewerkingen. Een **ReadOnly** vergrendeling op een App Service-resource voorkomt dat Visual Studio Server Explorer weergeven van bestanden voor de resource omdat die interactie schrijftoegang is vereist.

Wanneer u een vergrendeling op een overkoepelend bereik toepast, nemen alle resources binnen dat bereik de dezelfde vergrendeling. Zelfs resources die u later toevoegen overnemen de vergrendeling van het bovenliggende item. De meest beperkende vergrendeling in de overname voorrang.

Resource Manager-vergrendelingen gelden alleen voor bewerkingen die in de beheerlaag, die uit bewerkingen die worden verzonden optreden bestaat naar `https://management.azure.com`. De vergrendelingen beperken niet hoe de hun eigen functies voor het verwerken van resources. Wijzigingen van resources zijn beperkt, maar de bewerkingen van resources zijn niet beperkt. Bijvoorbeeld, een alleen-lezenvergrendeling op een SQL-Database wordt voorkomen dat u verwijderen of aanpassen van de database. Het wordt niet voorkomen dat u uit het maken, bijwerken of verwijderen van gegevens in de database. Gegevenstransacties zijn toegestaan, omdat deze bewerkingen niet naar verzonden worden `https://management.azure.com`.

### <a name="who-can-create-or-delete-locks-in-your-organization"></a>Wie kunt maken of verwijderen van vergrendelingen in uw organisatie
Als u beheervergrendelingen wilt maken of verwijderen, moet u toegang hebben tot `Microsoft.Authorization/locks/*`-acties. Van de ingebouwde rollen worden deze acties alleen toegekend aan **Eigenaar** en **Administrator voor gebruikerstoegang**.
