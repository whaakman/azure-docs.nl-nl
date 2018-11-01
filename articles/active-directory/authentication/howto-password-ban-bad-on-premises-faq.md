---
title: On-premises Azure AD-wachtwoordbeveiliging Veelgestelde vragen over
description: On-premises Azure AD-wachtwoordbeveiliging Veelgestelde vragen over
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 59c89c81f618876de48de66a38e1063eb658fba4
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50743294"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Voorbeeld: Azure AD-beveiliging on-premises wachtwoord - Veelgestelde vragen

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="general-questions"></a>Algemene vragen

**V: wanneer wordt Azure AD-wachtwoordbeveiliging algemene beschikbaarheid (GA) bereiken?**

We hebben een datum GA nog niet aangekondigd.

**V: wordt on-premises beveiliging met Azure AD wachtwoorden in niet-openbare clouds worden ondersteund?**

Nee, on-premises Azure AD wachtwoordbeveiliging wordt alleen ondersteund in de openbare cloud.

**V: hoe kan ik voordelen van Azure AD wachtwoord beveiliging toepassen op een subset van mijn on-premises gebruikers?**

Wordt niet ondersteund. Eenmaal geïmplementeerd en ingeschakeld, Azure AD-wachtwoordbeveiliging geen onderscheid - ontvangen gelijk beveiligingsvoordelen van alle gebruikers.

**Vraag: is dit dan ondersteund voor het installeren van Azure AD-wachtwoordbeveiliging naast andere producten op basis van wachtwoord-filter?**

Ja. Ondersteuning voor meerdere geregistreerde wachtwoord filter-dll's een functie van de Windows core is en niet specifiek voor Azure AD-wachtwoordbeveiliging. Alle geregistreerde wachtwoord filter dll-bestanden moet accepteren voordat een wachtwoord wordt geaccepteerd.

**V: Waarom is de DFS-replicatie voor sysvol-replicatie vereist?**

FRS (de voorafgaande-technologie DFSR) heeft tal van bekende problemen en wordt volledig ondersteund in nieuwere versies van Windows Server Active Directory. Nul testen van Azure AD-wachtwoordbeveiliging wordt uitgevoerd op domeinen FRS geconfigureerd.

Zie de volgende artikelen voor meer informatie:

[De aanvraag voor sysvol-replicatie migreren naar DFS-replicatie](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr)

[Het einde is Nigh voor DRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs)

**V: Waarom wordt opnieuw opstarten vereist om te installeren of upgraden van de DC-agentsoftware?**

Deze vereiste wordt veroorzaakt door core Windows gedrag.

**V: is er manier om een DC-agent voor het gebruik van een bepaalde proxyserver te configureren?**

Nee.

## <a name="next-steps"></a>Volgende stappen

Als u een on-premises Azure AD wachtwoord beveiliging vraag die hier niet wordt beantwoord hebt, dienen een feedbackitem hieronder - Hartelijk dank!

[Wachtwoordbeveiliging in Azure AD implementeren](howto-password-ban-bad-on-premises-deploy.md)
