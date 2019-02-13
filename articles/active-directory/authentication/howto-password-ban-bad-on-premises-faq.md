---
title: On-premises bescherming van Azure AD-wachtwoord Veelgestelde vragen
description: On-premises bescherming van Azure AD-wachtwoord Veelgestelde vragen
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a4528cde92c5f738fa3fa7f4a649d84b1e79431
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56175945"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Preview: Azure AD wachtwoordbeveiliging on-premises - Veelgestelde vragen over

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="general-questions"></a>Algemene vragen

**V: Wanneer wordt Azure AD-wachtwoord beveiliging algemene beschikbaarheid (GA) bereikt?**

Algemene beschikbaarheid is gepland voor Q1 CY2019 (vóór het einde van maart 2019). Hartelijk dank aan iedereen die is opgegeven feedback over de functie op datum - we stellen het op prijs!

**V: Kan on-premises bescherming van Azure AD-wachtwoord in niet-openbare clouds worden ondersteund?**

Nee, on-premises Azure AD-wachtwoord beveiliging wordt alleen ondersteund in de openbare cloud.

**V: Hoe kan ik voordelen van Azure AD-wachtwoord beveiliging toepassen op een subset van mijn on-premises gebruikers?**

Wordt niet ondersteund. Zodra de geïmplementeerd en ingeschakeld, beveiliging van Azure AD-wachtwoord geen onderscheid - ontvangen gelijk beveiligingsvoordelen van alle gebruikers.

**V: Is dit dan ondersteund voor het installeren van Azure AD-wachtwoord bescherming naast andere producten op basis van wachtwoord-filter?**

Ja. Ondersteuning voor meerdere geregistreerde wachtwoord filter-dll's een functie van de Windows core is en niet specifiek voor Azure AD-wachtwoord beveiliging. Alle geregistreerde wachtwoord filter dll-bestanden moet accepteren voordat een wachtwoord wordt geaccepteerd.

**V: Waarom is de DFS-replicatie voor sysvol-replicatie vereist?**

FRS (de voorafgaande-technologie DFSR) heeft tal van bekende problemen en wordt volledig ondersteund in nieuwere versies van Windows Server Active Directory. Nul testen van de beveiliging van Azure AD-wachtwoord wordt uitgevoerd op domeinen FRS geconfigureerd.

Zie de volgende artikelen voor meer informatie:

[De aanvraag voor sysvol-replicatie migreren naar DFS-replicatie](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr)

[Het einde is Nigh voor FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs)

**V: Waarom is opnieuw opstarten vereist om te installeren of upgraden van de DC-agentsoftware?**

Deze vereiste wordt veroorzaakt door core Windows gedrag.

**V: Is er manier om een DC-agent voor het gebruik van een bepaalde proxyserver te configureren?**

Nee.

## <a name="next-steps"></a>Volgende stappen

Als u een on-premises bescherming van Azure AD-wachtwoord-vraag die hier niet wordt beantwoord hebt, dienen een feedbackitem hieronder - Hartelijk dank!

[Wachtwoordbeveiliging in Azure AD implementeren](howto-password-ban-bad-on-premises-deploy.md)
