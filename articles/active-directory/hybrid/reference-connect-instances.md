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
ms.topic: reference
ms.date: 05/27/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c342eac5460d8d52422b0497b1283f367660eb3c
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66298820"
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

## <a name="microsoft-azure-government"></a>Microsoft Azure en de overheid
De [Microsoft Azure Government-cloud](https://azure.microsoft.com/features/gov/) is een cloudservice voor de Amerikaanse overheid.

Deze cloud werd ondersteund door eerdere releases van DirSync. Van build 1.1.180 van Azure AD Connect, wordt de volgende generatie van de cloud ondersteund. Deze generatie wordt met behulp van alleen-Verenigde Staten op basis van eindpunten en hebben een andere lijst met URL's te openen in uw proxy-server.

| URL's te openen in de webtoepassingsproxy-server |
| --- |
| \*.microsoftonline.com |
| \*.microsoftonline.us |
| \*. windows.net (vereist voor automatische detectie van Azure Government-tenant) |
| \*.gov.us.microsoftonline.com |
| + Intrekkingslijsten voor certificaten |

> [!NOTE]
> Vanaf Azure AD Connect versie 1.1.647.0, als de waarde AzureInstance in het register is niet langer vereist, mits *. windows.net is geopend op uw proxy-server (s). Voor klanten die geen internetverbinding vanaf hun Azure AD Connect-server (s) toestaan, kan de volgende handmatige configuratie worden gebruikt.

### <a name="manual-configuration"></a>Handmatige configuratie

De volgende handmatige configuratiestappen worden gebruikt om te controleren of dat Azure AD Connect maakt gebruik van Azure Government-eindpunten voor synchronisatie.

1. Start de Azure AD Connect-installatie.
2. Wanneer u de eerste pagina waar u de gebruiksrechtovereenkomst accepteren mag zien, ga niet verder maar laat u de installatiewizard uitgevoerd.
3. Start regedit en wijzigt u de registersleutel `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` op de waarde `4`.
4. Ga terug naar de Azure AD Connect-installatiewizard, accepteer de gebruiksrechtovereenkomst en doorgaan. Zorg ervoor dat u tijdens de installatie van de **aangepaste configuratie** installatie pad (en niet de Express-installatie), ga vervolgens door de installatie van de normale wijze te gebruiken.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).
