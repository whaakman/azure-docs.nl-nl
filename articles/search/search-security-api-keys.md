---
title: Maken, beheren en beveiligen-beheerder en query api-sleutels - Azure Search
description: API-sleutels beheren de toegang tot het service-eindpunt. Beheersleutels bieden toegang voor schrijven. Querysleutels kunnen worden gemaakt voor alleen-lezen toegang.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: heidist
ms.openlocfilehash: a59451c659effb55a2e16236b359b7601eb31cd4
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58286598"
---
# <a name="create-and-manage-api-keys-for-an-azure-search-service"></a>Api-sleutels voor Azure Search-service maken en beheren

Alle aanvragen naar een zoekservice moeten een alleen-lezen-api-sleutel die is gegenereerd voor uw service. De api-sleutel is de enige manier voor het verifiëren van toegang tot het eindpunt van de search-service en bij elke aanvraag moet worden opgenomen. In [REST oplossingen](search-get-started-nodejs.md#update-the-configjs-with-your-search-service-url-and-api-key), de api-sleutel wordt doorgaans opgegeven in de header van een aanvraag. In [.NET-oplossingen](search-howto-dotnet-sdk.md#core-scenarios), een sleutel is vaak worden opgegeven als een configuratie-instelling en vervolgens doorgegeven als [referenties](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.credentials) (admin key) of [SearchCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.searchcredentials) (querysleutel) op [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient).

Sleutels worden gemaakt met uw search-service tijdens het inrichten van de service. U kunt bekijken en verkrijgen van belangrijke waarden in de [Azure-portal](https://portal.azure.com).

![Portal-pagina, instellingen voor gedeelde met productcodes](media/search-manage/azure-search-view-keys.png)

## <a name="what-is-an-api-key"></a>Wat is een api-sleutel

Een api-sleutel is een tekenreeks die bestaat uit een willekeurig gegenereerde cijfers en letters bevatten. Via [rol gebaseerde machtigingen](search-security-rbac.md), u kunt verwijderen of lezen van de sleutels, maar u niet kunt vervangen door een sleutel met een door de gebruiker gedefinieerd wachtwoord of Active Directory gebruiken als de primaire verificatie methodologie voor het openen van zoekopdrachten. 

Twee typen sleutels worden gebruikt voor toegang tot uw search-service: beheerder (lezen-schrijven) en query (alleen-lezen).

|Sleutel|Description|Limieten|  
|---------|-----------------|------------|  
|Gemeente|De volledige rechten verleent voor alle bewerkingen, inclusief de mogelijkheid voor het beheren van de service, maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen.<br /><br /> Twee beheersleutels, aangeduid als *primaire* en *secundaire* sleutels in de portal worden gegenereerd wanneer de service wordt gemaakt en kan afzonderlijk worden gegenereerd op aanvraag. Twee sleutels betekent dat, kunt u één sleutel tijdens het gebruik van de tweede sleutel voor blijvende toegang tot de service worden meegenomen.<br /><br /> Beheersleutels zijn alleen opgegeven in de HTTP-aanvraagheaders. U kunt een admin api-sleutel in een URL plaatsen.|Maximaal 2 per service|  
|Query’s uitvoeren|Geeft het alleen-lezentoegang tot indexen en documenten, en worden doorgaans verleend aan clienttoepassingen die zoekaanvragen.<br /><br /> Querysleutels worden op aanvraag gemaakt. U kunt ze handmatig maken in de portal of programmatisch via de [Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/).<br /><br /> Querysleutels kunnen worden opgegeven in de header van een HTTP-aanvraag voor zoeken, suggesties of zoekbewerking is. U kunt ook een querysleutel als een parameter doorgeven op een URL. Afhankelijk van hoe uw clienttoepassing formuleert voor de aanvraag, is het misschien gemakkelijker om door te geven van de sleutel als een queryparameter:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2017-11-11&api-key=[query key]`|50 per service|  

 Visueel, is er geen onderscheid tussen een beheersleutel en querysleutel. Beide sleutels zijn tekenreeksen die bestaat uit 32 willekeurig gegenereerde alfanumerieke tekens. Als u het bijhouden van welk type sleutel is opgegeven in uw toepassing verliest, kunt u [controleren van de belangrijkste waarden in de portal](https://portal.azure.com) of gebruik de [REST-API](https://docs.microsoft.com/rest/api/searchmanagement/) om de waarde en het type sleutel te retourneren.  

> [!NOTE]  
>  Het wordt beschouwd als een slechte beveiligingsprocedure om door te geven gevoelige gegevens, zoals een `api-key` in de aanvraag-URI. Om deze reden accepteert Azure Search alleen een querysleutel als een `api-key` in de query-tekenreeks, en u dient te vermijden in dat geval, tenzij de inhoud van de index openbaar beschikbaar moet. Als in het algemeen wordt aangeraden te geven uw `api-key` als een aanvraagheader.  

## <a name="find-existing-keys"></a>Bestaande sleutels vinden

Kunt u toegangssleutels in de portal of via de [Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/). Zie voor meer informatie, [admin en query-api-sleutels beheren](search-security-api-keys.md).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Lijst met de [zoekservices](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) voor uw abonnement.
3. Selecteer de service en klik op de pagina overzicht **instellingen** >**sleutels** om admin en query sleutels weer te geven.

   ![Portal-pagina, instellingen voor gedeelde met productcodes](media/search-security-overview/settings-keys.png)

## <a name="create-query-keys"></a>Querysleutels maken

Querysleutels worden gebruikt voor alleen-lezen toegang tot documenten in een index. Beperken van toegang en bewerkingen in client-apps is essentieel voor het beveiligen van de assets zoeken op uw service. Gebruik altijd een querysleutel in plaats van een administratorsleutel voor elke query die afkomstig zijn van een client-app.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Lijst met de [zoekservices](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) voor uw abonnement.
3. Selecteer de service en klik op de pagina overzicht **instellingen** >**sleutels**.
4. Klik op **querysleutels beheren**.
5. Gebruik de query die al worden gegenereerd voor uw service of maximaal 50 nieuwe querysleutels maken. De sleutel van de query standaard is niet met de naam, maar aanvullende querysleutels kunnen met de naam voor de beheerbaarheid.

   ![Maken of de querysleutel te gebruiken](media/search-security-overview/create-query-key.png) 


> [!Note]
> Een voorbeeld van query-sleutelgebruik kunt u vinden in [query uitvoeren op een Azure Search-index in C# ](search-query-dotnet.md).

## <a name="regenerate-admin-keys"></a>Beheerder sleutels opnieuw genereren

Twee beheersleutels gemaakt voor elke service, zodat u kunt een primaire sleutel draaien met behulp van de secundaire sleutel voor blijvende toegang.

Als u een primaire en secundaire sleutel op hetzelfde moment genereren, worden toepassingen die gebruikmaken van beide sleutels voor toegang tot servicebewerkingen niet langer toegang tot de service hebben.

1. In de **instellingen** >**sleutels** pagina, kopieert u de secundaire sleutel.
2. Voor alle toepassingen, werkt u de instellingen van de api-sleutel voor het gebruik van de secundaire sleutel.
3. De primaire sleutel opnieuw genereren.
4. Werk alle toepassingen kunnen gebruikmaken van de nieuwe primaire sleutel.

## <a name="secure-api-keys"></a>Api-sleutels beveiligen
Sleutelbeveiliging wordt gegarandeerd door het beperken van toegang via de portal of Resource Manager-interfaces (PowerShell of opdrachtregelinterface). Zoals is vermeld, worden alle abonnementsbeheerders kunnen weergeven en alle api-sleutels opnieuw genereren. Bekijk uit voorzorg roltoewijzingen voor meer informatie over wie toegang heeft tot de beheersleutels.

+ In het dashboard, klikt u op **toegangsbeheer (IAM)** en vervolgens de **roltoewijzingen** tabblad om roltoewijzingen voor uw service.

Leden van de volgende rollen kunnen weergeven en opnieuw genereren van sleutels: Eigenaar, Inzender, [Service inzenders zoeken](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor)

> [!Note]
> Voor toegang op basis van identiteit via zoekresultaten, kunt u beveiligingsfilters Trim resultaten door identiteit, documenten waarvoor de aanvrager mag geen toegang verwijderen. Zie voor meer informatie, [beveiligingsfilters](search-security-trimming-for-azure-search.md) en [beveiligen met Active Directory](search-security-trimming-for-azure-search-with-aad.md).

## <a name="see-also"></a>Zie ook

+ [Op rollen gebaseerd toegangsbeheer in Azure Search](search-security-rbac.md)
+ [Beheren met PowerShell](search-manage-powershell.md) 
+ [Prestaties en optimalisatie van het artikel](search-performance-optimization.md)