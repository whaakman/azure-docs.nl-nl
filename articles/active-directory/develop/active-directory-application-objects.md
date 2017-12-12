---
title: Azure Active Directory-toepassing en Service-Principal-objecten
description: Een bespreking van de relatie tussen de toepassing en service-principal objecten in Azure Active Directory
documentationcenter: dev-center-name
author: bryanla
manager: mtillman
services: active-directory
editor: 
ms.assetid: adfc0569-dc91-48fe-92c3-b5b4833703de
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/19/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 85731afd18304e848f8577d8a6665dca3f9ee5d8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="application-and-service-principal-objects-in-azure-active-directory-azure-ad"></a>Toepassing en service-principal objecten in Azure Active Directory (Azure AD)
Soms kan de betekenis van de term 'application' verkeerd worden begrepen wanneer gebruikt in de context van Azure AD. Het doel van dit artikel is om te verduidelijken conceptuele en concrete aspecten van de integratie van Azure AD-toepassingen, met een afbeelding van de registratie en toestemming voor op een [multitenant toepassing](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Overzicht
Een toepassing die is geïntegreerd met Azure AD heeft gevolgen die verdergaan dan het aspect van software. 'Application' wordt vaak gebruikt als een algemene term verwijst naar het niet alleen de toepassingssoftware, maar ook de Azure AD-registratie en rol in verificatie/autorisatie 'conversaties' tijdens runtime. Per definitie een toepassing kan worden gebruikt een [client](active-directory-dev-glossary.md#client-application) rol (verbruikt een resource), een [bronserver](active-directory-dev-glossary.md#resource-server) functie blootstellen van API's (clients) of zelfs beide. Het conversatie-protocol wordt gedefinieerd door een [OAuth 2.0 Authorization Grant stroom](active-directory-dev-glossary.md#authorization-grant), zodat de client of de resource toegang/van de bron om gegevens te beveiligen respectievelijk. Nu gaan we een dieper niveau en Zie hoe een toepassing op het moment van ontwerp en runtime-Hiermee geeft u het model van Azure AD-toepassing. 

## <a name="application-registration"></a>Toepassing registreren
Wanneer het registreren van een Azure AD-toepassing in de [Azure-portal][AZURE-Portal], twee objecten worden gemaakt in uw Azure AD-tenant: een object voor de toepassing en een service-principal-object.

#### <a name="application-object"></a>Object voor de toepassing
Een Azure AD-toepassing wordt gedefinieerd door de één en slechts application-object dat zich in de Azure AD-tenant waarvoor de toepassing is geregistreerd bevindt, bekend als de toepassing "home" tenant. De Azure AD Graph [Toepassingsentiteit] [ AAD-Graph-App-Entity] definieert het schema voor de eigenschappen van een toepassingsobject. 

#### <a name="service-principal-object"></a>Service-principal-object
Toegang tot resources die zijn beveiligd met een Azure AD-tenant wordt wordt de entiteit waarvoor toegang voorgesteld door een beveiligings-principal. Dit geldt voor gebruikers (user principal) en toepassingen (service-principal). De beveiligings-principal definieert het beleid voor toegang en machtigingen voor de gebruikerstoepassing in deze tenant. Hiermee kunt u core functies zoals verificatie van de gebruikerstoepassing tijdens het aanmelden en verificatie tijdens toegang tot bedrijfsbronnen.

Wanneer een toepassing een machtiging verleend voor toegang tot bronnen in een tenant (bij de registratie of [toestemming](active-directory-dev-glossary.md#consent)), een service-principal-object is gemaakt. De Azure AD Graph [ServicePrincipal entiteit] [ AAD-Graph-Sp-Entity] definieert het schema voor de eigenschappen van een service principal-object.  

#### <a name="application-and-service-principal-relationship"></a>Toepassing en service-principal relatie
Houd rekening met het toepassingsobject als de *globale* weergave van de toepassing voor gebruik voor alle tenants en de service-principal als de *lokale* weergave voor gebruik in een specifieke tenant. De toepassing-object fungeert als de sjabloon van welke algemene en de standaardeigenschappen zijn *afgeleid* voor gebruik bij het maken van de bijbehorende service-principals. Een application-object heeft daarom een 1:1-relatie met de toepassing en een 1:many relaties met de bijbehorende objecten van de service-principal.

Een service-principal moet worden gemaakt in elke tenant waar de toepassing wordt gebruikt, waardoor het tot stand brengen van een identiteit voor het aanmelden en/of toegang tot bronnen worden beveiligd door de tenant. Een toepassing voor één tenant heeft slechts één service-principal (in de thuis-tenant), gemaakt en ingestemd voor gebruik tijdens de toepassingsregistratie. Een multitenant webtoepassing/API heeft ook een service-principal in elke tenant wordt gemaakt wanneer een gebruiker van deze tenant heeft ingestemd met het gebruik ervan.  

> [!NOTE]
> U alle wijzigingen naar uw application-object worden ook weergegeven in de service-principal-object in de toepassing thuis-tenant alleen (de tenant waarvoor deze is geregistreerd). Voor multitenant-toepassingen, wijzigingen in het toepassingsobject worden niet doorgevoerd in elke consumer tenants service-principals, totdat de toegang wordt verwijderd de [toepassing Toegangsvenster](https://myapps.microsoft.com) en opnieuw worden toegekend.
><br>  
> Let ook op systeemeigen toepassingen standaard worden geregistreerd als meerdere tenants.
> 
> 

## <a name="example"></a>Voorbeeld
Het volgende diagram illustreert de relatie tussen de application-object van een toepassing en de bijbehorende service SPN-objecten in de context van een voorbeeldtoepassing multitenant aangeroepen **HR app**. Er zijn drie Azure AD-tenants in dit scenario: 

* **Adatum** -de tenant die wordt gebruikt door het bedrijf die ontwikkeld de **HR-app**
* **Contoso** -de tenant die wordt gebruikt door de organisatie Contoso is een gebruiker van de **HR-app**
* **Fabrikam** -de tenant die wordt gebruikt door de organisatie Fabrikam, die ook verbruikt de **HR-app**

![Relatie tussen een object voor de toepassing en een service-principal-object](./media/active-directory-application-objects/application-objects-relationship.png)

Stap 1 is in het vorige diagram wordt het proces van het maken van de toepassing en service-principal objecten in de toepassing thuis-tenant.

In stap 2 wanneer beheerders van Contoso en Fabrikam toestemming te geven voltooien, is een service-principal-object gemaakt in Azure AD-tenant van hun bedrijf en toegewezen de machtigingen die de beheerder worden verleend. Houd er ook rekening mee dat de HR-app kan worden geconfigureerd/zo ontworpen dat toestemming door gebruikers voor persoonlijk gebruik.

In stap 3 hebben de consument tenants van de HR-toepassing (Contoso en Fabrikam) elk hun eigen service-principal-object. Elke vertegenwoordigt het gebruik van een exemplaar van de toepassing tijdens runtime, bepaald door de machtigingen ingestemd door de respectieve beheerder.

## <a name="next-steps"></a>Volgende stappen
Object van de toepassing van een toepassing toegankelijk via de Azure AD Graph API de [Azure portal] [ AZURE-Portal] application manifest-editor of [Azure AD PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), zoals wordt weergegeven met de OData [Toepassingsentiteit][AAD-Graph-App-Entity].

Service principal-object van een toepassing toegankelijk zijn via de Azure AD Graph API of [Azure AD PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), zoals wordt weergegeven met de OData [ServicePrincipal entiteit][AAD-Graph-Sp-Entity].

De [Explorer van Azure AD Graph](https://graphexplorer.azurewebsites.net/) is nuttig voor het opvragen van de toepassing en de service-principals.

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Portal]: https://portal.azure.com
