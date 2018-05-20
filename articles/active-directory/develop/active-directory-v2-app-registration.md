---
title: Een toepassing registreren met het Azure AD v2.0-eindpunt met de portal | Microsoft Docs
description: Het registreren van een app met Microsoft voor het inschakelen aanmelden en toegang tot Microsoft-services met behulp van het v2.0-eindpunt
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
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Het registreren van een app met het v2.0-eindpunt
Maken van een app die zowel persoonlijke Microsoft-account (MSA) & werk accepteert of school aanmeldingspagina account (Azure AD), moet u eerst een app registreren bij Microsoft. Op dit moment niet mogelijk om eventuele bestaande apps u met Azure AD hebt wellicht te gebruiken of MSA - u moet een geheel nieuwe maken.

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt. Om te bepalen of moet u het v2.0-eindpunt, meer informatie over de [v2.0 beperkingen](active-directory-v2-limitations.md).


## <a name="visit-the-microsoft-app-registration-portal"></a>Ga naar de Microsoft app-registratieportal
Eerst, navigeer naar de Microsoft app-registratieportal op [ https://apps.dev.microsoft.com/ ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). 

Meld u aan met ofwel een persoonlijke of werk of school Microsoft-account. Als u geen ofwel hebt, moet u zich aanmelden voor een nieuw persoonlijk account.

Uitgevoerd? U moet nu worden kijken naar de lijst met Microsoft-apps, die waarschijnlijk leeg is. Hiermee kunt wijzigen.

Klik op **een app toevoegen**, en een naam geven. De portal toewijzen uw app een globally unique identifier toepassings-ID die u later in uw code. Als uw app een serverzijde-onderdeel bevat dat toegangstokens moet voor aanroepen API's (denkt: Office, Azure of uw eigen web-API), moet u maken een **Toepassingsgeheim** hier ook.

Vervolgens voegt u de **Platforms** die door uw app wordt gebruikt.

* Voor het web gebaseerde apps, geeft u een **omleidings-URI** waar aanmelden berichten kunnen worden verzonden.
* Noteer de omleidings-URI automatisch voor u gemaakt van de standaard voor mobiele apps.
* Voor web-API's, een standaardbereik toegang tot de Web-API automatisch voor u gemaakt. U kunt kiezen om toe te voegen extra scopes met behulp van de **Add Scope** knop. U kunt ook alle toepassingen die vooraf zijn gemachtigd voor het gebruik van uw Web-API met toevoegen de **vooraf geautoriseerde toepassingen** formulier. 

Desgewenst kunt u het uiterlijk van de aanmeldingspagina in de **profiel** sectie. Zorg ervoor dat u **opslaan** voordat u doorgaat.

> [!NOTE]
> Wanneer u maakt een toepassing met behulp [ https://apps.dev.microsoft.com/ ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), de toepassing wordt geregistreerd in de oorspronkelijke tenant van het account waarmee u zich aanmeldt bij de portal. Dit betekent dat u een toepassing in uw Azure AD-tenant met behulp van een persoonlijk Microsoft-account niet kunt registreren. Als u expliciet een toepassing in een bepaalde tenant te registreren, aanmelden met een account die oorspronkelijk is gemaakt in deze tenant.


## <a name="build-a-quickstart-app"></a>Quick Start-app bouwen
Nu dat u een Microsoft-app hebt, kunt u een van de Quick Start-zelfstudies v2.0 voltooien. Hier volgen enkele aanbevelingen:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

