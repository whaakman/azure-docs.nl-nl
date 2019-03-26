---
title: Controle en logboekregistratie in Azure AD-wachtwoord beveiliging - Azure Active Directory
description: Inzicht in Azure AD-wachtwoord beveiliging, controle en logboekregistratie
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
ms.openlocfilehash: ae2d18541788e769e4f1b44319aa1be200921b88
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58437540"
---
# <a name="azure-ad-password-protection-monitoring-and-logging"></a>Azure AD-wachtwoordbeveiliging bewaking en logboekregistratie

Zijn essentiële taken na de implementatie van Azure AD-wachtwoord beveiliging, controle en rapportage. In dit artikel gaat in detail om te begrijpen van verschillende bewaking technieken, inclusief waar elke service registreert informatie en hoe u om te rapporteren over het gebruik van Azure AD-wachtwoord beveiliging.

Controle en rapportage moeten worden uitgevoerd door gebeurtenislogboekberichten of door het uitvoeren van PowerShell-cmdlets. De DC-agent en -proxy services beide logboek gebeurtenislogboekberichten. Alle PowerShell-cmdlets die hieronder worden beschreven, zijn alleen beschikbaar op de proxyserver (Zie de AzureADPasswordProtection PowerShell-module). De DC-agentsoftware een PowerShell-module niet geïnstalleerd.

## <a name="dc-agent-event-logging"></a>Logboekregistratie van DC-agent

Op elke domeincontroller schrijft de DC-agentsoftware service de resultaten van elke afzonderlijke wachtwoord validatie-bewerking (en andere status) naar een lokale gebeurtenislogboek:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

De beheerder logboek van de DC-agent is de primaire bron van informatie over hoe de software wordt gedragen.

Houd er rekening mee dat het traceerlogboek standaard uitgeschakeld is.

Gebeurtenissen vastgelegd door de verschillende onderdelen van de DC-agent kunnen worden onderverdeeld in de volgende bereiken:

|Onderdeel |Gebeurtenis-ID-bereik|
| --- | --- |
|DC-Agent wachtwoord filter dll-bestand| 10000-19999|
|Hostproces van de DC-agent-service| 20000-29999|
|Validatielogica van DC-agent-service-beleid| 30000-39999|

## <a name="dc-agent-admin-event-log"></a>DC-agent Admin-gebeurtenislogboek

### <a name="password-validation-outcome-events"></a>Wachtwoord validatie resultaat gebeurtenissen

Op elke domeincontroller schrijft de DC-agentsoftware service de resultaten van elke afzonderlijke wachtwoordvalidatie naar het gebeurtenislogboek van DC-agent-beheerder.

Voor een geslaagde bewerking voor validatie, er is doorgaans een gebeurtenis vastgelegd van de DC-agent wachtwoord filter-dll. Voor een mislukte validatie-bewerking wachtwoord, er zijn doorgaans twee gebeurtenissen vastgelegd, één van de DC-agent-service en één van de DC Agent wachtwoord filter-dll-bestand.

Afzonderlijke gebeurtenissen vast te leggen van deze situaties worden vastgelegd, gebaseerd op de volgende factoren:

* Of een bepaald wachtwoord wordt ingesteld of gewijzigd.
* Validatie van een bepaald wachtwoord doorgegeven of is mislukt.
* Of validatie is mislukt vanwege het globale beleid van Microsoft, het organisatiebeleid of een combinatie.
* Of modus alleen controleren is op dit moment in- of uitschakelen voor de huidige wachtwoordbeleid.

De sleutel wachtwoord-validatie-gerelateerde gebeurtenissen zijn als volgt:

|   |Wachtwoord wijzigen |Wachtwoorden in te stellen|
| --- | :---: | :---: |
|Geslaagd |10014 |10015|
|Mislukt (als gevolg van de klant wachtwoordbeleid)| 10016, 30002| 10017, 30003|
|Mislukt (als gevolg van Microsoft-wachtwoordbeleid)| 10016, 30004| 10017, 30005|
|Mislukt (als gevolg van gecombineerde Microsoft en de klant wachtwoordbeleid)| 10016, 30026| 10017, 30027|
|Alleen controle Pass (zou niet hebben klanten wachtwoordbeleid)| 10024, 30008| 10025, 30007|
|Alleen controle Pass (zou niet hebben Microsoft wachtwoordbeleid)| 10024, 30010| 10025, 30009|
|Alleen controle Pass (zou niet hebben gecombineerde wachtwoordbeleid van Microsoft en de klant)| 10024, 30028| 10025, 30029|

De aanvragen in de bovenstaande tabel die naar 'gecombineerde beleid verwijzen' verwijzen naar situaties waarin het wachtwoord van een gebruiker bevat ten minste één token uit de lijst met Microsoft uitgesloten wachtwoorden en de lijst met uitgesloten klant wachtwoorden is gevonden.

Wanneer een combinatie van gebeurtenissen bij elkaar wordt geregistreerd, worden beide gebeurtenissen expliciet door de dezelfde CorrelationId gekoppeld.

### <a name="password-validation-summary-reporting-via-powershell"></a>Wachtwoord Validatieoverzicht melden via PowerShell

De `Get-AzureADPasswordProtectionSummaryReport` cmdlet kan worden gebruikt voor het produceren van een overzicht van de Standaardactiviteit voor groepsvalidatie voor wachtwoord. Een voorbeeld van uitvoer van deze cmdlet is als volgt:

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

De `Get-AzureADPasswordProtectionSummaryReport` cmdlet werkt volgens het opvragen van het gebeurtenislogboek van DC-agent-beheerder en vervolgens het tellen van het totale aantal gebeurtenissen die met elke categorie weergegeven resultaat overeenkomen. De volgende tabel bevat de toewijzingen tussen elk resultaat en de bijbehorende gebeurtenis-id:

|Get-AzureADPasswordProtectionSummaryReport eigenschap |Corresponderende gebeurtenis-id|
| :---: | :---: |
|PasswordChangesValidated |10014|
|PasswordSetsValidated |10015|
|PasswordChangesRejected |10016|
|PasswordSetsRejected |10017|
|PasswordChangeAuditOnlyFailures |10024|
|PasswordSetAuditOnlyFailures |10025|
|PasswordChangeErrors |10012|
|PasswordSetErrors |10013|

Houd er rekening mee dat de `Get-AzureADPasswordProtectionSummaryReport` cmdlet in PowerShell-script formulier is verzonden en indien nodig mei worden verwezen naar rechtstreeks op de volgende locatie:

`%ProgramFiles%\WindowsPowerShell\Modules\AzureADPasswordProtection\Get-AzureADPasswordProtectionSummaryReport.ps1`

> [!NOTE]
> Deze cmdlet werkt door het openen van een PowerShell-sessie op elke domeincontroller. Als u wilt slagen, PowerShell-ondersteuning voor externe sessie moet zijn ingeschakeld op elke domeincontroller en de client moet voldoende bevoegdheden hebben. Voor meer informatie over vereisten voor PowerShell-sessie 'Get-Help about_Remote_Troubleshooting' worden uitgevoerd in een PowerShell-venster.

> [!NOTE]
> Deze cmdlet werkt volgens het gebeurtenislogboek voor elk DC agent-service beheer op afstand uitvoeren van query's. Als de gebeurtenislogboeken bevatten een groot aantal gebeurtenissen, kan de cmdlet lang duren om uit te voeren. Bulksgewijs netwerk query's van grote gegevenssets mogelijk ook van invloed op prestaties van domeincontrollers. Deze cmdlet moet daarom zorgvuldig worden gebruikt in een productieomgeving.

### <a name="sample-event-log-message-for-event-id-10014-successful-password-change"></a>Bericht van de voorbeeld-gebeurtenislogboek voor gebeurtenis-ID 10014 (geslaagde wachtwoordwijziging)

```text
The changed password for the specified user was validated as compliant with the current Azure password policy.

 UserName: BPL_02885102771
 FullName:
```

### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Bericht van de voorbeeld-gebeurtenislogboek voor gebeurtenis-ID 10017 en 30003 (mislukte wachtwoorden in te stellen)

10017:

```text
The reset password for the specified user was rejected because it did not comply with the current Azure password policy. Please see the correlated event log message for more details.

 UserName: BPL_03283841185
 FullName:
```

30003:

```text
The reset password for the specified user was rejected because it matched at least one of the tokens present in the per-tenant banned password list of the current Azure password policy.

 UserName: BPL_03283841185
 FullName:
```

### <a name="sample-event-log-message-for-event-id-30001-password-accepted-due-to-no-policy-available"></a>Bericht van de voorbeeld-gebeurtenislogboek voor gebeurtenis-ID 30001 (wachtwoord geaccepteerd omdat er geen beleid beschikbaar)

```text
The password for the specified user was accepted because an Azure password policy is not available yet

UserName: SomeUser
FullName: Some User

This condition may be caused by one or more of the following reasons:%n

1. The forest has not yet been registered with Azure.

   Resolution steps: an administrator must register the forest using the Register-AzureADPasswordProtectionForest cmdlet.

2. An Azure AD password protection Proxy is not yet available on at least one machine in the current forest.

   Resolution steps: an administrator must install and register a proxy using the Register-AzureADPasswordProtectionProxy cmdlet.

3. This DC does not have network connectivity to any Azure AD password protection Proxy instances.

   Resolution steps: ensure network connectivity exists to at least one Azure AD password protection Proxy instance.

4. This DC does not have connectivity to other domain controllers in the domain.

   Resolution steps: ensure network connectivity exists to the domain.
```

### <a name="sample-event-log-message-for-event-id-30006-new-policy-being-enforced"></a>Bericht van de voorbeeld-gebeurtenislogboek voor gebeurtenis-ID 30006 (het nieuwe beleid wordt afgedwongen)

```text
The service is now enforcing the following Azure password policy.

 Enabled: 1
 AuditOnly: 1
 Global policy date: ‎2018‎-‎05‎-‎15T00:00:00.000000000Z
 Tenant policy date: ‎2018‎-‎06‎-‎10T20:15:24.432457600Z
 Enforce tenant policy: 1
```

### <a name="sample-event-log-message-for-event-id-30019-azure-ad-password-protection-is-disabled"></a>Bericht van de voorbeeld-gebeurtenislogboek voor gebeurtenis-ID 30019 (Azure AD-wachtwoord beveiliging is uitgeschakeld)

```text
The most recently obtained Azure password policy was configured to be disabled. All passwords submitted for validation from this point on will automatically be considered compliant with no processing performed.

No further events will be logged until the policy is changed.%n

```

## <a name="dc-agent-operational-log"></a>DC-Agent operationele logboek

De DC-agent-service wordt ook operationele met betrekking tot gebeurtenissen registreren bij het volgende logboekbestand:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

## <a name="dc-agent-trace-log"></a>Traceerlogboek van DC-Agent

De DC-agent-service kan zich ook aanmelden uitgebreide foutopsporingsniveau traceringsgebeurtenissen voor het volgende logboekbestand:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

Traceerlogboekregistratie is standaard uitgeschakeld.

> [!WARNING]
> Wanneer dit is ingeschakeld, wordt het traceerlogboek ontvangt van een groot aantal gebeurtenissen en mogelijk van invloed op prestaties van domeincontrollers. Daarom dit uitgebreide logboek moet alleen worden ingeschakeld wanneer een probleem is vereist voor nader onderzoek en vervolgens alleen voor een minimale hoeveelheid tijd.

## <a name="dc-agent-text-logging"></a>Logboekregistratie in een tekstbestand DC-Agent

De DC-agent-service kan worden geconfigureerd om te schrijven naar een logboek met tekst door in te stellen van de volgende registerwaarde:

```text
HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionDCAgent\Parameters!EnableTextLogging = 1 (REG_DWORD value)
```

Logboekregistratie in een tekstbestand is standaard uitgeschakeld. Het opnieuw opstarten van de DC-agent-service is vereist voor wijzigingen in deze waarde pas van kracht. Wanneer de domeincontroller ingeschakeld agent-service worden geschreven naar een logboekbestand bevindt zich onder:

`%ProgramFiles%\Azure AD Password Protection DC Agent\Logs`

> [!TIP]
> De tekstlogboek ontvangt de dezelfde foutopsporingsniveau vermeldingen die naar het traceerlogboek kunnen worden geregistreerd, maar is doorgaans in een indeling gemakkelijker om te controleren en analyseren.

> [!WARNING]
> Wanneer dit is ingeschakeld, wordt dit logboek ontvangt van een groot aantal gebeurtenissen en mogelijk van invloed op prestaties van domeincontrollers. Daarom dit uitgebreide logboek moet alleen worden ingeschakeld wanneer een probleem is vereist voor nader onderzoek en vervolgens alleen voor een minimale hoeveelheid tijd.

## <a name="dc-agent-performance-monitoring"></a>Prestatiebewaking voor DC-agent

De DC-agent-service-software wordt geïnstalleerd voor een prestatiemeteritem-object met de naam **Protection voor Azure AD-wachtwoord**. De volgende prestatiemeteritems zijn momenteel beschikbaar:

|Naam van het prestatiemeteritem voor prestaties | Description|
| --- | --- |
|Wachtwoorden verwerkt |Deze teller geeft het totale aantal wachtwoorden verwerkt (geaccepteerd of geweigerd) sinds de laatste keer opnieuw opstarten.|
|Wachtwoorden geaccepteerd |Deze teller geeft het totale aantal wachtwoorden die zijn geaccepteerd sinds de laatste keer opnieuw opstarten.|
|Wachtwoorden geweigerd |Deze teller geeft het totale aantal wachtwoorden dat sinds de laatste keer opnieuw opstarten is geweigerd.|
|Wachtwoord filteren aanvragen in uitvoering |Dit item geeft het aantal aanvragen voor wachtwoord filteren op dit moment wordt uitgevoerd.|
|Piek wachtwoord filteren aanvragen |Deze teller geeft het hoogste aantal gelijktijdige wachtwoord filteren aanvragen sinds de laatste keer opnieuw opstarten.|
|Wachtwoord filter aanvraag fouten |Deze teller geeft het totale aantal aanvragen van een wachtwoord filteren dat is mislukt vanwege een fout sinds de laatste keer opnieuw opstarten. Fouten kunnen optreden wanneer de Azure AD-wachtwoord DC Protection agent-service niet wordt uitgevoerd.|
|Wachtwoord filteren aanvragen/sec |Deze teller geeft de snelheid waarmee wachtwoorden worden verwerkt.|
|Wachtwoord filter aanvraag verwerkingstijd |Dit item geeft de gemiddelde tijd die nodig is om een wachtwoord filter-aanvraag te verwerken.|
|Piek wachtwoord filter-aanvraagtijd verwerking |Dit item geeft de piek wachtwoord filter aanvraagverwerking tijd sinds de laatste keer opnieuw opstarten.|
|Wachtwoorden geaccepteerd vanwege modus controleren |Deze teller geeft het totale aantal wachtwoorden die zouden normaal geweigerd, maar zijn geaccepteerd omdat het wachtwoordbeleid is geconfigureerd om te worden in de controlemodus (sinds de laatste keer opnieuw opstarten).|

## <a name="dc-agent-discovery"></a>DC-agentdetectie

De `Get-AzureADPasswordProtectionDCAgent` cmdlet kan worden gebruikt om algemene informatie over de verschillende DC-agents die worden uitgevoerd in een domein of forest weer te geven. Deze informatie wordt opgehaald uit de serviceConnectionPoint-objecten die door de actieve DC-agent (s) zijn geregistreerd.

Een voorbeeld van uitvoer van deze cmdlet is als volgt:

```PowerShell
Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
PasswordPolicyDateUTC : 2/16/2018 8:35:01 AM
HeartbeatUTC          : 2/16/2018 8:35:02 AM
```

De verschillende eigenschappen worden bijgewerkt door elke agent-service van de DC bij benadering gebeurt op uurbasis. De gegevens zijn nog steeds onderworpen aan replicatievertraging van Active Directory.

Het bereik van de query van de cmdlet kan worden beïnvloed met behulp van de parameters Forest of -domein.

Als de waarde HeartbeatUTC afkomstig is verlopen, kan dit een erop duiden dat de Azure AD-wachtwoord beveiliging DC-Agent op de domeincontroller wordt niet uitgevoerd of is verwijderd of de machine is gedegradeerd en is niet langer een domeincontroller.

Als de waarde opgehaald voor PasswordPolicyDateUTC verlopen, kan dit een erop duiden dat de Azure AD-wachtwoord-DC-beveiligingsagent op deze machine is niet goed werkt.

## <a name="proxy-service-event-logging"></a>Logboekregistratie van Proxy-service

De Proxy-service verzendt een minimale set gebeurtenissen naar de volgende Logboeken:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace`

Houd er rekening mee dat het traceerlogboek standaard uitgeschakeld is.

> [!WARNING]
> Wanneer dit is ingeschakeld, het traceerlogboek een groot aantal gebeurtenissen ontvangt en dit kan van invloed op prestaties van de proxyhost. Daarom dit logboek moet alleen worden ingeschakeld wanneer een probleem is vereist voor nader onderzoek en vervolgens alleen voor een minimale hoeveelheid tijd.

Gebeurtenissen worden vastgelegd door de diverse onderdelen van een Proxy met behulp van de volgende bereiken:

|Onderdeel |Gebeurtenis-ID-bereik|
| --- | --- |
|Hostproces voor Proxy-service| 10000-19999|
|Proxy-service core zakelijke logica| 20000-29999|
|PowerShell-cmdlets| 30000-39999|

## <a name="proxy-service-text-logging"></a>Logboekregistratie in een tekstbestand Proxy-service

De Proxy-service kan worden geconfigureerd om te schrijven naar een logboek met tekst door in te stellen van de volgende registerwaarde:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionProxy\Parameters!EnableTextLogging = 1 (REG_DWORD value)

Logboekregistratie in een tekstbestand is standaard uitgeschakeld. Het opnieuw opstarten van de Proxy-service is vereist voor wijzigingen in deze waarde pas van kracht. Wanneer de service worden geschreven naar een logboekbestand bevindt zich onder Proxy ingeschakeld:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

> [!TIP]
> De tekstlogboek ontvangt de dezelfde foutopsporingsniveau vermeldingen die naar het traceerlogboek kunnen worden geregistreerd, maar is doorgaans in een indeling gemakkelijker om te controleren en analyseren.

> [!WARNING]
> Wanneer dit is ingeschakeld, wordt dit logboek ontvangt van een groot aantal gebeurtenissen en mogelijk van invloed op de prestaties van de machine. Daarom dit uitgebreide logboek moet alleen worden ingeschakeld wanneer een probleem is vereist voor nader onderzoek en vervolgens alleen voor een minimale hoeveelheid tijd.

## <a name="powershell-cmdlet-logging"></a>PowerShell-cmdlet-logboekregistratie

PowerShell-cmdlets die leiden tot een wijziging in de status (bijvoorbeeld, Register-AzureADPasswordProtectionProxy) wordt normaal gesproken een gebeurtenis resultaat wordt vastgelegd in het operationele logboek.

De meeste van de Azure AD-wachtwoord Protection PowerShell-cmdlets wordt bovendien schrijven naar een tekstlogboek bevindt zich onder:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

Als een cmdlet-fout optreedt en de oorzaak en/of-oplossing niet direct duidelijk is, kunnen deze tekstlogboeken ook worden geraadpleegd.

## <a name="proxy-discovery"></a>Proxy-detectie

De `Get-AzureADPasswordProtectionProxy` cmdlet kan worden gebruikt om algemene informatie over de verschillende Azure AD-wachtwoord beveiliging Proxy-services die worden uitgevoerd in een domein of forest weer te geven. Deze informatie wordt opgehaald uit de serviceConnectionPoint-objecten die door de actieve Proxy (s) zijn geregistreerd.

Een voorbeeld van uitvoer van deze cmdlet is als volgt:

```PowerShell
Get-AzureADPasswordProtectionProxy
ServerFQDN            : bplProxy.bplchild2.bplRootDomain.com
Domain                : bplchild2.bplRootDomain.com
Forest                : bplRootDomain.com
HeartbeatUTC          : 12/25/2018 6:35:02 AM
```

De verschillende eigenschappen worden bijgewerkt door elke service Proxy bij benadering gebeurt op uurbasis. De gegevens zijn nog steeds onderworpen aan replicatievertraging van Active Directory.

Het bereik van de query van de cmdlet kan worden beïnvloed met behulp van de parameters Forest of -domein.

Als de waarde HeartbeatUTC afkomstig is verlopen, kan dit een erop duiden dat de Proxy van de bescherming van Azure AD-wachtwoord op die computer wordt niet uitgevoerd of is verwijderd.

## <a name="next-steps"></a>Volgende stappen

[Probleemoplossing voor Azure AD-wachtwoord](howto-password-ban-bad-on-premises-troubleshoot.md)

Zie het artikel voor meer informatie over de algemene en aangepaste uitgesloten wachtwoordenlijsten [onjuiste wachtwoorden blokkeren](concept-password-ban-bad.md)
