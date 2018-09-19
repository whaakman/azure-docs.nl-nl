---
title: 'Azure AD Connect: Exemplaren van de service synchroniseren | Microsoft Docs'
description: Deze pagina worden speciale overwegingen voor exemplaren van Azure AD.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
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
ms.openlocfilehash: a41b236182c18a83b6c83a38fd8420a013313d56
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46315089"
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Speciale overwegingen voor exemplaren
Azure AD Connect wordt meestal gebruikt met de world wide-exemplaar van Azure AD en Office 365. Maar er zijn ook andere exemplaren en deze hebben verschillende vereisten voor URL's en andere speciale overwegingen.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Duitsland
De [Microsoft Cloud Duitsland](http://www.microsoft.de/cloud-deutschland) is een onafhankelijke cloud bediend door een Duitse Gegevensbeheerder.

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
