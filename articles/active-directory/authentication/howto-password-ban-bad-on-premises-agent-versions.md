---
title: On-premises versiegeschiedenis van Azure AD wachtwoord protection agent
description: Versie van documenten en het gedrag wijzigingsoverzicht
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 11/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 1d27aa46148e0169909a7aaf05baaac18b050a34
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54423660"
---
# <a name="preview--azure-ad-password-protection-agent-version-history"></a>Preview:  Azure AD wachtwoord protection agent versiegeschiedenis

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="12250"></a>1.2.25.0

Uitgebracht op: 11/01/2018

Opgelost:

* DC-agent en proxy-service moet niet meer mislukken vanwege certificaatfouten vertrouwen.
* DC-agent en proxy-service beschikken over de aanvullende oplossingen voor FIPS-compatibele computers.
* Proxy-service wordt nu werken alleen goed in een TLS 1.2 alleen-lezen-netwerkomgeving.
* Secundaire prestaties en robuustheid oplossingen
* Dankzij de verbeterde logboekregistratie

Wijzigingen in:

* De vereiste minimumversie van OS-niveau voor de Proxy-service is nu Windows Server 2012 R2. De minimaal vereiste besturingssysteemniveau hebben voor de DC-agent-service blijft op Windows Server 2012.
* Het wachtwoord validatie-algoritme maakt gebruik van een uitgebreide teken normalisatie-tabel. Dit kan leiden tot wachtwoorden worden geweigerd en die zijn aanvaard in eerdere versies.

## <a name="12100"></a>1.2.10.0

Uitgebracht op: 8/17/2018

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
* Secundaire prestaties en robuustheid oplossingen
* Dankzij de verbeterde logboekregistratie

## <a name="11103"></a>1.1.10.3

Uitgebracht op: 6/15/2018

Eerste openbare preview-versie

## <a name="next-steps"></a>Volgende stappen

[Wachtwoordbeveiliging in Azure AD implementeren](howto-password-ban-bad-on-premises-deploy.md)
