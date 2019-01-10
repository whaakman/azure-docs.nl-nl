---
title: Oplossen van problemen en logboekregistratie in Azure AD wachtwoord protection preview
description: Inzicht in Azure AD-wachtwoordbeveiliging logboekregistratie en het oplossen van algemene preview
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: d9f3ba642a5d00594aa6bdef597d5db43c2fc121
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188977"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Preview: Azure AD wachtwoord beveiliging bewaking, rapportage en probleemoplossing

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Zijn essentiële taken na de implementatie van Azure AD wachtwoordbeveiliging, controle en rapportage. In dit artikel gaat in detail om u te helpen dat u leert waar elke service registreert informatie en hoe u een rapport over het gebruik van Azure AD-wachtwoordbeveiliging.

## <a name="on-premises-logs-and-events"></a>On-premises logboeken en gebeurtenissen

### <a name="dc-agent-admin-log"></a>DC agent admin-logboek

Op elke domeincontroller schrijft de DC-agentsoftware service de resultaten van de validaties wachtwoord (en andere status) naar een lokale gebeurtenislogboek:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin`

Gebeurtenissen worden vastgelegd door de verschillende DC-agent-onderdelen met behulp van de volgende bereiken:

|Onderdeel |Gebeurtenis-ID-bereik|
| --- | --- |
|DC-Agent wachtwoord filter dll-bestand| 10000 19999|
|Hostproces van de DC-agent-service| 20000-29999|
|Validatielogica van DC-agent-service-beleid| 30000 39999|

Voor een geslaagde bewerking voor validatie, er is doorgaans een gebeurtenis vastgelegd van de DC-agent wachtwoord filter-dll. Voor een mislukte validatie-bewerking wachtwoord, er zijn doorgaans twee gebeurtenissen vastgelegd, één van de DC-agent-service en één van de DC Agent wachtwoord filter-dll-bestand.

Afzonderlijke gebeurtenissen vast te leggen van deze situaties worden vastgelegd, gebaseerd op de volgende factoren:

* Of een bepaald wachtwoord wordt ingesteld of gewijzigd.
* Validatie van een bepaald wachtwoord doorgegeven of is mislukt.
* Of validatie is mislukt vanwege het globale beleid Microsoft versus het organisatiebeleid.
* Of modus alleen controleren is op dit moment in- of uitschakelen voor de huidige wachtwoordbeleid.

De sleutel wachtwoord-validatie-gerelateerde gebeurtenissen zijn als volgt:

|   |Wachtwoord wijzigen |Wachtwoorden in te stellen|
| --- | :---: | :---: |
|Geslaagd |10014 |10015|
|Mislukt (is niet geslaagd klant wachtwoordbeleid)| 10016, 30002| 10017, 30003|
|Mislukt (is niet geslaagd voor Microsoft-wachtwoordbeleid)| 10016, 30004| 10017, 30005|
|Alleen controle Pass (zou niet hebben klanten wachtwoordbeleid)| 10024, 30008| 10025, 30007|
|Alleen controle Pass (zou niet hebben Microsoft wachtwoordbeleid)| 10024, 30010| 10025, 30009|

> [!TIP]
> Binnenkomende wachtwoorden worden gevalideerd aan de hand van de lijst met Microsoft globale wachtwoord eerst; Als dat mislukt, wordt er is geen verdere verwerking uitgevoerd. Dit is hetzelfde gedrag omdat van wachtwoordwijzigingen in Azure wordt uitgevoerd.

#### <a name="sample-event-log-message-for-event-id-10014-successful-password-set"></a>Voorbeeld van bericht van gebeurtenislogboek voor gebeurtenis-ID 10014 (geslaagde wachtwoorden in te stellen)

```
The changed password for the specified user was validated as compliant with the current Azure password policy.

 UserName: BPL_02885102771
 FullName:
```

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Bericht van de voorbeeld-gebeurtenislogboek voor gebeurtenis-ID 10017 en 30003 (mislukte wachtwoorden in te stellen)

10017:

```
The reset password for the specified user was rejected because it did not comply with the current Azure password policy. Please see the correlated event log message for more details.

 UserName: BPL_03283841185
 FullName:
```

30003:

```
The reset password for the specified user was rejected because it matched at least one of the tokens present in the per-tenant banned password list of the current Azure password policy.

 UserName: BPL_03283841185
 FullName:
```

#### <a name="sample-event-log-message-for-event-id-30001-password-accepted-due-to-no-policy-available"></a>Bericht van de voorbeeld-gebeurtenislogboek voor gebeurtenis-ID 30001 (wachtwoord geaccepteerd omdat er geen beleid beschikbaar)

```
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

#### <a name="sample-event-log-message-for-event-id-30006-new-policy-being-enforced"></a>Bericht van de voorbeeld-gebeurtenislogboek voor gebeurtenis-ID 30006 (het nieuwe beleid wordt afgedwongen)

```
The service is now enforcing the following Azure password policy.

 Enabled: 1
 AuditOnly: 1
 Global policy date: ‎2018‎-‎05‎-‎15T00:00:00.000000000Z
 Tenant policy date: ‎2018‎-‎06‎-‎10T20:15:24.432457600Z
 Enforce tenant policy: 1
```

#### <a name="dc-agent-operational-log"></a>DC-Agent operationele logboek

De DC-agent-service wordt ook operationele met betrekking tot gebeurtenissen registreren bij het volgende logboekbestand:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

#### <a name="dc-agent-trace-log"></a>Traceerlogboek van DC-Agent

De DC-agent-service kan zich ook aanmelden uitgebreide foutopsporingsniveau traceringsgebeurtenissen voor het volgende logboekbestand:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

Traceerlogboekregistratie is standaard uitgeschakeld.

> [!WARNING]
>  Wanneer dit is ingeschakeld, wordt het traceerlogboek ontvangt van een groot aantal gebeurtenissen en mogelijk van invloed op prestaties van domeincontrollers. Daarom dit uitgebreide logboek moet alleen worden ingeschakeld wanneer een probleem is vereist voor nader onderzoek en vervolgens alleen voor een minimale hoeveelheid tijd.

#### <a name="dc-agent-text-logging"></a>Logboekregistratie in een tekstbestand DC-Agent

De DC-agent-service kan worden geconfigureerd om te schrijven naar een logboek met tekst door in te stellen van de volgende registerwaarde:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionDCAgent\Parameters! EnableTextLogging = 1 (REG_DWORD-waarde)

Logboekregistratie in een tekstbestand is standaard uitgeschakeld. Het opnieuw opstarten van de DC-agent-service is vereist voor wijzigingen in deze waarde pas van kracht. Wanneer de domeincontroller ingeschakeld agent-service worden geschreven naar een logboekbestand bevindt zich onder:

`%ProgramFiles%\Azure AD Password Protection DC Agent\Logs`

> [!TIP]
> De tekstlogboek ontvangt de dezelfde foutopsporingsniveau vermeldingen die naar het traceerlogboek kunnen worden geregistreerd, maar is doorgaans in een indeling gemakkelijker om te controleren en analyseren.

> [!WARNING]
> Wanneer dit is ingeschakeld, wordt dit logboek ontvangt van een groot aantal gebeurtenissen en mogelijk van invloed op prestaties van domeincontrollers. Daarom dit uitgebreide logboek moet alleen worden ingeschakeld wanneer een probleem is vereist voor nader onderzoek en vervolgens alleen voor een minimale hoeveelheid tijd.

### <a name="azure-ad-password-protection-proxy-service"></a>Azure AD wachtwoord protection proxy-service

#### <a name="proxy-service-event-logs"></a>Proxy-service-gebeurtenislogboeken

De Proxy-service verzendt een minimale set gebeurtenissen naar de volgende Logboeken:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational`

De Proxy-service kan zich ook aanmelden uitgebreide foutopsporingsniveau traceringsgebeurtenissen voor het volgende logboekbestand:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace`

Traceerlogboekregistratie is standaard uitgeschakeld.

> [!WARNING]
> Wanneer dit is ingeschakeld, het traceerlogboek een groot aantal gebeurtenissen ontvangt en dit kan van invloed op prestaties van de proxyhost. Daarom dit logboek moet alleen worden ingeschakeld wanneer een probleem is vereist voor nader onderzoek en vervolgens alleen voor een minimale hoeveelheid tijd.

#### <a name="proxy-service-text-logging"></a>Logboekregistratie in een tekstbestand Proxy-service

De Proxy-service kan worden geconfigureerd om te schrijven naar een logboek met tekst door in te stellen van de volgende registerwaarde:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionProxy\Parameters! EnableTextLogging = 1 (REG_DWORD-waarde)

Logboekregistratie in een tekstbestand is standaard uitgeschakeld. Het opnieuw opstarten van de Proxy-service is vereist voor wijzigingen in deze waarde pas van kracht. Wanneer de service worden geschreven naar een logboekbestand bevindt zich onder Proxy ingeschakeld:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

> [!TIP]
> De tekstlogboek ontvangt de dezelfde foutopsporingsniveau vermeldingen die naar het traceerlogboek kunnen worden geregistreerd, maar is doorgaans in een indeling gemakkelijker om te controleren en analyseren.

> [!WARNING]
> Wanneer dit is ingeschakeld, wordt dit logboek ontvangt van een groot aantal gebeurtenissen en mogelijk van invloed op prestaties van domeincontrollers. Daarom dit uitgebreide logboek moet alleen worden ingeschakeld wanneer een probleem is vereist voor nader onderzoek en vervolgens alleen voor een minimale hoeveelheid tijd.

#### <a name="powershell-cmdlet-logging"></a>PowerShell-cmdlet-logboekregistratie

De meeste van de Azure AD wachtwoordbeveiliging Powershell-cmdlets wordt geschreven naar een tekstlogboek bevindt zich onder:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

Als een cmdlet-fout optreedt en de oorzaak en/of-oplossing niet direct duidelijk is, kunnen deze tekstlogboeken ook worden geraadpleegd.

### <a name="emergency-remediation"></a>Herstel voor noodgevallen

Als een situatie zich waar de DC-agentservice problemen veroorzaakt voordoet, kan de DC-agent-service worden onmiddellijk afgesloten. Filter-dll voor de DC-agent-wachtwoord nog steeds roept de service niet actief en waarschuwing gebeurtenissen (10012, 10013), maar alle binnenkomende wachtwoorden gedurende die tijd worden geaccepteerd. De DC-agent-service kan vervolgens ook worden geconfigureerd via de Windows-Service Control Manager met een opstarttype 'Uitgeschakeld' indien nodig.

### <a name="performance-monitoring"></a>Prestatiebewaking

De DC-agent-service-software wordt geïnstalleerd voor een prestatiemeteritem-object met de naam **Protection voor Azure AD-wachtwoord**. De volgende prestatiemeteritems zijn momenteel beschikbaar:

|Naam van het prestatiemeteritem voor prestaties | Description|
| --- | --- |
|Wachtwoorden verwerkt |Deze teller geeft het totale aantal wachtwoorden verwerkt (geaccepteerd of geweigerd) sinds de laatste keer opnieuw opstarten.|
|Wachtwoorden geaccepteerd |Deze teller geeft het totale aantal wachtwoorden die zijn geaccepteerd sinds de laatste keer opnieuw opstarten.|
|Wachtwoorden geweigerd |Deze teller geeft het totale aantal wachtwoorden dat sinds de laatste keer opnieuw opstarten is geweigerd.|
|Wachtwoord filteren aanvragen in uitvoering |Dit item geeft het aantal aanvragen voor wachtwoord filteren op dit moment wordt uitgevoerd.|
|Piek wachtwoord filteren aanvragen |Deze teller geeft het hoogste aantal gelijktijdige wachtwoord filteren aanvragen sinds de laatste keer opnieuw opstarten.|
|Wachtwoord filter aanvraag fouten |Deze teller geeft het totale aantal aanvragen van een wachtwoord filteren dat is mislukt vanwege een fout sinds de laatste keer opnieuw opstarten. Fouten kunnen optreden wanneer de agent-service van Azure AD wachtwoord DC-beveiliging wordt niet uitgevoerd.|
|Wachtwoord filteren aanvragen/sec |Deze teller geeft de snelheid waarmee wachtwoorden worden verwerkt.|
|Wachtwoord filter aanvraag verwerkingstijd |Dit item geeft de gemiddelde tijd die nodig is om een wachtwoord filter-aanvraag te verwerken.|
|Piek wachtwoord filter-aanvraagtijd verwerking |Dit item geeft de piek wachtwoord filter aanvraagverwerking tijd sinds de laatste keer opnieuw opstarten.|
|Wachtwoorden geaccepteerd vanwege modus controleren |Deze teller geeft het totale aantal wachtwoorden die zouden normaal geweigerd, maar zijn geaccepteerd omdat het wachtwoordbeleid is geconfigureerd om te worden in de controlemodus (sinds de laatste keer opnieuw opstarten).|

## <a name="directory-services-repair-mode"></a>Directory Services Repair Mode

Als de domeincontroller wordt opgestart naar Directory Services Repair Mode, detecteert de DC-agentservice dit veroorzaakt door alle wachtwoordvalidatie of activiteiten worden uitgeschakeld, ongeacht de configuratie van het actieve beleid afdwingen.

## <a name="domain-controller-demotion"></a>Degradatie van domeincontrollers

Als u het verlagen van een domeincontroller waarop de DC-agentsoftware nog steeds wordt ondersteund. Beheerders moeten rekening houden echter dat de DC-agentsoftware blijft actief en blijft het huidige wachtwoordbeleid afgedwongen tijdens de degradatie procedure. Nieuwe lokale wachtwoord van de beheerder (opgegeven als onderdeel van het degraderen) wordt gevalideerd, zoals elk ander wachtwoord. Microsoft raadt aan dat veilige wachtwoorden voor lokale Administrator-accounts worden gekozen als onderdeel van een domeincontroller degraderen procedure; maar de validatie van nieuwe lokale wachtwoord van de beheerder van de DC-agentsoftware mogelijk verstorend voor reeds bestaande degradatie operationele procedures.

Zodra de degradatie is voltooid en de domeincontroller opnieuw is opgestart en wordt opnieuw uitgevoerd als een normale lidserver, wordt de DC-agentsoftware die wordt uitgevoerd in een passieve modus. Deze kan vervolgens worden verwijderd op elk gewenst moment.

## <a name="removal"></a>Verwijdering

Als deze wordt besloten dat u wilt de openbare preview-software en opschonen alle gerelateerde status verwijderen uit de domeinen en forests, kan deze taak worden uitgevoerd met behulp van de volgende stappen uit:

> [!IMPORTANT]
> Het is belangrijk dat u deze stappen in volgorde uitvoeren. Als een willekeurig exemplaar van de Proxy-service actief wordt periodiek opnieuw gemaakt het object serviceConnectionPoint. Als een willekeurig exemplaar van de DC-agent-service actief wordt periodiek opnieuw gemaakt het object serviceConnectionPoint en de sysvol-status.

1. Verwijder de wachtwoordbeveiliging proxysoftware van alle computers. Deze stap wordt **niet** moeten worden opgestart.
2. Verwijder de DC-agentsoftware van alle domeincontrollers. Deze stap **vereist** opnieuw worden opgestart.
3. Verwijder handmatig alle service connection points van Proxy in elke naamgevingscontext voor domein. De locatie van deze objecten kan worden gedetecteerd met de volgende Active Directory Powershell-opdracht:

   ```Powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Voer het sterretje niet weglaten ("*") aan het einde van de variabele waarde $keywords.

   De resulterende objecten gevonden de `Get-ADObject` opdracht kan worden doorgesluisd naar `Remove-ADObject`, of handmatig verwijderd. 

4. Verwijder handmatig alle connection points van de DC-agent in elke naamgevingscontext voor domein. Er kan ook een deze objecten per domeincontroller in het forest, afhankelijk van hoe ver de openbare preview-software is geïmplementeerd. De locatie van het object kan worden gedetecteerd met de volgende Active Directory Powershell-opdracht:

   ```Powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   De resulterende objecten gevonden de `Get-ADObject` opdracht kan worden doorgesluisd naar `Remove-ADObject`, of handmatig verwijderd.

5. Verwijder handmatig de status van de configuratie van de op forestniveau. De status van het forest-configuratie wordt bijgehouden in een container in de configuratienaamgevingscontext van Active Directory. Het kan worden gedetecteerd en verwijderd als volgt:

   ```Powershell
   $passwordProtectonConfigContainer = "CN=Azure AD Password Protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject -Recursive $passwordProtectonConfigContainer
   ```

6. Handmatig verwijderen verwijdert alle sysvol status door handmatig van de volgende map en alle bijbehorende inhoud:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Indien nodig, kan dit pad ook worden geopend lokaal op een bepaalde domeincontroller. de standaardlocatie is ongeveer het volgende pad:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Dit pad verschilt als de sysvol-share op de locatie van een niet-standaard is geconfigureerd.

## <a name="next-steps"></a>Volgende stappen

Zie het artikel voor meer informatie over de algemene en aangepaste uitgesloten wachtwoordenlijsten [onjuiste wachtwoorden blokkeren](concept-password-ban-bad.md)
