---
title: On-premises Azure AD-wachtwoordbeveiliging Veelgestelde vragen over
description: On-premises Azure AD-wachtwoordbeveiliging Veelgestelde vragen over
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: f690625fda07bdbff671567f0292cc8e1cabeda1
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078692"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Preview: Azure AD-beveiliging on-premises wachtwoord - Veelgestelde vragen

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="general-questions"></a>Algemene vragen

**V: Wanneer wordt Azure AD-wachtwoordbeveiliging algemene beschikbaarheid (GA) bereikt?**

We hebben een datum GA nog niet aangekondigd.

**V: Kan on-premises beveiliging met Azure AD wachtwoorden in niet-openbare clouds worden ondersteund?**

Nee, on-premises Azure AD wachtwoordbeveiliging wordt alleen ondersteund in de openbare cloud.

**V: Hoe kan ik de voordelen van Azure AD wachtwoord beveiliging toepassen op een subset van mijn on-premises gebruikers?**

Wordt niet ondersteund. Eenmaal geïmplementeerd en ingeschakeld, Azure AD-wachtwoordbeveiliging geen onderscheid - ontvangen gelijk beveiligingsvoordelen van alle gebruikers.

**V: Is dit dan ondersteund voor het installeren van Azure AD-wachtwoordbeveiliging naast andere producten op basis van wachtwoord-filter?**

Ja. Ondersteuning voor meerdere geregistreerde wachtwoord filter-dll's een functie van de Windows core is en niet specifiek voor Azure AD-wachtwoordbeveiliging. Alle geregistreerde wachtwoord filter dll-bestanden moet accepteren voordat een wachtwoord wordt geaccepteerd.

**V: Waarom is de DFS-replicatie voor sysvol-replicatie vereist?**

FRS (de voorafgaande-technologie DFSR) heeft tal van bekende problemen en wordt volledig ondersteund in nieuwere versies van Windows Server Active Directory. Nul testen van Azure AD-wachtwoordbeveiliging wordt uitgevoerd op domeinen FRS geconfigureerd.

Zie de volgende artikelen voor meer informatie:

[De aanvraag voor sysvol-replicatie migreren naar DFS-replicatie](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr)

[Het einde is Nigh voor FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs)

**V: Waarom is opnieuw opstarten vereist om te installeren of upgraden van de DC-agentsoftware?**

Deze vereiste wordt veroorzaakt door core Windows gedrag.

**V: Is er manier om een DC-agent voor het gebruik van een bepaalde proxyserver te configureren?**

Nee.

## <a name="next-steps"></a>Volgende stappen

Als u een on-premises Azure AD wachtwoord beveiliging vraag die hier niet wordt beantwoord hebt, dienen een feedbackitem hieronder - Hartelijk dank!

[Wachtwoordbeveiliging in Azure AD implementeren](howto-password-ban-bad-on-premises-deploy.md)
