---
title: 'Azure AD Connect: Service-exemplaren synchroniseren | Microsoft Docs'
description: Deze pagina worden speciale overwegingen voor exemplaren van Azure AD.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: billmath
ms.openlocfilehash: 0b3f274c2bf457760a1d62d5cc369ebdb0c52c59
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Speciale overwegingen voor exemplaren
Azure AD Connect wordt meestal gebruikt met het wereldwijde exemplaar van Azure AD en Office 365. Maar er zijn ook andere exemplaren en deze hebben verschillende vereisten voor de URL's en andere speciale overwegingen.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Duitsland
De [Microsoft Cloud Duitsland](http://www.microsoft.de/cloud-deutschland) is een soevereine cloud beheerd door een beheerder Duitse gegevens.

| URL's openen in de proxyserver |
| --- |
| \*.microsoftonline.de |
| \*.windows.net |
| + Intrekkingslijsten voor certificaten |

Wanneer u zich bij uw Azure AD-tenant aanmelden, moet u een account in het domein onmicrosoft.de gebruiken.

Functies die momenteel niet aanwezig in de Microsoft Cloud-Duitsland:

* **Azure AD Connect Health** is niet beschikbaar.
* **Automatische updates** is niet beschikbaar.
* **Wachtwoord terugschrijven** is beschikbaar voor het voorbeeld met Azure AD Connect versie 1.1.570.0 en na.
* Andere Azure AD Premium-services zijn niet beschikbaar.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure Government cloud
De [Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/) is een cloudservice voor de overheid.

Deze cloud werd ondersteund door eerdere releases van DirSync. Vanaf build 1.1.180 van Azure AD Connect, zijn de volgende generatie van de cloud wordt ondersteund. Deze generatie alleen VS gebaseerde eindpunten gebruikt en een andere lijst met URL's openen in uw proxy-server hebben.

| URL's openen in de proxyserver |
| --- |
| \*.microsoftonline.com |
| \*.microsoftonline.us |
| \*. windows.net (vereist voor automatische detectie van Azure AD government tenant) |
| \*.gov.us.microsoftonline.com |
| + Intrekkingslijsten voor certificaten |

> [!NOTE]
> Vanaf AAD Connect versie 1.1.647.0 AzureInstance instellingswaarde in het register is niet langer vereist op voorwaarde dat *. windows.net is geopend op uw proxy-server (s).

Functies die momenteel niet aanwezig in de Microsoft Azure Government cloud:

* **Azure AD Connect Health** is niet beschikbaar.
* **Automatische updates** is niet beschikbaar.
* **Wachtwoord terugschrijven** is beschikbaar voor het voorbeeld met Azure AD Connect versie 1.1.570.0 en na.
* Andere Azure AD Premium-services zijn niet beschikbaar.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).
