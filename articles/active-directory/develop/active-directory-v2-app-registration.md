---
title: Een toepassing registreren met het Azure AD v2.0-eindpunt met de portal | Microsoft Docs
description: Het registreren van een app met Microsoft voor het inschakelen aanmelden en toegang tot Microsoft-services met behulp van het v2.0-eindpunt
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: e6202aa8665c906382666fe08a561421e50e0a8d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Het registreren van een app met het v2.0-eindpunt
Als u wilt maken van een app die zowel MSA & Azure AD accepteert aanmelden, u moet eerst een app registreren bij Microsoft.  Op dit moment niet mogelijk om eventuele bestaande apps u met Azure AD hebt wellicht te gebruiken of MSA - u moet een geheel nieuwe maken.

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt.  Meer informatie over om te bepalen of moet u het v2.0-eindpunt, [v2.0 beperkingen](active-directory-v2-limitations.md).
> 
> 

## <a name="visit-the-microsoft-app-registration-portal"></a>Ga naar de Microsoft app-registratieportal
Eerste dingen-: Ga naar [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Dit is een nieuwe app-portal voor wachtwoordregistratie waar u uw Microsoft-apps kunt beheren.

Meld u aan met ofwel een persoonlijke of werk of school Microsoft-account.  Als u geen ofwel hebt, moet u zich aanmelden voor een nieuw persoonlijk account. Opwekken, won't duurt lang - wij hier wachten.

Uitgevoerd? U moet nu worden kijken naar de lijst met Microsoft-apps, die waarschijnlijk leeg is.  Hiermee kunt wijzigen.

Klik op **een app toevoegen**, en een naam geven.  De portal toewijzen uw app een globally unique identifier toepassings-Id die u later in uw code.  Als uw app een serverzijde-onderdeel bevat dat toegangstokens moet voor aanroepen API's (denkt: Office, Azure of uw eigen web-API), moet u maken een **Toepassingsgeheim** hier ook.

Voeg vervolgens de Platforms die uw app wilt gebruiken.

* Voor op basis van web-apps, geeft u een **omleidings-URI** waar aanmelden berichten kunnen worden verzonden.
* Noteer de standaard voor mobiele apps omleidings-uri die automatisch voor u gemaakt.

U kunt desgewenst het uiterlijk van de aanmeldingspagina in de sectie profiel aanpassen.  Zorg ervoor dat u **opslaan** voordat u doorgaat.

> [!NOTE]
> Wanneer u maakt een toepassing met behulp [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), de toepassing wordt geregistreerd in de oorspronkelijke tenant van het account waarmee u zich aanmeldt bij de portal.  Dit betekent dat u een toepassing in uw Azure AD-tenant met behulp van een persoonlijk Microsoft-account niet kunt registreren.  Als u expliciet een toepassing in een bepaalde tenant te registreren, aanmelden met een account die oorspronkelijk is gemaakt in deze tenant.
> 
> 

## <a name="build-a-quick-start-app"></a>Een App snel starten
Nu dat u een Microsoft-app hebt, kunt u een van onze v2.0 snel starten-zelfstudie te voltooien.  Hier volgen enkele aanbevelingen:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

