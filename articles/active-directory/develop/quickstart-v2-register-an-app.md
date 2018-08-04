---
title: Een toepassing registreren met het Azure AD v2.0-eindpunt met behulp van de portal | Microsoft Docs
description: Informatie over het registreren van een app met Microsoft voor het inschakelen van aanmelden en toegang tot Microsoft-services met behulp van het v2.0-eindpunt
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: 8ab4e6b5b2813a216b6dd6f0fc108a09239ca9a6
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39506547"
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Informatie over het registreren van een app met het v2.0-eindpunt
Een app maken die zowel persoonlijke Microsoft-account (MSA) & werk accepteert of school aanmelding bij het account (Azure AD), moet u eerst een app registreren bij Microsoft. Op dit moment kunt u zich niet aan alle bestaande apps die u met Azure AD hebt gebruikt of MSA -, moet u een compleet nieuwe toepassing maken.

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt. Om te bepalen als u het v2.0-eindpunt moet gebruiken, meer informatie over de [v2.0 beperkingen](active-directory-v2-limitations.md).


## <a name="visit-the-microsoft-app-registration-portal"></a>Ga naar de Microsoft app-registratieportal
Navigeer naar de Microsoft app-registratieportal op [ https://apps.dev.microsoft.com/ ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). 

Meld u aan met ofwel een persoonlijke of werk- of schoolaccount van Microsoft-account. Als u geen, dient u zich hebt aanmelden voor een nieuw persoonlijk account.

Gedaan? U moet nu worden kijken naar de lijst met Microsoft-apps, die waarschijnlijk leeg is. Laten we die wijzigen.

Klik op **een app toevoegen**, en geef deze een naam. De portal wordt uw app een wereldwijd unieke toepassings-ID die u later in uw code toewijzen. Als uw app een server-side-onderdeel bevat dat toegangstokens nodig zijn voor het aanroepen van API's (denkt: Office, Azure of uw eigen web-API), moet u maakt een **Toepassingsgeheim** hier ook.

Voeg vervolgens de **Platforms** dat door uw app wordt gebruikt.

* Voor het web gebaseerde apps bieden een **omleidings-URI** waar aanmelden berichten kunnen worden verzonden.
* Voor mobiele apps, noteert u de standaard-redirect die URI automatisch voor u gemaakt.
* Voor web-API's, een bereik van standaard toegang tot de Web-API automatisch voor u gemaakt. U kunt kiezen om toe te voegen extra scopes met behulp van de **bereik toevoegen** knop. U kunt ook alle toepassingen die vooraf zijn gemachtigd voor het gebruik van uw Web-API met toevoegen de **vooraf geautoriseerde toepassingen** formulier. 

(Optioneel) u kunt het uiterlijk van de aanmeldingspagina in de **profiel** sectie. Zorg ervoor dat u **opslaan** voordat u doorgaat.

> [!NOTE]
> Wanneer u maakt een toepassing met behulp [ https://apps.dev.microsoft.com/ ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), de toepassing wordt geregistreerd in de starttenant van het account waarmee u zich bij de portal. Dit betekent dat u een toepassing niet kunt registreren in uw Azure AD-tenant met behulp van een persoonlijk Microsoft-account. Als u expliciet een toepassing registreren in een bepaalde tenant wilt, zich aanmelden met een oorspronkelijk is gemaakt in deze tenant.


## <a name="build-a-quickstart-app"></a>Quick Start-app bouwen
Nu dat u een Microsoft-app hebt, kunt u een van de zelfstudies v2.0 kunt voltooien. Hier volgen enkele aanbevelingen:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

