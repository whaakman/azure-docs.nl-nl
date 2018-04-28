---
title: Maken, beheren en beveiligen van de beheerder en de query api-sleutels voor Azure Search | Microsoft Docs
description: API-sleutels toegang tot het service-eindpunt. Beheersleutels bieden toegang voor schrijven. Querysleutels kunnen worden gemaakt voor alleen-lezen toegang.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 03/20/2018
ms.author: heidist
ms.openlocfilehash: 83a082eb7a18c65a5824bf272e0397b18883277f
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="create-and-manage-api-keys-for-an-azure-search-service"></a>Maken en beheren van de api-sleutels voor een Azure Search-service

Alle aanvragen voor een search-service moeten een api-sleutel die is gegenereerd specifiek voor uw service. Deze api-sleutel is het enige mechanisme voor het verifiëren van toegang tot uw search-service-eindpunt. 

Een api-sleutel is een tekenreeks van willekeurige getallen en letters bestaan. Via [rol gebaseerde machtigingen](search-security-rbac.md), kunt u verwijderen of de toetsen niet lezen, maar u kunt een sleutel vervangen met een door de gebruiker gedefinieerd wachtwoord of gebruikmaken van Active Directory als de primaire verificatie methodologie voor het openen van zoekopdrachten. 

Twee soorten sleutels worden gebruikt voor toegang tot uw search-service: admin (lezen / schrijven) en de query (alleen-lezen).

|Sleutel|Beschrijving|Limieten|  
|---------|-----------------|------------|  
|Beheer|De volledige rechten verleent voor alle bewerkingen, inclusief de mogelijkheid voor het beheren van de service maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen.<br /><br /> Twee administratorsleutels genoemd *primaire* en *secundaire* sleutels in de portal worden gegenereerd als de service wordt gemaakt en afzonderlijk kan worden hersteld op aanvraag. Met twee sleutels, kunt u één sleutel overschakelen tijdens het gebruik van de tweede sleutel voor permanente toegang tot de service.<br /><br /> Administratorsleutels alleen in de HTTP-aanvraagheaders opgegeven. U kunt een admin api-sleutel in een URL plaatsen.|Maximaal 2 per service|  
|Query’s uitvoeren|Alleen-lezen toegang verleent tot indexen en documenten en worden doorgaans verleend aan clienttoepassingen die zoekaanvragen.<br /><br /> Querysleutels worden op verzoek gemaakt. U kunt ze handmatig maken in de portal of programmatisch via de [Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/).<br /><br /> Querysleutels kunnen worden opgegeven in de header van een HTTP-aanvraag voor het zoeken, suggestie of zoekbewerking. U kunt ook een querysleutel doorgeven als een parameter van een URL. Afhankelijk van hoe uw clienttoepassing formuleert voor de aanvraag, is het mogelijk dat het eenvoudiger om door te geven van de sleutel als een queryparameter:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01&api-key=A8DA81E03F809FE166ADDB183E9ED84D`|50 per service|  

 Visueel, bestaat er geen onderscheid tussen een beheersleutel en querysleutel. Beide sleutels zijn tekenreeksen die bestaat uit 32 willekeurig gegenereerd alfanumerieke tekens. Als u het bijhouden van welk type sleutel dat is opgegeven in uw toepassing verliest, kunt u [controleren de sleutelwaarden in de portal](https://portal.azure.com) of gebruik de [REST-API](https://docs.microsoft.com/rest/api/searchmanagement/) om de waarde en sleuteltype te retourneren.  

> [!NOTE]  
>  Dit wordt beschouwd als een slechte beveiligingsprocedure om door te geven gevoelige gegevens, zoals een `api-key` in de aanvraag-URI. Om deze reden accepteert Azure Search alleen een querysleutel als een `api-key` in de query string en u moet voorkomen in dat geval tenzij de inhoud van de index moeten openbaar zijn. Als in het algemeen wordt aangeraden doorgeven uw `api-key` als een aanvraag-header.  

## <a name="find-api-keys-for-your-service"></a>Api-sleutels voor uw service vinden

U kunt verkrijgen toegangstoetsen in de portal of via de [Management REST API](https://docs.microsoft.com/rest/api/searchmanagement/). Zie voor meer informatie [admin en de query api-sleutels beheren](search-security-api-keys.md).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Lijst met de [services zoeken](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) voor uw abonnement.
3. Selecteer de service en vinden op de servicepagina **instellingen** >**sleutels** admin en query sleutels weergeven.

![Portal-pagina, instellingen, sleutels sectie](media/search-security-overview/settings-keys.png)

## <a name="regenerate-admin-keys"></a>Administratorsleutels genereren

Twee administratorsleutels worden gemaakt voor elke service, zodat u overschakeling van de primaire sleutel kunt, met behulp van de secundaire sleutel voor permanente toegang.

Als u de primaire en secundaire sleutels op hetzelfde moment genereren, wordt toegang tot de service niet langer in toepassingen die gebruikmaken van sleutel voor het openen van servicebewerkingen hebben.

1. In de **instellingen** >**sleutels** pagina, kopieert u de secundaire sleutel.
2. Voor alle toepassingen, werkt de instellingen van de api-sleutel voor het gebruik van de secundaire sleutel.
3. De primaire sleutel opnieuw genereren.
4. Werk alle toepassingen toe aan de nieuwe primaire sleutel gebruiken.

## <a name="secure-api-keys"></a>Api-sleutels beveiligen
Sleutelbeveiliging wordt gegarandeerd door het beperken van toegang via de portal of de Resource Manager-interfaces (PowerShell of opdrachtregelinterface). Zoals is aangegeven, kunnen abonnementbeheerders weergeven en opnieuw genereren van alle api-sleutels. Bekijk de roltoewijzingen om te begrijpen wie toegang heeft tot de beheersleutels uit voorzorg.

+ Klik in het servicedashboard op **toegangsbeheer (IAM)** roltoewijzingen voor uw service weergeven.

Leden van de volgende rollen kunnen weergeven en opnieuw genereren van sleutels: eigenaar, bijdrager, [Search-Service de medewerkers](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor)

> [!Note]
> Voor toegang op basis van identiteit via zoekresultaten wilt weergeven, kunt u beveiligingsfilters zodat deze resultaten door de identiteit, worden deze documenten waarvoor de aanvrager mag geen toegang verwijderen. Zie voor meer informatie [beveiligingsfilters](search-security-trimming-for-azure-search.md) en [Secure met Active Directory](search-security-trimming-for-azure-search-with-aad.md).

## <a name="see-also"></a>Zie ook

+ [Toegangsbeheer op basis van rollen in Azure Search](search-security-rbac.md)
+ [Beheren met PowerShell](search-manage-powershell.md) 
+ [Prestaties en optimalisatie van het artikel](search-performance-optimization.md)