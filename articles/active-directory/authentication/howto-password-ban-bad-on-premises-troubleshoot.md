---
title: Oplossen van problemen en logboekregistratie in Azure AD wachtwoord protection preview
description: Inzicht in Azure AD-wachtwoordbeveiliging logboekregistratie en het oplossen van algemene preview
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: c164c2465a4e9d60159ea9596139f2e4938f792f
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242497"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Voorbeeld: Azure AD wachtwoord beveiliging bewaking, rapportage en probleemoplossing

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord en de lijst met aangepaste uitgesloten wachtwoorden zijn openbare preview-functies van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Zijn essentiële taken na de implementatie van Azure AD-wachtwoordbeveiliging bewaking en rapportage. In dit artikel gaat in detail om u te helpen dat u leert waar elke service registreert informatie en hoe u een rapport over het gebruik van Azure AD-wachtwoordbeveiliging.

## <a name="on-premises-logs-and-events"></a>On-premises logboeken en gebeurtenissen

### <a name="dc-agent-service"></a>DC-agent-service

Op elke domeincontroller de DC-agentsoftware service schrijft de resultaten van de validaties wachtwoord (en andere status) naar een lokale gebeurtenislogboek: \Applications en Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin

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

#### <a name="sample-event-log-message-for-event-id-10014-successful-password-set"></a>Bericht van de voorbeeld-gebeurtenislogboek voor gebeurtenis-ID 10014 geslaagde wachtwoorden in te stellen

Het gewijzigde wachtwoord voor de opgegeven gebruiker is gevalideerd als compatibel met het huidige Azure-wachtwoordbeleid.

 Gebruikersnaam: BPL_02885102771 FullName:

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Bericht van de voorbeeld-gebeurtenislogboek voor gebeurtenis-ID 10017 en 30003 kan geen wachtwoorden in te stellen

10017:

Het wachtwoord opnieuw instellen voor de opgegeven gebruiker is geweigerd omdat deze niet voldeed aan de huidige Azure-wachtwoordbeleid. Raadpleeg het gebeurtenislogboek van gecorreleerde bericht voor meer informatie.

 Gebruikersnaam: BPL_03283841185 FullName:

30003:

Het wachtwoord opnieuw instellen voor de opgegeven gebruiker is geweigerd omdat deze overeenkomt met ten minste een van de tokens die aanwezig zijn in de lijst met wachtwoorden per tenant uitgesloten van het huidige Azure-wachtwoordbeleid.

 Gebruikersnaam: BPL_03283841185 FullName:

Enkele andere belangrijke gebeurtenislogboekberichten rekening mee moet houden zijn:

#### <a name="sample-event-log-message-for-event-id-30001"></a>Bericht van de voorbeeld-gebeurtenislogboek voor gebeurtenis-ID 30001

Het wachtwoord voor de opgegeven gebruiker is geaccepteerd omdat een Azure-wachtwoordbeleid nog niet beschikbaar is

Gebruikersnaam: <user> FullName: <user>

Dit probleem kan worden veroorzaakt door een of meer van de volgende redenen: % n

1. Het forest is nog niet geregistreerd bij Azure.

   Stappen voor het oplossen: een beheerder moet het forest met de cmdlet Register-AzureADPasswordProtectionForest registreren.

2. Een Azure AD-wachtwoordbeveiliging Proxy is nog niet beschikbaar is op ten minste één computer in het huidige forest.

   Stappen voor het oplossen: een beheerder moet installeren en registreren van een proxy met de cmdlet Register-AzureADPasswordProtectionProxy.

3. Deze domeincontroller heeft geen netwerkverbinding met een Proxy-exemplaren van Azure AD wachtwoord beveiliging.

   Stappen voor het oplossen: Zorg ervoor dat er een netwerkverbinding met ten minste één Azure AD wachtwoord beveiliging Proxy-exemplaar.

4. Deze domeincontroller heeft geen connectiviteit met andere domeincontrollers in het domein.

   Stappen voor het oplossen: Zorg ervoor dat er een netwerkverbinding met het domein.

#### <a name="sample-event-log-message-for-event-id-30006"></a>Bericht van de voorbeeld-gebeurtenislogboek voor gebeurtenis-ID 30006

De service is nu de volgende Azure-wachtwoordbeleid afdwingen.

 AuditOnly: 1

 Datum van globaal beleid: 2018-05-15T00:00:00.000000000Z

 Tenant-beleid datum: 2018-06-10T20:15:24.432457600Z

 Afdwingen van tenantbeleid: 1

#### <a name="dc-agent-log-locations"></a>DC-Agent logboeklocaties

De DC-agent-service wordt ook operationele-gerelateerde gebeurtenissen registreren bij het volgende logboekbestand: \Applications en Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational

De DC-agent-service kunt zich ook aanmelden uitgebreide foutopsporingsniveau traceringsgebeurtenissen voor het volgende logboekbestand: \Applications en Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace

> [!WARNING]
> Het traceerlogboek is standaard uitgeschakeld. Wanneer dit is ingeschakeld, wordt dit logboek ontvangt van een groot aantal gebeurtenissen en mogelijk van invloed op prestaties van domeincontrollers. Daarom dit uitgebreide logboek moet alleen worden ingeschakeld wanneer een probleem is vereist voor nader onderzoek en vervolgens alleen voor een minimale hoeveelheid tijd.

### <a name="azure-ad-password-protection-proxy-service"></a>Azure AD wachtwoord protection proxy-service

De wachtwoordbeveiliging Proxy-service verzendt een minimale set gebeurtenissen in het volgende gebeurtenislogboek: \Applications en Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational

De wachtwoordbeveiliging Proxy-service kan zich ook aanmelden uitgebreide foutopsporingsniveau traceringsgebeurtenissen naar het volgende logboekbestand: \Applications en Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace

> [!WARNING]
> Het traceerlogboek is standaard uitgeschakeld. Wanneer dit is ingeschakeld, dit logboek een groot aantal gebeurtenissen ontvangt en dit kan van invloed op prestaties van de proxyhost. Daarom dit logboek moet alleen worden ingeschakeld wanneer een probleem is vereist voor nader onderzoek en vervolgens alleen voor een minimale hoeveelheid tijd.

### <a name="dc-agent-discovery"></a>DC-agentdetectie

De `Get-AzureADPasswordProtectionDCAgent` cmdlet kan worden gebruikt om algemene informatie over de verschillende DC-agents die worden uitgevoerd in een domein of forest weer te geven. Deze informatie wordt opgehaald uit de serviceConnectionPoint-objecten die door de actieve DC-agent (s) zijn geregistreerd. Een voorbeeld van uitvoer van deze cmdlet is als volgt:

```
PS C:\> Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
Heartbeat             : 2/16/2018 8:35:01 AM
```

De verschillende eigenschappen worden bijgewerkt door elke agent-service van de DC bij benadering gebeurt op uurbasis. De gegevens zijn nog steeds onderworpen aan replicatievertraging van Active Directory.

Het bereik van de query van de cmdlet kan worden beïnvloed met behulp van de parameters Forest of -domein.

### <a name="emergency-remediation"></a>Herstel voor noodgevallen

Als een erg vervelend situatie zich waar de DC-agentservice problemen veroorzaakt voordoet, kan de DC-agent-service worden onmiddellijk afgesloten. De DC-agent wachtwoord filter-dll roept de service niet actief en waarschuwing gebeurtenissen (10012, 10013), maar alle binnenkomende wachtwoorden gedurende die tijd worden geaccepteerd. De DC-agent-service kan vervolgens ook worden geconfigureerd via de Windows-Service Control Manager met een opstarttype 'Uitgeschakeld' indien nodig.

### <a name="performance-monitoring"></a>Prestatiebewaking

De DC-agent-service-software wordt geïnstalleerd voor een prestatiemeteritem-object met de naam **Azure AD-wachtwoordbeveiliging**. De volgende prestatiemeteritems zijn momenteel beschikbaar:

|Naam van het prestatiemeteritem voor prestaties | Beschrijving|
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
> Het is belangrijk dat u deze stappen in volgorde uitvoeren. Als een willekeurig exemplaar van de wachtwoordbeveiliging Proxy-service wordt wordt uitgevoerd, het object serviceConnectionPoint periodiek opnieuw maken, evenals periodiek de sysvol-status opnieuw te maken.

1. Verwijder de wachtwoordbeveiliging proxysoftware van alle computers. Deze stap wordt **niet** moeten worden opgestart.
2. Verwijder de DC-agentsoftware van alle domeincontrollers. Deze stap **vereist** opnieuw worden opgestart.
3. Verwijder handmatig alle service connection points van proxy in elke naamgevingscontext voor domein. De locatie van deze objecten kan worden gedetecteerd met de volgende Active Directory Powershell-opdracht:
   ```
   $scp = "serviceConnectionPoint"
   $keywords = "{EBEFB703-6113-413D-9167-9F8DD4D24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Voer het sterretje niet weglaten ("*") aan het einde van de variabele waarde $keywords.

   Het resulterende object gevonden de `Get-ADObject` opdracht kan worden doorgesluisd naar `Remove-ADObject`, of handmatig verwijderd. 

4. Verwijder handmatig alle connection points van de DC-agent in elke naamgevingscontext voor domein. Er kan ook een deze objecten per domeincontroller in het forest, afhankelijk van hoe ver de openbare preview-software is geïmplementeerd. De locatie van het object kan worden gedetecteerd met de volgende Active Directory Powershell-opdracht:

   ```
   $scp = "serviceConnectionPoint"
   $keywords = "{B11BB10A-3E7D-4D37-A4C3-51DE9D0F77C9}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Het resulterende object gevonden de `Get-ADObject` opdracht kan worden doorgesluisd naar `Remove-ADObject`, of handmatig verwijderd.

5. Verwijder handmatig de status van de configuratie van de op forestniveau. De status van het forest-configuratie wordt bijgehouden in een container in de configuratienaamgevingscontext van Active Directory. Het kan worden gedetecteerd en verwijderd als volgt:

   ```
   $passwordProtectonConfigContainer = "CN=Azure AD password protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject $passwordProtectonConfigContainer
   ```

6. Handmatig verwijderen verwijdert alle sysvol status door handmatig van de volgende map en alle bijbehorende inhoud:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Indien nodig, kan dit pad ook worden geopend lokaal op een bepaalde domeincontroller. de standaardlocatie is ongeveer het volgende pad:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Dit pad verschilt als de sysvol-share op de locatie van een niet-standaard is geconfigureerd.

## <a name="next-steps"></a>Volgende stappen

Zie het artikel voor meer informatie over de algemene en aangepaste uitgesloten wachtwoordenlijsten [onjuiste wachtwoorden blokkeren](concept-password-ban-bad.md)
