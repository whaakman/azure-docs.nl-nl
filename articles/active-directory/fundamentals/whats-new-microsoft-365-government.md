---
title: Wat is er nieuw voor Azure Active Directory in Microsoft 365 Government - Azure Active Directory | Microsoft Docs
description: Meer informatie over enkele wijzigingen aan Azure Active Directory (Azure AD) in de Microsoft 365 Government cloud-exemplaar, die mogelijk van invloed heeft op u.
services: active-directory
author: eross-msft
manager: daveba
ms.author: lizross
ms.reviewer: sumitp
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b077c7b5efbad2add971d42ff31938b56f6bc33
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66258892"
---
# <a name="whats-new-for-azure-active-directory-in-microsoft-365-government"></a>Wat is er nieuw voor Azure Active Directory in Microsoft 365 Government

We hebben enkele wijzigingen aangebracht in Azure Active Directory (Azure AD) in de Microsoft 365 Government cloud-instantie is van toepassing op klanten die gebruikmaken van de volgende services:

- Microsoft Azure en de overheid

- Microsoft 365 Government – GCC hoog

- Microsoft 365 Government – DoD

In dit artikel geldt niet voor Microsoft 365 Government – GCC-klanten.

## <a name="changes-to-the-initial-domain-name"></a>Wijzigingen in de initiële domeinnaam

Tijdens de van uw organisatie zich aanmelden voor een onlineservice van Microsoft 365 Government, zijn u gevraagd om u te kiezen van de domeinnaam van uw organisatie, `<your-domain-name>.onmicrosoft.com`. Als u al een domeinnaam met het achtervoegsel .com, verandert er niets op.

Echter, als u bent aangemeld voor een nieuwe Microsoft 365 Government-service, u wordt gevraagd naar een domein naam met behulp van de `.us` achtervoegsel. Het is dus `<your-domain-name>.onmicrosoft.us`.

>[!Note]
>Deze wijziging is niet van toepassing op alle klanten die worden beheerd door cloud-serviceproviders (CSP's).

## <a name="changes-to-portal-access"></a>Wijzigingen in de portal-toegang

We hebben de eindpunten van de portal voor Microsoft Azure Government, Microsoft 365 Government – GCC hoog, en Microsoft 365 Government – Amerikaanse ministerie van defensie, bijgewerkt, zoals wordt weergegeven in de [eindpunt toewijzingstabel](#endpoint-mapping).

Klanten kunnen eerder zich aanmelden met behulp van de wereldwijde Azure (portal.azure.com) en Office 365 (portal.office.com)-Portal. Met deze update moeten de klanten nu zich aanmelden met behulp van de specifieke Microsoft Azure Government, Microsoft 365 Government - hoog GCC, en Microsoft 365 Government - DoD-portals.

## <a name="endpoint-mapping"></a>Toewijzing van eindpunt

De volgende tabel bevat de eindpunten voor alle klanten:

| Name | details van eindpunt |
|------|------------------|
| Portals |Microsoft Azure Government: https://portal.azure.us<p>Microsoft 365 Government – GCC hoge: https://portal.office365.us<p>Microsoft 365 Government – DoD: https://portal.apps.mil |
| Azure Active Directory-instantie-eindpunt | https://login.microsoftonline.us |
| Azure Active Directory Graph API | https://graph.windows.net |
| Microsoft Graph-API voor Microsoft 365 Government - GCC hoog | https://graph.microsoft.us |
| Microsoft Graph-API voor Microsoft 365 Government - Amerikaanse ministerie van defensie | https://dod-graph.microsoft.us |
| Eindpunten voor Azure Government-services | Zie voor meer informatie, [Ontwikkelaarshandleiding voor Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-developer-guide) |
| Microsoft 365 Government - hoge GCC-eindpunten | Zie voor meer informatie, [Office 365 US GCC hoge Government-eindpunten](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-gcc-high-endpoints) |
| Microsoft 365 Government - DoD | Zie voor meer informatie, [Office 365 US Government DoD-eindpunten](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-dod-endpoints) |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen:

- [Wat is Azure Government?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)

- [Azure Government AAD-instantie Endpoint Update](https://devblogs.microsoft.com/azuregov/azure-government-aad-authority-endpoint-update/)

- [Microsoft Graph-eindpunten in US Government-cloud](https://developer.microsoft.com/graph/blogs/new-microsoft-graph-endpoints-in-us-government-cloud/)

- [Office 365 US Government GCC hoog en DoD](https://docs.microsoft.com/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/gcc-high-and-dod)