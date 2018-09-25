---
title: Het gebruik van de Azure AD Graph API
description: De Graph-API van Azure Active Directory (Azure AD) biedt programmatische toegang tot Azure AD via REST-API OData-eindpunten. Toepassingen kunnen Azure AD Graph API gebruiken om uit te voeren maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op mapgegevens en -objecten.
services: active-directory
documentationcenter: n/a
author: CelesteDG
manager: mtillman
editor: ''
tags: ''
ms.assetid: 9dc268a9-32e8-402c-a43f-02b183c295c5
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: sureshja
ms.custom: aaddev
ms.openlocfilehash: 7b7f2133634a91b828d7e978c9041b12873fa3a1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46997213"
---
# <a name="how-to-use-the-azure-ad-graph-api"></a>Hoe: de Azure AD Graph API gebruiken

De Graph-API van Azure Active Directory (Azure AD) biedt programmatische toegang tot Azure AD via REST-API OData-eindpunten. Toepassingen kunnen Azure AD Graph API gebruiken om uit te voeren maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op mapgegevens en -objecten. Bijvoorbeeld, kunt u Azure AD Graph API om een nieuwe gebruiker maken, weergeven of bijwerken van eigenschappen van de gebruiker, wijzigt het wachtwoord van gebruiker, Controleer het groepslidmaatschap van voor toegang op basis van rollen, uitschakelen of verwijderen van de gebruiker. Zie voor meer informatie over Azure AD Graph API-functies en scenario's application [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) en [vereisten voor Azure AD Graph API](https://msdn.microsoft.com/library/hh974476.aspx).

> [!IMPORTANT]
> Wij raden u ten zeerste aan om [Microsoft Graph](https://developer.microsoft.com/graph) te gebruiken in plaats van Azure AD Graph API om toegang te krijgen tot Azure Active Directory-resources. We richten ons momenteel op ontwikkelingen in Microsoft Graph. Voor Azure AD Graph API zijn geen verdere verbeteringen gepland. Er is maar een beperkt aantal scenario's waarvoor Azure AD Graph API nog steeds geschikt is. Zie het blogbericht [Mogelijk Graph of Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) in het Office-ontwikkelaarscentrum voor meer informatie.

## <a name="how-to-construct-a-graph-api-url"></a>Het maken van een Graph API-URL

In Graph-API voor toegang tot directory-gegevens en objecten (met andere woorden, resources of entiteiten) op basis waarvan u wilt uitvoeren van CRUD-bewerkingen, kunt u URL's op basis van de OData (Open Data)-Protocol gebruiken. De URL's die worden gebruikt in de Graph API bestaat uit vier hoofdonderdelen: basis, tenant-id, resourcepad en opties voor queryreeksen-service: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Het voorbeeld nemen van de volgende URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

* **Service-hoofdmap**: In Azure AD Graph API, is de hoofdmap van de service altijd https://graph.windows.net.
* **Tenant-id**: in deze sectie is een geverifieerde domeinnaam (geregistreerd), in het voorgaande voorbeeld contoso.com. Ook kan zijn de object-ID van een tenant of de "MijnOrganisatie" of "me" alias. Zie voor meer informatie, [adressering entiteiten en bewerkingen in Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview).
* **Resourcepad**: in dit gedeelte van een URL identificeert de bron die moet worden gecommuniceerd met (gebruikers, groepen, een bepaalde gebruiker of een bepaalde groep, enz.) In het bovenstaande voorbeeld is de op het hoogste niveau 'groepen'-adres dat resource ingesteld. U kunt ook een bepaalde entiteit, bijvoorbeeld oplossen ' gebruikers / {id} ' of ' gebruikers/userPrincipalName'.
* **Queryparameters**: een vraagteken (?) worden gescheiden van de resource-padsectie van het gedeelte van de parameters van de query. De queryparameter 'api-versie' is vereist op alle aanvragen in Azure AD Graph API. Azure AD Graph API ondersteunt ook de volgende OData-queryopties: **$filter**, **$orderby**, **$expand**, **$top**, en **$format**. De volgende opties voor query's worden momenteel niet ondersteund: **$count**, **$inlinecount**, en **$skip**. Zie voor meer informatie, [ondersteunde query's, Filters en opties voor het wisselbestand in Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Graph-API-versies

U geeft de versie voor een Graph API-aanvraag in de queryparameter 'api-versie'. Voor versie 1.5 of hoger, die u gebruikt de versiewaarde van een numerieke; API-version = 1.6. Voor eerdere versies die u gebruikt een tekenreeks met datum die in overeenstemming is met de indeling JJJJ-MM-DD; bijvoorbeeld, api-version = 2013-11-08. Voor preview-functies, gebruikt u de tekenreeks "bètaversie"; bijvoorbeeld, api-version = beta. Zie voor meer informatie over de verschillen tussen de Graph API-versies [Azure AD Graph API-versiebeheer](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Graph-API-metagegevens

Resultaat van het bestand met Azure AD Graph API-metagegevens, voegt u het segment "$metadata" na de tenant-id in het voorbeeld-URL voor, retourneert de volgende URL metagegevens voor een bedrijf demo: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. U kunt deze URL invoeren in de adresbalk van een webbrowser om te zien van de metagegevens. Het document met CSDL metagegevens geretourneerd beschrijving van de entiteiten en complexe typen, hun eigenschappen, en de functies en acties die worden weergegeven door de versie van de Graph-API die u hebt aangevraagd. De api-versie-parameter wordt weggelaten, retourneert metagegevens voor de meest recente versie.

## <a name="common-queries"></a>Algemene query 's

[Azure AD Graph API algemene query's](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) een lijst met algemene query's die kunnen worden gebruikt met de Azure AD Graph, met inbegrip van query's die kunnen worden gebruikt voor toegang tot op het hoogste niveau van resources in uw directory en bewerkingen uit te voeren in uw directory-query's.

Bijvoorbeeld, `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` retourneert bedrijfsgegevens voor directory contoso.com.

Of `https://graph.windows.net/contoso.com/users?api-version=1.6` geeft een lijst van alle gebruikersobjecten in de map contoso.com.

## <a name="using-the-azure-ad-graph-explorer"></a>Met behulp van Azure AD Graph Explorer
U kunt de Azure AD Graph Explorer voor de Azure AD Graph API gebruiken om op te vragen van de directory-gegevens tijdens het ontwikkelen van uw toepassing.

De volgende schermafbeelding is de uitvoer u ziet als u gaat u naar de Azure AD Graph Explorer, meld u aan en voer zijn `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` om weer te geven van alle gebruikers in de aangemelde gebruiker in de map:

![Azure AD graph api-Verkenner](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Laden van de Azure AD Graph Explorer**: voor het laden van het hulpprogramma, gaat u naar [ https://graphexplorer.azurewebsites.net/ ](https://graphexplorer.azurewebsites.net/). Klik op **aanmelding** en meld u met de referenties van uw Azure AD-account om uit te voeren van de Azure AD Graph Explorer op basis van uw tenant. Als u Azure AD Graph Explorer in uw eigen tenant uitvoert, moet u of uw beheerder om in te stemmen tijdens het aanmelden. Als u een Office 365-abonnement hebt, hebt u automatisch een Azure AD-tenant. De referenties die u zich kunt aanmelden bij Office 365 zijn, in feite Azure AD-accounts, en u kunt deze referenties gebruiken met Azure AD Graph Explorer.

**Een query uitvoeren**: als u wilt een query uitvoert, typt u uw query in het tekstvak van de aanvraag en op **ophalen** of klik op de **Voer** sleutel. De resultaten worden weergegeven in het antwoord. Bijvoorbeeld, `https://graph.windows.net/myorganization/groups?api-version=1.6` geeft een lijst van alle groepsobjecten in de aangemelde gebruiker in de directory.

Houd rekening met de volgende functies en beperkingen van de Azure AD Graph Explorer:

* Mogelijkheid van automatisch aanvullen voor de resource wordt ingesteld. Als u wilt deze functionaliteit ziet, klikt u op het tekstvak voor aanvraag (waarbij de URL van het bedrijf wordt weergegeven). U kunt een resource die is ingesteld in de vervolgkeuzelijst selecteren.
* Geschiedenis van de aanvraag.
* Biedt ondersteuning voor de 'ik' en 'MijnOrganisatie' adressering aliassen. Bijvoorbeeld, kunt u `https://graph.windows.net/me?api-version=1.6` om terug te keren van het gebruikersobject van de gebruiker is aangemeld of `https://graph.windows.net/myorganization/users?api-version=1.6` te retourneren van alle gebruikers in de aangemelde gebruiker in de directory.
* Biedt ondersteuning voor volledige CRUD-bewerkingen op basis van uw eigen directory via `POST`, `GET`, `PATCH` en `DELETE`.
* Een sectie van de antwoord-headers. In deze sectie kan worden gebruikt bij het oplossen van problemen die optreden bij het uitvoeren van query's.
* Een JSON-viewer voor de reactie met mogelijkheden voor uitvouwen en samenvouwen.
* Geen ondersteuning voor het weergeven of een miniatuur foto uploaden.

## <a name="using-fiddler-to-write-to-the-directory"></a>Met behulp van Fiddler in de map schrijven

Voor de toepassing van deze QuickStart, kunt u het foutopsporingsprogramma Fiddler Web oefenen met het uitvoeren van bewerkingen op uw Azure AD-directory schrijven. U kunt bijvoorbeeld verkrijgen en uploaden van een gebruiker profielfoto (dit is niet mogelijk is met Azure AD Graph Explorer). Zie voor meer informatie en voor het installeren van Fiddler [ http://www.telerik.com/fiddler ](http://www.telerik.com/fiddler).

In het onderstaande voorbeeld gebruikt u Fiddler Web foutopsporingsprogramma 'MyTestGroup' van een nieuwe beveiligingsgroep maken in uw Azure AD-directory.

**Een toegangstoken verkrijgen**: voor toegang tot Azure AD Graph, clients moeten eerst naar Azure AD verifiëren. Zie voor meer informatie, [Verificatiescenario's voor Azure AD](authentication-scenarios.md).

**Produceer en een query uitvoeren**: Voer de volgende stappen uit:

1. Fiddler Web Debugger openen en schakel over naar de **Composer** tabblad.
2. Omdat u een nieuwe beveiligingsgroep maken wilt, selecteert u **Post** als de HTTP-methode van de vervolgkeuzelijst. Zie voor meer informatie over bewerkingen en machtigingen voor een groepsobject [groep](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#group-entity) binnen de [Azure AD Graph REST API-naslaginformatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. In het veld naast **Post**, typt u de volgende aanvraag-URL: `https://graph.windows.net/{mytenantdomain}/groups?api-version=1.6`.
   
   > [!NOTE]
   > U moet {mytenantdomain} vervangen door de domeinnaam van uw eigen Azure AD-directory.

4. In het veld direct onder het keuzemenu boeken, typt u de volgende HTTP-header:
   
    ```
   Host: graph.windows.net
   Authorization: Bearer <your access token>
   Content-Type: application/json
   ```
   
   > [!NOTE]
   > Vervang uw &lt;uw toegangstoken&gt; met het toegangstoken voor uw Azure AD-directory.

5. In de **aanvraagtekst** veld, typt u de volgende JSON:
   
    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
   ```
   
    Zie voor meer informatie over het maken van groepen, [groep maken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Zie voor meer informatie over Azure AD-entiteiten en typen die beschikbaar worden gesteld door Graph en informatie over de bewerkingen die kunnen worden uitgevoerd op deze met Graph [Azure AD Graph REST API-naslaginformatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
* Meer informatie over [Azure AD Graph API-Machtigingsbereiken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)