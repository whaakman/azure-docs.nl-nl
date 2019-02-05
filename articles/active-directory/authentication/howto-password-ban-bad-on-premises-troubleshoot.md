---
title: Problemen oplossen in Azure AD wachtwoord protection preview
description: Inzicht in Azure AD wachtwoord protection preview algemene problemen oplossen
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 7474027368949d5ad2202881ac68096fac2b8bd2
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55693901"
---
# <a name="preview-azure-ad-password-protection-troubleshooting"></a>Preview: Azure AD-wachtwoordbeveiliging oplossen

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Na de implementatie van Azure AD-wachtwoord beveiliging, probleemoplossing mogelijk zijn vereist. In dit artikel gaat gedetailleerd inzicht in bepaalde algemene stappen voor probleemoplossing.

## <a name="weak-passwords-are-not-getting-rejected-as-expected"></a>Zwakke wachtwoorden zijn niet ophalen van geweigerd zoals verwacht

Dit kan verschillende oorzaken hebben:

1. De DC-agent (s) nog niet een beleid gedownload. De als gevolg hiervan is 30001 gebeurtenissen in het gebeurtenislogboek van DC-agent-beheerder.

    Mogelijke oorzaken voor dit probleem zijn:

    1. Forest nog niet is geregistreerd
    2. Proxy nog niet is geregistreerd
    3. Problemen met de netwerkverbinding zijn zo wordt voorkomen dat de Proxy-service communiceert met Azure (vereisten controleren HTTP-Proxy)

2. De modus wachtwoord beleid afdwingen is nog steeds ingesteld op Audit. Als dit het geval is, deze opnieuw configureren op afdwingen met behulp van de portal van de beveiliging van Azure AD-wachtwoord. Raadpleeg [inschakelen wachtwoordbeveiliging](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

3. Het wachtwoordbeleid is uitgeschakeld. Als dit het geval is, moet opnieuw worden ingeschakeld met behulp van de portal voor Azure AD-wachtwoord beveiliging configureren. Raadpleeg [inschakelen wachtwoordbeveiliging](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

4. Het algoritme voor gegevensvalidatie gebruikt wachtwoord werkt zoals verwacht. Raadpleeg [hoe wachtwoorden worden geëvalueerd](concept-password-ban-bad.md#how-are-passwords-evaluated).

## <a name="directory-services-repair-mode"></a>Directory Services Repair Mode

Als de domeincontroller wordt opgestart naar Directory Services Repair Mode, wordt de DC-agent-service detecteert DPM dit en zorgt ervoor dat alle wachtwoordvalidatie of activiteiten worden uitgeschakeld, ongeacht de configuratie van het actieve beleid afdwingen.

## <a name="emergency-remediation"></a>Herstel voor noodgevallen

Als een situatie zich waar de DC-agentservice problemen veroorzaakt voordoet, kan de DC-agent-service worden onmiddellijk afgesloten. Filter-dll voor de DC-agent-wachtwoord nog steeds roept de service niet actief en waarschuwing gebeurtenissen (10012, 10013), maar alle binnenkomende wachtwoorden gedurende die tijd worden geaccepteerd. De DC-agent-service kan vervolgens ook worden geconfigureerd via de Windows-Service Control Manager met een opstarttype 'Uitgeschakeld' indien nodig.

Een andere meting voor herstel zou worden omgezet naar de modus inschakelen ingesteld op Nee in de portal voor Azure AD-wachtwoord beveiliging. Zodra het bijgewerkte beleid is gedownload, elke DC agent-service op een quiescent modus wanneer alle wachtwoorden worden geaccepteerd als gaat-is. Zie voor meer informatie, [modus afdwingen](howto-password-ban-bad-on-premises-operations.md#enforce-mode).

## <a name="domain-controller-demotion"></a>Degradatie van domeincontrollers

Als u het verlagen van een domeincontroller waarop de DC-agentsoftware nog steeds wordt ondersteund. Beheerders moeten rekening houden echter dat de DC-agentsoftware blijft het huidige beleid worden afgedwongen tijdens de degradatie procedure. Nieuwe lokale wachtwoord van de beheerder (opgegeven als onderdeel van het degraderen) wordt gevalideerd, zoals elk ander wachtwoord. Microsoft raadt aan dat veilige wachtwoorden voor lokale Administrator-accounts worden gekozen als onderdeel van een domeincontroller degraderen procedure; maar de validatie van nieuwe lokale wachtwoord van de beheerder van de DC-agentsoftware mogelijk verstorend voor reeds bestaande degradatie operationele procedures.

Zodra de degradatie is voltooid en de domeincontroller opnieuw is opgestart en wordt opnieuw uitgevoerd als een normale lidserver, wordt de DC-agentsoftware die wordt uitgevoerd in een passieve modus. Deze kan vervolgens worden verwijderd op elk gewenst moment.

## <a name="removal"></a>Verwijdering

Als deze wordt besloten dat u wilt de openbare preview-software en opschonen alle gerelateerde status verwijderen uit de domeinen en forests, kan deze taak worden uitgevoerd met behulp van de volgende stappen uit:

> [!IMPORTANT]
> Het is belangrijk dat u deze stappen in volgorde uitvoeren. Als een willekeurig exemplaar van de Proxy-service actief wordt periodiek opnieuw gemaakt het object serviceConnectionPoint. Als een willekeurig exemplaar van de DC-agent-service actief wordt periodiek opnieuw gemaakt het object serviceConnectionPoint en de sysvol-status.

1. Verwijder de Proxy-software van alle computers. Deze stap wordt **niet** moeten worden opgestart.
2. Verwijder de DC-agentsoftware van alle domeincontrollers. Deze stap **vereist** opnieuw worden opgestart.
3. Verwijder handmatig alle service connection points van Proxy in elke naamgevingscontext voor domein. De locatie van deze objecten kan worden gedetecteerd met de volgende Active Directory PowerShell-opdracht:

   ```PowerShell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Voer het sterretje niet weglaten ("*") aan het einde van de variabele waarde $keywords.

   De resulterende objecten gevonden de `Get-ADObject` opdracht kan worden doorgesluisd naar `Remove-ADObject`, of handmatig verwijderd. 

4. Verwijder handmatig alle connection points van de DC-agent in elke naamgevingscontext voor domein. Er kan ook een deze objecten per domeincontroller in het forest, afhankelijk van hoe ver de openbare preview-software is geïmplementeerd. De locatie van het object kan worden gedetecteerd met de volgende Active Directory PowerShell-opdracht:

   ```PowerShell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   De resulterende objecten gevonden de `Get-ADObject` opdracht kan worden doorgesluisd naar `Remove-ADObject`, of handmatig verwijderd.

   Voer het sterretje niet weglaten ("*") aan het einde van de variabele waarde $keywords.

5. Verwijder handmatig de status van de configuratie van de op forestniveau. De status van het forest-configuratie wordt bijgehouden in een container in de configuratienaamgevingscontext van Active Directory. Het kan worden gedetecteerd en verwijderd als volgt:

   ```PowerShell
   $passwordProtectionConfigContainer = "CN=Azure AD Password Protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject -Recursive $passwordProtectionConfigContainer
   ```

6. Handmatig verwijderen verwijdert alle sysvol status door handmatig van de volgende map en alle bijbehorende inhoud:

   `\\<domain>\sysvol\<domain fqdn>\AzureADPasswordProtection`

   Indien nodig, kan dit pad ook worden geopend lokaal op een bepaalde domeincontroller. de standaardlocatie is ongeveer het volgende pad:

   `%windir%\sysvol\domain\Policies\AzureADPasswordProtection`

   Dit pad verschilt als de sysvol-share op de locatie van een niet-standaard is geconfigureerd.

## <a name="next-steps"></a>Volgende stappen

[Veelgestelde vragen over Azure AD-wachtwoord beveiliging](howto-password-ban-bad-on-premises-faq.md)

Zie het artikel voor meer informatie over de algemene en aangepaste uitgesloten wachtwoordenlijsten [onjuiste wachtwoorden blokkeren](concept-password-ban-bad.md)
