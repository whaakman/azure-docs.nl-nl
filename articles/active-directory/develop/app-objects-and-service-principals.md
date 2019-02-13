---
title: Toepassing en service-principalobjecten in Azure Active Directory
description: Meer informatie over de relatie tussen de toepassing en service-principalobjecten in Azure Active Directory.
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
services: active-directory
editor: ''
ms.assetid: adfc0569-dc91-48fe-92c3-b5b4833703de
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: 41eeb4c45e4ef2f04a5e53fde081a2c46093d379
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56186077"
---
# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Toepassing en service-principalobjecten in Azure Active Directory

Soms kan de betekenis van de term 'application' verkeerd worden begrepen wanneer gebruikt in de context van Azure Active Directory (Azure AD). In dit artikel wordt uitleg gegeven over de concepten en concrete aspecten van de integratie van Azure AD-toepassingen, met een afbeelding van de registratie en toestemming voor een [toepassing met meerdere tenants](developer-glossary.md#multi-tenant-application).

## <a name="overview"></a>Overzicht

Een toepassing die is geïntegreerd met Azure AD heeft gevolgen die verder gaan dan de aspect van de software. 'Application' wordt vaak gebruikt als een algemene term die verwijzen naar het niet alleen de toepassingssoftware, maar ook de Azure AD-registratie en de rol in verificatie/autorisatie "gesprekken' tijdens runtime.

Per definitie kunt een toepassing werken in deze rollen:

- [Client](developer-glossary.md#client-application) rol (verbruikt een resource)
- [Resource-server](developer-glossary.md#resource-server) rol (API's geven aan clients)
- Zowel de rol van de client en resource-serverfunctie

Een [stroom voor OAuth 2.0 machtiging Grant](developer-glossary.md#authorization-grant) definieert de conversatie-protocol, waarmee de client/resource toegang/van een resource om gegevens te beveiligen, respectievelijk.

In de volgende secties ziet u hoe een App op het moment van ontwerp en runtime-Hiermee geeft u de Azure AD-toepassingsmodel.

## <a name="application-registration"></a>Een toepassing registreren

Wanneer u een Azure AD-toepassing in registreert de [Azure-portal][AZURE-Portal], twee objecten worden gemaakt in uw Azure AD-tenant:

- Een toepassingsobject, en
- Een service-principal-object

### <a name="application-object"></a>toepassingsobject

Een Azure AD-toepassing wordt gedefinieerd door de 1 en alleen application-object, dat zich in de Azure AD-tenant waar de toepassing is geregistreerd bevindt, aangeduid als "home" tenant van de toepassing. Azure AD Graph [Toepassingsentiteit] [ AAD-Graph-App-Entity] definieert het schema voor de eigenschappen van een toepassingsobject.

### <a name="service-principal-object"></a>service-principal-object

Voor toegang tot resources die zijn beveiligd met een Azure AD-tenant, moet de entiteit waarvoor toegang tot de worden vertegenwoordigd door een beveiligings-principal. Dit geldt voor zowel gebruikers (user principal) en toepassingen (service-principal).

De beveiligings-principal definieert het beleid voor toegang en machtigingen voor de gebruiker/toepassing in de Azure AD-tenant. Hiermee kunt u core-functies, zoals verificatie van de gebruiker/toepassing tijdens de aanmelding en autorisatie tijdens toegang tot bronnen.

Wanneer een toepassing een machtiging verleend voor toegang tot resources in een tenant (bij de registratie of [toestemming geven](developer-glossary.md#consent)), een service-principal-object wordt gemaakt. Azure AD Graph [ServicePrincipal entiteit] [ AAD-Graph-Sp-Entity] definieert het schema voor de eigenschappen van een service principal-object.

### <a name="application-and-service-principal-relationship"></a>Toepassing en service-principal-relatie

Houd rekening met het toepassingsobject als de *globale* weergave van uw toepassing voor gebruik voor alle tenants en de service-principal als de *lokale* weergave voor gebruik in een specifieke tenant.

De toepassing-object fungeert als de sjabloon van welke gemeenschappelijke en de standaard-eigenschappen zijn *afgeleid* voor gebruik bij het maken van de bijbehorende service-principalobjecten. Een toepassingsobject is daarom een 1:1-relatie met de softwaretoepassing en een 1:many relaties met de bijbehorende objecten van de service-principal.

Een service-principal moet worden gemaakt op elke tenant waar de toepassing wordt gebruikt, waardoor het een identiteit instellen voor aanmelden en/of toegang tot resources die worden beveiligd door de tenant. Een toepassing met één tenant heeft slechts één service-principal (in de starttenant), gemaakt en toestemming gegeven voor gebruik tijdens de toepassingsregistratie. Een multitenant webtoepassing /-API heeft ook een service-principal in elke tenant gemaakt wanneer een gebruiker van deze tenant heeft ingestemd met het gebruik ervan. 

> [!NOTE]
> Eventuele wijzigingen die u aanbrengt in uw toepassingsobject, worden ook weergegeven in de service-principal-object in van de toepassing de starttenant alleen (de tenant waar deze is geregistreerd). Voor toepassingen met meerdere tenants, wijzigingen in de toepassingsobject worden niet weergegeven in elke consument tenants service-principalobjecten, totdat de toegang is verwijderd door de [Toegangsvenster](https://myapps.microsoft.com) en gemachtigd voor het opnieuw.
>
> Let ook op dat systeemeigen toepassingen standaard worden geregistreerd als meerdere tenants.

## <a name="example"></a>Voorbeeld

Het volgende diagram illustreert de relatie tussen het toepassingsobject en de bijbehorende service-principal-objecten in de context van een voorbeeld van multitenant-toepassing met de naam van een toepassing **HR app**. Er zijn drie Azure AD-tenants in dit voorbeeldscenario:

- **Adatum** -de tenant die wordt gebruikt door het bedrijf dat ontwikkeld de **HR-app**
- **Contoso** -de tenant die wordt gebruikt door de organisatie Contoso is een gebruiker van de **HR-app**
- **Fabrikam** -de tenant die worden gebruikt door de Fabrikam organisatie, die ook verbruikt de **HR-app**

![Relatie tussen een toepassingsobject en een service-principal-object](./media/app-objects-and-service-principals/application-objects-relationship.png)

In dit voorbeeldscenario:

| Stap | Description |
|------|-------------|
| 1    | Is het proces voor het maken van de toepassing en service-principalobjecten in de starttenant van de toepassing. |
| 2    | Wanneer beheerders van Contoso en Fabrikam toestemming hebt voltooid, wordt een service-principal-object gemaakt in Azure AD-tenant van hun bedrijf en de machtigingen die de beheerder van de toegewezen toegewezen. Houd er ook rekening mee dat de HR-app kan worden geconfigureerd/die is ontworpen om toe te staan van toestemming door gebruikers voor individueel gebruik. |
| 3    | De consument tenants van de HR-toepassing (Contoso en Fabrikam) elke hebben hun eigen service-principal-object. Elke geeft het gebruik van een exemplaar van de toepassing tijdens runtime, gelden de machtigingen gegeven door de beheerder van de respectieve. |

## <a name="next-steps"></a>Volgende stappen

- U kunt de [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/) om op te vragen van de toepassing en de service-principalobjecten.
- U kunt toegang tot application-object van een toepassing met de Azure AD Graph-API, de [van Azure portal] [ AZURE-Portal] application manifest editor of [Azure AD PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), zoals wordt weergegeven met de OData [Toepassingsentiteit][AAD-Graph-App-Entity].
- U kunt van een toepassing service-principal-object openen via de Azure AD Graph API of [Azure AD PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), zoals wordt weergegeven met de OData [ServicePrincipal entiteit] [ AAD-Graph-Sp-Entity].

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Portal]: https://portal.azure.com
