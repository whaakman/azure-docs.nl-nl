---
title: On-premises versiegeschiedenis van Azure AD wachtwoord protection agent
description: Versie van documenten en het gedrag wijzigingsoverzicht
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: c52c84a1311c30c19356bb8a1287b203faf476fc
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50743259"
---
# <a name="preview--azure-ad-password-protection-agent-version-history"></a>Voorbeeld: Versiegeschiedenis van Azure AD wachtwoord protection agent

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="12100"></a>1.2.10.0

Releasedatum: 17-8-2018

Opgelost:

* Register-AzureADPasswordProtectionProxy en registreer AzureADPasswordProtectionForest bieden nu ondersteuning voor meervoudige verificatie
* Register-AzureADPasswordProtectionProxy vereist een WS2012 of een latere versie domeincontroller in het domein om te voorkomen dat versleutelingsfouten.
* DC-agentservice is betrouwbaarder over het aanvragen van een nieuw wachtwoordbeleid van Azure bij het opstarten.
* DC-agent-service wordt van Azure om het uur indien nodig een nieuw wachtwoordbeleid aanvragen, maar wordt nu doen op een willekeurig geselecteerde begintijd.
* DC-agent-service wordt niet langer een onbepaalde vertraging veroorzaken in nieuwe DC-aankondiging wanneer geïnstalleerd op een server vóór de promotie als een replica.
* DC-agent-service wordt nu in acht neemt de 'Wachtwoordbeveiliging op Windows Server Active Directory inschakelen' configuratie-instelling
* Beide DC-agent en -proxy installatieprogramma's ondersteunen nu in-place upgrade bij het upgraden naar toekomstige versies.

> [!WARNING]
> In-place upgrade van versie 1.1.10.3 wordt niet ondersteund en zal leiden tot een fout bij de installatie. Op een upgrade uitvoeren naar versie 1.2.10 of hoger, u moet eerst volledig verwijderen van de DC-agent en -proxy-service-software, en vervolgens de nieuwe versie installeert helemaal. Registratie van de Azure AD-wachtwoordbeveiliging Proxy-service is vereist.  Het is niet vereist voor het forest opnieuw te registreren.

> [!NOTE]
> In-place upgrades van de DC-agentsoftware moet opnieuw worden opgestart.

* DC-agent en proxy-service bieden nu ondersteuning voor die worden uitgevoerd op een server die is geconfigureerd voor het alleen gebruik compatibele FIPS-algoritmen.
* Dankzij de verbeterde logboekregistratie
* Secundaire prestaties en robuustheid oplossingen

## <a name="11103"></a>1.1.10.3

Uitgebracht op: 15-6-2018

Eerste openbare preview-versie

## <a name="next-steps"></a>Volgende stappen

[Wachtwoordbeveiliging in Azure AD implementeren](howto-password-ban-bad-on-premises-deploy.md)
