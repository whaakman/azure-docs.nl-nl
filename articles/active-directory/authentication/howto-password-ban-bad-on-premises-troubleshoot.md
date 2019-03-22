---
title: Problemen oplossen in Azure AD-wachtwoordbeveiliging - Azure Active Directory
description: Inzicht in Azure AD wachtwoord beveiliging algemene probleemoplossing
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51f214688aa1e33bd58e8460baab75228d7c5d1a
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58317235"
---
# <a name="azure-ad-password-protection-troubleshooting"></a>Azure AD-wachtwoordbeveiliging oplossen

Na de implementatie van Azure AD-wachtwoord beveiliging, probleemoplossing mogelijk zijn vereist. In dit artikel gaat gedetailleerd inzicht in bepaalde algemene stappen voor probleemoplossing.

## <a name="the-dc-agent-cannot-locate-a-proxy-in-the-directory"></a>De DC-agent kan een proxy niet vinden in de map

De belangrijkste symptoom van dit probleem is 30017 gebeurtenissen in het gebeurtenislogboek van DC-agent-beheerder.

De gebruikelijke oorzaak van dit probleem is dat een proxy nog niet is geregistreerd. Als een proxy is geregistreerd, kunnen er enigszins vertraagd vanwege replicatievertraging van AD totdat een bepaalde DC-agent is proxyinstellingen zien.

## <a name="the-dc-agent-is-not-able-to-communicate-with-a-proxy"></a>De DC-agent is niet kan communiceren met een proxy

De belangrijkste symptoom van dit probleem is 30018 gebeurtenissen in het gebeurtenislogboek van DC-agent-beheerder. Dit kan verschillende oorzaken hebben:

1. De DC-agent bevindt zich in een geïsoleerde-gedeelte van het netwerk dat de netwerkverbinding met de geregistreerde proxy(s) niet toelaat. Dit probleem mogelijk daarom expected\benign, zolang andere DC-agents met de proxy(s) communiceren kunnen om te downloaden van beleid voor wachtwoorden van Azure, die vervolgens worden verkregen door de geïsoleerde DC via replicatie van de bestanden in de sysvol-share.

1. De proxy-hostmachine blokkeert de toegang tot het hulpprogramma voor het toewijzen van de RPC-eindpunt (poort 135)

   Het installatieprogramma van Azure AD-wachtwoord beveiliging Proxy maakt automatisch een inkomende Windows Firewall-regel waarmee de toegang tot poort 135. Als deze regel wordt later verwijderd of uitgeschakeld, worden DC agents kan niet communiceren met de Proxy-service. Als de ingebouwde Windows Firewall is uitgeschakeld in plaats van een ander firewallproduct, moet u de firewall voor toegang tot poort 135 configureren.

1. De proxy-hostmachine blokkeert de toegang tot de RPC-eindpunt (dynamisch of statisch) geluisterd op door de Proxy-service

   Het installatieprogramma van Azure AD-wachtwoord beveiliging Proxy maakt automatisch een Windows-Firewall binnenkomende regel waarmee toegang tot alle poorten voor inkomend verkeer hebben geluisterd naar door de Azure AD-wachtwoord beveiliging Proxy-service. Als deze regel wordt later verwijderd of uitgeschakeld, worden DC agents kan niet communiceren met de Proxy-service. Als de ingebouwde Windows Firewall is uitgeschakeld in plaats van een ander firewallproduct, moet u configureren dat firewall voor toegang tot alle poorten voor inkomend verkeer hebben geluisterd naar door de Azure AD-wachtwoord beveiliging Proxy-service. Deze configuratie kan worden gemaakt, specifiekere als de Proxy-service is geconfigureerd om te luisteren naar een specifieke statische RPC-poort (met behulp van de `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet).

## <a name="the-proxy-service-can-receive-calls-from-dc-agents-in-the-domain-but-is-unable-to-communicate-with-azure"></a>De Proxy-service kunt aanroepen ontvangen van DC-agents in het domein, maar kan niet communiceren met Azure

Zorg ervoor dat de proxy-computer is verbonden met de eindpunten die worden vermeld in de [implementatievereisten](howto-password-ban-bad-on-premises-deploy.md).

## <a name="the-dc-agent-is-unable-to-encrypt-or-decrypt-password-policy-files-and-other-state"></a>De DC-agent is kan niet versleutelen of ontsleutelen van bestanden van het wachtwoord en andere status

Dit probleem kan het manifest met tal van problemen, maar heeft meestal een veelvoorkomende oorzaak van de hoofdmap.

Azure AD-wachtwoord beveiliging heeft een kritieke afhankelijkheid van de versleuteling en ontsleuteling functionaliteit verstrekt door de Microsoft Key Distribution-Service is beschikbaar op domeincontrollers met Windows Server 2012 en hoger. De KDS-service moet zijn ingeschakeld en functioneel op alle Windows Server 2012 en hoger domeincontrollers in een domein.

Standaard de KDS is startmodus van service-service geconfigureerd als handmatige (Trigger starten). Deze configuratie betekent dat de eerste keer dat een client probeert de service, gebruiken deze is gestart op aanvraag. De startmodus van deze standaard-service is geschikt voor de beveiliging met Azure AD-wachtwoord.

Als de startmodus van de KDS-service is geconfigureerd op uitgeschakeld, kan deze configuratie moet worden opgelost voordat de beveiliging van Azure AD-wachtwoord werkt goed.

Een eenvoudige test voor dit probleem is de KDS-service, via de Service management MMC-console handmatig te starten of met behulp van andere hulpprogramma's voor service management (bijvoorbeeld 'net start kdssvc' vanaf een opdrachtprompt-console worden uitgevoerd). De KDS-service wordt gestart en blijven altijd werken verwacht.

De meest voorkomende oorzaak voor de KDS-service wordt kan niet worden gestart, is dat de Active Directory domain controller object bevindt zich buiten de standaard-domeincontrollers. Deze configuratie wordt niet ondersteund door de KDS-service en is niet beperkt tot die zijn opgelegd door Azure AD-wachtwoord beveiliging. De oplossing voor dit probleem is de domain controller object verplaatsen naar een locatie in de standaard-domeincontrollers.

## <a name="weak-passwords-are-being-accepted-but-should-not-be"></a>Zwakke wachtwoorden worden geaccepteerd, maar mag niet zijn

Dit probleem kan verschillende oorzaken hebben.

1. Uw DC-agent (s) kan een beleid niet downloaden of kan niet ontsleutelen van bestaande beleidsregels. Controleren op mogelijke oorzaken in de bovenstaande onderwerpen.

1. De modus wachtwoord beleid afdwingen is nog steeds ingesteld op Audit. Als deze configuratie van kracht is, deze opnieuw configureren op afdwingen met behulp van de portal van de beveiliging van Azure AD-wachtwoord. Zie [inschakelen wachtwoordbeveiliging](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

1. Het wachtwoordbeleid is uitgeschakeld. Als deze configuratie van kracht is, moet opnieuw worden ingeschakeld met behulp van de portal voor Azure AD-wachtwoord beveiliging configureren. Zie [inschakelen wachtwoordbeveiliging](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

1. U hebt de agentsoftware DC niet geïnstalleerd op alle domeincontrollers in het domein. In dit geval is het moeilijk zijn om ervoor te zorgen dat externe Windows-clients gericht op een bepaalde domeincontroller tijdens een wachtwoordwijziging. Als u hebt denkt dat u hebt een bepaalde domeincontroller waarop de software van DC-agent is geïnstalleerd is gericht, kunt u controleren door het gebeurtenislogboek van DC-agent admin dubbele: ongeacht resultaat, zullen er ten minste één gebeurtenis vastleggen van het resultaat van het wachtwoord validatie. Als er geen gebeurtenis aanwezig zijn voor de gebruiker waarvan het wachtwoord is gewijzigd, is klikt u vervolgens de wachtwoordwijziging waarschijnlijk verwerkt door een andere domeincontroller.

   Als een alternatieve test, proberen setting\changing wachtwoorden terwijl u aangemeld rechtstreeks naar een domeincontroller waarop de software van DC-agent is geïnstalleerd. Deze techniek wordt niet aanbevolen voor productie Active Directory-domeinen.

   Stapsgewijze implementatie van de DC-agentsoftware onderworpen aan deze beperkingen wordt ondersteund, wordt aangeraden de DC-agentsoftware zo snel mogelijk op alle domeincontrollers in een domein is geïnstalleerd.

1. Het algoritme voor gegevensvalidatie gebruikt wachtwoord daadwerkelijk werkt zoals verwacht. Zie [hoe wachtwoorden worden geëvalueerd](concept-password-ban-bad.md#how-are-passwords-evaluated).

## <a name="directory-services-repair-mode"></a>Directory Services Repair Mode

Als de domeincontroller wordt opgestart naar Directory Services Repair Mode, wordt de DC-agent-service detecteert deze voorwaarde en zorgt ervoor dat alle wachtwoordvalidatie of activiteiten worden uitgeschakeld, ongeacht de configuratie van het actieve beleid afdwingen.

## <a name="emergency-remediation"></a>Herstel voor noodgevallen

Als een situatie zich waar de DC-agentservice problemen veroorzaakt voordoet, kan de DC-agent-service worden onmiddellijk afgesloten. Filter-dll voor de DC-agent-wachtwoord nog steeds roept de service niet actief en waarschuwing gebeurtenissen (10012, 10013), maar alle binnenkomende wachtwoorden gedurende die tijd worden geaccepteerd. De DC-agent-service kan vervolgens ook worden geconfigureerd via de Windows-Service Control Manager met een opstarttype 'Uitgeschakeld' indien nodig.

Een andere meting voor herstel zou worden omgezet naar de modus inschakelen ingesteld op Nee in de portal voor Azure AD-wachtwoord beveiliging. Zodra het bijgewerkte beleid is gedownload, elke DC agent-service op een quiescent modus wanneer alle wachtwoorden worden geaccepteerd als gaat-is. Zie voor meer informatie, [modus afdwingen](howto-password-ban-bad-on-premises-operations.md#enforce-mode).

## <a name="domain-controller-demotion"></a>Degradatie van domeincontrollers

Als u het verlagen van een domeincontroller waarop de DC-agentsoftware nog steeds wordt ondersteund. Beheerders moeten rekening houden echter dat de DC-agentsoftware blijft het huidige beleid worden afgedwongen tijdens de degradatie procedure. Nieuwe lokale wachtwoord van de beheerder (opgegeven als onderdeel van het degraderen) wordt gevalideerd, zoals elk ander wachtwoord. Microsoft raadt aan dat veilige wachtwoorden voor lokale Administrator-accounts worden gekozen als onderdeel van een domeincontroller degraderen procedure; maar de validatie van nieuwe lokale wachtwoord van de beheerder van de DC-agentsoftware mogelijk verstorend voor reeds bestaande degradatie operationele procedures.

Zodra de degradatie is voltooid en de domeincontroller opnieuw is opgestart en wordt opnieuw uitgevoerd als een normale lidserver, wordt de DC-agentsoftware die wordt uitgevoerd in een passieve modus. Deze kan vervolgens worden verwijderd op elk gewenst moment.

## <a name="removal"></a>Verwijdering

Als de software voor de beveiliging van Azure AD wachtwoord en opschonen alle gerelateerde status verwijderen uit de domeinen en forest wordt besloten, kan deze taak worden uitgevoerd met behulp van de volgende stappen uit:

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

4. Verwijder handmatig alle connection points van de DC-agent in elke naamgevingscontext voor domein. Er kan ook een deze objecten per domeincontroller in het forest, afhankelijk van hoe ver de software is geïmplementeerd. De locatie van het object kan worden gedetecteerd met de volgende Active Directory PowerShell-opdracht:

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
