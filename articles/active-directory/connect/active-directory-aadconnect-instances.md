---
title: 'Azure AD Connect: Service-exemplaren synchroniseren | Microsoft Docs'
description: Deze pagina worden speciale overwegingen voor exemplaren van Azure AD.
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e8321c3d16253226a5931cacbce6fa5d50b697bd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Speciale overwegingen voor exemplaren
Azure AD Connect wordt meestal gebruikt met het wereldwijde exemplaar van Azure AD en Office 365. Maar er zijn ook andere exemplaren en deze hebben verschillende vereisten voor de URL's en andere speciale overwegingen.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Duitsland
De [Microsoft Cloud Duitsland](http://www.microsoft.de/cloud-deutschland) is een soevereine cloud beheerd door een beheerder Duitse gegevens.

| URL's openen in de proxyserver |
| --- |
| \*. microsoftonline.de |
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
| \*. microsoftonline.us |
| \*. gov.us.microsoftonline.com |
| + Intrekkingslijsten voor certificaten |

Azure AD Connect kan niet automatisch detecteren dat uw Azure AD-tenant bevindt zich in de cloud van de overheid. In plaats daarvan moet u de volgende acties uitvoeren als u Azure AD Connect installeert.

1. Start de installatie van Azure AD Connect.
2. Wanneer u de eerste pagina waar u gewoonlijk accepteer de gebruiksrechtovereenkomst ziet, niet worden voortgezet, maar laat u de installatiewizard uitgevoerd.
3. Start regedit en wijzigt u de registersleutel `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` op de waarde `2`.
4. Ga terug naar de Azure AD Connect-installatiewizard, accepteer de gebruiksrechtovereenkomst en doorgaan. Controleer of u tijdens de installatie van de **aangepaste configuratie** installatie pad (en niet de Express-installatie). Ga vervolgens door gewoon de installatie.

Functies die momenteel niet aanwezig in de Microsoft Azure Government cloud:

* **Azure AD Connect Health** is niet beschikbaar.
* **Automatische updates** is niet beschikbaar.
* **Wachtwoord terugschrijven** is beschikbaar voor het voorbeeld met Azure AD Connect versie 1.1.570.0 en na.
* Andere Azure AD Premium-services zijn niet beschikbaar.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).
