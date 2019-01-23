---
title: 'Azure AD Connect: Exemplaren van de service synchroniseren | Microsoft Docs'
description: Deze pagina worden speciale overwegingen voor exemplaren van Azure AD.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: be20f50b68ab3715e2b7a98db208dcd81a995f1b
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54463878"
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Speciale overwegingen voor exemplaren
Azure AD Connect wordt meestal gebruikt met de world wide-exemplaar van Azure AD en Office 365. Maar er zijn ook andere exemplaren en deze hebben verschillende vereisten voor URL's en andere speciale overwegingen.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Duitsland
De [Microsoft Cloud Duitsland](https://www.microsoft.de/cloud-deutschland) is een onafhankelijke cloud bediend door een Duitse Gegevensbeheerder.

| URL's te openen in de webtoepassingsproxy-server |
| --- |
| \*.microsoftonline.de |
| \*.windows.net |
| + Intrekkingslijsten voor certificaten |

Wanneer u zich bij uw Azure AD-tenant aanmelden, moet u een account in het domein onmicrosoft.de gebruiken.

Functies die momenteel niet aanwezig zijn in de Microsoft Cloud Duitsland:

* **Wachtwoord terugschrijven** is beschikbaar voor Preview-versie met Azure AD Connect versie 1.1.570.0 en na.
* Andere Azure AD Premium-services zijn niet beschikbaar.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure Government-cloud
De [Microsoft Azure Government-cloud](https://azure.microsoft.com/features/gov/) is een cloudservice voor de Amerikaanse overheid.

Deze cloud werd ondersteund door eerdere releases van DirSync. Van build 1.1.180 van Azure AD Connect, wordt de volgende generatie van de cloud ondersteund. Deze generatie wordt met behulp van alleen-Verenigde Staten op basis van eindpunten en hebben een andere lijst met URL's te openen in uw proxy-server.

| URL's te openen in de webtoepassingsproxy-server |
| --- |
| \*.microsoftonline.com |
| \*.microsoftonline.us |
| \*. windows.net (vereist voor automatische detectie van Azure AD government-tenant) |
| \*.gov.us.microsoftonline.com |
| + Intrekkingslijsten voor certificaten |

> [!NOTE]
> Vanaf AAD Connect versie 1.1.647.0 AzureInstance waarde in het register is niet langer vereist, mits *. windows.net is geopend op uw proxy-server (s).

Functies die momenteel niet aanwezig zijn in de Microsoft Azure Government-cloud:

* **Wachtwoord terugschrijven** is beschikbaar voor Preview-versie met Azure AD Connect versie 1.1.570.0 en na.
* Andere Azure AD Premium-services zijn niet beschikbaar.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).
