---
title: Snelstartgids voor Azure AD Graph API | Microsoft Docs
description: Azure Active Directory Graph API biedt programmatische toegang tot Azure AD via REST-API voor OData-eindpunten. Toepassingen kunnen de Graph API gebruiken om uit te voeren maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op Active directory-gegevens en objecten.
services: active-directory
documentationcenter: n/a
author: viv-liu
manager: mbaldwin
editor: 
tags: 
ms.assetid: 9dc268a9-32e8-402c-a43f-02b183c295c5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: viviali
ms.custom: aaddev
ms.openlocfilehash: fad5c315a247673b7a2ad52b4a78b49c567a997a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="quickstart-for-the-azure-ad-graph-api"></a>Snelstartgids voor Azure AD Graph API
De Azure Active Directory (AD) Graph API biedt programmatische toegang tot Azure AD via REST-API voor OData-eindpunten. Toepassingen kunnen de Graph API gebruiken om uit te voeren maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op Active directory-gegevens en objecten. Bijvoorbeeld, kunt u de Graph API een nieuwe gebruiker maken, weergeven of bijwerken van de eigenschappen van de gebruiker, het wachtwoord van gebruiker wijzigen, Controleer het lidmaatschap voor op rollen gebaseerde toegang uitschakelen of verwijderen van de gebruiker. Zie voor meer informatie over de Graph API-functies en toepassingsscenario's [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) en [vereisten voor Azure AD Graph API](https://msdn.microsoft.com/library/hh974476.aspx). 

> [!IMPORTANT]
> Wij raden u ten zeerste aan om [Microsoft Graph](https://developer.microsoft.com/graph) te gebruiken in plaats van Azure AD Graph API om toegang te krijgen tot Azure Active Directory-resources. We richten ons momenteel op ontwikkelingen in Microsoft Graph. Voor Azure AD Graph API zijn geen verdere verbeteringen gepland. Er is maar een beperkt aantal scenario's waarvoor Azure AD Graph API nog steeds geschikt is. Zie het blogbericht [Mogelijk Graph of Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) in het Office-ontwikkelaarscentrum voor meer informatie.
> 
> 

## <a name="how-to-construct-a-graph-api-url"></a>Het maken van een Graph API-URL
Voor toegang tot Active directory-gegevens en objecten (met andere woorden, resources of entiteiten) op basis waarvan u wilt CRUD-bewerkingen uitvoeren in Graph API kunt u URL's op basis van het OData (Open Data)-Protocol gebruiken. De URL's gebruikt in Graph API bestaat uit vier belangrijke onderdelen: service-hoofdmap, tenant-id, bronpad en opties voor query-tekenreeks: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Uitvoeren van het voorbeeld van de volgende URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

* **Service Root**: In Azure AD Graph API de hoofdmap van de service is altijd https://graph.windows.net.
* **Tenant-id**: in deze sectie mag een geverifieerde domeinnaam (geregistreerde), in het voorgaande voorbeeld contoso.com. Ook kan zijn een tenant-ID of het 'MijnOrganisatie' of 'me' alias. Zie voor meer informatie [adressering entiteiten en bewerkingen in de Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
* **Bronpad**: in dit gedeelte van een URL geeft de bron om te worden gecommuniceerd met (gebruikers, groepen, een bepaalde gebruiker of een bepaalde groep, enz.) In het bovenstaande voorbeeld is het hoogste niveau 'groepen' naar het adres dat resource ingesteld. U kunt ook een specifieke entiteit bijvoorbeeld oplossen ' gebruikers / {objectId} ' of ' gebruikers/userPrincipalName'.
* **Queryparameters**: een vraagteken (?) worden gescheiden van de resource-padsectie van het gedeelte van de parameters van de query. De queryparameter 'api-version' is vereist op alle aanvragen in de Graph API. De Graph API ondersteunt ook de volgende opties voor OData-query: **$filter**, **$orderby**, **$expand-**, **$top**, en **$format**. De volgende queryopties worden momenteel niet ondersteund: **$count**, **$inlinecount**, en **$skip**. Zie voor meer informatie [ondersteund query's, Filters en opties van het wisselbestand in Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Graph API-versies
U geeft de versie voor een Graph API-aanvraag in de queryparameter 'api-version'. Voor de versie 1.5 en hoger u een van de numerieke Versiewaarde; API-versie 1.6 =. Voor eerdere versies die u gebruikt een datumtekenreeks die in overeenstemming met de indeling JJJJ-MM-DD; bijvoorbeeld, api-version = 2013-11-08. Gebruik voor de preview-functies, de tekenreeks "bètaversie"; bijvoorbeeld, api-version = beta. Zie voor meer informatie over de verschillen tussen versies Graph API [Azure AD Graph API Versioning](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Graph API-metagegevens
Het metagegevensbestand Graph API retourneren, het segment '$metadata' na de tenant-id in het voorbeeld-URL voor toevoegen, de volgende URL metagegevens retourneert voor een voorbeeldbedrijf: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. U kunt deze URL invoeren in de adresbalk van een webbrowser om te zien van de metagegevens. Het metagegevensdocument van CSDL geretourneerd beschrijft de entiteiten en complexe typen, hun eigenschappen en de functies en acties die worden weergegeven door de versie van Graph-API die u hebt aangevraagd. Als de api-versie-parameter wordt weggelaten, retourneert de metagegevens voor de meest recente versie.

## <a name="common-queries"></a>Algemene query 's
[Azure AD Graph API algemene query's](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) een lijst met algemene query's die kunnen worden gebruikt met Azure AD Graph, met inbegrip van query's die kunnen worden gebruikt voor toegang tot op het hoogste niveau van de resources in uw directory en query's kunt u bewerkingen uitvoeren in uw directory.

Bijvoorbeeld, `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` retourneert bedrijfsgegevens voor directory contoso.com.

Of `https://graph.windows.net/contoso.com/users?api-version=1.6` geeft een lijst van alle gebruikersobjecten in de directory contoso.com.

## <a name="using-the-graph-explorer"></a>Met behulp van de grafiek Explorer
De grafiek Explorer voor de Azure AD Graph API kunt u de directory-gegevens opvragen bij het bouwen van uw toepassing.

Hieronder ziet u de uitvoer u ziet of u was gaat u naar de grafiek Explorer, meld u aan en voer `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` om alle gebruikers in de map de aangemelde gebruiker weer te geven:

![Azure AD graph api explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Laden van de grafiek Explorer**: als u het hulpprogramma wilt, gaat u naar [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/). Klik op **aanmelding** en meld u met de referenties van uw Azure AD-account aan de grafiek Explorer uitvoeren op uw tenant. Als u een grafiek Explorer op uw eigen tenant uitvoeren, moet u of uw beheerder toestemming geven tijdens het aanmelden. Als u een Office 365-abonnement hebt, hebt u automatisch een Azure AD-tenant. De referenties die u aan te melden bij Office 365 zijn, in feite Azure AD-accounts, en u kunt deze referenties grafiek Explorer.

**Een query uitvoert**: Typ de query in het tekstvak voor de aanvraag om een query uitvoert, en klik op **ophalen** of klik op de **Voer** sleutel. De resultaten worden weergegeven in het antwoord. Bijvoorbeeld: `https://graph.windows.net/myorganization/groups?api-version=1.6` geeft een lijst van alle groepsbeleidsobjecten in map de aangemelde gebruiker.

Houd rekening met de volgende functies en beperkingen van de grafiek Explorer:

* Hiermee stelt u automatisch aanvullen mogelijkheid op resource. Deze functionaliteit, klik op het tekstvak voor aanvraag (waar de bedrijfs-URL wordt weergegeven). U kunt een resource die is ingesteld in de vervolgkeuzelijst selecteren.
* Biedt ondersteuning voor de 'me' en 'MijnOrganisatie' adressering aliassen. U kunt bijvoorbeeld `https://graph.windows.net/me?api-version=1.6` te retourneren van het gebruikersobject van de aangemelde gebruiker of `https://graph.windows.net/myorganization/users?api-version=1.6` te retourneren van alle gebruikers in de huidige map.
* Een antwoord headers-sectie. Deze sectie kan worden gebruikt bij het oplossen van problemen die optreden bij het uitvoeren van query's.
* Een JSON-viewer voor het antwoord met mogelijkheden voor uitvouwen en samenvouwen.
* Er is geen ondersteuning voor het weergeven van een foto van miniatuurformaat.

## <a name="using-fiddler-to-write-to-the-directory"></a>Gebruik Fiddler om te schrijven naar de map
Voor de doeleinden van deze handleiding Quick Start, kunt u het foutopsporingsprogramma Fiddler Web oefenen met het uitvoeren van bewerkingen op uw Azure AD-directory schrijven. Zie voor meer informatie en voor het installeren van Fiddler [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

In het onderstaande voorbeeld u Fiddler Web foutopsporingsprogramma maakt een nieuwe beveiligingsgroep 'MyTestGroup' in uw Azure AD-directory.

**Verkrijgen van een toegangstoken**: voor toegang tot Azure AD Graph moet clients nodig zijn voor het eerst naar Azure AD verifiëren. Zie voor meer informatie [verificatie scenario's voor Azure AD](active-directory-authentication-scenarios.md).

**Opstellen en een query uitvoert**: de volgende stappen uit:

1. Fiddler Web Debugger openen en schakel over naar de **Composer** tabblad.
2. Omdat u een nieuwe beveiligingsgroep maken wilt, selecteert u **Post** als de HTTP-methode van de vervolgkeuzelijst. Zie voor meer informatie over bewerkingen en -machtigingen op een groepsobject [groep](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) binnen de [Azure AD Graph REST API-verwijzing](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. In het veld naast **Post**, typt u de volgende stappen uit als de aanvraag-URL: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.
   
   > [!NOTE]
   > U moet mytenantdomain met de domeinnaam van uw eigen Azure AD-directory vervangen.
   > 
   > 
4. In het veld direct onder Post pull-down, typ het volgende:
   
    ```
   Host: graph.windows.net
   Authorization: Bearer <your access token>
   Content-Type: application/json
   ```
   
   > [!NOTE]
   > Vervang uw &lt;uw toegangstoken&gt; met het toegangstoken voor uw Azure AD-directory.
   > 
   > 
5. In de **aanvraagtekst** veld, typt u het volgende:
   
    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
   ```
   
    Zie voor meer informatie over het maken van groepen [groep maken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Zie voor meer informatie over Azure AD-entiteiten en typen die door de grafiek beschikbaar worden gesteld en informatie over de bewerkingen die kunnen worden uitgevoerd op deze met grafiek [Azure AD Graph REST API-verwijzing](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
* Meer informatie over [Azure AD Graph API-Machtigingsbereiken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

