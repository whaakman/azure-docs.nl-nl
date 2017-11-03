---
title: Wat zijn verzamelingen van Power BI werkruimte?
description: Power BI Embedded kunt u Power BI-rapporten integreren in uw web- of mobiele toepassingen, dus u hoeft niet te aangepaste oplossingen bouwen.
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ROBOTS: NOINDEX
ms.assetid: 03649b72-b7d7-40ca-b077-12356d72d4f3
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: asaxton
ms.openlocfilehash: 7df172895bb926f1715370b941964e2c29ab393d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-are-power-bi-workspace-collections"></a>Wat zijn verzamelingen van Power BI werkruimte?

Met **Power BI werkruimte verzamelingen**, kunt u Power BI-rapporten integreren in uw web- of mobiele toepassingen.

![diagram van de toepassing](media/what-are-power-bi-workspace-collections/what-is.png)

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

Power BI werkruimte verzamelingen zijn een **Azure service** waarmee ISV's en app-ontwikkelaars kunnen surface ervaringen van Power BI gegevens binnen hun toepassingen. Als een ontwikkelaar u toepassingen hebt gemaakt en deze toepassingen hebben hun eigen gebruikers en een afzonderlijke reeks functies. Deze apps kunnen ook gebeuren hebben een aantal ingebouwde gegevenselementen zoals grafieken en rapporten die u kunnen nu door Microsoft Power BI werkruimte verzamelingen worden ingeschakeld. U hoeft niet een Power BI-account om uw app te gebruiken. U kunt zich aanmelden bij uw toepassing net als voorheen kunt en kunnen zien en gebruiken met Power BI-ervaring zonder aanvullende licenties reporting blijven.

## <a name="licensing-for-microsoft-power-bi-workspace-collections"></a>Licenties voor Microsoft Power BI-werkruimte verzamelingen

In de **Microsoft Power BI werkruimte verzamelingen** gebruiksmodel, licentieverlening voor Power BI niet de verantwoordelijkheid van de eindgebruiker is.  In plaats daarvan **sessies** zijn aangeschaft door de ontwikkelaar van de app de visuele elementen verbruikt en aan het abonnement dat eigenaar is van deze resources in rekening worden gebracht. 

## <a name="microsoft-power-bi-workspace-collections-conceptual-model"></a>Microsoft Power BI werkruimte verzamelingen conceptuele model

![Stroom van de toepassing met verzamelingen van de werkruimte](media/what-are-power-bi-workspace-collections/model.png)

Net als elke andere service in Azure-resources voor Power BI werkruimte verzamelingen zijn ingericht via de [Azure Resource Manager-API's](https://msdn.microsoft.com/library/mt712306.aspx). In dit geval wordt de resource die inrichten is een **Power BI-Werkruimteverzameling**.

## <a name="workspace-collection"></a>Werkruimteverzameling

Een **Werkruimteverzameling** is op het hoogste niveau Azure container voor bronnen met 0 of meer **werkruimten**.  Een **werkruimte** **verzameling** heeft alle van de standaardeigenschappen in Azure, evenals het volgende:

* **Toegangssleutels** – sleutels die worden gebruikt bij het veilig aanroepen van de Power BI-API's (beschreven in een volgende sectie).
* **Gebruikers** : Azure Active Directory (AAD)-gebruikers met beheerdersrechten om de Power BI-Werkruimteverzameling via de Azure portal of Azure Resource Manager-API te beheren.
* **Regio** : als onderdeel van het inrichten van een **Werkruimteverzameling**, kunt u een regio om te worden ingericht. Zie voor meer informatie [Azure-gebieden](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Werkruimte

Een **werkruimte** is een container van Power BI-inhoud, die kan bestaan uit gegevenssets en rapporten. Een **werkruimte** is leeg wanneer het eerst hebt gemaakt. U gaat ontwerpen inhoud met behulp van Power BI Desktop- en u hebt de PBIX programmatisch implementeren in uw werkruimte met de [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx). U kunt ook programmatisch maken uw gegevensset en maak vervolgens rapporten in uw toepassing in plaats van Power BI Desktop.

## <a name="using-workspace-collections-and-workspaces"></a>Met behulp van de werkruimte verzamelingen en werkruimten

**Werkruimte verzamelingen** en **werkruimten** zijn containers voor inhoud die worden gebruikt en georganiseerd in welke manier best past bij het ontwerp van de toepassing die u wilt maken. Er zijn veel verschillende manieren dat u de inhoud ervan kan rangschikken. U kunt alle inhoud in een werkruimte te plaatsen en vervolgens later met app-tokens meer onderverdelen in de inhoud onder uw klanten. U kunt ook uw klanten in de afzonderlijke werkruimten plaatsen zodat er een scheiding tussen deze twee. Of u kunt ervoor kiezen om te organiseren gebruikers per regio in plaats van door de klant. Dit flexibele ontwerp kunt u de beste manier om inhoud te ordenen.

## <a name="cached-datasets"></a>In de cache gegevenssets

In de cache gegevenssets kan worden gebruikt.  Echter, u gegevens in de cache niet vernieuwen zodra deze is geladen in **Microsoft Power BI werkruimte verzamelingen**. Een gegevensset in de cache betekent dat u de gegevens in Power BI Desktop hebt geïmporteerd in plaats van DirectQuery.

## <a name="authentication-and-authorization-with-app-tokens"></a>Verificatie en autorisatie met app-tokens

**Microsoft Power BI werkruimte verzamelingen** past omleiding naar uw toepassing uit te voeren van de benodigde verificatie en autorisatie. Er is geen expliciete vereiste dat uw eindgebruikers klanten van Azure Active Directory (Azure AD zijn).  In plaats daarvan wordt uitgedrukt voor uw toepassing **Microsoft Power BI werkruimte verzamelingen** autorisatie een Power BI-rapport weergeven met behulp van **verificatietokens toepassing (App-Tokens)**.  Deze **App-Tokens** worden gemaakt op die nodig zijn wanneer uw app wil een rapport weergeven.

![Diagram van App-token gebruik](media/what-are-power-bi-workspace-collections/app-tokens.png)

**Verificatietokens toepassing (App-Tokens)** worden gebruikt voor het authenticeren **Microsoft Power BI werkruimte verzamelingen**.  Er zijn drie soorten **App-Tokens**:

1. Inrichting van Tokens - die worden gebruikt bij het inrichten van een nieuwe **werkruimte** in een **Werkruimteverzameling**
2. Ontwikkeling van Tokens - gebruikt bij het aanroepen rechtstreeks naar de **Power BI REST-API's**
3. Tokens - gebruikt bij het aanroepen voor het weergeven van een rapport in de ingesloten iframe insluiten

Deze tokens worden gebruikt voor de verschillende fasen van de interacties met **Microsoft Power BI werkruimte verzamelingen**.  De tokens zijn zo ontworpen dat u machtigingen in uw app naar Power BI delegeren kunt. Zie voor meer informatie [App Token stromen](app-token-flow.md).

## <a name="create-or-edit-reports-within-your-application"></a>Maken of bewerken van rapporten in uw toepassing

U kunt nu bestaande rapporten bewerken of maken van nieuwe rapporten rechtstreeks in uw toepassing zonder gebruik te maken van Power BI Desktop. Dit is vereist dat een gegevensset bestaat in uw werkruimte.

## <a name="see-also"></a>Zie ook

[Algemene scenario's voor Microsoft Power BI werkruimte verzamelingen](scenarios.md)  
[Aan de slag met Microsoft Power BI werkruimte verzamelingen](get-started.md)  
[Aan de slag met het voorbeeld](get-started-sample.md)  
[Een rapport insluiten](embed-report.md)  
[Authenticating and authorizing in Power BI Workspace Collections](app-token-flow.md) (Verifiëren en autoriseren in Power BI Workspace Collections)  
[Voorbeeld van ingesloten JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI-CSharp Git-opslagplaats](https://github.com/Microsoft/PowerBI-CSharp)  
[Power BI-knooppunt Git-opslagplaats](https://github.com/Microsoft/PowerBI-Node)  

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)
