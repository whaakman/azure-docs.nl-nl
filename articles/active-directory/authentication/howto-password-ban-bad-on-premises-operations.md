---
title: Azure AD-wachtwoordbeveiliging preview-bewerkingen en rapportage
description: Azure AD-wachtwoordbeveiliging preview na de implementatie-bewerkingen en rapportage
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
ms.openlocfilehash: 1fda79f16560a5c96e1283f4d9d9f14dbe503d61
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56175239"
---
# <a name="preview-azure-ad-password-protection-operational-procedures"></a>Preview: Azure AD-wachtwoordbeveiliging operationele procedures

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Nadat u hebt de [installatie van Azure AD-wachtwoord Protection](howto-password-ban-bad-on-premises-deploy.md) on-premises, er zijn een aantal items die moeten worden geconfigureerd in Azure portal.

## <a name="configure-the-custom-banned-password-list"></a>De lijst met aangepaste uitgesloten wachtwoorden configureren

Volg de instructies in het artikel [configureren van de lijst met uitgesloten wachtwoorden aangepaste](howto-password-ban-bad-configure.md) voor stappen voor het aanpassen van de lijst met uitgesloten wachtwoorden voor uw organisatie.

## <a name="enable-password-protection"></a>Schakel de wachtwoordbeveiliging

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory**, **verificatiemethoden**, klikt u vervolgens **wachtwoordbeveiliging (Preview)**.
1. Stel **wachtwoordbeveiliging inschakelen op Windows Server Active Directory** naar **Ja**
1. Zoals vermeld in de [Implementatiehandleiding](howto-password-ban-bad-on-premises-deploy.md#deployment-strategy), het verdient aanbeveling om in te stellen in eerste instantie de **modus** naar **Audit**
   * Nadat u vertrouwd met de functie bent, kunt u schakelen de **modus** naar **afgedwongen**
1. Klik op **Opslaan**.

![Wachtwoordbeveiliging van Azure AD-onderdelen in Azure portal inschakelen](./media/howto-password-ban-bad-on-premises-operations/authentication-methods-password-protection-on-prem.png)

## <a name="audit-mode"></a>Controlemodus

De controlemodus is bedoeld als een manier om uit te voeren van de software in een 'wat als'-modus. Elke DC agent-service evalueert een inkomend wachtwoord op basis van het actieve beleid. Als het huidige beleid is geconfigureerd om te worden in de controlemodus, 'slechte' wachtwoorden resulteren in berichten in Logboeken, maar worden geaccepteerd. Dit is het enige verschil tussen de controle en afdwingen; alle andere bewerkingen uitvoeren hetzelfde.

> [!NOTE]
> Microsoft raadt aan dat initiële implementatie en het testen van altijd in de controlemodus eerst. Gebeurtenissen in het gebeurtenislogboek moeten vervolgens worden gecontroleerd om te anticiperen op of een bestaande operationele processen zou worden verstoord zodra de modus afdwingen is ingeschakeld.

## <a name="enforce-mode"></a>Afdwingingsmodus

Afdwingen modus is bedoeld als de uiteindelijke configuratie. Zoals in de controlemodus is voorgaande, elke DC-agent-service beoordeelt wat de binnenkomende wachtwoorden op basis van het actieve beleid. Als de modus afdwingen is ingeschakeld als u een wachtwoord dat wordt beschouwd als niet-beveiligd op basis van het beleid wordt geweigerd.

Wanneer een wachtwoord is afgewezen in de modus afdwingen door de Azure AD-wachtwoord beveiliging DC-Agent, is de zichtbaar impact gezien door een eindgebruiker identiek aan wat ze zien zouden als het wachtwoord is geweigerd door traditionele on-premises-wachtwoordafdwinging complexiteit. Een gebruiker ziet bijvoorbeeld mogelijk het volgende traditionele foutbericht op het scherm van Windows logon\change wachtwoord:

`Unable to update the password. The value provided for the new password does not meet the length, complexity, or history requirements of the domain.`

Dit bericht is slechts één voorbeeld van de mogelijke resultaten. Het specifieke foutbericht kan variëren afhankelijk van het aantal daadwerkelijke software of het scenario dat wordt geprobeerd een niet-beveiligd wachtwoord in te stellen.

Betrokken eindgebruikers mogelijk om te werken met hun IT-afdeling om te begrijpen van de nieuwe vereisten en meer kunnen beveiligde wachtwoorden moeten kiezen.

## <a name="enable-mode"></a>Modus inschakelen

Deze instelling moet normaal gesproken blijven in de standaardstaat ingeschakeld (Ja). Deze instelling uitgeschakeld (Nee) zorgt ervoor dat alle geïmplementeerde Azure AD-wachtwoord beveiliging DC agents om te gaan op een quiescent modus wanneer alle wachtwoorden worden geaccepteerd als-is, en geen validatie-activiteiten worden uitgevoerd dan ook (bijvoorbeeld, zelfs niet controlegebeurtenissen zal worden verzonden).

## <a name="next-steps"></a>Volgende stappen

[Bewaking voor de beveiliging van Azure AD-wachtwoord](howto-password-ban-bad-on-premises-monitor.md)
