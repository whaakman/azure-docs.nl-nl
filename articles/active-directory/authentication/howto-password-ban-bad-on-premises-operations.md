---
title: Azure AD-wachtwoord protection preview bewerkingen en rapportage
description: Azure AD wachtwoord protection preview na de implementatie-bewerkingen en rapportage
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 11/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 8d7002a014fc6cfab1888a6bc97c0f864de1d99d
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55080868"
---
# <a name="preview-azure-ad-password-protection-operational-procedures"></a>Preview: Azure AD wachtwoord beveiliging operationele procedures

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Nadat u hebt de [installatie van Azure AD-wachtwoordbeveiliging](howto-password-ban-bad-on-premises-deploy.md) on-premises, er zijn een aantal items die moeten worden geconfigureerd in Azure portal.

## <a name="configure-the-custom-banned-password-list"></a>De lijst met aangepaste uitgesloten wachtwoorden configureren

Volg de instructies in het artikel [configureren van de lijst met uitgesloten wachtwoorden aangepaste](howto-password-ban-bad-configure.md) voor stappen voor het aanpassen van de lijst met uitgesloten wachtwoorden voor uw organisatie.

## <a name="enable-password-protection"></a>Schakel de wachtwoordbeveiliging

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory**, **verificatiemethoden**, klikt u vervolgens **wachtwoordbeveiliging (Preview)**.
1. Stel **wachtwoordbeveiliging inschakelen op Windows Server Active Directory** naar **Ja**
1. Zoals vermeld in de [Implementatiehandleiding](howto-password-ban-bad-on-premises-deploy.md#deployment-strategy), het verdient aanbeveling om in te stellen in eerste instantie de **modus** naar **Audit**
   * Nadat u vertrouwd met de functie bent, kunt u schakelen de **modus** naar **afgedwongen**
1. Klik op **Opslaan**.

![Azure AD wachtwoord beveiliging onderdelen in Azure portal inschakelen](./media/howto-password-ban-bad-on-premises-operations/authentication-methods-password-protection-on-prem.png)

## <a name="audit-mode"></a>Controlemodus

De controlemodus is bedoeld als een manier om uit te voeren van de software in een 'wat als'-modus. Elke DC agent-service evalueert een inkomend wachtwoord op basis van het actieve beleid. Als het huidige beleid is geconfigureerd om te worden in de controlemodus, 'slechte' wachtwoorden resulteren in berichten in Logboeken, maar worden geaccepteerd. Dit is het enige verschil tussen de controle en afdwingen; alle andere bewerkingen uitvoeren hetzelfde.

> [!NOTE]
> Microsoft raadt aan dat initiële implementatie en het testen van altijd in de controlemodus eerst. Gebeurtenissen in het gebeurtenislogboek moeten vervolgens worden gecontroleerd om te anticiperen op of een bestaande operationele processen zou worden verstoord zodra de modus afdwingen is ingeschakeld.

## <a name="enforce-mode"></a>Afdwingingsmodus

Afdwingen modus is bedoeld als de uiteindelijke configuratie. Zoals in de controlemodus is voorgaande, elke DC-agent-service beoordeelt wat de binnenkomende wachtwoorden op basis van het actieve beleid. Als de modus afdwingen is ingeschakeld als u een wachtwoord dat wordt beschouwd als niet-beveiligd op basis van het beleid wordt geweigerd.

Wanneer u een wachtwoord is geweigerd in de modus afdwingen door de Azure AD-wachtwoordbeveiliging DC-Agent, is de zichtbaar impact gezien door een eindgebruiker identiek aan wat ze zien zouden als het wachtwoord is geweigerd door traditionele on-premises-wachtwoordafdwinging complexiteit. Een gebruiker ziet bijvoorbeeld mogelijk het volgende traditionele foutbericht op het scherm van Windows logon\change wachtwoord:

`Unable to update the password. The value provided for the new password does not meet the length, complexity, or history requirements of the domain.`

Dit bericht is slechts één voorbeeld van de mogelijke resultaten. Het specifieke foutbericht kan variëren afhankelijk van het aantal daadwerkelijke software of het scenario dat wordt geprobeerd een niet-beveiligd wachtwoord in te stellen.

Betrokken eindgebruikers mogelijk om te werken met hun IT-afdeling om te begrijpen van de nieuwe vereisten en meer kunnen beveiligde wachtwoorden moeten kiezen.

## <a name="enable-mode"></a>Modus inschakelen

Deze instelling moet normaal gesproken blijven in de standaardstaat ingeschakeld (Ja). Deze instelling uitgeschakeld (Nee) zorgt ervoor dat alle geïmplementeerde Azure AD-wachtwoord beveiligingsagents DC om te gaan op een quiescent modus wanneer alle wachtwoorden worden geaccepteerd als-is, en geen validatie-activiteiten worden uitgevoerd dan ook (bijvoorbeeld, zelfs niet controlegebeurtenissen zal worden verzonden).

## <a name="usage-reporting"></a>Rapportage over het gebruik

De `Get-AzureADPasswordProtectionSummaryReport` cmdlet kan worden gebruikt voor het produceren van een overzicht van de activiteit. Een voorbeeld van uitvoer van deze cmdlet is als volgt:

```PowerShell
Get-AzureADPasswordProtectionSummaryReport -DomainController bplrootdc2
DomainController                : bplrootdc2
PasswordChangesValidated        : 6677
PasswordSetsValidated           : 9
PasswordChangesRejected         : 10868
PasswordSetsRejected            : 34
PasswordChangeAuditOnlyFailures : 213
PasswordSetAuditOnlyFailures    : 3
PasswordChangeErrors            : 0
PasswordSetErrors               : 1
```

Het bereik van de rapportage van de cmdlet kan worden beïnvloed met behulp van een van de parameters-Forest, - domein of -DomainController. Een parameter niet opgeeft impliceert-Forest.

> [!NOTE]
> Deze cmdlet werkt door het openen van een PowerShell-sessie op elke domeincontroller. Als u wilt slagen, PowerShell-ondersteuning voor externe sessie moet zijn ingeschakeld op elke domeincontroller en de client moet voldoende bevoegdheden hebben. Voor meer informatie over vereisten voor PowerShell-sessie 'Get-Help about_Remote_Troubleshooting' worden uitgevoerd in een PowerShell-venster.

> [!NOTE]
> Deze cmdlet werkt volgens het gebeurtenislogboek voor elk DC agent-service beheer op afstand uitvoeren van query's. Als de gebeurtenislogboeken bevatten een groot aantal gebeurtenissen, kan de cmdlet lang duren om uit te voeren. Bulksgewijs netwerk query's van grote gegevenssets mogelijk ook van invloed op prestaties van domeincontrollers. Deze cmdlet moet daarom zorgvuldig worden gebruikt in een productieomgeving.

## <a name="dc-agent-discovery"></a>DC-agentdetectie

De `Get-AzureADPasswordProtectionDCAgent` cmdlet kan worden gebruikt om algemene informatie over de verschillende DC-agents die worden uitgevoerd in een domein of forest weer te geven. Deze informatie wordt opgehaald uit de serviceConnectionPoint-objecten die door de actieve DC-agent (s) zijn geregistreerd. Een voorbeeld van uitvoer van deze cmdlet is als volgt:

```PowerShell
Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
Heartbeat             : 2/16/2018 8:35:01 AM
```

De verschillende eigenschappen worden bijgewerkt door elke agent-service van de DC bij benadering gebeurt op uurbasis. De gegevens zijn nog steeds onderworpen aan replicatievertraging van Active Directory.

Het bereik van de query van de cmdlet kan worden beïnvloed met behulp van de parameters Forest of -domein.

## <a name="next-steps"></a>Volgende stappen

[Probleemoplossing en bewaking voor de beveiliging van Azure AD-wachtwoord](howto-password-ban-bad-on-premises-troubleshoot.md)
