---
title: Azure Active Directory Graph API | Microsoft Docs
description: Een overzicht en Quick Start guide voor de Graph-API die programmatische toegang tot Azure AD via REST API-eindpunten biedt.
services: active-directory
documentationcenter: 
author: viv-liu
manager: mtillman
editor: mbaldwin
ms.assetid: 5471ad74-20b3-44df-a2b5-43cde2c0a045
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: viviali
ms.custom: aaddev
ms.openlocfilehash: 815b9f75864ba3a08f623af12417391fba430f7d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API
> [!IMPORTANT]
> Wij raden u ten zeerste aan om [Microsoft Graph](https://graph.microsoft.io/) te gebruiken in plaats van Azure AD Graph API om toegang te krijgen tot Azure Active Directory-resources. We richten ons momenteel op ontwikkelingen in Microsoft Graph. Voor Azure AD Graph API zijn geen verdere verbeteringen gepland. Er is maar een beperkt aantal scenario's waarvoor Azure AD Graph API nog steeds geschikt is. Zie het blogbericht [Mogelijk Graph of Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) in het Office-ontwikkelaarscentrum voor meer informatie.
> 
> 

Azure Active Directory Graph API biedt programmatische toegang tot Azure AD via REST API-eindpunten. Toepassingen kunnen de Graph API gebruiken om uit te voeren maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op Active directory-gegevens en objecten. De Graph API ondersteunt bijvoorbeeld de volgende algemene bewerkingen voor een gebruikersobject:

* Een nieuwe gebruiker in een map maken
* Gedetailleerde eigenschappen voor een gebruiker, zoals de groepen ophalen
* Eigenschappen van een gebruiker, zoals de locatie en het telefoonnummer, bijwerken of hun wachtwoord wijzigen
* Controleer het lidmaatschap van een gebruiker voor toegang op basis van rollen
* Een gebruikersaccount uitschakelen of volledig verwijderen

U kunt vergelijkbare bewerkingen op andere objecten, zoals groepen en toepassingen kunt uitvoeren naast user-objecten. Als u wilt de Graph API aanroepen in een map, de toepassing moet zijn geregistreerd bij Azure AD en worden geconfigureerd voor toegang tot de map. Dit is normaal gesproken bereikt door middel van een gebruiker of beheerder toestemming-stroom.

Om te beginnen met de Azure Active Directory Graph API, Zie de [Graph API Quick Start Guide](active-directory-graph-api-quickstart.md), of Raadpleeg de [interactieve Graph API-naslagdocumentatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Functies
De Graph API biedt de volgende functies:

* **REST-API-eindpunten**: de Graph API is een RESTful-service bestaat uit eindpunten die worden geopend met behulp van standaard HTTP-aanvragen. De Graph API biedt ondersteuning voor XML- of -notatie JSON (Javascript Object) inhoudstypen voor aanvragen en antwoorden. Zie voor meer informatie [Azure AD Graph REST API-verwijzing](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Verificatie met Azure AD**: elke aanvraag voor Graph API moet worden geverifieerd door een JSON Web Token (JWT) in de autorisatie-header van de aanvraag toe te voegen. Dit token wordt verkregen door het maken van een aanvraag naar Azure AD-tokeneindpunt en geldige referenties opgeeft. U kunt de clientreferentiestroom van OAuth 2.0 of de autorisatiecodetoekenning te verkrijgen van een token voor het aanroepen van de grafiek. Voor meer informatie [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Op basis van rollen autorisatie (RBAC)**: beveiligingsgroepen worden gebruikt voor het uitvoeren van RBAC in de Graph API. Bijvoorbeeld, als u wilt bepalen of een gebruiker toegang tot een specifieke bron heeft, de toepassing kunt aanroepen de [groepslidmaatschap controleren (transitieve)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) bewerking, die resulteert in waar of ONWAAR.
* **Differentiële Query**: als u controleren op wijzigingen in een map tussen de twee perioden wilt zonder vaak query's in de Graph API aanbrengen, kunt u een differentiële queryaanvraag. Dit type aanvraag retourneert alleen de wijzigingen die zijn aangebracht tussen het vorige differentiële-aanvraag en de huidige aanvraag. Zie voor meer informatie [Azure AD Graph API differentiële Query](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Directory-uitbreidingen**: als u een toepassing die moet worden gelezen of geschreven unieke eigenschappen voor directory-objecten ontwikkelt, kunt u registreren en extensie waarden gebruiken met behulp van de Graph API. Bijvoorbeeld, als uw toepassing een Skype-ID-eigenschap voor elke gebruiker vereist, kunt u de nieuwe eigenschap registreren in de map en het beschikbaar zal zijn op elk gebruikersobject. Zie voor meer informatie [Azure AD Graph API Directory-Schemauitbreidingen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Beveiligd door machtigingsbereiken**: beschrijft de AAD Graph API-machtigingsbereiken die secure/ingestemd toegang tot de AAD-gegevens en ondersteuning van tal van client-app-typen, met inbegrip van:
  
  * die met een gebruikersinterface die zijn opgegeven overgedragen toegang tot gegevens via autorisatie van de aangemelde gebruiker (gedelegeerd)
  * die worden gebruikt definieert toepassing-op rollen gebaseerde toegangsbeheer zoals service/daemon-clients (app-functies)
    
    Beide gedelegeerd en app-functie-machtigingsbereiken vertegenwoordigen een bevoegdheid die worden weergegeven door de Graph API en kunnen worden aangevraagd door clienttoepassingen via registratie Toepassingsmachtigingen [functies in de Azure-portal](https://portal.azure.com). Clients kunnen controleren of de machtigingsbereiken verleend aan hen door te inspecteren van de claim bereik ('scp') is ontvangen in het toegangstoken voor gedelegeerde machtigingen en de rollen ('functies') claim voor app-rolmachtigingen. Meer informatie over [Azure AD Graph API-Machtigingsbereiken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).

## <a name="scenarios"></a>Scenario's
De Graph API kunt veel toepassingsscenario's. De volgende scenario's zijn de meest voorkomende:

* **Line-of-Business (één Tenant)-toepassing**: In dit scenario wordt een enterprise-ontwikkelaar werkt voor een organisatie met Office 365-abonnement. De ontwikkelaar is bezig met een webtoepassing die communiceert met Azure AD taken uit te voeren die een licentie toewijzen aan een gebruiker. Deze taak vereist toegang tot de Graph-API, zodat de ontwikkelaar registers Eén toepassing in Azure AD-tenant en configureert u lees- en heeft voor de Graph API schrijfmachtigingen. Vervolgens is de toepassing geconfigureerd voor gebruik van zijn eigen referenties zijn of die van de gebruiker momenteel aanmeldingspagina te verkrijgen van een token voor het aanroepen van de Graph API.
* **Software als een Service-toepassing (Multitenant)**: In dit scenario is een onafhankelijke softwareleverancier (ISV) ontwikkelen gehoste multitenant-webtoepassing waarmee de beheerfuncties van de gebruiker voor andere organisaties die gebruikmaken van Azure AD. Deze functies vereisen toegang tot de directory-objecten en dus de toepassing moet de Graph API aanroepen. De ontwikkelaar registreert u de toepassing in Azure AD, configureert om te vereisen lees- en schrijfmachtigingen heeft voor de Graph API en hebt u externe toegang zodat andere organisaties kunnen toestemming voor het gebruik van de toepassing in de directory. Wanneer een gebruiker in een andere organisatie geverifieerd bij de toepassing voor de eerste keer, worden ze een dialoogvenster instemming met de machtigingen van de toepassing aanvragen weergegeven.  Verlenen toestemming geeft vervolgens de toepassing die machtigingen voor Graph API in de map van de gebruiker heeft aangevraagd. Zie voor meer informatie over het framework toestemming [overzicht van het Framework toestemming](active-directory-integrating-applications.md).

## <a name="see-also"></a>Zie ook
[Snelstartgids voor Azure AD Graph API](active-directory-graph-api-quickstart.md)

[Documentatie voor AD-grafiek REST](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Ontwikkelaarshandleiding voor Azure Active Directory](active-directory-developers-guide.md)

