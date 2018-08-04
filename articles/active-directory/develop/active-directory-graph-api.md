---
title: Azure Active Directory Graph API | Microsoft Docs
description: Een overzicht en Quick Start-handleiding voor Azure AD Graph-API die programmatische toegang biedt tot Azure AD via REST API-eindpunten.
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
ms.openlocfilehash: 3ec6bbcb9250626f8d6ef75330991c5ebac053a7
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504714"
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API
> [!IMPORTANT]
> Wij raden u ten zeerste aan om [Microsoft Graph](https://graph.microsoft.io/) te gebruiken in plaats van Azure AD Graph API om toegang te krijgen tot Azure Active Directory-resources. We richten ons momenteel op ontwikkelingen in Microsoft Graph. Voor Azure AD Graph API zijn geen verdere verbeteringen gepland. Er is maar een beperkt aantal scenario's waarvoor Azure AD Graph API nog steeds geschikt is. Zie het blogbericht [Mogelijk Graph of Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) in het Office-ontwikkelaarscentrum voor meer informatie.
> 
> 

De Azure Active Directory Graph API biedt programmatische toegang tot Azure AD via REST API-eindpunten. Toepassingen kunnen Azure AD Graph API gebruiken om uit te voeren maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op mapgegevens en -objecten. Azure AD Graph API ondersteunt bijvoorbeeld de volgende algemene bewerkingen voor een gebruikersobject:

* Een nieuwe gebruiker in een map maken
* Gedetailleerde eigenschappen van een gebruiker, zoals hun groepen ophalen
* Bijwerken van de eigenschappen van een gebruiker, zoals de locatie en het telefoonnummer of hun wachtwoord wijzigen
* Controleer het lidmaatschap van een gebruiker voor toegang op basis van rollen
* Een gebruikersaccount uitschakelen of volledig verwijderen

Bovendien kunt u dezelfde bewerkingen op andere objecten, zoals groepen en toepassingen uitvoeren. Voor het aanroepen van Azure AD Graph API in een map, moet uw toepassing worden geregistreerd bij Azure AD. Uw toepassing moet ook toegang krijgt tot Azure AD Graph API. Deze toegang wordt normaal gesproken bereikt via een gebruiker of beheerder toestemming-stroom.

Als u wilt beginnen met de Azure Active Directory Graph API, Zie de [Azure AD Graph API-snelstartgids](active-directory-graph-api-quickstart.md), of weergeven van de [interactieve Azure AD Graph API-referentiedocumentatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Functies
Azure AD Graph API biedt de volgende functies:

* **REST API-eindpunten**: Azure AD Graph API is een RESTful-service bestaat uit eindpunten die worden geopend met behulp van standaard HTTP-aanvragen. Azure AD Graph API biedt ondersteuning voor XML-index of Javascript Object Notation (JSON) inhoudstypen voor aanvragen en antwoorden. Zie voor meer informatie, [Azure AD Graph REST API-naslaginformatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Verificatie met Azure AD**: elke aanvraag voor Azure AD Graph API moet worden geverifieerd door een JSON Web Token (JWT) in de autorisatie-header van de aanvraag toe te voegen. Dit token wordt verkregen door die een aanvraag verzendt naar Azure AD-token-eindpunt en geldige referenties op te geven. U kunt de OAuth 2.0-clientreferentiestroom gebruiken of de autorisatiecode verlenen stroom om een token voor het aanroepen van de grafiek te verkrijgen. Voor meer informatie, [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Autorisatie op basis van rollen (RBAC)**: beveiligingsgroepen worden gebruikt voor het uitvoeren van RBAC in Azure AD Graph API. Bijvoorbeeld, als u wilt bepalen of een gebruiker toegang tot een specifieke resource heeft, de toepassing kunt aanroepen de [groepslidmaatschap controleren (transitieve)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/functions-and-actions#checkMemberGroups) bewerking, die resulteert in waar of ONWAAR.
* **Differentiële Query**: differentiële query kunt u het bijhouden van wijzigingen in een map tussen de twee perioden zonder te hoeven aanbrengen regelmatig query's in Azure AD Graph API. Dit type aanvraag retourneert alleen de wijzigingen die zijn gemaakt tussen de vorige differentiële queryaanvraag en de huidige aanvraag. Zie voor meer informatie, [Azure AD Graph API differentiële Query](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Mapextensies**: U kunt aangepaste eigenschappen voor directory-objecten toevoegen zonder een externe gegevensarchief. Bijvoorbeeld, als uw toepassing een Skype-ID-eigenschap voor elke gebruiker vereist, kunt u de nieuwe eigenschap registreren in de map en deze is beschikbaar voor gebruik op elke gebruikersobject. Zie voor meer informatie, [Azure AD Graph API Directory-Schemauitbreidingen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Beveiligd door machtigingsbereiken**: Azure AD Graph API-machtigingsbereiken waarmee veilige toegang tot Azure AD-gegevens met behulp van OAuth 2.0 wordt aangegeven. Deze ondersteuning biedt voor tal van client app-typen, met inbegrip van:
  
  * gebruikersinterfaces voor gedelegeerde toegang tot gegevens via de autorisatie van de gebruiker is aangemeld krijgen (gedelegeerd)
  * Service/daemon-toepassingen die op de achtergrond uitgevoerd zonder een aangemelde gebruiker aanwezig zijn en toepassingsspecifieke op rollen gebaseerd toegangsbeheer gebruiken
    
    Beide gedelegeerd en Toepassingsmachtigingen vertegenwoordigen een van de bevoegdheden die worden weergegeven door de Azure AD Graph-API en kunnen worden aangevraagd door clienttoepassingen via toepassing registratie machtigingen functies in de [Azure-portal](https://portal.azure.com). [Azure AD Graph API-Machtigingsbereiken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) bevat informatie over wat er beschikbaar is voor gebruik door de clienttoepassing.

## <a name="scenarios"></a>Scenario's
Azure AD Graph API kunt veel toepassingsscenario's. De volgende scenario's zijn de meest voorkomende:

* **Line-of-Business (enkele Tenant)-toepassing**: In dit scenario, de ontwikkelaar in een organisatie werkt voor een organisatie met Office 365-abonnement. De ontwikkelaar is het bouwen van een web-App die met Azure AD om uit te voeren taken communiceert zoals het toewijzen van een licentie aan een gebruiker. Deze taak is vereist voor toegang tot de Azure AD Graph-API, zodat de ontwikkelaar, registreert u de toepassing voor één tenant in Azure AD en configureert u lees- en schrijfmachtigingen heeft voor Azure AD Graph API. Vervolgens is de toepassing geconfigureerd voor het gebruik van zijn eigen referenties of die van de gebruiker op dat moment aanmelden om een token voor het aanroepen van de Azure AD Graph API te verkrijgen.
* **Software als een Service-toepassing (met meerdere Tenants)**: In dit scenario wordt een independent software vendor (ISV) is het ontwikkelen van een gehoste multitenant webtoepassing waarmee de gebruiker-beheerfuncties voor andere organisaties die gebruikmaken van Azure AD. Deze functies vereist toegang tot directory-objecten, zodat de toepassing moet de Azure AD Graph-API aan te roepen. De ontwikkelaar, registreert u de toepassing in Azure AD, configureert om te vereisen lees- en schrijfmachtigingen heeft voor Azure AD Graph API en vervolgens een VM externe toegang ingeschakeld zodat andere organisaties toestemming geven kunnen voor het gebruik van de toepassing in de directory. Wanneer een gebruiker in een andere organisatie wordt geverifieerd aan de toepassing voor de eerste keer, worden ze weergegeven een dialoogvenster met de machtigingen van die de toepassing aanvraagt. Verleent toestemming krijgt vervolgens de toepassing die machtigingen voor Azure AD Graph API in de map van de gebruiker gevraagd. Zie voor meer informatie over het toestemmingsframework [overzicht van het Framework toestemming geven](quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="see-also"></a>Zie ook
[Snelstartgids voor Azure AD Graph API](active-directory-graph-api-quickstart.md)

[Documentatie voor Azure AD Graph REST](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Ontwikkelaarshandleiding voor Azure Active Directory](azure-ad-developers-guide.md)

