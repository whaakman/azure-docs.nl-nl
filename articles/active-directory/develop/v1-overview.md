---
title: About v1.0 | Azure
description: In dit artikel biedt een overzicht van aanmelding bij Microsoft werken en schoolaccounts met behulp van de Azure Active Directory v1.0 eindpunt en het platform.
services: active-directory
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: a8cd7fca52809b1e093100f47ae0eb3a1438ca85
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56097284"
---
# <a name="about-v10"></a>Over v1.0

Azure Active Directory (Azure AD) is een identiteitsservice in de cloud waarmee ontwikkelaars apps kunnen maken die ervoor zorgen dat elke gebruiker met een Microsoft-werkaccount of -schoolaccount zich veilig kan aanmelden. Azure AD biedt ondersteuning voor ontwikkelaars die Line-Of-Business-apps met één tenant maken en voor ontwikkelaars die apps met meerdere tenants willen ontwikkelen. Naast eenvoudige aanmelding maakt Azure AD ook mogelijk dat apps Microsoft-API's zoals [Microsoft Graph](https://docs.microsoft.com/graph/overview) en aangepaste API's die zijn gemaakt op het Azure AD-platform aanroepen. In deze documentatie wordt beschreven hoe u Azure AD-ondersteuning aan uw app kunt toevoegen met standaardprotocollen zoals OAuth 2.0 en OpenID Connect.

> [!NOTE]
> De meeste van de inhoud op deze pagina is gericht op de v1.0 eindpunt en platform, die ondersteuning biedt voor alleen Microsoft werk- of schoolaccount. Als u wilt aan te melden bij de consument of persoonlijke Microsoft-accounts, raadpleegt u de informatie op de [v2.0-eindpunt en platform](v2-overview.md). Het v2.0-eindpunt biedt een uniforme ervaring voor ontwikkelaars voor apps die u wilt aan te melden bij alle Microsoft-identiteiten.

| | |
| --- | --- |
|[De basisbeginselen van verificatie](authentication-scenarios.md) | Een inleiding tot verificatie met Azure AD. |
|[Soorten toepassingen](app-types.md) | Een overzicht van de verificatiescenario's die worden ondersteund door Azure AD. |
| | |

## <a name="get-started"></a>Aan de slag

De v1.0 snelstartgidsen en zelfstudies leert u hoe het bouwen van een app op uw gewenste platform met behulp van de Azure AD Authentication Library (ADAL) SDK. Zie de **v1.0 snelstartgidsen** en **v1.0 zelfstudies** in [identiteitsplatform van Microsoft (Azure Active Directory voor ontwikkelaars)](index.yml) aan de slag.

## <a name="how-to-guides"></a>Handleidingen

Zie de **v1.0 instructies begeleidt** voor gedetailleerde informatie en scenario's van de meest algemene taken in Azure AD.

## <a name="reference-topics"></a>Onderwerpen met naslaginformatie

De volgende artikelen bieden gedetailleerde informatie over API's, protocolberichten en termen die worden gebruikt in Azure AD.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Verificatiebibliotheken (ADAL)](active-directory-authentication-libraries.md)   | Een overzicht van de bibliotheken en SDK's die worden geleverd door Azure AD. |
| [Codevoorbeelden](sample-v1-code.md)                                  | Een lijst met alle Azure AD-codevoorbeelden. |
| [Woordenlijst](developer-glossary.md)                                      | Termen en definities van woorden die in deze documenten worden gebruikt. |
|  |  |


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
