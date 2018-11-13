---
title: Een app registreren bij het Azure Active Directory v2.0-eindpunt | Microsoft Docs
description: Informatie over hoe u een app registreert bij Microsoft voor het inschakelen van aanmelding bij en toegang tot Microsoft-services met behulp van het Azure Active Directory v2.0-eindpunt.
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
ms.topic: quickstart
ms.date: 11/02/2018
ms.author: celested
ms.reviewer: lenalepa
ms.custom: aaddev
ms.openlocfilehash: c0bf5bbdf496a23a5ed66a149933f25a059984a9
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913275"
---
# <a name="quickstart-register-an-app-with-the-azure-active-directory-v20-endpoint"></a>Snelstart: Een app registreren bij het Azure Active Directory v2.0-eindpunt

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Als u een app wilt bouwen die zowel aanmelding via een persoonlijk Microsoft-account (MSA) als een werk- of schoolaccount (Azure Active Directory) accepteert, moet u een app registreren bij het Azure Active Directory-v2.0-eindpunt (Azure AD). Op dit moment kunt u geen van de bestaande apps gebruiken die u bij Azure Active Directory of MSA hebt - u moet u een compleet nieuwe app maken.

Niet alle Azure Active Directory-scenario's en -functies worden ondersteund door het v2.0-eindpunt. Lees meer informatie over de [beperkingen van v2.0](active-directory-v2-limitations.md) om te bepalen of u het v2.0-eindpunt moet gebruiken.

> [!NOTE]
> Een nieuwe app registreren? Probeer de nieuwe **App-registraties-ervaring (preview)** uit in de Azure-portal. Zie [Een app registreren (preview)](quickstart-register-app.md) om aan de slag te gaan.

## <a name="step-1-sign-in-to-the-microsoft-application-registration-portal"></a>Stap 1: Aanmelden bij de Microsoft-portal voor toepassingsregistratie

1. Navigeer naar de Microsoft-portal voor toepassingsregistratie op [ https://apps.dev.microsoft.com/ ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).
1. Meld u aan met ofwel een persoonlijk of werk- of schoolaccount van Microsoft. Als u geen van beide hebt, moet u zich aanmelden voor een nieuw persoonlijk account.
1. Klaar? U zou nu moeten kijken naar de lijst met Microsoft-apps, die waarschijnlijk leeg is. Daar gaan we verandering in brengen.

## <a name="step-2-register-an-app"></a>Stap 2: Een app registreren

1. Selecteer **Een app toevoegen** en geef de app een naam.
    Door de portal wordt aan uw app een wereldwijd unieke toepassings-id toegewezen die u later in uw code gaat gebruiken. Als uw app een serveronderdeel bevat dat toegangstokens nodig heeft voor het aanroepen van API's (denk aan: Office, Azure of uw eigen web-API) moet u hier ook een **Toepassingsgeheim** maken.
1. Voeg vervolgens de **Platforms** toe die door uw app worden gebruikt.
    * Geef voor web-apps een **Omleidings-URI** op waarnaar aanmeldingberichten kunnen worden verzonden.
    * Noteer voor mobiele apps de standaardomleidings-URI die automatisch wordt gemaakt.
    * Voor web-API's wordt automatisch een standaardbereik voor toegang tot de web-API gemaakt.
        U kunt aanvullende bereiken toevoegen met de knop **Bereik toevoegen**. U kunt ook toepassingen toevoegen die vooraf zijn gemachtigd voor het gebruik van uw web-API met behulp van het formulier **Vooraf gemachtigde toepassingen**.
1. Pas desgewenst de weergave van de aanmeldingspagina aan in de sectie **Profiel**. 
1. **Sla uw wijzigingen op** voordat u doorgaat.

> [!NOTE]
> Wanneer u een toepassing registreert met behulp van [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), wordt de toepassing geregistreerd in de starttenant van het account waarmee u zich bij de portal aanmeldt. Dit betekent dat u een toepassing niet kunt registreren in uw Azure AD-tenant met behulp van een persoonlijk Microsoft-account. Als u een toepassing expliciet in een bepaalde tenant wilt registreren, meldt u zich aan met een account dat oorspronkelijk is gemaakt in die tenant.

## <a name="next-steps"></a>Volgende stappen

Nu u een Microsoft-app hebt, kunt u een van de snelstarts over v2.0 volgen. Hier volgen enkele aanbevelingen:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]
