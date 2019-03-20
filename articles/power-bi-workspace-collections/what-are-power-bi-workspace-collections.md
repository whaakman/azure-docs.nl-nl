---
title: Wat zijn Power BI-werkruimteverzamelingen?
description: Power BI Embedded kunt u Power BI-rapporten integreren in uw web- of mobiele toepassingen, zodat u niet hoeft te maken van aangepaste oplossingen.
services: power-bi-embedded
author: markingmyname
ROBOTS: NOINDEX
ms.assetid: 03649b72-b7d7-40ca-b077-12356d72d4f3
ms.service: power-bi-embedded
ms.topic: article
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: maghan
ms.openlocfilehash: 6650eed6bf2559e9fb76b573189179a7f5df81ca
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57896701"
---
# <a name="what-are-power-bi-workspace-collections"></a>Wat zijn Power BI-werkruimteverzamelingen?

Met **Power BI Workspace Collections**, kunt u Power BI-rapporten integreren in uw web- of mobiele toepassingen.

![Een diagram van toepassing](media/what-are-power-bi-workspace-collections/what-is.png)

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

Power BI Workspace Collections is een **Azure-service** waarmee ISV's en app-ontwikkelaars voor Power BI-ervaring in hun toepassingen. Als ontwikkelaar u toepassingen hebt gemaakt en deze toepassingen hebben hun eigen gebruikers en een afzonderlijke set met functies. Deze apps kunnen ook gebeuren dat bepaalde gegevenselementen van de ingebouwde, zoals grafieken en rapporten die u nu mogelijk door Microsoft Power BI Workspace Collections gemaakt kunnen. U hoeft geen Power BI-account om uw app te gebruiken. U kunt zich aanmelden bij uw toepassing net als voorheen, en zien en gebruiken met de Power BI-ervaring zonder eventuele aanvullende licenties rapportage.

## <a name="licensing-for-microsoft-power-bi-workspace-collections"></a>Licentieverlening voor Microsoft Power BI-Werkruimteverzamelingen

In de **Microsoft Power BI Workspace Collections** gebruiksmodel, licenties voor Power BI de verantwoordelijkheid van de eindgebruiker is.  In plaats daarvan **sessies** worden aangeschaft door de ontwikkelaar van de app die van de visuele elementen gebruikmaakt al en worden in rekening gebracht voor het abonnement dat eigenaar is van deze resources. 

## <a name="microsoft-power-bi-workspace-collections-conceptual-model"></a>Microsoft Power BI Workspace Collections conceptuele model

![De stroom met werkruimteverzamelingen van toepassing](media/what-are-power-bi-workspace-collections/model.png)

Net als elke andere service in Azure-resources voor Power BI Workspace Collections zijn ingericht via de [Azure Resource Manager-API's](https://msdn.microsoft.com/library/mt712306.aspx). In dit geval wordt de bron die is inrichten, een **Power BI-Werkruimteverzameling**.

## <a name="workspace-collection"></a>Werkruimteverzameling

Een **Werkruimteverzameling** is de op het hoogste niveau Azure container voor resources met 0 of meer **werkruimten**.  Een **werkruimte** **verzameling** heeft alle van de standaard Azure-eigenschappen, evenals het volgende:

* **Toegangssleutels** : sleutels die worden gebruikt bij het veilig aanroepen van de Power BI-API's (beschreven in een volgende sectie).
* **Gebruikers** : Azure Active Directory (AAD)-gebruikers die over beheerdersrechten beschikken voor het beheren van de Power BI-Werkruimteverzameling via Azure portal of Azure Resource Manager-API.
* **Regio** : als onderdeel van de inrichting van een **Werkruimteverzameling**, kunt u een regio om te worden ingericht in. Zie voor meer informatie, [Azure-regio's](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Werkruimte

Een **werkruimte** is een container van Power BI-inhoud, waaronder kunt gegevenssets en rapporten. Een **werkruimte** is leeg wanneer het eerst wordt gemaakt. U gaat ontwerpen inhoud met behulp van Power BI Desktop en u zult de PBIX programmatisch implementeren in uw werkruimte met de [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx). U kunt ook programmatisch maken uw gegevensset en vervolgens rapporten maken in uw toepassing in plaats van met Power BI Desktop.

## <a name="using-workspace-collections-and-workspaces"></a>Met behulp van werkruimteverzamelingen en werkruimten

**Werkruimteverzamelingen** en **werkruimten** zijn containers voor inhoud die worden gebruikt en ingedeeld in de beste manier past bij het ontwerp van de toepassing die u wilt maken. Er zijn veel verschillende manieren dat u de inhoud ervan kan rangschikken. U kunt het plaats alle inhoud in een werkruimte en app-tokens later verder onderverdelen de inhoud door uw klanten te gebruiken. U kunt ook uw klanten in afzonderlijke werkruimten plaatsen zodat er een scheiding tussen beide. Of u kunt kiezen om deel gebruikers per regio in plaats van door de klant. Deze flexibele ontwerp kunt u kiest de beste manier om inhoud te organiseren.

## <a name="cached-datasets"></a>In de cache gegevenssets

In de cache gegevenssets kan worden gebruikt.  Echter, u gegevens in de cache niet vernieuwen nadat deze zijn geladen in **Microsoft Power BI Workspace Collections**. Een gegevensset in de cache betekent dat u de gegevens in Power BI Desktop hebt geïmporteerd in plaats van met behulp van DirectQuery.

## <a name="authentication-and-authorization-with-app-tokens"></a>Verificatie en autorisatie met app-tokens

**Microsoft Power BI Workspace Collections** wordt uitgesteld tot uw toepassing tot het uitvoeren van alle benodigde verificatie en autorisatie. Er is geen expliciete vereiste dat uw eindgebruikers klanten van Azure Active Directory (Azure AD worden).  In plaats daarvan uw toepassing om te worden uitgedrukt **Microsoft Power BI Workspace Collections** autorisatie om weer te geven van een Power BI-rapport met behulp van **verificatietokens toepassing (App-Tokens)**.  Deze **App-Tokens** worden gemaakt die nodig zijn wanneer uw app wil om een rapport weer te geven.

![Diagram van App-gebruik](media/what-are-power-bi-workspace-collections/app-tokens.png)

**Verificatietokens toepassing (App-Tokens)** worden gebruikt voor verificatie op basis **Microsoft Power BI Workspace Collections**.  Er zijn drie soorten **App-Tokens**:

1. Inrichting van Tokens - die worden gebruikt bij het inrichten van een nieuwe **werkruimte** in een **Werkruimteverzameling**
2. Ontwikkeling van Tokens - gebruikt bij het aanroepen rechtstreeks naar de **Power BI REST-API's**
3. Tokens - gebruikt bij het aanroepen om weer te geven van een rapport in de ingesloten iframe insluiten

Deze tokens worden gebruikt voor de verschillende fasen van het gebruik van **Microsoft Power BI Workspace Collections**.  De tokens zijn zo ontworpen dat u machtigingen in uw app naar Power BI delegeren kunt. Zie voor meer informatie, [App Token Flow](app-token-flow.md).

## <a name="create-or-edit-reports-within-your-application"></a>Maken of bewerken van rapporten in uw toepassing

U kunt nu bestaande rapporten bewerken of maken van nieuwe rapporten rechtstreeks in uw toepassing zonder gebruik te maken van Power BI Desktop. Dit is vereist dat er een gegevensset bestaat in uw werkruimte.

## <a name="see-also"></a>Zie ook

[Algemene scenario's voor Microsoft Power BI-Werkruimteverzamelingen](scenarios.md)  
[Aan de slag met Microsoft Power BI-Werkruimteverzamelingen](get-started.md)  
[Aan de slag met het voorbeeld](get-started-sample.md)  
[Een rapport insluiten](embed-report.md)  
[Authenticating and authorizing in Power BI Workspace Collections](app-token-flow.md) (Verifiëren en autoriseren in Power BI Workspace Collections)  
[Voorbeeld van ingesloten JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Git-opslagplaats voor Power BI-CSharp](https://github.com/Microsoft/PowerBI-CSharp)  
[Git-opslagplaats voor Power BI-knooppunt](https://github.com/Microsoft/PowerBI-Node)  

Nog vragen? [Probeer de Power BI-community](https://community.powerbi.com/)
