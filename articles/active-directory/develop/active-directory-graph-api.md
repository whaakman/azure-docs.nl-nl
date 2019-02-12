---
title: Azure Active Directory Graph API | Microsoft Docs
description: Een overzicht en Quick Start-handleiding voor Azure AD Graph-API die programmatische toegang biedt tot Azure AD via REST API-eindpunten.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: 5471ad74-20b3-44df-a2b5-43cde2c0a045
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: dkershaw, sureshja
ms.custom: aaddev
ms.openlocfilehash: 2c95e19fe47e23cda6d14fc047fbe6c30100d78e
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56097761"
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API

> [!IMPORTANT]
> Wij raden u ten zeerste aan om [Microsoft Graph](https://developer.microsoft.com/graph/) te gebruiken in plaats van Azure AD Graph API om toegang te krijgen tot Azure Active Directory-resources. We richten ons momenteel op ontwikkelingen in Microsoft Graph. Voor Azure AD Graph API zijn geen verdere verbeteringen gepland. Er is maar een beperkt aantal scenario's waarvoor Azure AD Graph API nog steeds geschikt is. Zie het blogbericht [Mogelijk Graph of Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) in het Office-ontwikkelaarscentrum voor meer informatie.

In dit artikel is van toepassing op Azure AD Graph API. Zie voor vergelijkbare informatie met betrekking tot Microsoft Graph API [gebruik van de Microsoft Graph API](https://docs.microsoft.com/graph/use-the-api). 

De Azure Active Directory Graph API biedt programmatische toegang tot Azure AD via REST API-eindpunten. Toepassingen kunnen Azure AD Graph API gebruiken om uit te voeren maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op mapgegevens en -objecten. Azure AD Graph API ondersteunt bijvoorbeeld de volgende algemene bewerkingen voor een gebruikersobject:

* Een nieuwe gebruiker in een map maken
* Gedetailleerde eigenschappen van een gebruiker, zoals hun groepen ophalen
* Bijwerken van de eigenschappen van een gebruiker, zoals de locatie en het telefoonnummer of hun wachtwoord wijzigen
* Controleer het lidmaatschap van een gebruiker voor toegang op basis van rollen
* Een gebruikersaccount uitschakelen of volledig verwijderen

Bovendien kunt u dezelfde bewerkingen op andere objecten, zoals groepen en toepassingen uitvoeren. Voor het aanroepen van Azure AD Graph API in een map, moet uw toepassing worden geregistreerd bij Azure AD. Uw toepassing moet ook toegang krijgt tot Azure AD Graph API. Deze toegang wordt normaal gesproken bereikt via een gebruiker of beheerder toestemming-stroom.

Om te beginnen met de Azure Active Directory Graph API, Zie de [introductiehandleiding voor Azure AD Graph API](active-directory-graph-api-quickstart.md), of weergeven van de [interactieve Azure AD Graph API-referentiedocumentatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Functies

Azure AD Graph API biedt de volgende functies:

* **REST API-eindpunten**: Azure AD Graph API is een RESTful-service bestaat uit eindpunten die worden geopend met behulp van standaard HTTP-aanvragen. Azure AD Graph API biedt ondersteuning voor XML-index of Javascript Object Notation (JSON) inhoudstypen voor aanvragen en antwoorden. Zie voor meer informatie, [Azure AD Graph REST API-verwijzing](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Verificatie met Azure AD**: Elke aanvraag voor Azure AD Graph API moet worden geverifieerd door een JSON Web Token (JWT) in de autorisatie-header van de aanvraag toe te voegen. Dit token wordt verkregen door die een aanvraag verzendt naar Azure AD-token-eindpunt en geldige referenties op te geven. U kunt de OAuth 2.0-clientreferentiestroom gebruiken of de autorisatiecode verlenen stroom om een token voor het aanroepen van de grafiek te verkrijgen. Voor meer informatie, [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Autorisatie op basis van rollen (RBAC)**: Beveiligingsgroepen worden gebruikt voor het uitvoeren van RBAC in Azure AD Graph API. Bijvoorbeeld, als u wilt bepalen of een gebruiker toegang tot een specifieke resource heeft, de toepassing kunt aanroepen de [Controleer het lidmaatschap (transitieve)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/functions-and-actions#checkMemberGroups) bewerking, die resulteert in waar of ONWAAR.
* **Differentiële Query**: Differentiële query kunt u het bijhouden van wijzigingen in een map tussen de twee perioden zonder te hoeven aanbrengen regelmatig query's in Azure AD Graph API. Dit type aanvraag retourneert alleen de wijzigingen die zijn gemaakt tussen de vorige differentiële queryaanvraag en de huidige aanvraag. Zie voor meer informatie, [Azure AD Graph API differentiële query](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Mapextensies**: U kunt aangepaste eigenschappen voor directory-objecten toevoegen zonder een externe gegevensarchief. Bijvoorbeeld, als uw toepassing een Skype-ID-eigenschap voor elke gebruiker vereist, kunt u de nieuwe eigenschap registreren in de map en deze is beschikbaar voor gebruik op elke gebruikersobject. Zie voor meer informatie, [Azure AD Graph API directory-schema-uitbreidingen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Beveiligd door machtigingsbereiken**: Azure AD Graph API wordt aangegeven dat het inschakelen van beveiligde toegang tot Azure AD-gegevens met behulp van OAuth 2.0-machtigingsbereiken. Deze ondersteuning biedt voor tal van client app-typen, met inbegrip van:
  
  * gebruikersinterfaces voor gedelegeerde toegang tot gegevens via de autorisatie van de gebruiker is aangemeld krijgen (gedelegeerd)
  * Service/daemon-toepassingen die op de achtergrond uitgevoerd zonder een aangemelde gebruiker aanwezig zijn en toepassingsspecifieke op rollen gebaseerd toegangsbeheer gebruiken
    
    Beide gedelegeerd en Toepassingsmachtigingen vertegenwoordigen een van de bevoegdheden die worden weergegeven door de Azure AD Graph-API en kunnen worden aangevraagd door clienttoepassingen via toepassing registratie machtigingen functies in de [Azure-portal](https://portal.azure.com). [Azure AD Graph API-machtigingsbereiken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) bevat informatie over wat er beschikbaar is voor gebruik door de clienttoepassing.

## <a name="scenarios"></a>Scenario's

Azure AD Graph API kunt veel toepassingsscenario's. De volgende scenario's zijn de meest voorkomende:

* **Line-of-Business (enkele Tenant)-toepassing**: In dit scenario werkt de ontwikkelaar in een organisatie voor een organisatie met Office 365-abonnement. De ontwikkelaar is het bouwen van een web-App die met Azure AD om uit te voeren taken communiceert zoals het toewijzen van een licentie aan een gebruiker. Deze taak is vereist voor toegang tot de Azure AD Graph-API, zodat de ontwikkelaar, registreert u de toepassing voor één tenant in Azure AD en configureert u lees- en schrijfmachtigingen heeft voor Azure AD Graph API. Vervolgens is de toepassing geconfigureerd voor het gebruik van zijn eigen referenties of die van de gebruiker op dat moment aanmelden om een token voor het aanroepen van de Azure AD Graph API te verkrijgen.
* **Software als een servicetoepassing (met meerdere Tenants)**: In dit scenario is een independent software vendor (ISV) het ontwikkelen van een gehoste multitenant webtoepassing waarmee de gebruiker-beheerfuncties voor andere organisaties die gebruikmaken van Azure AD. Deze functies vereist toegang tot directory-objecten, zodat de toepassing moet de Azure AD Graph-API aan te roepen. De ontwikkelaar, registreert u de toepassing in Azure AD, configureert om te vereisen lees- en schrijfmachtigingen heeft voor Azure AD Graph API en vervolgens een VM externe toegang ingeschakeld zodat andere organisaties toestemming geven kunnen voor het gebruik van de toepassing in de directory. Wanneer een gebruiker in een andere organisatie wordt geverifieerd aan de toepassing voor de eerste keer, worden ze weergegeven een dialoogvenster met de machtigingen van die de toepassing aanvraagt. Verleent toestemming krijgt vervolgens de toepassing die machtigingen voor Azure AD Graph API in de map van de gebruiker gevraagd. Zie voor meer informatie over het toestemmingsframework [overzicht van het toestemmingsframework](consent-framework.md).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen om te beginnen met de Azure Active Directory Graph API:

* [Snelstartgids voor Azure AD Graph API](active-directory-graph-api-quickstart.md)
* [Documentatie voor Azure AD Graph REST](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
