---
title: Azure Active Directory Graph API | Microsoft Docs
description: Een overzicht en Quick Start guide voor Azure AD Graph API die programmatische toegang biedt tot Azure AD via REST API-eindpunten.
services: active-directory
documentationcenter: ''
author: mtillman
manager: mtillman
editor: mbaldwin
ms.assetid: 5471ad74-20b3-44df-a2b5-43cde2c0a045
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: mtillman
ms.custom: aaddev
ms.openlocfilehash: 4b4f698042f6688e3db484f7d96ccfb06c5cdd4f
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API
> [!IMPORTANT]
> Wij raden u ten zeerste aan om [Microsoft Graph](https://graph.microsoft.io/) te gebruiken in plaats van Azure AD Graph API om toegang te krijgen tot Azure Active Directory-resources. We richten ons momenteel op ontwikkelingen in Microsoft Graph. Voor Azure AD Graph API zijn geen verdere verbeteringen gepland. Er is maar een beperkt aantal scenario's waarvoor Azure AD Graph API nog steeds geschikt is. Zie het blogbericht [Mogelijk Graph of Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) in het Office-ontwikkelaarscentrum voor meer informatie.
> 
> 

Azure Active Directory Graph API biedt programmatische toegang tot Azure AD via REST API-eindpunten. Toepassingen kunnen Azure AD Graph API gebruiken om uit te voeren maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op Active directory-gegevens en objecten. Azure AD Graph API ondersteunt bijvoorbeeld de volgende algemene bewerkingen voor een gebruikersobject:

* Een nieuwe gebruiker in een map maken
* Gedetailleerde eigenschappen voor een gebruiker, zoals de groepen ophalen
* Eigenschappen van een gebruiker, zoals de locatie en het telefoonnummer, bijwerken of hun wachtwoord wijzigen
* Controleer het lidmaatschap van een gebruiker voor toegang op basis van rollen
* Een gebruikersaccount uitschakelen of volledig verwijderen

Bovendien kunt u vergelijkbare bewerkingen op andere objecten, zoals groepen en toepassingen uitvoeren. Azure AD Graph API aanroepen in een map, moet uw toepassing worden geregistreerd bij Azure AD. Uw toepassing moet ook toegang worden verleend aan Azure AD Graph API. Deze toegang wordt normaal gesproken door middel van een gebruiker of beheerder toestemming stroom bereikt.

Om te beginnen met de Azure Active Directory Graph API, Zie de [Azure AD Graph API Quick Start Guide](active-directory-graph-api-quickstart.md), of Raadpleeg de [interactieve Azure AD Graph API-naslagdocumentatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Functies
Azure AD Graph API biedt de volgende functies:

* **REST-API-eindpunten**: Azure AD Graph API is een RESTful-service bestaat uit eindpunten die worden geopend met behulp van standaard HTTP-aanvragen. Azure AD Graph API biedt ondersteuning voor XML- of -notatie JSON (Javascript Object) inhoudstypen voor aanvragen en antwoorden. Zie voor meer informatie [Azure AD Graph REST API-verwijzing](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Verificatie met Azure AD**: elke aanvraag naar de Azure AD Graph API moet worden geverifieerd door een JSON Web Token (JWT) in de autorisatie-header van de aanvraag toe te voegen. Dit token wordt verkregen door het maken van een aanvraag naar Azure AD-tokeneindpunt en geldige referenties opgeeft. U kunt de clientreferentiestroom van OAuth 2.0 of de autorisatiecodetoekenning te verkrijgen van een token voor het aanroepen van de grafiek. Voor meer informatie [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Op basis van rollen autorisatie (RBAC)**: beveiligingsgroepen worden gebruikt voor het uitvoeren van RBAC in Azure AD Graph API. Bijvoorbeeld, als u wilt bepalen of een gebruiker toegang tot een specifieke bron heeft, de toepassing kunt aanroepen de [groepslidmaatschap controleren (transitieve)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/functions-and-actions#checkMemberGroups) bewerking, die resulteert in waar of ONWAAR.
* **Differentiële Query**: differentiële query kunt u het bijhouden van wijzigingen in een map tussen de twee perioden zonder vaak query's in Azure AD Graph API aanbrengen. Dit type aanvraag retourneert alleen de wijzigingen die zijn aangebracht tussen het vorige differentiële-aanvraag en de huidige aanvraag. Zie voor meer informatie [Azure AD Graph API differentiële Query](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Directory-uitbreidingen**: U kunt aangepaste eigenschappen voor directory-objecten toevoegen zonder een externe gegevensarchief. Bijvoorbeeld, als uw toepassing een Skype-ID-eigenschap voor elke gebruiker vereist, kunt u de nieuwe eigenschap registreren in de map en is beschikbaar voor gebruik op elk gebruikersobject. Zie voor meer informatie [Azure AD Graph API Directory-Schemauitbreidingen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Beveiligd door machtigingsbereiken**: Azure AD Graph API-machtigingsbereiken waarmee veilige toegang tot Azure AD-gegevens met behulp van OAuth 2.0 maakt. Deze ondersteuning biedt voor tal van client-app-typen, met inbegrip van:
  
  * gebruikersinterfaces waarmee gedelegeerde toegang tot gegevens via de autorisatie van de aangemelde gebruiker krijgen (gedelegeerd)
  * Service/daemon-toepassingen die op de achtergrond uitgevoerd zonder een aangemelde gebruiker aanwezig en toepassingsspecifieke rollen gebaseerd toegangsbeheer gebruiken
    
    Beide gedelegeerd en Toepassingsmachtigingen vertegenwoordigen een bevoegdheid die worden weergegeven door de Azure AD Graph API en kunnen worden aangevraagd door clienttoepassingen via registratie machtigingen toepassingsfuncties in de [Azure-portal](https://portal.azure.com). [Azure AD Graph API-Machtigingsbereiken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) bevat informatie over wat er beschikbaar is voor gebruik door de clienttoepassing.

## <a name="scenarios"></a>Scenario's
Azure AD Graph API kunt veel toepassingsscenario's. De volgende scenario's zijn de meest voorkomende:

* **Line-of-Business (één Tenant)-toepassing**: In dit scenario wordt een enterprise-ontwikkelaar werkt voor een organisatie met Office 365-abonnement. De ontwikkelaar is bezig met een webtoepassing die communiceert met Azure AD taken uit te voeren die een licentie toewijzen aan een gebruiker. Deze taak vereist toegang tot Azure AD Graph API, zodat de ontwikkelaar, registreert u de toepassing voor één tenant in Azure AD en configureert u lees- en schrijfmachtigingen heeft voor Azure AD Graph API. Vervolgens is de toepassing geconfigureerd voor gebruik van zijn eigen referenties zijn of die van de gebruiker momenteel aanmeldingspagina te verkrijgen van een token voor het aanroepen van Azure AD Graph API.
* **Software als een Service-toepassing (Multitenant)**: In dit scenario is een onafhankelijke softwareleverancier (ISV) ontwikkelen gehoste multitenant-webtoepassing waarmee de beheerfuncties van de gebruiker voor andere organisaties die gebruikmaken van Azure AD. Deze functies vereisen toegang tot de directory-objecten en dus de toepassing moet Azure AD Graph API aan te roepen. De ontwikkelaar registreert u de toepassing in Azure AD, configureert om te vereisen lees- en schrijfmachtigingen heeft voor Azure AD Graph API en hebt u externe toegang zodat andere organisaties kunnen toestemming voor het gebruik van de toepassing in de directory. Wanneer een gebruiker in een andere organisatie geverifieerd bij de toepassing voor de eerste keer, worden ze een dialoogvenster instemming met de machtigingen van de toepassing aanvragen weergegeven. Verlenen toestemming geeft vervolgens de toepassing die machtigingen voor Azure AD Graph API in de map van de gebruiker heeft aangevraagd. Zie voor meer informatie over het framework toestemming [overzicht van het Framework toestemming](active-directory-integrating-applications.md).

## <a name="see-also"></a>Zie ook
[Snelstartgids voor Azure AD Graph API](active-directory-graph-api-quickstart.md)

[Azure AD Graph REST-documentatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Ontwikkelaarshandleiding voor Azure Active Directory](active-directory-developers-guide.md)

